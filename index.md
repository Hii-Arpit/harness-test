---
description: >-
  Install and manage the Harness AI DLC Agent to collect AI coding agent
  telemetry data and power the AI Engineering dashboard.
---

# Harness AI DLC Agent [Beta]

{% if "HAS_FEATURE_FLAG" === true %}
```
{
    "featureFlags": [{
        "key": "USE_AIDI_MFE",
        "name": "Enable AI Engineering Insights",
        "status": "GA",
        "description": "Enables the AI Engineering Insights experience in AI DLC Insights, replacing the existing AI Insights experience with enhanced AI capabilities and a redesigned interface."
    }]
}
```
{% endif %}

The Harness AI DLC Agent is a lightweight, user-space daemon installed on developer workstations to collect telemetry data from AI coding tools such as Claude Code, Cursor, and GitHub Copilot. It correlates this telemetry with git activity and transmits it to the [AI Engineering Insights](../../view-insights/ai-engineering.md) dashboard, where engineering leaders can measure AI coding tool adoption, optimize token spend, and quantify the impact of AI on software delivery.

<figure><img src="../../../.gitbook/assets/ai-eng-17.png" alt=""><figcaption></figcaption></figure>

&#x20;_The AI Engineering dashboard displaying metrics such as session tracking, token spend, commit attribution, ship rate, and error rate._

### Supported coding agents <a href="#supported-coding-agents" id="supported-coding-agents"></a>

The following table lists coding agents and the telemetry capabilities the Harness AI DLC Agent can capture for each.

| Agent           | Code Attribution | Transcripts | Token Usage |
| --------------- | ---------------- | ----------- | ----------- |
| Claude Code     | Yes              | Yes         | Yes         |
| Cursor          | Yes              | Yes         | Yes         |
| GitHub Copilot  | Yes              | Yes         | Yes         |
| Codex           | No               | No          | No          |
| Windsurf        | No               | No          | No          |
| Devin           | No               | No          | No          |
| Gemini          | No               | No          | No          |
| Droid           | No               | No          | No          |

{% hint style="info" %}
Claude Code support includes Claude Code via Bedrock, Vertex, and other cloud vendors, as well as the Claude Code CLI. GitHub Copilot support includes Copilot CLI and Copilot on any IDE (agent mode only). Cursor support includes Cursor IDE and Cursor agent mode.
{% endhint %}

### Scope of data collection <a href="#scope-of-data-collection" id="scope-of-data-collection"></a>

| Always collected (metadata)                  | Configurable (off by default) | Never collected               |
| -------------------------------------------- | ----------------------------- | ----------------------------- |
| Token counts (input, output, cache)          | Prompt content                | Code content or file contents |
| Tool usage events and session durations      | AI response content           | Credentials or secrets        |
| AI vs. human line attribution counts         |                               |                               |
| Cost estimates                               |                               |                               |
| Session details (tool, duration, timestamps) |                               |                               |

{% hint style="info" %}
Prompt and AI response collection is disabled by default. An administrator can enable it during MDM deployment. When disabled, only metadata (token counts, tool usage, attribution) is transmitted.
{% endhint %}

### Security <a href="#security" id="security"></a>

Individual developers do not need to take action beyond having the agent installed and running, which is handled via MDM deployment (IT/EngOps owned). The agent runs in the background and captures signals from AI coding tool activity.

