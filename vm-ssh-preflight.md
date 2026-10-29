---
description: Use the AutoStopping SSH pre-flight script to reach a stopped VM with an ordinary ssh command.
---


# Connect to AutoStopping VMs over SSH

AutoStopping shuts down a VM when nobody is using it and starts it again when traffic arrives. For HTTP workloads this is transparent, but a plain `ssh` command gives up before a stopped VM can finish starting. The SSH pre-flight script closes that gap: it starts the VM, waits for it to accept connections, and then lets your normal `ssh` command run without any change.

{% hint style="success" %}
### Consider TCP mode instead

If your users can adopt a new connection command, the AutoStopping proxy's [TCP mode](../../references/onboarding/autostopping/proxy.md) is simpler and needs no script. Use the pre-flight script only when the `ssh` command must stay the same.
{% endhint %}

---

## What you will learn from this topic

- Why SSH connections fail against stopped AutoStopping VMs, and how the pre-flight script fixes it.
- How the connection flow works once the script is set up.
- How to install, configure, and wire the script into the way your users already connect.
- How to verify it, troubleshoot common issues, and what the current limitations are.

---

## Why SSH needs a pre-flight step

AutoStopping lowers your cloud bill by stopping idle VMs and starting them again when they are needed. For web traffic this is seamless: the AutoStopping proxy receives the first request, starts the VM, and holds the request until the VM is ready. The user sees only a slower first page load.

SSH behaves differently. The `ssh` command expects a response within a few seconds and fails if it does not receive one. A stopped VM needs 30 to 60 seconds to boot and start its SSH service (`sshd`), which is longer than `ssh` waits, so the connection fails with `Connection timed out during banner exchange` before the VM is ready.

Increasing the `ssh` timeout is not a reliable workaround. The `ConnectTimeout` setting is often restricted by security policy, and a higher value slows down every failed connection, not only the first start.

The pre-flight script runs before `ssh` and performs the wait itself. It starts the VM, waits until the VM is running and accepting SSH connections, and then runs your `ssh` command. Because the VM is already running when `ssh` connects, no timeout occurs and no SSH configuration changes are required.

---

## How it works

When the script is installed and wired in, a connection to a stopped VM follows this path:

1. A user runs their usual `ssh` command, or the wrapper or automation that calls the script.
2. The script sends the target address to the AutoStopping API, which finds the rules covering that address and starts them.
3. The script polls until Harness reports the VM running, then waits for the SSH port to answer. The cloud marks a VM running as soon as the hypervisor starts it, a few seconds before `sshd` is listening, so the script waits out that gap too.
4. The script hands over, and the `ssh` command connects normally against a VM that is already up.
5. A background process reports every two minutes that the session is alive, so AutoStopping does not treat the VM as idle while someone is connected.
6. When the session ends, the background process stops reporting on its own, and the rule returns to its normal idle countdown.

{% hint style="info" %}
The script only starts VMs and keeps them running. It cannot stop a VM, change a rule, or block a connection, and starting a VM grants no access to it: who may log in is still decided by the SSH accounts and keys on the VM. If anything fails, the script prints a short message and lets `ssh` run anyway, so the worst outcome of a failure is the behavior you have today.
{% endhint %}

---

## Before you begin

- **A VM-based AutoStopping rule:** An idle time of 10 minutes or more is recommended. The minimum of 5 minutes works, but leaves less margin if an activity report is lost. Go to [AutoStopping rules](../../cost-optimization/autostopping-rules/README.md) to create or review a rule.
- **A shared host you control:** A bastion, jump box, build agent, or developer workstation where you can install a script and where your users run `ssh`.
- **A supported host OS:** RHEL, Ubuntu, or any Linux with `bash` and `curl`. `curl` is often absent on Ubuntu cloud images, so install it first.
- **Network access** from that host to `app.harness.io` (or your Harness cluster URL) on port 443.
- **A Harness API key** whose role grants **View** on **AutoStopping Rules** in CCM. No other permission is needed; the script does not create or modify rules.

{% hint style="info" %}
The rule must match the address your users connect to. Private IP addresses survive a stop and start and are the expected case. An automatically assigned public IP changes every time a VM restarts, so use a static or Elastic address if your users connect over the public network.
{% endhint %}

---

## Set up SSH pre-flight access

Set up the script once on the shared host, then wire it into the way your users already connect.

{% stepper %}
{% step %}
### Install the script

Download the current version and verify its checksum:

