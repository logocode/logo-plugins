# LOGO.com

Create a logo and brand basics (colors, fonts) from Claude, Codex, ChatGPT, or Cursor. This plugin connects to your [LOGO.com](https://logo.com) account through one remote MCP server, `https://mcp.logo.com/mcp`. Create a logo here, then continue on [logo.com](https://logo.com) for brand kit or a website.

**Use it when the user wants to:** create a logo, design a logo, make a logo for a business, generate logo ideas, get a brand mark with colors and fonts, or create a logo for a website.

## Requirements

- A LOGO.com premium plan. The tools refuse requests from free accounts.
- Your LOGO.com sign-in. Your client opens a LOGO.com sign-in page the first time it connects.

## Tools

| Tool | What it does |
| --- | --- |
| `list_logos` | Lists up to 50 of the most recent logos in your account, with previews and editor links. |
| `get_logo` | Shows one logo's colors, fonts, previews and download files. |
| `generate_logos` | Creates up to 4 new logo designs from your business name and ideas. This uses your account's AI credits. |
| `get_logo_generation` | Poll for designs still generating. |

New designs are drafts until you open one in the LOGO.com editor and save it. Website building and brand-kit extras live on logo.com after you save; they are not tools in this plugin.

## Try it

- "Create a logo for my coffee shop, Bean There"
- "Design a brand logo for my startup and show the colors and fonts"
- "I need a logo for my new website — design a few options"

## Cursor and Grok Bot

The plugin isn't in the Cursor Marketplace yet. Once it's listed, install LOGO.com from **Customize** in Cursor, or from **Plugins** in Grok Bot. Until then, copy this folder to `~/.cursor/plugins/local/logo-com` and Cursor loads it; see the [repo README](https://github.com/logocode/logo-plugins#cursor-and-grok-bot). Sign in to the `logo-com` MCP server the first time you use it.

## Troubleshooting

- **The tools refuse your request.** They need a LOGO.com premium plan, and creating designs also needs AI credits and permission to create in your organization. Check your account at [logo.com](https://logo.com).
- **Sign-in fails, or the tools stop working.** Sign in again:
  - Claude Code: run `/mcp`, choose `plugin:logo-com:logo-com` and authenticate.
  - Claude on the web or desktop: open the plugin's **Connectors** tab and connect LOGO.com again.
  - Codex: run `codex mcp logout logo-com`, then `codex mcp login logo-com`.
  - Cursor or Grok Bot: open the plugin in **Customize** in Cursor, or **Plugins** in Grok Bot, and sign in again.
- **Designs are still being created.** Ask Claude or Codex to check on them in a moment. It should keep checking the same job rather than start a new generation.
- **A new design is missing from your account.** New designs are drafts until you open one in the LOGO.com editor and save it.

## Help and links

- Help: [help.logo.com](https://help.logo.com)
- Website: [logo.com](https://logo.com)
- Privacy policy: [logo.com/privacy-policy](https://logo.com/privacy-policy)
- Terms: [logo.com/terms-and-conditions](https://logo.com/terms-and-conditions)
