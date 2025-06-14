# Auto-Approving Actions

> ⚠️ **SECURITY WARNING:** Auto-approve settings bypass confirmation prompts, giving Kilo Code direct access to your system. This can result in **data loss, file corruption, or worse**. Command line access is particularly dangerous, as it can potentially execute harmful operations that could damage your system or compromise security. Only enable auto-approval for actions you fully trust.

Auto-approve settings speed up your workflow by eliminating repetitive confirmation prompts, but they significantly increase security risks.

## Quick Start Guide

1. Click the Auto-Approve Toolbar above the chat input
2. Select which actions Kilo Code can perform without asking permission
3. Use the master toggle (leftmost checkbox) to quickly enable/disable all permissions

## Auto-Approve Toolbar

<img src="/docs/img/auto-approving-actions/auto-approving-actions.png" alt="Auto-approve toolbar collapsed state" width="600" />

*Prompt box and Auto-Approve Toolbar showing enabled permissions*

Click the toolbar to expand it and configure individual permissions:

<img src="/docs/img/auto-approving-actions/auto-approving-actions-1.png" alt="Auto-approve toolbar expanded state" width="600" />

*Prompt text box and Expanded toolbar with all options*

### Available Permissions

| Permission | What it does | Risk level |
|------------|--------------|------------|
| **Read files and directories** | Lets Kilo Code access files without asking | Medium |
| **Edit files** | Lets Kilo Code modify files without asking | **High** |
| **Execute approved commands** | Runs whitelisted terminal commands automatically | **High** |
| **Use the browser** | Allows headless browser interaction | Medium |
| **Use MCP servers** | Lets Kilo Code use configured MCP services | Medium-High |
| **Switch modes** | Changes between Kilo Code modes automatically | Low |
| **Create & complete subtasks** | Manages subtasks without confirmation | Low |
| **Retry failed requests** | Automatically retries failed API requests | Low |

## Master Toggle for Quick Control

The leftmost checkbox works as a master toggle:

<img src="/docs/img/auto-approving-actions/auto-approving-actions-14.png" alt="Master toggle in Auto-approve toolbar" width="600" />

*Master toggle (checkbox) controls all auto-approve permissions at once*

Use the master toggle when:
- Working in sensitive code (turn off)
- Doing rapid development (turn on)
- Switching between exploration and editing tasks

## Advanced Settings Panel

The settings panel provides detailed control with important security context:

> **Allow Kilo Code to automatically perform operations without requiring approval. Enable these settings only if you fully trust the AI and understand the associated security risks.**

To access these settings:

1. Click <Codicon name="gear" /> in the top-right corner
2. Navigate to Auto-Approve Settings

<img src="/docs/img/auto-approving-actions/auto-approving-actions-4.png" alt="Settings panel auto-approve options" width="550" />

*Complete settings panel view*

### Read Operations

:::caution Read Operations
<img src="/docs/img/auto-approving-actions/auto-approving-actions-6.png" alt="Read-only operations setting" width="550" />

**Setting:** "Always approve read-only operations"

**Description:** "When enabled, Kilo Code will automatically view directory contents and read files without requiring you to click the Approve button."

**Risk level:** Medium

While this setting only allows reading files (not modifying them), it could potentially expose sensitive data. Still recommended as a starting point for most users, but be mindful of what files Kilo Code can access.
:::

### Write Operations

:::caution Write Operations
<img src="/docs/img/auto-approving-actions/auto-approving-actions-7.png" alt="Write operations setting with delay slider" width="550" />

**Setting:** "Always approve write operations"

**Description:** "Automatically create and edit files without requiring approval"

**Delay slider:** "Delay after writes to allow diagnostics to detect potential problems" (Default: 1000ms)

**Risk level:** High

This setting allows Kilo Code to modify your files without confirmation. The delay timer is crucial:
- Higher values (2000ms+): Recommended for complex projects where diagnostics take longer
- Default (1000ms): Suitable for most projects
- Lower values: Use only when speed is critical and you're in a controlled environment
- Zero: No delay for diagnostics (not recommended for critical code)

