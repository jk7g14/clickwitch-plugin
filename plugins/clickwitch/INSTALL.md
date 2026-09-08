# Install ClickWitch in the current agent host

This guide is for an agent asked to install the ClickWitch plugin. Public source: `https://github.com/jk7g14/clickwitch-plugin`. Marketplace name: `clickwitch`. Plugin selector: `clickwitch@clickwitch`.

Install only in the host the user is currently using: **Codex** or **Claude Code**. Do not install both because both CLIs happen to be present. Use the current conversation's host context; if it is unavailable, ask which host is intended before changing settings. Follow the host's normal permissions and organization policies. Do not alter those policies, expose credentials, or install another runtime to get around a missing capability.

## 1. Check the current host and existing installation

Read the relevant CLI help and list the current plugin/marketplace state. Do not open credential files or replace the user's configuration. Keep unrelated plugins and MCP servers intact.

For Codex:

```sh
codex --version
codex plugin marketplace list --json
codex plugin list --json
```

For Claude Code:

```sh
claude --version
claude plugin marketplace list
claude plugin list --json
```

If the CLI is missing or does not support plugin commands, state that limitation and provide the two installation commands below for a compatible local host. Do not report a plugin installed merely because a guide was downloaded or an MCP endpoint was configured.

If marketplace `clickwitch` already points to `jk7g14/clickwitch-plugin` (its GitHub HTTPS or SSH equivalent is the same source), reuse it. If the plugin is already installed from that source, skip installation and continue to activation/connection checks. Check scope and enabled state as well as the name; a disabled or project-scoped plugin is not necessarily active in this session. Report that condition without changing the user's chosen scope or enablement automatically.

If `clickwitch` names a different marketplace, or the user has an existing `clickwitch` plugin from `clickwitch-local`, `personal`, or a standalone MCP configuration, preserve it. Explain the specific collision and the intended source before any migration. Do not force an overwrite, uninstall a working plugin, or register a duplicate MCP server to make verification pass. Continue read-only inspection where possible.

## 2. Install from GitHub

Run only missing steps for the current host after the checks above.

### Codex

```sh
codex plugin marketplace add jk7g14/clickwitch-plugin
codex plugin add clickwitch@clickwitch
```

### Claude Code

```sh
claude plugin marketplace add jk7g14/clickwitch-plugin
claude plugin install clickwitch@clickwitch --scope user
```

Use the official CLI's output to determine whether installation succeeded. Do not claim success from an exit code alone if output reports a skipped, disabled, failed, or unavailable plugin. Do not silently fall back to a different repository or marketplace. A network failure is a pending installation, not a successful one.

## 3. Activate and connect the account

For **Codex**, list the installed plugin again and start a new task to discover its skill and MCP tools. If the current task cannot reload them, report installation as verified and activation as pending; do not manufacture a tool call or configuration entry. Use the host's account-connection UI for the plugin's actual ClickWitch MCP server when authentication is required.

For **Claude Code**, run `/reload-plugins` when the installed version supports it, or restart the session. Open `/mcp` to authenticate the plugin's ClickWitch connection. The skill is `/clickwitch:generate-detail-page-images`. If the terminal agent cannot operate a session command, give that exact activation step and retain the verified installation result.

OAuth requires the user's own ClickWitch sign-in and consent. Use the host-supported flow; never ask the user to paste passwords or tokens into chat. Reuse a valid existing connection. Do not add a second standalone `clickwitch` MCP server merely because the plugin connection needs authentication.

## 4. Verify without producing content

After activation, inspect the actual available tools. Look for the ClickWitch plugin's skill and connected MCP tools, including `clickwitch_get_quota` and `clickwitch_prepare_section_image`; host-qualified tool names may differ. When the quota tool is available and authenticated, call it once as a read-only connection check. Do not create a pack, generate an image, import a product, upload a file or merge a page during installation.

A Free-plan coaching response can verify connection while showing that operational tools require Pro/Growth. Record missing capabilities or authentication precisely. Tool discovery alone proves availability, not a working authenticated account; a successful quota response does not prove image generation or saved/merged completion.

Report:

- Which host and plugin version were verified, and whether installation was new or reused.
- Whether a new session/reload or OAuth connection remains pending.
- Whether the read-only connection check succeeded or was not possible.

Once ready, the user can provide a ClickWitch result URL and request missing section images. Image generation, browser/computer control and file transfer come from the host, not this plugin. Follow the installed execution skill for that later task.

## References

Codex syntax was checked against installed `codex-cli 0.144.1` plugin CLI help. Claude Code `2.1.263` CLI and official documentation support [GitHub marketplaces](https://code.claude.com/docs/en/plugin-marketplaces), [plugin activation](https://code.claude.com/docs/en/discover-plugins), and [MCP OAuth](https://code.claude.com/docs/en/mcp). If a host version differs, inspect its help and report incompatible commands rather than editing settings by guesswork.
