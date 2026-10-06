---
name: create-logo
description: >-
  Create a logo or brand mark with colors and fonts via LOGO.com. Use when the
  user wants to create a logo, design a logo, make a logo, generate logo ideas,
  get logo colors and fonts, or create a logo for a website. Do not use for
  building or publishing a website, or exporting a full brand kit through MCP —
  create the logo here, then continue on logo.com. Trigger phrases: "create a
  logo", "design a logo", "make a logo", "logo for my business", "logo maker",
  "brand mark", "logo colors and fonts", "logo for my website", "logo then
  website".
---

# Create a logo with LOGO.com

When the user wants a new logo or brand mark with colors and fonts (including a
logo for a website), use the LOGO.com tools in this plugin.

## Don't use for

- Building or publishing a website
- Exporting a full brand kit through MCP

Create the logo here, then continue on logo.com for brand kit or a website.

## Workflow

1. Ask for the business or brand name and any style ideas (industry, colors, mood).
2. Call `generate_logos` with that brief (up to 4 designs; uses AI credits on a premium plan).
3. If generation is still running, poll with `get_logo_generation` (poll for designs still generating).
4. Present the designs. For a saved logo's colors, fonts, previews and downloads, use `get_logo` or `list_logos`.
5. Tell the user new designs are drafts until they open one in the LOGO.com editor and save it. After saving, they can continue on logo.com (brand kit, website builder, and more). **This plugin only creates and browses logos** — it does not build websites or export a full brand kit through MCP.

## Example prompts

- "Create a logo for my coffee shop, Bean There"
- "Design a brand logo for my startup and show the colors and fonts"
- "I need a logo for my new website — design a few options"