```sh
base=https://lightwing-downloads.s3.amazonaws.com/autostopping-ssh-preflight
version=1.0.0

curl -fsSLO "$base/harness-autostop-ssh-preflight-$version.tar.gz"
curl -fsSLO "$base/harness-autostop-ssh-preflight-$version.tar.gz.sha256"
sha256sum -c "harness-autostop-ssh-preflight-$version.tar.gz.sha256"
tar -xzf "harness-autostop-ssh-preflight-$version.tar.gz"
cd "harness-autostop-ssh-preflight-$version"
```

The archive contains the script, a sample configuration file, and an offline copy of this page. The script is plain `bash` and is meant to be read before you install it; there is deliberately no one-line installer that pipes a download into a root shell.

Install it where its users can run it but not modify it:

```sh
sudo install -o root -g root -m 755 harness_autostop_ssh_proxy.sh \
    /usr/local/bin/harness-autostop-ssh-proxy

harness-autostop-ssh-proxy --version
```
{% endstep %}

{% step %}
### Configure the script

Create the configuration file. It is read as plain `key = value` data and is never executed, so it cannot be used to run commands:

```sh
sudo mkdir -p /etc/harness
sudo tee /etc/harness/autostop-preflight.conf >/dev/null <<'CONF'
account_id      = <your Harness account ID>
api_url         = https://app.harness.io/gateway/lw/api
api_key_file    = /etc/harness/autostop-api-key
timeout         = 300
report_interval = 120
CONF
sudo chmod 644 /etc/harness/autostop-preflight.conf
```

Then store the API key in a file that only the user running the script can read. The script refuses a key file that anyone else can read, group-readable included, because the key can start VMs in your account:

```sh
sudo install -m 600 -o "$USER" /dev/null /etc/harness/autostop-api-key
sudo sh -c 'printf %s "<your API key>" > /etc/harness/autostop-api-key'
```

{% hint style="warning" %}
The key file must be owned by the account that runs the script. If it is left owned by `root` and your engineers connect as `ec2-user` or `ubuntu`, the script cannot read the key, reports that it cannot, and hands over to `ssh` without starting the VM.

Do not work around this by running the script under `sudo`: `ssh` would then look for root's SSH keys instead of the user's.
{% endhint %}

Where several people share the host and a single owner-only file will not do, supply the key through the `HARNESS_API_KEY` environment variable instead, set from each user's profile or from your secret manager's agent. Never pass the key as a command-line argument; arguments are visible to every user on the host through `ps`.
{% endstep %}

{% step %}
### Wire it into how people connect

Choose the pattern that matches how your users connect today.

**Through a wrapper script.** Add one line above the existing `ssh` command. Nothing else changes:

```sh
harness-autostop-ssh-proxy --host "$target" || true
ssh "$target" "$@"
```

The `|| true` is a safeguard: the script already succeeds on every failure path, but it makes the intent plain to whoever reads the wrapper next. If the wrapper ends in `exec ssh …`, activity reporting follows the wrapper's process, and `exec` keeps the same process, so reporting tracks the SSH session and stops when it ends.

**Directly with `ssh`.** Use OpenSSH's own `Match exec` to run the script before any connection to the managed hosts. Each user adds this to `~/.ssh/config`, or you add it to `/etc/ssh/ssh_config`:

```
Match host 10.0.1.* exec "harness-autostop-ssh-proxy --host %h --port %p"
```

`ssh` runs the `exec` command before connecting, so the VM is started and ready by the time the connection is attempted. Users keep typing `ssh 10.0.1.23` exactly as they do today.

**For automation and one-off commands.** Let the script run the command itself, so it tracks that command's lifetime precisely:

```sh
harness-autostop-ssh-proxy --host 10.0.1.23 -- ssh 10.0.1.23 'systemctl status myapp'
```

This is the most reliable form, because the script knows exactly when the session ends. It is the right choice for cron jobs, CI steps, and backup scripts.
{% endstep %}

{% step %}
### Verify it

Stop the VM through its AutoStopping rule, wait for the rule to show as stopped, then connect the way your users do. With `--verbose` you can watch what happens:

```
$ harness-autostop-ssh-proxy --host 10.0.1.23 --verbose
harness-autostop-ssh-proxy: starting 10.0.1.23, waiting up to 300s
harness-autostop-ssh-proxy: 10.0.1.23 started after 21s
harness-autostop-ssh-proxy: 10.0.1.23 is ready after 22s
```

The connection then succeeds immediately. In the Harness UI the rule shows as active, and it stays active for as long as you remain connected.
{% endstep %}
{% endstepper %}

