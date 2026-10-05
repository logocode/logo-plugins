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
- For shape, compare a published plugin that wraps a remote MCP server: [Linear's Claude plugin](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/linear), or Canva's Codex plugin in the `openai-curated` marketplace that Codex clones to `~/.codex/.tmp/plugins/`.

## Steps

1. **MCP config.** `plugins/logo-com/.mcp.json` holds one server, `logo-com`, inside an `mcpServers` object, with `"type": "http"` and `"url": "https://mcp.logo.com/mcp"`. Both hosts read this one file.
   - Claude's checklist blocks a remote server without a `type` of `http`, `sse` or `ws`. Codex accepts `http`. [Checklist](https://claude.com/docs/plugins/pre-submission-checklist)
   - Add no headers, tokens or `oauth` block. Each client registers itself with dynamic client registration, Claude takes the `logos` scope from the server's `WWW-Authenticate` header, and Codex from `scopes_supported` in its protected resource metadata. [Claude](https://claude.com/docs/connectors/building/authentication), [Codex](https://developers.openai.com/codex/mcp)
   - If Codex sign-in fails because the token's audience is wrong, add `"oauth_resource": "https://mcp.logo.com/mcp"` to the server. The Codex docs don't cover it, but OpenAI's own Linear package sets it and Claude's validator accepts it.
2. **Claude manifest.** `.claude-plugin/plugin.json`. [Fields](https://code.claude.com/docs/en/plugins/manifest-reference)
   - Only `name` is required, but claude.ai and Cowork list a plugin only if it has this file. Set `displayName`, `version`, `description`, `author.name`, `homepage`, `repository` and `license`.
   - `icon` is the path to the directory icon. Claude Code ignores it. The directory also reads `supportUrl`, `privacyPolicyUrl`, `termsOfServiceUrl` and `documentationUrl`, all https.
   - The checklist needs `license` here or a `LICENSE` file, and a `README.md` of at least 40 words in the plugin folder. Words in code blocks don't count. [Checklist](https://claude.com/docs/plugins/pre-submission-checklist)
   - Raise `version` on every release. Claude Code pins installs to it.
3. **Codex manifest.** `.codex-plugin/plugin.json`. [Fields](https://developers.openai.com/plugins/deploy/submission)
   - Required: `name`, `version` (semver), `description` (up to 4,000 characters), `author.name`, `mcpServers: "./.mcp.json"` and an `interface` block.
   - `interface` needs `displayName` and `shortDescription` (30 characters each for the directory), `longDescription`, `developerName`, `category`, `capabilities` (`[]` if none), `websiteURL`, `supportURL`, `privacyPolicyURL`, `termsOfServiceURL` (all https), `logo`, `composerIcon` and up to 3 `defaultPrompt` entries of up to 128 characters, with no @mentions. `brandColor` needs 2:1 contrast against white.
   - Leave out `apps` and hooks. A ZIP that has them can't be submitted.
   - OpenAI now prefers a root `plugin.json` with OpenAI settings under `extensions.com.openai`, and keeps `.codex-plugin/plugin.json` as a supported fallback. We use the fallback so each host has its own file. Moving is a layout change, so agree it first. [Build](https://developers.openai.com/plugins/build/plugins)
4. **Assets.** `assets/logo.png` is the approved circular icon that logo.com serves at `/apple-icon.png` (180 px). Both manifests point at it.
   - OpenAI wants square PNG, JPEG, WebP or SVG icons of 48 to 4,096 px and at most 5 MiB. [Submission](https://developers.openai.com/plugins/deploy/submission)
   - Claude allows PNG, JPEG, GIF, WebP and SVG, and holds `.ico` files for a reviewer.
   - Ask for new brand artwork rather than drawing it.
5. **End-user skills (optional).** Add `skills/<name>/SKILL.md` only for a workflow the tools don't make obvious, such as how to brief a logo design. Every skill ships to users, so keep each one short. With skills, Codex also needs `"skills": "./skills/"`.
6. **Marketplaces.** Both are named `logo-plugins`, so the plugin installs as `logo-com@logo-plugins` in both hosts.
   - `.claude-plugin/marketplace.json` needs `name`, `owner.name`, and `plugins[]` entries with `name` and a `source` starting with `./`. Add a `description`, or validate warns. Some names are reserved. [Reference](https://code.claude.com/docs/en/plugins/marketplace-reference)
   - `.agents/plugins/marketplace.json` has `name`, `interface.displayName`, and entries with `source: {"source": "local", "path": "./plugins/logo-com"}`, `policy.installation` (`AVAILABLE`), `policy.authentication` (`ON_INSTALL` or `ON_USE`) and `category`. [Build](https://developers.openai.com/plugins/build/plugins)
   - Use the same `category` as the Codex manifest. OpenAI doesn't publish the list of allowed values. Canva, Figma and Adobe use `Creativity` in the `openai-curated` marketplace. Confirm it in the dashboard when submitting.

## Validate

1. Claude: `claude plugin validate --strict ./plugins/logo-com`, then `claude plugin validate --strict .` for the marketplace. A marketplace run doesn't check the plugin's own files.
2. Codex has no validate command. Some versions of `$plugin-creator` ship `scripts/validate_plugin.py`, but it isn't always installed, and it wrongly rejects `interface.supportURL`, which the submission page requires. Validate by installing (step 3) and checking that `codex mcp get logo-com` shows the server.
3. Install from your checkout:
   - Claude Code: `claude plugin marketplace add ./`, then `claude plugin install logo-com@logo-plugins`. `claude mcp list` should show `plugin:logo-com:logo-com` as needing authentication.
   - Codex: `codex plugin marketplace add ./`, then `codex plugin add logo-com@logo-plugins`.
4. Sign in with a premium account, then ask for your logos and confirm `list_logos` returns them.
   - Claude Code: run `/mcp` in a session, choose `plugin:logo-com:logo-com` and authenticate.
   - Codex: `codex mcp login logo-com`, then start a new thread. The docs describe `mcp login` for servers in `config.toml`, but Codex also resolves the plugin's server by this name.

## Done when

- Both hosts install the plugin, `claude plugin validate --strict` passes, and you can explain any warning that's left.
- A fresh install in Claude Code and in Codex signs in and runs `list_logos`.
- Nothing in the diff is a secret, a token or a reviewer credential.
