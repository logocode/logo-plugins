---
name: build-plugin
description: >-
  Create or change LOGO.com's plugin package (the Claude, Codex and Cursor
  manifests, .mcp.json and mcp.json, assets and end-user skills) or this repo's
  marketplace files, then validate it and install it locally. Use for any edit
  under plugins/ or to a marketplace.json. For store listings, use
  prepare-store-submission instead.
---

# Build the plugin

One package under `plugins/logo-com/` serves Claude, Codex and Cursor. It connects to `https://mcp.logo.com/mcp`, which signs each user in with OAuth the first time they use it.

## Before you start

- Read `AGENTS.md` for the layout and the rules.
- Read the current docs for the host you're changing, because field names and limits change. The links are in `AGENTS.md`.
- For shape, compare a published plugin that wraps a remote MCP server: [Linear's Claude plugin](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/linear), or Canva's Codex plugin in the `openai-curated` marketplace that Codex clones to `~/.codex/.tmp/plugins/`.

## Steps

1. **MCP config.** `plugins/logo-com/.mcp.json` holds one server, `logo-com`, inside an `mcpServers` object, with `"type": "http"` and `"url": "https://mcp.logo.com/mcp"`. Claude and Codex read this file. Cursor's copy is `mcp.json`; see [Cursor](#cursor).
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
4. **Assets.** `assets/logo.png` is the approved circular icon that logo.com serves at `/apple-icon.png` (180 px). The Claude and Codex manifests point at it.
   - `assets/logo.svg` is the same icon as an SVG, for Cursor. Its path data is copied unchanged from the mark in logo.com's header, drawn in white on a `#1890FF` circle and placed to match `logo.png`. Change the two together.
   - OpenAI wants square PNG, JPEG, WebP or SVG icons of 48 to 4,096 px and at most 5 MiB. [Submission](https://developers.openai.com/plugins/deploy/submission)
   - Claude allows PNG, JPEG, GIF, WebP and SVG, and holds `.ico` files for a reviewer.
   - Ask for new brand artwork rather than drawing it.
5. **End-user skills (optional).** Add `skills/<name>/SKILL.md` only for a workflow the tools don't make obvious, such as how to brief a logo design. Every skill ships to users, so keep each one short. With skills, Codex also needs `"skills": "./skills/"`.
6. **Marketplaces.** All three are named `logo-plugins`, so the plugin installs as `logo-com@logo-plugins` in Claude Code and Codex.
   - `.claude-plugin/marketplace.json` needs `name`, `owner.name`, and `plugins[]` entries with `name` and a `source` starting with `./`. Add a `description`, or validate warns. Some names are reserved. [Reference](https://code.claude.com/docs/en/plugins/marketplace-reference)
   - `.agents/plugins/marketplace.json` has `name`, `interface.displayName`, and entries with `source: {"source": "local", "path": "./plugins/logo-com"}`, `policy.installation` (`AVAILABLE`), `policy.authentication` (`ON_INSTALL` or `ON_USE`) and `category`. [Build](https://developers.openai.com/plugins/build/plugins)
   - `.cursor-plugin/marketplace.json` is Cursor's. See [Cursor](#cursor).
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

## Cursor

Cursor reads its own manifest from the same package. Grok Bot installs plugins from the Cursor Marketplace, so this covers it too. [Reference](https://cursor.com/docs/reference/plugins)

1. **Manifest.** `.cursor-plugin/plugin.json`. Only `name` is required. Set `displayName`, `version`, `description`, `author.name`, `homepage`, `repository`, `license`, `keywords` and `logo`.
   - The reference doesn't list `displayName`, but Cursor's [plugin template](https://github.com/cursor/plugin-template) sets it and Cursor 3.24.9 reads it.
   - `logo` is a path relative to the plugin folder, `assets/logo.svg`. For a marketplace listing, Cursor turns it into a raw.githubusercontent.com URL at the listed commit.
   - Leave out `mcpServers`, `skills` and the other component paths. A path in the manifest replaces Cursor's discovery for that component.
   - Every path must be relative, with no `..`.
   - Cursor 3.24.9 looks for `.cursor-plugin/plugin.json` first and then `.claude-plugin/plugin.json`, so without this file it reads the Claude manifest.
2. **MCP config.** `mcp.json`, with no dot, at the plugin root. It holds one server, `logo-com`, with only `"url": "https://mcp.logo.com/mcp"`. Cursor infers the transport from the URL. [MCP](https://cursor.com/docs/mcp)
   - Keep the server name and URL the same as in `.mcp.json`. Cursor 3.24.9 also reads `.mcp.json`, before `mcp.json`, and accepts its `"type": "http"`. If the two disagree, `.mcp.json` wins.
   - Add no headers, `auth` block or client ID. Cursor registers itself with dynamic client registration. The redirect URIs it registers are in `submissions/cursor-marketplace.md`.
3. **Skills.** Cursor finds each `skills/<name>/SKILL.md`. It needs `name` (kebab-case) and `description` in the frontmatter, which Claude and Codex need anyway.
4. **Marketplace.** `.cursor-plugin/marketplace.json` needs `name`, `owner.name` and `plugins[]` entries with a unique `name` and a `source` path to the plugin folder. Cursor documents only `name` and `email` for `owner`. The plugin's own manifest overrides the entry's fields.

### Validate and install in Cursor

1. Run Cursor's template validator from the repo root. It only reads files. Expect one warning, that `hooks/hooks.json` is missing, because we have no hooks.
   ```sh
   curl -fsSL https://raw.githubusercontent.com/cursor/plugin-template/46216072ac5750f782f95bb325b4d12b7c3ae9c9/scripts/validate-template.mjs -o /tmp/validate-template.mjs
   node /tmp/validate-template.mjs
   ```
   Cursor publishes no JSON schema for its manifest or marketplace. The [Agent Plugins schemas](https://agent-plugins.org/schemas) are for the root `plugin.json` format, which we don't use.
2. Walk the submission checklist in the reference and record the result in `submissions/cursor-marketplace.md`.
3. Install a local copy, then open **Customize** in Cursor and check for LOGO.com, the `create-logo` skill and the `logo-com` MCP server. [Test locally](https://cursor.com/docs/plugins#test-plugins-locally)
   ```sh
   mkdir -p ~/.cursor/plugins/local
   cp -R plugins/logo-com ~/.cursor/plugins/local/
   ```
   - Copy the folder. Cursor skips a symlink that points outside `~/.cursor/plugins/local`.
   - Cursor 3.24.9 loads the folder within seconds, without a reload. It connects to the MCP server at once and registers an OAuth client with `https://logo.com` before anyone clicks to sign in. While new production clients aren't wanted (ENG-9751), leave out both `.mcp.json` and `mcp.json`, or point `mcp.json` at a local stand-in.
   - Remove it with `rm -rf ~/.cursor/plugins/local/logo-com`.
4. Sign in from **Customize** with a premium account and confirm `list_logos` returns your logos.

## Done when

- Both hosts install the plugin, `claude plugin validate --strict` passes, and you can explain any warning that's left.
- Cursor's template validator passes, and Cursor loads a local copy with the skill and the MCP server.
- A fresh install in Claude Code and in Codex signs in and runs `list_logos`.
- Nothing in the diff is a secret, a token or a reviewer credential.
