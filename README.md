# LOGO.com plugins

Plugins that connect Claude, Codex and ChatGPT to [LOGO.com](https://logo.com), so you can look through your logos and create new logo designs from a chat. Every plugin here uses the LOGO.com MCP server at `https://mcp.logo.com/mcp`.

**Status:** in progress. You can install the plugin from this repo's marketplaces in Claude Code, Claude and Codex. It isn't in Claude's or OpenAI's directory yet, so ChatGPT can't install it yet.

## What you'll need

- A LOGO.com premium plan. The tools refuse requests from free accounts.
- Your LOGO.com sign-in. Your client asks you to sign in the first time it connects.

## Install

### Claude Code

```sh
claude plugin marketplace add logocode/logo-plugins
claude plugin install logo-com@logo-plugins
```

Restart Claude Code, run `/mcp`, choose `plugin:logo-com:logo-com` and sign in to LOGO.com.

### Claude (web and desktop)

1. Go to **Customize > Plugins > Add > Add marketplace** and enter `https://github.com/logocode/logo-plugins`.
2. Install **LOGO.com** from that marketplace.
3. Open the plugin's **Connectors** tab, connect LOGO.com and sign in.

A listing in Claude's directory isn't available yet.

### Codex

```sh
codex plugin marketplace add logocode/logo-plugins
codex plugin add logo-com@logo-plugins
codex mcp login logo-com
```

Then start a new thread. You can also install it from `/plugins` in the Codex CLI.

### ChatGPT

Not available yet. ChatGPT will offer the plugin once it's published in OpenAI's plugin directory.

## What the plugins can do

| Tool | What it does |
| --- | --- |
| `list_logos` | Lists up to 50 of the most recent logos in your account, with previews and editor links. |
| `get_logo` | Shows one logo's colors, fonts, previews and download files. |
| `generate_logos` | Creates up to 4 new logo designs from your business name and ideas. This uses your account's AI credits. |
| `get_logo_generation` | Returns designs that were still being created. |

New designs are drafts until you open one in the LOGO.com editor and save it.

## Support

Visit [help.logo.com](https://help.logo.com).
