---
name: build-plugin
description: >-
  Create or change LOGO.com's plugin package (the Claude and Codex manifests,
  .mcp.json, assets and end-user skills) or this repo's marketplace files, then
  validate it and install it locally. Use for any edit under plugins/ or to a
  marketplace.json. For store listings, use prepare-store-submission instead.
---

# Build the plugin

One package under `plugins/logo-com/` serves both hosts. It connects to `https://mcp.logo.com/mcp`, which signs each user in with OAuth the first time they use it.

## Before you start

- Read `AGENTS.md` for the layout and the rules.
- Read the current docs for the host you're changing, because field names and limits change. The links are in `AGENTS.md`.
- Copy the shape of a published plugin that wraps a remote MCP server, such as [Linear's](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/linear).

## Steps

1. **MCP config.** Write `plugins/logo-com/.mcp.json` with one HTTP server named `logo-com` at `https://mcp.logo.com/mcp`. Add no headers or tokens: each client gets its own token through OAuth. Use one file for both hosts if both validators accept it, and split it only if one rejects it.
2. **Claude manifest.** Write `.claude-plugin/plugin.json` with `name`, `displayName`, `version`, `description`, `author` and `license`. [Fields](https://claude.com/docs/plugins/build)
3. **Codex manifest.** Write `.codex-plugin/plugin.json` with `name`, `version`, `description`, `author.name`, `mcpServers: "./.mcp.json"` and an `interface` block. The block holds `displayName`, `shortDescription`, `longDescription`, `developerName`, `category`, `websiteURL`, `privacyPolicyURL`, `termsOfServiceURL`, `logo`, `composerIcon` and up to 3 `defaultPrompt` entries. OpenAI's directory caps `displayName` and `shortDescription` at 30 characters, and every URL must be https. [Fields](https://developers.openai.com/plugins/deploy/submission)
4. **Assets.** Put icons in `assets/`. OpenAI wants square PNG, JPEG, WebP or SVG files between 48 and 4096 px and at most 5 MiB. Start from the approved circular icon that logo.com serves at `/apple-icon.png`, and ask for new brand artwork rather than drawing it.
5. **End-user skills (optional).** Add `skills/<name>/SKILL.md` only for a workflow the tools don't make obvious, such as how to brief a logo design. Every skill ships to users, so keep each one short.
6. **Marketplaces.** Add a `logo-com` entry to both files.
   - `.claude-plugin/marketplace.json` needs `name`, `owner`, and `plugins[]` entries with `name` and `source`. [Claude Code](https://code.claude.com/docs/en/plugin-marketplaces)
   - `.agents/plugins/marketplace.json` entries need `source`, `policy.installation`, `policy.authentication` and `category`. [Codex](https://developers.openai.com/plugins/build/plugins)

## Validate

1. Claude: `claude plugin validate ./plugins/logo-com`, then `claude plugin validate .` for the marketplace.
2. Codex: run the validator that `$plugin-creator` ships (`scripts/validate_plugin.py`) against `plugins/logo-com`.
3. Install from your checkout, and sign in with a premium test account:
   - Claude Code: `claude plugin marketplace add ./`, then `claude plugin install logo-com@<marketplace name>`.
   - Codex: `codex plugin marketplace add ./`, then `codex plugin add logo-com@<marketplace name>`, then start a new thread.
4. In each client, ask for your logos and confirm `list_logos` returns them.

## Done when

- Both validators pass, and you can explain any warning that's left.
- A fresh install in Claude Code and in Codex signs in and runs `list_logos`.
- Nothing in the diff is a secret, a token or a reviewer credential.
