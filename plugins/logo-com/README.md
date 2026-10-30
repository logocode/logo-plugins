# LOGO.com

Create a logo and brand basics (colors, fonts) from Claude, Codex, ChatGPT, or Cursor. This plugin connects to your [LOGO.com](https://logo.com) account through one remote MCP server, `https://mcp.logo.com/mcp`. You can also read account summaries, owned domain settings and health, and website status. Continue on [logo.com](https://logo.com) for brand-kit exports, DNS changes or website building.

**Use it when the user wants to:** create a logo, design a logo, make a logo for a business, generate logo ideas, get a brand mark with colors and fonts, or create a logo for a website.

## Requirements

- A LOGO.com account. Account summaries work on free plans.
- Premium access for logo tools, plus creation permission and existing AI credits for generation. Domain and website detail tools require a premium plan (including an active trial) for the selected organization. An account's `is_premium` flag alone does not guarantee access to those tools.
- Your LOGO.com sign-in. Your client opens a LOGO.com sign-in page the first time it connects.

## Tools

| Tool | What it does |
| --- | --- |
| `get_account` | Reads the selected organization's account summary, plan, AI credit balances, domains, business email setup, websites and product counts. Works on free plans and does not read email messages. |
| `list_logos` | Lists up to 50 recently created saved logos owned by you in the selected organization, with previews and editor links. |
| `get_logo` | Shows one logo's colors, fonts, previews and download files. |
| `generate_logos` | Creates up to 4 new logo designs from your business name and ideas. This uses your account's AI credits. |
| `get_logo_generation` | Checks the same generation job while it is pending. |
| `get_domain` | Reads an owned domain's settings, DNS records, forwarding, linked-site and email status when available. |
| `check_domain_health` | Checks an owned domain's live DNS health and reports problems and suggested fixes without changing DNS. |
| `get_site` | Lists websites or reads one returned site ID's publication and custom-domain status without editing or publishing. |

New designs are drafts until you open one in the LOGO.com editor and save it. Account, domain and website tools provide read-only status and settings. Website building, DNS changes and brand-kit exports happen on logo.com.

## Try it

- "Create a logo for my coffee shop, Bean There"
- "Design a brand logo for my startup and show the colors and fonts"
- "I need a logo for my new website. Design a few options"

## Cursor and Grok Bot

The plugin isn't in the Cursor Marketplace yet. Once it's listed, install LOGO.com from **Customize** in Cursor, or from **Plugins** in Grok Bot. Until then, copy this folder to `~/.cursor/plugins/local/logo-com` and Cursor loads it; see the [repo README](https://github.com/logocode/logo-plugins#cursor-and-grok-bot). Sign in to the `logo-com` MCP server the first time you use it.

## Troubleshooting

- **A tool refuses your request.** Account summaries work on free plans. Logo tools require premium access; domain and website detail tools require a premium plan (including an active trial) for the selected organization. Generation also needs existing AI credits and permission to create in your organization. Check your account at [logo.com](https://logo.com).
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
