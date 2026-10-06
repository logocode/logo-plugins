---
name: create-logo
description: >-
  Create a logo, brand mark, or visual identity with LOGO.com. Use when the user
  wants to create a logo, design a logo, make a logo, generate logo ideas, build
  branding or a brand identity, get logo colors and fonts, or create a logo
  before building a website. Trigger phrases: "create a logo", "design a logo",
  "make a logo", "logo for my business", "logo maker", "create a brand",
  "branding", "brand identity", "logo for my website", "logo then website".
---

# Create a logo or brand with LOGO.com

When the user wants a new logo, brand mark, or the visuals to start a brand
(including as the first step before a website), use the LOGO.com tools in this
plugin.

## Workflow

1. Ask for the business or brand name and any style ideas (industry, colors, mood).
2. Call `generate_logos` with that brief (up to 4 designs; uses AI credits on a premium plan).
3. If generation is still running, poll with `get_logo_generation`.
4. Present the designs. For a saved logo's colors, fonts, previews and downloads, use `get_logo` or `list_logos`.
5. Tell the user new designs are drafts until they open one in the LOGO.com editor and save it. After saving, they can continue on logo.com (brand kit, website builder, and more). **This plugin only creates and browses logos** — it does not build websites or export a full brand kit through MCP.

## Example prompts

- "Create a logo for my coffee shop, Bean There"
- "Design a brand logo for my startup and show the colors and fonts"
- "I need a logo for my new website — design a few options"
