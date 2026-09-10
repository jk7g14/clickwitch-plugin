# ClickWitch plugin for Codex and Claude Code

Use your AI agent to choose or reuse ClickWitch detail-page inputs, generate missing section images, save them to ClickWitch, and assemble the final page. ClickWitch supplies exact prompts and ordered references; your host's image tools or your signed-in ChatGPT UI render the images.

Source: [jk7g14/clickwitch-plugin](https://github.com/jk7g14/clickwitch-plugin) · [Agent installation guide](https://raw.githubusercontent.com/jk7g14/clickwitch-plugin/main/INSTALL.md) · [MIT license](https://github.com/jk7g14/clickwitch-plugin/blob/main/LICENSE) · [한국어 법률·적용 범위](https://github.com/jk7g14/clickwitch-plugin/blob/main/LEGAL.ko.md)

## What is installed

This repository ships an installable plugin for **both Codex and Claude Code**. Each host loads its own manifest and marketplace catalog, with a shared execution skill and remote MCP configuration:

```text
.agents/plugins/marketplace.json       # Codex marketplace
.claude-plugin/marketplace.json        # Claude Code marketplace
plugins/clickwitch/
  .codex-plugin/plugin.json            # Codex plugin manifest
  .claude-plugin/plugin.json           # Claude Code plugin manifest
  .mcp.json                           # ClickWitch HTTP MCP connection
  skills/generate-detail-page-images/
    SKILL.md                          # Generate, save and merge workflow
    agents/openai.yaml                # Codex skill metadata
  LICENSE                             # Standard MIT license
  LEGAL.ko.md                         # Korean legal context and scope
```

`INSTALL.md` guides installation of these files; it is not the plugin itself. Codex and Claude Code support depend on a compatible host CLI, account connection, and the image/browser capabilities described below. This repository does not install into Claude web or supply an image-generation runtime.

## Copy to your agent

Paste this into **Codex or Claude Code**:

```text
Install the ClickWitch plugin in the agent host I am using now. Read https://raw.githubusercontent.com/jk7g14/clickwitch-plugin/main/INSTALL.md and follow its instructions. Preserve my existing plugins and settings. Verify installation, then tell me whether ClickWitch account connection or a new session is still needed. Do not create a pack or generate images during installation.
```

The agent reads the guide and uses your host's plugin commands. Copying this request does not itself install anything. Account connection uses ClickWitch OAuth in your host; credentials are never part of the plugin. If the host cannot execute installation commands, it supplies the exact commands below and identifies the missing capability.

## Install directly

These commands add the public GitHub marketplace named `clickwitch` and install its `clickwitch` plugin. See [INSTALL.md](https://github.com/jk7g14/clickwitch-plugin/blob/main/INSTALL.md) before reusing an existing installation or marketplace with the same name.

### Codex app / CLI

Requires a Codex CLI with `codex plugin` support, available in the environment used by your agent:

```sh
codex plugin marketplace add jk7g14/clickwitch-plugin
codex plugin add clickwitch@clickwitch
```

Start a **new Codex task** to pick up the installed skill and MCP tools. Use the host's ClickWitch account-connection prompt to sign in. A standalone MCP connection provides tools but does not install this plugin's execution skill.

### Claude Code

```sh
claude plugin marketplace add jk7g14/clickwitch-plugin
claude plugin install clickwitch@clickwitch --scope user
```

Run `/reload-plugins` when available, or restart Claude Code. Open `/mcp` and authenticate the plugin's ClickWitch connection. The skill is `/clickwitch:generate-detail-page-images`.

These instructions target **Claude Code**, not Claude web or Claude Desktop. Claude's [marketplace documentation](https://code.claude.com/docs/en/plugin-marketplaces), [installation documentation](https://code.claude.com/docs/en/discover-plugins), and [MCP documentation](https://code.claude.com/docs/en/mcp) describe GitHub installation, session activation, and OAuth. Codex commands were checked against `codex-cli 0.144.1` help; Claude commands against `2.1.263`. Host capabilities and organization policies may differ.

### Local source or ZIP fallback

A source checkout or the optional ClickWitch ZIP includes both marketplace catalogs and `plugins/clickwitch/`. From that folder, replace the GitHub source in the first command with `.` for Codex or `./` for Claude. The plugin selector is still `clickwitch@clickwitch`. Preserve hidden folders. Use this fallback only when the marketplace name is not already registered to a different source; do not overwrite an existing source to switch installation methods.

## Run and resume

After installation and account connection, provide either a result URL or the workspace/product set you want to rebuild.

> Continue this ClickWitch run. Generate only missing section images, save them to their existing sections, and assemble the final page: [paste your result URL].

For a new or regenerated pack, the skill first reuses existing workspaces, products, brands, concepts, categories and ready advertising models through ClickWitch inventory tools. It filters by workspace and query, applies category filters to categories/concepts, uses `modelMode` for concept/model eligibility, then keeps the selected product roles complete across languages such as Korean, Japanese and English. For comparison requests, provide the baseline run or manifest and the exact variants to test; the skill keeps those outputs separate instead of widening the experiment on its own.

The skill preserves the workspace, run and section IDs, product facts, chosen model references, chosen category/theme, product roles and custom-image sections. It passes the exact execution prompt with the complete ordered attachment set to the selected host image tool, ChatGPT UI or user-selected Chrome extension path. It respects installed browser, computer-use and image-tool instructions.

Selecting **이미지 선택** in the matching existing section automatically saves the file. The skill verifies the persisted thumbnail, selects the intended versions and order, then uses **최종 정리 → 세로 병합 이미지 만들기 → 최종 이미지 열기/다운로드**. Completion requires saved sections and a verified current merged image. On resume, it skips saved sections and uploads existing unsaved output files before generating again.

Full execution needs host-provided image generation or ChatGPT browser control, file transfer, and access to the ClickWitch web editor. This plugin contains no browser runtime or image renderer. When a capability is missing, it reports the exact pending step and a resumable section-to-file mapping.

A ClickWitch Pro/Growth account is required for operational MCP tools. Preparing an existing section costs zero ClickWitch credits. Creating a new pack consumes production credits and follows the tool's estimate and confirmation contract. Image rendering uses the selected host/service's plan and limits. A ChatGPT picker such as Latest, 6 Pro, Medium effort or an image-model setting changes the rendering surface, not the ClickWitch pack's category, concept or advertising-model identity. This plugin does not make provider allowances interchangeable or initiate subscription checkout.

## Source, license and contributions

The public repository contains the plugin, marketplace catalogs and installation documentation under the standard MIT license. [LEGAL.ko.md](https://github.com/jk7g14/clickwitch-plugin/blob/main/LEGAL.ko.md) explains the Korean legal context without adding conditions to MIT. The ClickWitch web service, private reference corpus and customer files are not included in this distribution; their terms or rights do not change the MIT permissions for the included source. GitHub distribution is separate from inclusion in a provider's curated plugin directory.

Use [GitHub issues](https://github.com/jk7g14/clickwitch-plugin/issues) for reproducible plugin problems and pull requests for small changes. Include host/version, synthetic fixtures, expected versus actual behavior and verification results. Never include credentials, private references or customer files. Keep exact prompts, attachment ordering, workspace ownership, credit confirmation and saved/merged completion checks intact.

Use [ClickWitch support](https://clickwitch.ai/dashboard/support) for account problems and private security reports. Privacy: [policy](https://clickwitch.ai/privacy); terms: [terms](https://clickwitch.ai/terms).

## Contributor validation

From the public repository root, validate Claude's plugin and marketplace manifests with a compatible Claude Code CLI:

```sh
claude plugin validate ./plugins/clickwitch
claude plugin validate ./.claude-plugin/marketplace.json
```

The repository's `plugins/clickwitch/SUBMISSION_TESTS.md` lists host integration scenarios; it is a test plan, not a claim that every scenario passed. Validate both host manifests, install from a disposable configuration, and verify the exposed skill and MCP connection before releasing changes. Keep the two plugin versions synchronized and bump the version for a new release. A complete generated/saved/merged run remains a separate execution test.
