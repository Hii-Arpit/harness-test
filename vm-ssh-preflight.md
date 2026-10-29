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

The pre-flight script removes the conflict by doing the waiting before `ssh` runs. By the time `ssh` connects, the VM is already up, so no timeout occurs and no SSH configuration changes are required.

---

## How it works

When a user connects to a stopped VM, the script starts the AutoStopping rules that cover the target address and waits until the VM is running and SSH is answering. It then hands over to `ssh`, which connects to a VM that is already up. While the session is open, the script reports activity every two minutes so AutoStopping does not treat the VM as idle and stop it mid-session. When the session ends, reporting stops and the rule resumes its normal idle countdown.

{% hint style="info" %}
### What the script can and cannot do

The script can only start VMs and keep them running while someone is connected. It cannot stop a VM, change an AutoStopping rule, or block anyone's connection. Starting a VM does not grant access to it either: as always, only the SSH accounts and keys on the VM decide who can log in.

If anything goes wrong, for example the Harness API is unreachable, the script prints a short message and runs `ssh` anyway. The worst that can happen is the same behavior you have today without it.
{% endhint %}

---

## Before you begin

- **A VM-based [AutoStopping rule](../../cost-optimization/autostopping-rules/README.md)** that covers the address your users connect to. The default idle time of 15 minutes or more leaves margin for a missed activity report.
- **A shared Linux host you control** (bastion, jump box, build agent, or workstation) where your users run `ssh` and where you have admin access to install the script. The script needs `bash` and `curl`, so confirm both are installed on the host and install them if they are not.
- **Network access** from that host to `app.harness.io`, or your Harness cluster URL, on port 443.
- **A Harness API key and account ID.** The key's role must view and start the covering rules; see [Granular RBAC for AutoStopping](rbac-autostopping.md). Use a key with a long or no expiration. Go to [Add and manage API keys](https://developer.harness.io/harness-ai/use-harness-platform/automation/api/add-and-manage-api-keys) to create one.

{% hint style="info" %}
The rule must match the address your users connect to. Private IP addresses survive a stop and start and are the expected case. An automatically assigned public IP changes every time a VM restarts, so use a static or Elastic address if your users connect over the public network.
{% endhint %}

---

## Set up SSH pre-flight access

Set up the script once on the shared host, then wire it into the way your users already connect.

### Install the script

1. Download the current version and verify its checksum:

   {% code overflow="wrap" %}
   ```sh
   base=https://lightwing-downloads.s3.amazonaws.com/autostopping-ssh-preflight
   version=1.0.0

   curl -fsSLO "$base/harness-autostop-ssh-preflight-$version.tar.gz"
   curl -fsSLO "$base/harness-autostop-ssh-preflight-$version.tar.gz.sha256"
   sha256sum -c "harness-autostop-ssh-preflight-$version.tar.gz.sha256"
   tar -xzf "harness-autostop-ssh-preflight-$version.tar.gz"
   cd "harness-autostop-ssh-preflight-$version"
   ```
   {% endcode %}

2. Install the script so users can run it but not change it. It is plain `bash`, so review it first if you want:

   {% code overflow="wrap" %}
   ```sh
   sudo install -o root -g root -m 755 harness_autostop_ssh_proxy.sh \
       /usr/local/bin/harness-autostop-ssh-proxy
   ```
   {% endcode %}

3. Confirm the version:

   {% code overflow="wrap" %}
   ```sh
   harness-autostop-ssh-proxy --version
   ```
   {% endcode %}

   {% code overflow="wrap" %}
   ```
   harness-autostop-ssh-proxy 1.0.0
   ```
   {% endcode %}

### Configure the script

With the script installed, the next step is to tell it how to reach your Harness account. The script reads these details, your account ID, the API endpoint, and an API key, from a configuration file.

1. Create the configuration file:

   {% code overflow="wrap" %}
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
   {% endcode %}

   This creates `/etc/harness/autostop-preflight.conf`. Replace `<your Harness account ID>` with your account ID, and leave `timeout` and `report_interval` at their defaults.

2. Provide your API key. The script reads it from one of two safe places. Choose the one that fits your host:

{% tabs %}
{% tab title="Key file (single user)" %}
Use a key file when a single user runs the script. Store the key in its own file, kept separate from the configuration so it is not exposed:

{% code overflow="wrap" %}
```sh
sudo install -m 600 -o "$USER" /dev/null /etc/harness/autostop-api-key
sudo sh -c 'printf %s "<your API key>" > /etc/harness/autostop-api-key'
```
{% endcode %}

Replace `<your API key>` with your Harness API key. If you do not have one yet, go to [Add and manage API keys](https://developer.harness.io/harness-ai/use-harness-platform/automation/api/add-and-manage-api-keys) to create a key with the permissions listed in [Before you begin](#before-you-begin). The commands lock the file to its owner, so only that user can run the script.

{% hint style="warning" %}
### Key file ownership

The key file must be owned by the user who runs the script. If `root` owns it while your engineers connect as `ec2-user` or `ubuntu`, the script cannot read the key and hands over to `ssh` without starting the VM. Running the script under `sudo` is not a valid approach, because `ssh` would then run as `root` and use root's SSH keys instead of the user's, so the user's own key is never offered and the login fails.
{% endhint %}
{% endtab %}

{% tab title="Environment variable (shared host)" %}
Use the `HARNESS_API_KEY` environment variable on a shared host, where several people log in under their own accounts. Each user supplies their own key in their own session, so no single owner-only file is needed:

{% code overflow="wrap" %}
```sh
export HARNESS_API_KEY="<your API key>"
```
{% endcode %}

Replace `<your API key>` with a Harness API key whose role can view and start the covering rules. If you do not have one yet, go to [Add and manage API keys](https://developer.harness.io/harness-ai/use-harness-platform/automation/api/add-and-manage-api-keys) to create a key with the permissions listed in [Before you begin](#before-you-begin).

Set the variable in each user's shell profile, such as `.bashrc`, so it persists across sessions, or have a secret manager agent inject it at login. Either way, each person's key stays in their own environment, out of everyone else's reach.
{% endtab %}
{% endtabs %}

Whichever method you use, never pass the key on the command line, for example `--api-key <key>`. Any user on the host can see running commands and their arguments with `ps`, so a key passed there is exposed to everyone. The key file and the environment variable both keep it hidden.

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

### Verify it

1. Stop the VM through its AutoStopping rule and wait for the rule to show as stopped.
2. Connect the way your users do, adding `--verbose` to watch what happens:

   {% code overflow="wrap" %}
   ```sh
   harness-autostop-ssh-proxy --host 10.0.1.23 --verbose
   ```
   {% endcode %}

   You see output similar to this:

   {% code overflow="wrap" %}
   ```
   harness-autostop-ssh-proxy: starting 10.0.1.23, waiting up to 300s
   harness-autostop-ssh-proxy: 10.0.1.23 started after 21s
   harness-autostop-ssh-proxy: 10.0.1.23 is ready after 22s
   ```
   {% endcode %}

The connection then succeeds immediately. In the Harness UI the rule shows as active, and it stays active for as long as you remain connected.

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