#### Write Delay & Problems Pane Integration

<img src="/docs/img/auto-approving-actions/auto-approving-actions-5.png" alt="VSCode Problems pane showing diagnostic information" width="600" />

*VSCode Problems pane that Kilo Code checks during the write delay*

When you enable auto-approval for writing files, the delay timer works with VSCode's Problems pane:

1. Kilo Code makes a change to your file
2. VSCode's diagnostic tools analyze the change
3. The Problems pane updates with any errors or warnings
4. Kilo Code notices these issues before continuing

This works like a human developer pausing to check for errors after changing code. You can adjust the delay time based on:

- Project complexity
- Language server speed
- How important error detection is for your workflow
:::

### Browser Actions

:::info Browser Actions
<img src="/docs/img/auto-approving-actions/auto-approving-actions-8.png" alt="Browser actions setting" width="550" />

**Setting:** "Always approve browser actions"

**Description:** "Automatically perform browser actions without requiring approval"

**Note:** "Only applies when the model supports computer use"

**Risk level:** Medium

Allows Kilo Code to control a headless browser without confirmation. This can include:
- Opening websites
- Navigating pages
- Interacting with web elements

Consider the security implications of allowing automated browser access.
:::

### API Requests

:::info API Requests
<img src="/docs/img/auto-approving-actions/auto-approving-actions-9.png" alt="API requests retry setting with delay slider" width="550" />

**Setting:** "Always retry failed API requests"

**Description:** "Automatically retry failed API requests when server returns an error response"

**Delay slider:** "Delay before retrying the request" (Default: 5s)

**Risk level:** Low

This setting automatically retries API calls when they fail. The delay controls how long Kilo Code waits before trying again:
- Longer delays are gentler on API rate limits
- Shorter delays give faster recovery from transient errors
:::

### MCP Tools

:::caution MCP Tools
<img src="/docs/img/auto-approving-actions/auto-approving-actions-10.png" alt="MCP tools setting" width="550" />

**Setting:** "Always approve MCP tools"

**Description:** "Enable auto-approval of individual MCP tools in the MCP Servers view (requires both this setting and the tool's individual 'Always allow' checkbox)"

**Risk level:** Medium-High (depends on configured MCP tools)

This setting works in conjunction with individual tool permissions in the MCP Servers view. Both this global setting and the tool-specific permission must be enabled for auto-approval.
:::

### Mode Switching

:::info Mode Switching
<img src="/docs/img/auto-approving-actions/auto-approving-actions-11.png" alt="Mode switching setting" width="550" />

**Setting:** "Always approve mode switching"

**Description:** "Automatically switch between different modes without requiring approval"

**Risk level:** Low

Allows Kilo Code to change between different modes (Code, Architect, etc.) without asking for permission. This primarily affects the AI's behavior rather than system access.
:::

### Subtasks

:::info Subtasks
<img src="/docs/img/auto-approving-actions/auto-approving-actions-12.png" alt="Subtasks setting" width="550" />

**Setting:** "Always approve creation & completion of subtasks"

**Description:** "Allow creation and completion of subtasks without requiring approval"

**Risk level:** Low

Enables Kilo Code to create and complete subtasks automatically. This relates to workflow organization rather than system access.
:::

### Command Execution

:::caution Command Execution
<img src="/docs/img/auto-approving-actions/auto-approving-actions-13.png" alt="Command execution setting with whitelist interface" width="550" />

**Setting:** "Always approve allowed execute operations"

**Description:** "Automatically execute allowed terminal commands without requiring approval"

**Command management:** "Command prefixes that can be auto-executed when 'Always approve execute operations' is enabled. Add * to allow all commands (use with caution)."

**Risk level:** High

This setting allows terminal command execution with controls. While risky, the whitelist feature limits what commands can run. Important security features:

- Whitelist specific command prefixes (recommended)
- Never use * wildcard in production or with sensitive data
- Consider security implications of each allowed command
- Always verify commands that interact with external systems

**Interface elements:**
- Text field to enter command prefixes (e.g., 'git')
- "Add" button to add new prefixes
- Clickable command buttons with X to remove them
:::
