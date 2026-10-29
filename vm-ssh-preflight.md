---
description: Use the AutoStopping SSH pre-flight script to reach a stopped VM with an ordinary ssh command.
---


# Connect to AutoStopping VMs over SSH

AutoStopping shuts down a VM when nobody is using it, and starts it again when traffic
arrives. For HTTP workloads the AutoStopping proxy handles that transparently: the first
request is held while the VM boots, and the user sees nothing but a slow page load.

SSH has no equivalent, because `ssh` gives up before a VM can finish starting. The SSH
pre-flight script closes that gap. It starts the VM, waits for it to accept connections,
and then lets your normal `ssh` command run — unchanged.

## When to use this

Use the pre-flight script when all of the following are true.

- You have AutoStopping rules on VMs that people reach over SSH, SFTP, SCP or rsync. Go to [AutoStopping rules](../../cost-optimization/autostopping-rules/README.md) to review how VM rules work.
- Those people connect from a shared host you control — a bastion, jump box, build agent
  or developer workstation — where you can install a script.
- You want the `ssh` command itself to stay exactly as it is today, including the hostname
  or IP that people have memorised, scripted and put in their `~/.ssh/config`.

If instead your users can be given a new connection command, the AutoStopping proxy's
[TCP mode](../../references/onboarding/autostopping/proxy.md) is simpler: it listens on a
per-rule port and requires no script. The pre-flight script exists for the case where
changing the command is not an option.

## Why ssh alone cannot wait

OpenSSH's `ConnectTimeout` covers both the TCP connection and the start of the protocol
handshake. Pointed at a stopped VM, `ssh` fails with
`Connection timed out during banner exchange` in a few seconds, while the VM typically
needs 30 to 60 seconds to boot and start `sshd`. Raising `ConnectTimeout` is often
disallowed by a hardened SSH configuration, and in any case it makes every genuine
failure slow for everyone.

Doing the wait **before** `ssh` starts avoids the conflict entirely. By the time `ssh`
runs, the VM is up, so no timeout is involved and no SSH setting changes.

## How it works

1. The script sends the target address to the AutoStopping API, which finds the rules
   covering that address and starts them.
2. It polls until Harness reports the VM running, then waits for the SSH port to answer.
   The cloud reports a VM as running as soon as the hypervisor has started it, which is
   some seconds before `sshd` is listening, so the script absorbs that gap too.
3. It hands over, and your `ssh` command runs normally.
4. A background process reports every two minutes that the session is alive, so
   AutoStopping does not treat the VM as idle while someone is connected. It watches the
   session and stops reporting on its own when the session ends.

The script only ever starts VMs and keeps them running. It cannot stop a VM, change a
rule, or block a connection. If anything goes wrong — the API is unreachable, the address
matches no rule, the credential is missing, the wait times out — it prints a short message
and lets `ssh` run anyway. The worst outcome of a failure is the behaviour you have today.

Being able to start a VM grants no access to it. Who may log in is still decided entirely
by the SSH accounts and keys on the VM, exactly as before.

## Before you begin

- A VM-based AutoStopping rule. An idle time of 10 minutes or more is recommended: the
  minimum of 5 minutes works, but leaves less margin if an activity report is lost.
- A host running RHEL, Ubuntu or any Linux with `bash` and `curl`. `curl` is often absent
  on Ubuntu cloud images — install it first.