Go to [Data Security](data-security.md#data-collected) to review the full list of collected data types and security controls.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* **Operating system:**
  * **macOS** 12+ (Monterey or later), Intel or Apple Silicon.
  * **Windows** 10 or Windows 11, on x64 or arm64.
* **PowerShell (Windows only):** Windows PowerShell 5.1 or later.
* **Privileges:** A standard user account is sufficient for **running** the agent — it operates entirely in user space and needs no elevated privileges. **Installing** it requires elevation on both platforms: run the installation script with `sudo` on macOS, or from an elevated (**Run as administrator**) PowerShell session on Windows. The background processes the script registers then run non-elevated as the signed-in user.
* **Network:** Outbound HTTPS to `*.harness.io` on port 443.
* **Harness account:** Active Harness account with the AI Engineering Insights module enabled.
* **Role:** You must be an **Account Admin** in Harness to generate a scoped service token.

### Install the agent <a href="#install-the-agent" id="install-the-agent"></a>

### Step 1: Generate an API token <a href="#step-1-generate-an-api-token" id="step-1-generate-an-api-token"></a>

1. Sign in to your Harness account and navigate to the **Engineering Insights** module.
2. Go to the **AI Engineering** tab and select **Diagnostics**.
3. Expand **AI DLC Agent Configuration**.
4. Under **Generate scoped token**, select **Generate scoped token**.
5. When **Token generated successfully** appears, a message below the token states that it is scoped to your organization and limits the agent to telemetry-write operations only. Select the copy icon and store the token securely. The token is masked by default. You need it for the installation script in Step 3.

![The AI DLC Agent Configuration panel, with the platform downloads and a generated scoped token](../../../.gitbook/assets/agent-5.png)

### Step 2: Download the agent binary <a href="#step-2-download-the-agent-binary" id="step-2-download-the-agent-binary"></a>

In **Download for your platform**, select the download icon for the operating system and architecture used by developers in your organization. The agent is distributed as a signed, versioned binary for macOS (**Mac (ARM64)** and **Mac (x64)**) and Windows (**Windows (ARM64)** and **Windows (x64)**).

### Step 3: Update the installation script <a href="#step-3-update-the-installation-script" id="step-3-update-the-installation-script"></a>

{% tabs %}
{% tab title="macOS" %}
1. Unzip the downloaded package. The folder contains two files: the agent binary and the installation script (`harness-aidlc-agent-macos-arm64-install.sh`).
2.  Open the installation script and set the token from Step 1 as the value of `SEI_SERVICE_TOKEN`, in the **Customer Config** block near the top of the file:

    ```shell
    SEI_SERVICE_TOKEN="<token-from-step-1>"
    ```
3. Save the script.
{% endtab %}

{% tab title="Windows" %}
1. Unzip the downloaded package. The folder contains two files: the agent binary and the PowerShell installation script. Windows packages ship a `.ps1` installation script rather than the `.sh` script used on macOS. The filenames match the architecture you downloaded in Step 2:

| Architecture | Binary                                | Installation script                           |
| ------------ | ------------------------------------- | --------------------------------------------- |
| x64          | harness-aidlc-agent-windows-amd64.exe | harness-aidlc-agent-windows-amd64-install.ps1 |
| arm64        | harness-aidlc-agent-windows-arm64.exe | harness-aidlc-agent-windows-arm64-install.ps1 |

2.  Open the installation script in a text editor and set the token from Step 1 as the value of `$SEI_SERVICE_TOKEN`, in the **Customer Config** block near the top of the file:

    ```shell
    $SEI_SERVICE_TOKEN = '<token-from-step-1>'
    ```
3. Save the script.
{% endtab %}
{% endtabs %}

### Step 4: Install the agent <a href="#step-4-install-the-agent" id="step-4-install-the-agent"></a>

{% tabs %}
{% tab title="Deploy via MDM" %}
Organization administrators distribute the agent to developer machines using MDM (Mobile Device Management).

If you are using macOS:

1. Zip the binary and the updated installation script from Step 3.
2. Push the `.zip` package to developer machines.
3. Create a post-install script (see below) to locate and execute the installation script automatically from the extracted zip.
4. Initialize the binary installation using the installation script. The post-install script runs the installation script, which verifies the binary, stores the token in the macOS Keychain, writes configuration, places the binary, registers the LaunchAgent, and starts the daemon.

**Post-install script for MDM zip deployment**: When installing from a `.zip` file via MDM, you need a post-install script to locate and execute the installation script automatically. Save the following as your post-install script in your MDM configuration:

```bash
#!/bin/bash
# 1. Define the location where the zip is extracted
BASE_DIR="<location-where-zip-is-pushed>"
INSTALL_SCRIPT="$BASE_DIR/harness-aidlc-agent-macos-arm64-install.sh"
# 2. Check if the script exists
if [[ -f "$INSTALL_SCRIPT" ]]; then
  echo "Found install script at $INSTALL_SCRIPT. Executing now..."
  # 3. Make the script executable
  chmod +x "$INSTALL_SCRIPT"
  # 4. Execute the script
  "$INSTALL_SCRIPT"
  # 5. Capture the exit status
  EXIT_CODE=$?
  if [ $EXIT_CODE -eq 0 ]; then
    echo "Installation script finished successfully."
    exit 0
  else
    echo "Installation script failed with error code $EXIT_CODE."
    exit "$EXIT_CODE"
  fi
else
  echo "Error: Installation script not found at $INSTALL_SCRIPT"
  exit 1
fi
```

If you are using Windows:

1. Zip the binary and the updated installation script from Step 3. Include the .exe.sha256 checksum sidecar if your package provides one — the installer verifies the binary against it and skips integrity verification when it is absent.
2. Push the `.zip` package to developer machines.
3. Create a post-install script (see below) to locate and execute the installation script automatically from the extracted zip.
4.  Initialize the binary installation using the installation script. The post-install script runs the installation script, which verifies the binary, stores the token in **Windows Credential Manager**, writes configuration, places the binary, registers the agent's scheduled tasks, and starts the daemons.

    **Note**: The installation script must run in the signed-in developer's own user session. The agent installs per-user, meaning that the binary, configuration, telemetry cache, and scheduled tasks all live under the developer's profile. Many MDM tools (including ManageEngine Endpoint Central) run deployment scripts as `SYSTEM` by default, which would install the agent into the `SYSTEM` profile instead of the developer's, producing an install that reports no telemetry.

    Before rollout, confirm your MDM tool is configured to run the post-install script as the logged-on user. If it can only run as `SYSTEM` (which is the default in ManageEngine Endpoint Central), you need a deploy wrapper that resolves the console user and relaunches the installer in that user's session.

**Post-install script for MDM zip deployment**: When installing from a `.zip` file via MDM, you need a post-install script to locate and execute the installation script automatically. Save the following as your post-install script in your MDM configuration:

```powershell
# 1. Define the location where the zip is extracted
$BaseDir       = "<location-where-zip-is-pushed>"
$InstallScript = Join-Path $BaseDir "harness-aidlc-agent-windows-amd64-install.ps1"

# 2. Check if the script exists
if (Test-Path $InstallScript) {
    Write-Host "Found install script at $InstallScript. Executing now..."

    # 3. Clear the mark-of-the-web flag carried over from the downloaded zip
    Unblock-File -Path $InstallScript -ErrorAction SilentlyContinue

    # 4. Execute the script. -NonInteractive is required for MDM runs.
    & powershell.exe -NoProfile -ExecutionPolicy Bypass -File $InstallScript -NonInteractive

    # 5. Capture the exit status
    $ExitCode = $LASTEXITCODE
    if ($ExitCode -eq 0) {
        Write-Host "Installation script finished successfully."
        exit 0
    } else {
        Write-Host "Installation script failed with error code $ExitCode."
        exit $ExitCode
    }
} else {
    Write-Host "Error: Installation script not found at $InstallScript"
    exit 1
}
```

Replace `<location-where-zip-is-pushed>` with the actual path where your MDM tool extracts the zip package on developer machines. On an arm64 fleet, substitute `harness-aidlc-agent-windows-arm64-install.ps1` for the `$InstallScript` filename.

**Note**: Always pass `-NonInteractive` for MDM deployments. Without it, the installation script can display a prompt and—because MDM scripts run in a hidden window with no one to answer it—the deployment blocks until your MDM tool's timeout expires. Set the script timeout to **600 seconds or more**.
{% endtab %}

{% tab title="Manual installation" %}
Use manual installation for individual developer machines or to test the agent before a broader MDM rollout.

If you are using macOS:

1. Complete Steps 1–3 above.
2.  Run the updated installation script:

    ```bash
    sudo bash harness-aidlc-agent-macos-arm64-install.sh
    ```

    Once the script completes, the agent begins collecting telemetry in the background.

If you are using Windows:

1. Complete Steps 1–3 above.
2. Open **PowerShell as an administrator**: right-click **Start**, then select **Terminal (Admin)** or **Windows PowerShell (Admin)**.
3. Allow the script to run for this session. Windows blocks unsigned scripts by default, and files extracted from a downloaded `.zip` also carry a mark-of-the-web flag that must be cleared:

```shell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force

Unblock-File .\harness-aidlc-agent-windows-amd64-install.ps1
```

`-Scope Process` applies only to the current PowerShell session, so the machine's execution policy is left unchanged.

4. Run the installation script: `.\harness-aidlc-agent-windows-amd64-install.ps1`. On an arm64 device, substitute `harness-aidlc-agent-windows-arm64-install.ps1` in Steps 3 and 4.

The script verifies the binary checksum, stores the token in Windows Credential Manager, writes configuration, places the binary, registers the agent's scheduled tasks, and starts the daemons.

**Note:** After the script completes, open a new, non-elevated terminal for the updated `PATH` to take effect. The agent's daemons start automatically at the developer's next sign-in. To start them in the current session without signing out, run these from that new non-elevated terminal:

```shell
harness-aidlc-agent tracer start

harness-aidlc-agent telemetry start
```

Do not start the daemons from an elevated terminal, they would inherit the administrator token, and subsequent commands run as the signed-in user would fail with `Access is denied`.
{% endtab %}
{% endtabs %}

### Deployment artifacts <a href="#deployment-artifacts" id="deployment-artifacts"></a>

{% tabs %}
{% tab title="macOS" %}
| Artifact         | Location                                              | Purpose                                                |
| ---------------- | ----------------------------------------------------- | ------------------------------------------------------ |
| Binary           | \~/.harness-aidlc-agent/bin/harness-aidlc-agent       | Daemon executable.                                     |
| LaunchAgent      | \~/Library/LaunchAgents/com.harness.aidlc-agent.plist | Starts the agent automatically on user login.          |
| Configuration    | \~/.sei/config.json                                   | Stores the endpoint and integration ID.                |
| Telemetry cache  | \~/.harness-aidlc-agent/telemetry/                    | Local queue for telemetry data before upload.          |
| Audit log        | \~/.sei/send.log                                      | Records telemetry transmission activity.               |
| IPC socket       | \~/.harness-aidlc-agent/internal/daemon               | Inter-process communication with 0600 permissions.     |
| Git hooks        | .git/hooks/post-commit                                | Per-repository attribution triggered on commit events. |
| Keychain entries | macOS System Keychain                                 | Stores the API key securely.                           |
{% endtab %}

{% tab title="Windows" %}
| Artifact                   | Location                                                                                                                                                  | Purpose                                                                                                                  |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Binary                     | %USERPROFILE%.harness-aidlc-agent\bin\harness-aidlc-agent.exe                                                                                             | Daemon executable.                                                                                                       |
| Scheduled tasks            | Task Scheduler: HarnessAIDLCAgentDaemon, HarnessAIDLCAgentTelemetry, HarnessAIDLCAgentMetricRecorder                                                      | Start the agent automatically at user sign-in. The Windows equivalent of the macOS LaunchAgent.                          |
| Configuration              | %USERPROFILE%.sei\config.json                                                                                                                             | Stores the endpoint and integration ID. Access-restricted to the installing user.                                        |
| Telemetry cache            | %USERPROFILE%.harness-aidlc-agent\telemetry\\                                                                                                             | Local queue for telemetry data before upload.                                                                            |
| Audit log                  | %USERPROFILE%.sei\send.log                                                                                                                                | Records telemetry transmission activity.                                                                                 |
| IPC socket                 | %USERPROFILE%.harness-aidlc-agent\internal\daemon\ (control.sock, trace2.sock)                                                                            | Inter-process communication, ACL-restricted to the installing user.                                                      |
| Git hooks                  | .git\hooks\post-commit                                                                                                                                    | Per-repository attribution triggered on commit events.                                                                   |
| Credential Manager entries | **Control Panel** → **Credential Manager** → **Windows Credentials**: `com.harness.aidlc-agent.service-token`, `com.harness.aidlc-agent.disabled-metrics` | Stores the API key and the data-collection configuration securely. The Windows equivalent of the macOS Keychain entries. |
| PATH entry                 | `%USERPROFILE%\.harness-aidlc-agent\bin` added to the user and system `PATH`, ahead of any Git entry                                                      | Lets the agent's `git.exe` shim observe git commands for attribution. The real `git` remains available as `git-og`.      |
{% endtab %}
{% endtabs %}

### Verify the installation <a href="#verify-the-installation" id="verify-the-installation"></a>

### With direct machine access <a href="#with-direct-machine-access" id="with-direct-machine-access"></a>

{% tabs %}
{% tab title="macOS" %}
Run the following command to confirm the agent binary is installed and check its version: `harness-aidlc-agent version`. This returns the version string of the installed binary.

To confirm the daemon is running, run `launchctl list | grep harness.aidlc`. A line with `com.harness.aidlc-agent` and a process ID confirms the daemon is active. A `0` exit status in the second column indicates the agent started successfully.

To view recent telemetry activity, check the audit log: `tail -20 ~/.sei/send.log`.
{% endtab %}

{% tab title="Windows" %}
Run these commands from a normal (non-elevated) PowerShell session.

Confirm the agent binary is installed and check its version: `harness-aidlc-agent version`. This returns the version string of the installed binary. If the command is not recognized, the `PATH` update has not been picked up yet; open a new terminal, or sign out and back in.

To confirm the agent's background tasks are registered and running:

```
Get-ScheduledTask -TaskName HarnessAIDLCAgentDaemon, HarnessAIDLCAgentTelemetry, HarnessAIDLCAgentMetricRecorder |
    Select-Object TaskName, State
```

A `State` of `Running` or `Ready` for each task confirms the agent is installed correctly. `Running` means the daemon is active now; `Ready` means it is registered and will start at the next sign-in.

To confirm the daemons are responding:

```
harness-aidlc-agent tracer status
harness-aidlc-agent telemetry status
```

To view recent telemetry activity, check the audit log: `Get-Content "$HOME\.sei\send.log" -Tail 20`.
{% endtab %}
{% endtabs %}

### Without direct machine access <a href="#without-direct-machine-access" id="without-direct-machine-access"></a>

1. Go to the **AI Engineering** tab and select **Diagnostics**. The **Developer installations** section shows the agent rollout progress, the developers yet to install the agent, the latest and last rolled-out versions, and a table of every developer for whom the agent was installed successfully, with the installed version, installation time, and last session.

   ![The Diagnostics tab, with rollout tiles and the Developer installations table](../../../.gitbook/assets/agent-3.png)

2. Use **Search by name or email** to find a specific developer.

Collected data is processed every 15 minutes, so allow up to 15 minutes after installation before expecting the developer to appear.

### Uninstall the agent <a href="#uninstall-the-agent" id="uninstall-the-agent"></a>

Use the platform-specific uninstall script to remove the agent from developer machines. The script runs silently by default and requires no interaction. Deploy it via MDM for organization-wide removal, or run it manually on individual machines.

{% tabs %}
{% tab title="macOS" %}
**Deploy via MDM**

1. Distribute `uninstall.sh` to developer machines using your MDM tool.
2. Configure a post-install action to execute the script in the signed-in user's context:

```bash
sudo bash /path/to/uninstall.sh
```

**Manual uninstallation**

1. Download `uninstall.sh` to the developer machine.
2. Run the script:

```bash
sudo bash uninstall.sh
```

The script stops the daemon, removes the binary, deletes configuration and telemetry data (`~/.harness-aidlc-agent`), unloads LaunchAgent plists, removes PATH entries from shell rc files, and cleans up Git and IDE hooks (Claude Code, Cursor, GitHub Copilot, Windsurf). Typical duration is 10–30 seconds.

{% code title="uninstall.sh" overflow="wrap" %}
```bash
#!/bin/bash
#
# Harness AIDLC Cleanup Script
#
# This script removes all harness-aidlc-agent installations, configurations, and data.
# Use this before reinstalling to ensure a clean slate.
#
# Usage:
#   ./uninstall.sh                      # Silent mode (no prompts, default)
#   ./uninstall.sh --interactive        # Interactive mode (prompts for confirmation)
#   ./uninstall.sh --confirm            # Interactive mode (prompts for confirmation)
#   ./uninstall.sh --clean-build        # Interactive mode + prompt for build cleanup
#
# Flags:
#   --interactive     Show confirmation prompt before cleanup (default: silent)
#   --confirm         Show confirmation prompt before cleanup (same as --interactive)
#   --clean-build     Show prompt for build artifact cleanup (interactive mode)
#

set -e

RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m'

print_header() { echo -e "${BLUE}━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━${NC}"; echo -e "${BLUE}  $1${NC}"; echo -e "${BLUE}━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━${NC}"; }
print_success() { echo -e "${GREEN}✓${NC} $1"; }
print_error()   { echo -e "${RED}✗${NC} $1"; }
print_warning() { echo -e "${YELLOW}⚠${NC} $1"; }
print_info()    { echo -e "${BLUE}ℹ${NC} $1"; }

_find_installed_agent_binary() {
    local candidate
    for candidate in \
        "$HOME/.harness-aidlc-agent/bin/harness-aidlc-agent" \
        "/usr/local/bin/harness-aidlc-agent" \
        "$HOME/.cargo/bin/harness-aidlc-agent" \
        "$HOME/bin/harness-aidlc-agent" \
        "$(command -v harness-aidlc-agent 2>/dev/null || true)"; do
        if [ -n "$candidate" ] && [ -x "$candidate" ]; then
            echo "$candidate"
            return 0
        fi
    done
    return 1
}

revert_headroom_claude_settings() {
    print_header "Reverting Headroom Claude Settings"
    local agent_bin
    agent_bin="$(_find_installed_agent_binary || true)"
    if [ -z "$agent_bin" ]; then
        print_info "No harness-aidlc-agent binary — skipping Headroom Claude settings revert"
        echo ""; return 0
    fi
    local registry="$HOME/.harness-aidlc-agent/headroom/projects.jsonl"
    local global_settings="$HOME/.claude/settings.json"
    if [ ! -f "$registry" ] && [ ! -f "$HOME/.harness-aidlc-agent/headroom/enabled" ]; then
        if [ ! -f "$global_settings" ] || ! grep -q "ANTHROPIC_BEDROCK_BASE_URL" "$global_settings" 2>/dev/null; then
            print_info "No Headroom configuration found — skipping Claude settings revert"
            echo ""; return 0
        fi
    fi
    print_info "Removing Headroom Bedrock routing from Claude settings..."
    if HOME="$HOME" "$agent_bin" headroom uninstall --settings-only; then
        print_success "Headroom Claude settings reverted"
        print_info "Restart Claude Code with /exit so workers reload settings"
    else
        print_warning "Headroom settings revert failed — Claude may still point at local proxy"
    fi
    echo ""
}

confirm_cleanup() {
    if [[ "$*" == *"--interactive"* ]] || [[ "$*" == *"--confirm"* ]] || [[ "$*" == *"--prompt"* ]]; then
        print_header "Harness AIDLC Cleanup Script"
        echo "This script will remove:"
        echo "  • All harness-aidlc-agent binaries"
        echo "  • Configuration files (~/.harness-aidlc-agent)"
        echo "  • Telemetry data (~/.harness-aidlc-agent/telemetry)"
        echo "  • LaunchAgent plists (both new and old names)"
        echo "  • Global git hooks configuration"
        echo "  • IDE hooks (Claude, Cursor, Copilot, Windsurf)"
        echo "  • Headroom Bedrock proxy settings"
        echo "  • Background daemon processes"
        echo ""
        print_warning "This action cannot be undone!"
        echo -n "Continue with cleanup? [y/N]: "
        read -r confirm
        if [[ ! "$confirm" =~ ^[Yy]$ ]]; then print_info "Cleanup cancelled"; exit 0; fi
        echo ""
    else
        print_header "Harness AIDLC Cleanup"
    fi
}

stop_processes() {
    print_header "Stopping Harness AIDLC Processes"
    local plist_dir="$HOME/Library/LaunchAgents"
    for plist in \
        "$plist_dir/com.harness.aidlc-agent.daemon.plist" \
        "$plist_dir/com.harness.aidlc-agent.telemetry.plist" \
        "$plist_dir/com.harness.aidlc-agent.headroom.plist" \
        "$plist_dir/com.harness-aidlc-agent.bg.plist" \
        "$plist_dir/com.harness-aidlc-agent.telemetry.plist" \
        "$plist_dir/com.harness.sei-ai.daemon.plist" \
        "$plist_dir/com.harness.sei-ai.telemetry.plist"; do
        if [ -f "$plist" ]; then
            launchctl unload "$plist" 2>/dev/null || true
        fi
    done
    sleep 1
    pkill -9 -x "harness-aidlc-agent" 2>/dev/null || true
    sleep 1
    rm -f "$HOME/.harness-aidlc-agent/internal/daemon/daemon.lock" \
          "$HOME/.harness-aidlc-agent/internal/daemon/control.sock" \
          "$HOME/.harness-aidlc-agent/internal/daemon/trace2.sock" 2>/dev/null || true
    print_success "Processes stopped"
    echo ""
}

remove_binaries() {
    print_header "Removing Harness AIDLC Binaries"
    local locations=(
        "/usr/local/bin/harness-aidlc-agent"
        "$HOME/.cargo/bin/harness-aidlc-agent"
        "$HOME/bin/harness-aidlc-agent"
        "$HOME/.local/bin/harness-aidlc-agent"
        "$HOME/.harness-aidlc-agent/bin/harness-aidlc-agent"
    )
    for location in "${locations[@]}"; do
        if [ -f "$location" ]; then
            if [[ "$location" == /usr/local/bin/* ]]; then
                sudo rm -f "$location" && print_success "Removed $location"
            else
                rm -f "$location" && print_success "Removed $location"
            fi
        fi
    done
    for rc_file in "$HOME/.zshrc" "$HOME/.bashrc" "$HOME/.bash_profile" "$HOME/.profile"; do
        if [ -f "$rc_file" ]; then
            if grep -qE "harness-aidlc-agent/bin" "$rc_file" 2>/dev/null; then
                sed -i.bak '/harness-aidlc-agent\/bin/d' "$rc_file" 2>/dev/null || true
                rm -f "${rc_file}.bak"
                print_success "Removed PATH entries from $rc_file"
            fi
        fi
    done
    echo ""
}

remove_config() {
    print_header "Removing Configuration & Data"
    local plist_dir="$HOME/Library/LaunchAgents"
    for plist in \
        "$plist_dir/com.harness.aidlc-agent.daemon.plist" \
        "$plist_dir/com.harness.aidlc-agent.telemetry.plist" \
        "$plist_dir/com.harness.aidlc-agent.headroom.plist" \
        "$plist_dir/com.harness-aidlc-agent.bg.plist" \
        "$plist_dir/com.harness-aidlc-agent.telemetry.plist" \
        "$plist_dir/com.harness.sei-ai.daemon.plist" \
        "$plist_dir/com.harness.sei-ai.telemetry.plist"; do
        [ -f "$plist" ] && rm -f "$plist"
    done
    for dir in "$HOME/.harness-aidlc-agent" "$HOME/.harness-sei-ai" "$HOME/.git-ai" "$HOME/.sei"; do
        if [ -d "$dir" ]; then
            rm -rf "$dir" && print_success "Removed $dir"
        fi
    done
    echo ""
}

remove_git_hooks() {
    print_header "Removing Git Hooks Configuration"
    local hooks_path
    hooks_path=$(git config --global core.hooksPath 2>/dev/null || echo "")
    if [ -n "$hooks_path" ] && [[ "$hooks_path" == *".sei/hooks"* ]]; then
        git config --global --unset core.hooksPath && print_success "Removed global core.hooksPath"
    fi
    local template_dir
    template_dir=$(git config --global init.templateDir 2>/dev/null || echo "")
    if [ -n "$template_dir" ] && [[ "$template_dir" == *".sei/git-templates"* ]]; then
        git config --global --unset init.templateDir && print_success "Removed global init.templateDir"
    fi
    local trace2_target
    trace2_target=$(git config --global trace2.eventtarget 2>/dev/null || echo "")
    if [ -n "$trace2_target" ] && [[ "$trace2_target" == *".harness-aidlc-agent"* ]]; then
        git config --global --unset trace2.eventtarget && print_success "Removed trace2.eventtarget"
    fi
    echo ""
}

verify_cleanup() {
    print_header "Verifying Cleanup"
    hash -r 2>/dev/null || true
    local remaining
    remaining=$(which harness-aidlc-agent 2>/dev/null || true)
    if [ -n "$remaining" ] && [ -f "$remaining" ]; then
        print_warning "harness-aidlc-agent still on disk at: $remaining"
    else
        print_success "No harness-aidlc-agent binary on disk"
    fi
    pgrep -x "harness-aidlc-agent" > /dev/null 2>&1 && print_warning "Processes still running" || print_success "No harness-aidlc-agent processes running"
    [ -d "$HOME/.harness-aidlc-agent" ] && print_warning "~/.harness-aidlc-agent still exists" || print_success "~/.harness-aidlc-agent removed"
    echo ""
}

main() {
    confirm_cleanup "$@"
    stop_processes
    revert_headroom_claude_settings
    remove_binaries
    remove_config
    remove_git_hooks
    verify_cleanup
    print_header "Cleanup Complete"
}

main "$@"
```
{% endcode %}

**Verify**

```bash
ps aux | grep harness-aidlc-agent
```

No output confirms the agent has been fully removed.
{% endtab %}
{% tab title="Windows" %}
**Deploy via MDM**

1. Distribute `uninstall.ps1` to developer machines using your MDM tool.
2. Configure a post-install action to execute the script. The script must run in the signed-in developer's user session, not as `SYSTEM`. Many MDM tools run deployment scripts as `SYSTEM` by default, which would leave agent artifacts in the `SYSTEM` profile rather than the developer's.

```powershell
powershell.exe -ExecutionPolicy Bypass -File "C:\path\to\uninstall.ps1"
```

**Manual uninstallation**

1. Download `uninstall.ps1` to the developer machine.
2. Open a PowerShell terminal and run:

```powershell
.\uninstall.ps1
```

The script stops the daemon processes, removes the binary and PATH entries, deletes configuration and telemetry data (`%USERPROFILE%\.harness-aidlc-agent`), unregisters Scheduled Tasks (`HarnessAIDLCAgentDaemon` and `HarnessAIDLCAgentTelemetry`), removes the service token from Windows Credential Manager, and cleans up Git and IDE hooks (Claude Code, Cursor, GitHub Copilot, Windsurf).

{% code title="uninstall.ps1" overflow="wrap" %}
```powershell
#Requires -Version 5.1
<#
.SYNOPSIS
    Harness AIDLC Cleanup Script for Windows

.DESCRIPTION
    Removes all harness-aidlc-agent installations, configurations, and data.
    Use this before reinstalling to ensure a clean slate.

.PARAMETER Interactive
    Show confirmation prompt before cleanup (default: silent).

.PARAMETER Confirm
    Alias for -Interactive.

.EXAMPLE
    .\uninstall.ps1                   # Silent mode (no prompts, default)
    .\uninstall.ps1 -Interactive      # Interactive mode (prompts for confirmation)
    .\uninstall.ps1 -Confirm          # Same as -Interactive
#>

[CmdletBinding(SupportsShouldProcess)]
param(
    [switch]$Interactive,
    [switch]$Confirm,
    [switch]$CleanBuild
)

$ErrorActionPreference = 'Continue'

$TASK_NAME           = 'HarnessAIDLCAgentDaemon'
$TASK_NAME_TELEMETRY = 'HarnessAIDLCAgentTelemetry'

function Print-Header  { param([string]$Msg) Write-Host "`n=== $Msg ===" -ForegroundColor Cyan }
function Print-Info    { param([string]$Msg) Write-Host "  [INFO]  $Msg" -ForegroundColor Cyan }
function Print-Success { param([string]$Msg) Write-Host "  [OK]    $Msg" -ForegroundColor Green }
function Print-Warning { param([string]$Msg) Write-Host "  [WARN]  $Msg" -ForegroundColor Yellow }
function Print-Failure { param([string]$Msg) Write-Host "  [FAIL]  $Msg" -ForegroundColor Red }

function Confirm-Cleanup {
    if ($Interactive -or $Confirm) {
        Print-Header "Harness AIDLC Cleanup Script"
        Write-Host "  This script will remove:"
        Write-Host "    * All harness-aidlc-agent binaries"
        Write-Host "    * Configuration files (~\.harness-aidlc-agent)"
        Write-Host "    * Telemetry data"
        Write-Host "    * Windows Scheduled Task entries"
        Write-Host "    * Global git hooks configuration"
        Write-Host "    * IDE hooks (Claude, Cursor, Copilot, Windsurf)"
        Write-Host "    * Windows Credential Manager entries (service token)"
        Write-Host ""
        Print-Warning "This action cannot be undone!"
        $answer = Read-Host "Continue with cleanup? [y/N]"
        if ($answer -notmatch '^[Yy]$') { Print-Info "Cleanup cancelled"; exit 0 }
    } else {
        Print-Header "Harness AIDLC Cleanup"
    }
}

function Stop-AgentProcesses {
    Print-Header "Stopping Harness AIDLC Processes"
    foreach ($taskName in @($TASK_NAME, $TASK_NAME_TELEMETRY)) {
        $task = Get-ScheduledTask -TaskName $taskName -ErrorAction SilentlyContinue
        if ($task) {
            Disable-ScheduledTask -TaskName $taskName -ErrorAction SilentlyContinue | Out-Null
            Unregister-ScheduledTask -TaskName $taskName -Confirm:$false -ErrorAction SilentlyContinue
            Print-Success "Removed scheduled task: $taskName"
        }
    }
    $procs = Get-Process -Name "harness-aidlc-agent" -ErrorAction SilentlyContinue
    if ($procs) {
        $procs | Stop-Process -Force -ErrorAction SilentlyContinue
        Start-Sleep -Seconds 1
        Print-Success "Processes terminated"
    } else {
        Print-Success "No harness-aidlc-agent processes running"
    }
    @(
        "$env:USERPROFILE\.harness-aidlc-agent\internal\daemon\daemon.lock",
        "$env:USERPROFILE\.harness-aidlc-agent\internal\daemon\control.sock",
        "$env:USERPROFILE\.harness-aidlc-agent\internal\daemon\trace2.sock"
    ) | ForEach-Object { if (Test-Path $_) { Remove-Item $_ -Force -ErrorAction SilentlyContinue } }
    Print-Success "Locks and sockets removed"
    Write-Host ""
}

function Remove-Binaries {
    Print-Header "Removing Harness AIDLC Binaries"
    $locations = @(
        "$env:USERPROFILE\.harness-aidlc-agent\bin\harness-aidlc-agent.exe",
        "$env:USERPROFILE\.local\bin\harness-aidlc-agent.exe",
        "$env:USERPROFILE\bin\harness-aidlc-agent.exe",
        "$env:LOCALAPPDATA\harness-aidlc-agent\harness-aidlc-agent.exe"
    )
    foreach ($loc in $locations) {
        if (Test-Path $loc) {
            Remove-Item $loc -Force -ErrorAction SilentlyContinue
            Print-Success "Removed $loc"
        }
    }
    $userPath = [Environment]::GetEnvironmentVariable("PATH", "User")
    if ($userPath) {
        $newParts = $userPath -split ';' | Where-Object { $_ -notmatch [regex]::Escape('harness-aidlc-agent\bin') }
        [Environment]::SetEnvironmentVariable("PATH", ($newParts -join ';'), "User")
        Print-Success "Cleaned harness-aidlc-agent from user PATH"
    }
    Write-Host ""
}

function Remove-Config {
    Print-Header "Removing Configuration and Data"
    foreach ($dir in @(
        "$env:USERPROFILE\.harness-aidlc-agent",
        "$env:USERPROFILE\.harness-sei-ai",
        "$env:USERPROFILE\.git-ai",
        "$env:USERPROFILE\.sei"
    )) {
        if (Test-Path $dir) {
            Remove-Item $dir -Recurse -Force -ErrorAction SilentlyContinue
            Print-Success "Removed $dir"
        }
    }
    foreach ($target in @("com.harness.aidlc-agent.service-token", "harness-aidlc-agent")) {
        & cmdkey /delete:$target 2>&1 | Out-Null
    }
    Print-Success "Removed Windows Credential Manager entries"
    Write-Host ""
}

function Remove-GitHooks {
    Print-Header "Removing Git Hooks Configuration"
    $hooksPath = & git config --global core.hooksPath 2>&1
    if ($LASTEXITCODE -eq 0 -and $hooksPath -match '\.sei[/\\]hooks') {
        & git config --global --unset core.hooksPath | Out-Null
        Print-Success "Removed global core.hooksPath"
    }
    $templateDir = & git config --global init.templateDir 2>&1
    if ($LASTEXITCODE -eq 0 -and $templateDir -match '\.sei[/\\]git-templates') {
        & git config --global --unset init.templateDir | Out-Null
        Print-Success "Removed global init.templateDir"
    }
    $trace2 = & git config --global trace2.eventtarget 2>&1
    if ($LASTEXITCODE -eq 0 -and $trace2 -match 'harness-aidlc-agent') {
        & git config --global --unset trace2.eventtarget | Out-Null
        Print-Success "Removed global trace2.eventtarget"
    }
    Write-Host ""
}

function Verify-Cleanup {
    Print-Header "Verifying Cleanup"
    $bin = Get-Command "harness-aidlc-agent" -ErrorAction SilentlyContinue
    if ($bin -and (Test-Path $bin.Source)) {
        Print-Warning "harness-aidlc-agent still on disk at: $($bin.Source)"
    } else {
        Print-Success "No harness-aidlc-agent binary on disk"
    }
    $procs = Get-Process -Name "harness-aidlc-agent" -ErrorAction SilentlyContinue
    if ($procs) { Print-Warning "Processes still running" } else { Print-Success "No processes running" }
    if (Test-Path "$env:USERPROFILE\.harness-aidlc-agent") {
        Print-Warning "~\.harness-aidlc-agent still exists"
    } else {
        Print-Success "~\.harness-aidlc-agent removed"
    }
    Write-Host ""
}

Confirm-Cleanup
Stop-AgentProcesses
Remove-Binaries
Remove-Config
Remove-GitHooks
Verify-Cleanup
Print-Header "Cleanup Complete"
```
{% endcode %}

**Verify**

```powershell
Get-Process -Name "harness-aidlc-agent"
```

No output confirms the agent has been fully removed.
{% endtab %}
{% endtabs %}

### Attribution capabilities <a href="#attribution-capabilities" id="attribution-capabilities"></a>

| Capability | Support | Details |
| ------------------------------------ | ------- | ------- |
| Edit, write, and patch tools | Yes | Hooks from coding agents capture accepted file changes and report them to the Harness Agent daemon. |
| Files created or changed through Bash and shell tools | Partial | Supported when the agent integration exposes the relevant shell-command events to the daemon. |
| Git worktrees | Yes | The daemon discovers linked worktrees and attributes commits to the parent monitored repository. |
| Background and cloud agents | No | Cloud-hosted agent activity is not currently captured. |
| Multiple agent sessions contributing to the same commit | Yes | A committed file can retain links to multiple contributing session IDs. |
| Human changes to AI-generated code | Yes | Harness reports original, retained, modified, and deleted AI lines plus similarity; it does not provide a user-facing line-by-line override view. |
| Sessions that produce no committed code | Yes | No view exists to track these types of sessions today. These will be surfaced as part of the OOTB experience in an upcoming release. |
| Added and deleted lines per session | Yes | Session drilldowns link to resulting commit and pull request additions and deletions, but lines are not exclusively allocated when multiple sessions contribute. |
| Tool-call-level attribution for a session | Yes | Tool-call identifiers used for local event matching are not exposed in Harness reports. |
| Tokens and cost per commit or pull request | Yes | — |
| Formatters | No | Lines materially rewritten by a formatter are reported as modified or human-authored. |
| Multi-repository agent roots | Partial | Direct file-edit events can match monitored repositories; shell-generated changes require the command to run with its working directory inside the target repository. |
| Very large change in a single commit | Yes | Commit size can affect daemon performance and attribution accuracy. Test with large commits before a broad rollout. |

### Local Git operations <a href="#local-git-operations" id="local-git-operations"></a>

| Operation | Support | Details |
| --------- | ------- | ------- |
| `git rebase` | Yes | Attribution is transferred for clean replays and recalculated when the patch changes. |
| `git cherry-pick` | Yes | Matching patches carry attribution to the new commit and branch. |
| `git stash` / `git stash pop` | Yes | Retained attribution is recovered when stashed content is reintroduced; conflict edits can change the result. |
| `git merge --squash` | Planned | The resulting commit is not guaranteed to inherit attribution from commits on the source branch. |
| `git reset --soft` | Planned | Attribution is retained when commits are combined and recommitted. |
| `git reset --mixed` | Planned | Working-tree attribution is retained for the subsequent commit. |
| `git reset --hard` | Planned | Attribution remains for commits that are still reachable or were already reported; discarded uncommitted work is not retained. |
| `git merge` | Planned | Source commits keep their attribution, and new merge-resolution changes are analyzed in the merge commit. |
| `git commit --amend` | Yes | The replacement commit is analyzed and the superseded local result is reconciled. |
| `git checkout` / `git switch` | Yes | Uncommitted attribution follows the working tree when switching branches. |
| `git pull` / `git pull --rebase` | Yes / Planned | Fast-forward and rebase workflows are reconciled. |
| `git push` / `git fetch` → rebase or merge | Yes / Planned | A push makes eligible commit metrics available to Harness via git hooks. Fetching does not remove existing attribution. |
| `git mv` | No | Attribution follows a renamed file, including a rename with subsequent human edits. |
| `git filter-branch` / `git filter-repo` | No | Bulk history rewrites are not tracked. |
| `git replace` | No | Git object replacements are not tracked. |

### Server-side pull request merge methods <a href="#server-side-pull-request-merge-methods" id="server-side-pull-request-merge-methods"></a>

{% hint style="warning" %}
All server-side Git operations are not currently supported by the agent.
{% endhint %}

| Operation | Support | Details |
| --------- | ------- | ------- |
| Squash and merge | Partial | Harness can report the original pull request commits when they were uploaded and matched by the source control connector, but the new server-created commit is not re-attributed by the local daemon. |
| Rebase and merge | Partial | Harness can report the original pull request commits, but server-created replacement commits do not inherit local daemon attribution. |

### Known scenarios <a href="#known-scenarios" id="known-scenarios"></a>

* **Pure AI deletions:** An AI change that only deletes lines cannot be disambiguated from a human deletion. Attribution requires at least one added line to anchor a checkpoint.
* **Cherry-pick without remote note sync:** Without fetching remote notes first, attribution falls back to direct commit diff only.
* **Non-integrated tools:** Activity from tools without a hook integration (for example, pasted ChatGPT output) produces no attribution data and is attributed as "Others".

### Troubleshooting <a href="#troubleshooting" id="troubleshooting"></a>

<details>

<summary>A developer is missing from the AI Engineering dashboard</summary>

Verify the agent is installed on their machine:

* **macOS:** `launchctl list | grep harness.aidlc`
* **Windows:** `Get-ScheduledTask -TaskName HarnessAIDLCAgentDaemon, HarnessAIDLCAgentTelemetry | Select-Object TaskName, State`

Check the **Last session** timestamp in the **Developer Installations** table. Note that data is processed every 15 minutes, so allow up to 15 minutes after a new installation or session before the developer appears. If the agent has not reported in, reinstall using the MDM package.

On Windows, also confirm the agent was installed in the developer's own user session and not in the `SYSTEM` profile; an install run as `SYSTEM` reports success but produces no telemetry. `Test-Path "$HOME\.harness-aidlc-agent\bin\harness-aidlc-agent.exe"` run as the developer should return `True`.

</details>

<details>

<summary>A developer appears in the dashboard but shows no AI activity</summary>

The agent is running but has not detected any AI coding tool sessions. Confirm the developer has an active AI coding tool (such as Claude Code or Cursor) installed and has used it within the selected time range.

</details>

<details>

<summary>The agent is installed but the daemon is not running</summary>

If you are using macOS:

The LaunchAgent may not have loaded. Run the following command to start it manually:

```bash
launchctl load ~/Library/LaunchAgents/com.harness.aidlc-agent.plist
```

If the plist file does not exist, reinstall the agent via MDM.

If you are using Windows:

The scheduled tasks may be registered but not yet started; they start automatically at sign-in. Check their state first:

```
Get-ScheduledTask -TaskName HarnessAIDLCAgentDaemon, HarnessAIDLCAgentTelemetry, HarnessAIDLCAgentMetricRecorder |
    Select-Object TaskName, State
```

If a task shows `Ready` rather than `Running`, start it from a normal (non-elevated) terminal:

```
schtasks.exe /run /tn HarnessAIDLCAgentDaemon
schtasks.exe /run /tn HarnessAIDLCAgentTelemetry
```

**Note**: Start the tasks from a non-elevated terminal, and do not use `Start-ScheduledTask` from an administrator session. A daemon started with an elevated token restricts its communication channel to administrators, and subsequent harness-aidlc-agent commands run as the signed-in user then fail with `Access is denied`. If this has already happened, sign out and back in—the tasks restart correctly at sign-in.

If the tasks do not exist at all, reinstall the agent via MDM.

</details>

<details>

<summary>Telemetry data is not appearing in the dashboard</summary>

Check network connectivity to `*.harness.io` on port 443, then review the audit log for transmission errors:

* **macOS:** `tail -50 ~/.sei/send.log`
* **Windows:** `Get-Content "$HOME\.sei\send.log" -Tail 50`

The agent queues data locally when the network is unavailable and retries automatically when connectivity is restored. The local queue is at `~/.harness-aidlc-agent/telemetry/` on macOS and `%USERPROFILE%\.harness-aidlc-agent\telemetry\` on Windows.

On Windows, also confirm the service token was stored: open **Control Panel → Credential Manager → Windows Credentials** and check for a `com.harness.aidlc-agent.service-token` entry. If it is missing, `$SEI_SERVICE_TOKEN` was left empty in the installation script (Step 3). Set it and re-run the install.

</details>

{% @harness-feedback/feedback %}
