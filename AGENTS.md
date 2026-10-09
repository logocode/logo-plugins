# Repository instructions

This public repo publishes LOGO.com's plugins for Claude, for Codex and ChatGPT, and for Cursor and Grok Bot, plus the self-hosted marketplaces that list them. Every plugin wraps the remote MCP server at `https://mcp.logo.com/mcp`. The server, its tools and its OAuth sign-in live in LOGO.com's private application repo, not here.

## Status

The plugin package and all three marketplaces exist. The plugin installs locally in Claude Code and Codex, and Cursor loads a local copy. Nothing is listed in Claude's or OpenAI's directory or the Cursor Marketplace yet. Draft listing materials are in `submissions/`. Change the package with the `build-plugin` skill.

## Target layout

| Path | Purpose |
| --- | --- |
| `plugins/logo-com/.claude-plugin/plugin.json` | Claude manifest |
| `plugins/logo-com/.codex-plugin/plugin.json` | Codex and ChatGPT manifest |
| `plugins/logo-com/.cursor-plugin/plugin.json` | Cursor manifest, which Grok Bot also uses |
| `plugins/logo-com/.mcp.json` | The MCP server the Claude and Codex manifests point at |
| `plugins/logo-com/mcp.json` | The same MCP server, where Cursor's docs say to put it |
| `plugins/logo-com/assets/` | Icons, and screenshots if a store asks for them |
| `plugins/logo-com/skills/` | Skills shipped to end users, if any |
| `.claude-plugin/marketplace.json` | Claude Code marketplace for this repo |
| `.agents/plugins/marketplace.json` | Codex marketplace for this repo |
| `.cursor-plugin/marketplace.json` | Cursor marketplace for this repo |
| `submissions/` | Listing copy, example prompts and test cases for each store |
| `.agents/skills/` | Skills for working on this repo. `.claude/skills` links here, and `CLAUDE.md` links to this file. |

## Rules

- This repo is public. Never commit secrets, tokens, reviewer credentials, customer data or internal URLs. Reviewer credentials go only into each store's submission portal.
- The plugin's `name`, `logo-com`, is permanent once published, because users install it under that name. Change `displayName` to relabel it.
- Keep `https://mcp.logo.com/mcp` as the MCP URL. OpenAI fixes a plugin's MCP origin when it's published.
- Tool names, descriptions and annotations come from the server. Fix them in the server, not by describing different behaviour here.

## Skills

| Skill | Use it to |
| --- | --- |
| `build-plugin` (this repo) | Create or change the plugin package or a marketplace file, then validate and install it. |
| `prepare-store-submission` (this repo) | Prepare or update a listing in Claude's directory, OpenAI's directory or the Cursor Marketplace. |
| `mcp-server-dev` (Anthropic) | Check the server against Claude's review criteria. In Claude Code: `/plugin install mcp-server-dev@claude-plugins-official`. |
| `$plugin-creator` (built into Codex) | Scaffold or validate a Codex manifest. |
| `chatgpt-app-submission` (OpenAI) | Draft OpenAI review answers and test cases. In a terminal: `codex plugin add openai-developers@openai-curated`. It targets an older submission form, so check its output against the current docs. |

## Sources of truth

Store rules change often. Check the official page before relying on a limit or field name, and fix the skill when it's out of date.

- Claude plugins: [build](https://claude.com/docs/plugins/build), [pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist), [submit](https://claude.com/docs/plugins/submit)
- Claude connectors: [submission](https://claude.com/docs/connectors/building/submission), [review criteria](https://claude.com/docs/connectors/building/review-criteria), [authentication](https://claude.com/docs/connectors/building/authentication)
- Claude Code: [plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces), [manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference), [marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference)
- Codex: [MCP and OAuth](https://developers.openai.com/codex/mcp)
- Cursor: [plugins](https://cursor.com/docs/plugins), [plugins reference and submission checklist](https://cursor.com/docs/reference/plugins), [MCP](https://cursor.com/docs/mcp), [plugin template](https://github.com/cursor/plugin-template), [Grok Bot plugins](https://cursor.com/help/grok-bot/connect-plugins)
- OpenAI: [build plugins](https://developers.openai.com/plugins/build/plugins), [submission](https://developers.openai.com/plugins/deploy/submission), [review](https://developers.openai.com/plugins/deploy/app-review), [guidelines](https://developers.openai.com/plugins/plugin-guidelines)