- Network access from that host to `app.harness.io` (or your Harness cluster's URL) on
  port 443.
- A Harness API key whose role grants **View** on **AutoStopping Rules** in CCM. No other
  permission is needed; the script does not create or modify anything.

{% hint style="info" %}
The rule must match the address people connect to. Private IP addresses survive a stop
and start and are the expected case. An automatically assigned public IP changes every
time a VM restarts, so it cannot be relied on — use a static or Elastic address if your
users connect over the public network.
{% endhint %}

## Install the script

Download the current version and verify it:

```sh
base=https://lightwing-downloads.s3.amazonaws.com/autostopping-ssh-preflight
version=1.0.0

curl -fsSLO "$base/harness-autostop-ssh-preflight-$version.tar.gz"
curl -fsSLO "$base/harness-autostop-ssh-preflight-$version.tar.gz.sha256"
sha256sum -c "harness-autostop-ssh-preflight-$version.tar.gz.sha256"
tar -xzf "harness-autostop-ssh-preflight-$version.tar.gz"
cd "harness-autostop-ssh-preflight-$version"
```

The archive contains the script, a sample configuration file and a copy of this
information for offline review. The script is plain `bash` and is meant to be read before
it is installed — there is deliberately no one-line installer that pipes a download into
a root shell.

Install it where its users can run it but not modify it:

```sh
sudo install -o root -g root -m 755 harness_autostop_ssh_proxy.sh \
    /usr/local/bin/harness-autostop-ssh-proxy

harness-autostop-ssh-proxy --version
```

## Configure it

Create the configuration file. It is read as plain `key = value` data and is never
executed, so it cannot be used to run commands:

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

Then store the API key in a file that only the user running the script can read. The
script refuses a key file that anyone else can read — group-readable included — because
the key can start VMs in your account:

```sh
sudo install -m 600 -o "$USER" /dev/null /etc/harness/autostop-api-key
sudo sh -c 'printf %s "<your API key>" > /etc/harness/autostop-api-key'
```

{% hint style="warning" %}
The key file must be owned by the account that runs the script. If it is left owned by
`root` and your engineers connect as `ec2-user` or `ubuntu`, the script cannot read the
key, reports that it cannot, and hands over to `ssh` without starting the VM.

Do not work around this by running the script under `sudo`: `ssh` would then look for
root's SSH keys instead of the user's.
{% endhint %}

Where several people share the host and a single owner-only file will not do, supply the
key through the `HARNESS_API_KEY` environment variable instead — set from each user's
profile, or from your secret manager's agent. Never pass the key as a command-line
argument; arguments are visible to every user on the host through `ps`.

## Wire it into how people connect

### If your users connect through a wrapper script

Add one line above the existing `ssh` command. Nothing else changes:

```sh
harness-autostop-ssh-proxy --host "$target" || true
ssh "$target" "$@"
```

The `|| true` is belt and braces — the script already succeeds on every failure path —
but it makes the intent plain to whoever reads the wrapper next.

If the wrapper ends in `exec ssh …`, you are done. Activity reporting follows the
wrapper's process, and `exec` keeps the same process, so reporting tracks the SSH session
and stops when it ends.

### If your users run ssh directly

Use OpenSSH's own `Match exec` to run the script before any connection to the managed
hosts. Each user adds this to `~/.ssh/config`, or you add it to `/etc/ssh/ssh_config`:

```
Match host 10.0.1.* exec "harness-autostop-ssh-proxy --host %h --port %p"
```

`ssh` runs the `exec` command before connecting, so the VM is started and ready by the
time the connection is attempted. Users keep typing `ssh 10.0.1.23` exactly as they do
today.

### For automation and one-off commands

Let the script run the command itself. It then tracks that command's lifetime precisely:

```sh
harness-autostop-ssh-proxy --host 10.0.1.23 -- ssh 10.0.1.23 'systemctl status myapp'
```

This is the most reliable form, because the script knows exactly when the session ends,
and it is the right choice for cron jobs, CI steps and backup scripts.

## Verify it

Stop the VM through its AutoStopping rule, wait for the rule to show as stopped, then
connect the way your users do. With `--verbose` you can watch what happens:

```
$ harness-autostop-ssh-proxy --host 10.0.1.23 --verbose
harness-autostop-ssh-proxy: starting 10.0.1.23, waiting up to 300s
harness-autostop-ssh-proxy: 10.0.1.23 started after 21s
harness-autostop-ssh-proxy: 10.0.1.23 is ready after 22s
```

The connection then succeeds immediately. In the Harness UI the rule shows as active, and
it stays active for as long as you remain connected.

## Options

Every option can also come from the configuration file or the environment, so that the
connection command itself needs nothing but `--host`.

| Option | Config key | Environment variable | Default |
| --- | --- | --- | --- |
| `--host <address>` | — | — | required |
| `--port <port>` | — | — | `22` |
| `--account-id <id>` | `account_id` | `HARNESS_ACCOUNT_ID` | — |
| `--api-url <url>` | `api_url` | `HARNESS_AUTOSTOP_API_URL` | `https://app.harness.io/gateway/lw/api` |
| `--api-key-file <path>` | `api_key_file` | `HARNESS_API_KEY_FILE` | — |
| `--config <path>` | — | `HARNESS_AUTOSTOP_CONFIG` | `/etc/harness/autostop-preflight.conf` |
| `--timeout <secs>` | `timeout` | — | `300` |
| `--report-interval <secs>` | `report_interval` | — | `120` |
| `--log-file <path>` | `log_file` | `HARNESS_AUTOSTOP_LOG` | discard |
| `--no-report` | — | — | off |
| `--verbose` | — | — | off |
| `--version` | — | — | — |

The API key itself is read only from `HARNESS_API_KEY` or from the key file. It is never
accepted as an argument.

{% hint style="success" %}
Keep `report_interval` well below half the rule's idle time. At the default of 120 seconds
with a 10-minute idle time, several reports have to be lost in a row before a VM could be
stopped under a live session.
{% endhint %}

## Troubleshooting

**`cannot read API key file`** — the file is not readable by the user running the script,
or it is readable by more than its owner and was refused. Check ownership with
`ls -l /etc/harness/autostop-api-key`; the owner must be the connecting user and the mode
must be `600`.

**`missing required commands`** — `curl` is usually the one absent, most often on Ubuntu
cloud images. The script names what is missing and lets `ssh` run anyway.

**The VM is not started, and the script says nothing useful** — run it with `--verbose`.
If it reports no matching rule, the address the users connect to is not the address
Harness has for the VM. Compare it with the VM's address on the rule's detail page. NAT,
PrivateLink and dynamically assigned public IPs all produce this.

**The VM starts but the connection still fails** — the script waited for the SSH port and
gave up within the timeout, which usually means a security group or firewall is blocking
the port from this host. The connection would have failed anyway; the script only makes it
fail later. Check `--timeout` has not been set too low as well.

**The VM is stopped while someone is still connected** — activity reporting ended early.
This happens when the script is called from a short-lived shell that exits before `ssh`
starts, because in that mode it watches the process that called it. Use the `Match exec`
or `-- <command>` forms instead, or make the wrapper end in `exec ssh`.

**Errors against a specific rule** — when Harness cannot start a rule for a session, the
reason is recorded in that rule's logs in the Harness UI, not on the host. The host only
ever sees that it should hand over to `ssh`.

## Limitations

- The address must be one Harness can see on the VM. Addresses translated in transit by
  NAT or PrivateLink never match, and the script falls through to a plain `ssh`.
- An automatically assigned public IP changes on every restart, so a remembered public
  address will not reach the VM regardless of this script. Use static addresses, or
  private ones.
- If the reporting process is killed without the session ending, the VM may be stopped
  mid-session. A generous idle time on the rule is the mitigation.
- The script covers SSH and anything built on it — SFTP, SCP, rsync, `git` over SSH, and
  port forwarding. It is not specific to interactive logins.

{% @harness-feedback/feedback %}
