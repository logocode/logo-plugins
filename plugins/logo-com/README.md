# LOGO.com

Connects Claude, Codex and ChatGPT to your [LOGO.com](https://logo.com) account, so you can look through your logos and create new logo designs from a chat. The plugin adds one remote MCP server, `https://mcp.logo.com/mcp`, and nothing else.

## Requirements

- A LOGO.com premium plan. The tools refuse requests from free accounts.
- Your LOGO.com sign-in. Your client opens a LOGO.com sign-in page the first time it connects.

## Tools

| Tool | What it does |
| --- | --- |
| `list_logos` | Lists up to 50 of the most recent logos in your account, with previews and editor links. |
| `get_logo` | Shows one logo's colors, fonts, previews and download files. |
| `generate_logos` | Creates up to 4 new logo designs from your business name and ideas. This uses your account's AI credits. |
| `get_logo_generation` | Returns designs that were still being created. |

New designs are drafts until you open one in the LOGO.com editor and save it.

## Try it

- "Show me my most recent logos"
- "What colors and fonts does my latest logo use?"
- "Design a logo for my coffee shop, Bean There"

## Troubleshooting

- **The tools refuse your request.** They need a LOGO.com premium plan. Check your plan at [logo.com](https://logo.com).
- **Sign-in fails, or the tools stop working.** Sign in again:
  - Claude Code: run `/mcp`, choose `plugin:logo-com:logo-com` and authenticate.
  - Claude on the web or desktop: open the plugin's **Connectors** tab, then disconnect and connect LOGO.com.
  - Codex: run `codex mcp logout logo-com`, then `codex mcp login logo-com`.
- **Designs are still being created.** Ask again in a moment. Claude or Codex checks the same job instead of starting a new one.
- **A new design is missing from your account.** New designs are drafts until you open one in the LOGO.com editor and save it.

## Help and links

- Help: [help.logo.com](https://help.logo.com)
- Website: [logo.com](https://logo.com)
- Privacy policy: [logo.com/privacy-policy](https://logo.com/privacy-policy)
- Terms: [logo.com/terms-and-conditions](https://logo.com/terms-and-conditions)