---

## Command-line options

Every option can also come from the configuration file or the environment, so the connection command itself needs nothing but `--host`.

| Option | Config key | Environment variable | Default |
| --- | --- | --- | --- |
| `--host <address>` | N/A | N/A | required |
| `--port <port>` | N/A | N/A | `22` |
| `--account-id <id>` | `account_id` | `HARNESS_ACCOUNT_ID` | N/A |
| `--api-url <url>` | `api_url` | `HARNESS_AUTOSTOP_API_URL` | `https://app.harness.io/gateway/lw/api` |
| `--api-key-file <path>` | `api_key_file` | `HARNESS_API_KEY_FILE` | N/A |
| `--config <path>` | N/A | `HARNESS_AUTOSTOP_CONFIG` | `/etc/harness/autostop-preflight.conf` |
| `--timeout <secs>` | `timeout` | N/A | `300` |
| `--report-interval <secs>` | `report_interval` | N/A | `120` |
| `--log-file <path>` | `log_file` | `HARNESS_AUTOSTOP_LOG` | discard |
| `--no-report` | N/A | N/A | off |
| `--verbose` | N/A | N/A | off |
| `--version` | N/A | N/A | N/A |

The API key itself is read only from `HARNESS_API_KEY` or from the key file. It is never accepted as an argument.

{% hint style="success" %}
Keep `report_interval` well below half the rule's idle time. At the default of 120 seconds with a 10-minute idle time, several reports must be lost in a row before a VM could be stopped under a live session.
{% endhint %}

---

## Troubleshooting

<details>

<summary>The script reports it cannot read the API key file</summary>

The file is not readable by the user running the script, or it is readable by more than its owner and was refused. Check ownership with `ls -l /etc/harness/autostop-api-key`: the owner must be the connecting user and the mode must be `600`.

</details>

<details>

<summary>The script reports missing required commands</summary>

`curl` is usually the one absent, most often on Ubuntu cloud images. The script names what is missing and lets `ssh` run anyway, so install the named command and try again.

</details>

<details>

<summary>The VM is not started and the script gives no useful message</summary>

Run the script with `--verbose`. If it reports no matching rule, the address your users connect to is not the address Harness holds for the VM. Compare it with the VM's address on the rule's detail page. NAT, PrivateLink, and dynamically assigned public IPs all cause this.

</details>

<details>

<summary>The VM starts but the connection still fails</summary>

The script waited for the SSH port and gave up within the timeout, which usually means a security group or firewall is blocking the port from this host. The connection would have failed anyway; the script only makes it fail later. Check that `--timeout` is not set too low as well.

</details>

<details>

<summary>The VM is stopped while someone is still connected</summary>

Activity reporting ended early. This happens when the script is called from a short-lived shell that exits before `ssh` starts, because in that mode it watches the process that called it. Use the `Match exec` or `-- <command>` form instead, or make the wrapper end in `exec ssh`.

</details>

<details>

<summary>A specific rule fails to start</summary>

When Harness cannot start a rule for a session, the reason is recorded in that rule's logs in the Harness UI, not on the host. The host only ever sees that it should hand over to `ssh`.

</details>

---

## Limitations

- **Addresses translated in transit do not match.** The address must be one Harness can see on the VM. Addresses translated by NAT or PrivateLink never match, and the script falls through to a plain `ssh`.
- **Dynamic public IPs are unreliable.** An automatically assigned public IP changes on every restart, so a remembered public address does not reach the VM regardless of this script. Use static or private addresses.
- **A killed reporter can end a session early.** If the reporting process is killed without the session ending, the VM may be stopped mid-session. A generous idle time on the rule is the mitigation.
- **It covers SSH and anything built on it.** SFTP, SCP, rsync, `git` over SSH, and port forwarding are all covered. It is not specific to interactive logins.

---

## Next steps

You have set up SSH pre-flight access on your shared host, and your users can reach AutoStopping-managed VMs over SSH without changing their `ssh` command or losing the savings from stopping idle VMs.

- [AutoStopping idle VMs behind a reverse proxy](vm-reverse-proxy.md): Apply the same warm-up behavior to HTTP workloads behind a proxy.
- [AutoStopping rules](../../cost-optimization/autostopping-rules/README.md): Review and tune the rules that cover your VMs.
- [AutoStopping proxy](../../references/onboarding/autostopping/proxy.md): Set up the proxy and its TCP mode for non-HTTP traffic.

{% @harness-feedback/feedback %}
