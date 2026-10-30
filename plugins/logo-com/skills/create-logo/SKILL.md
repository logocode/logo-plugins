---
name: create-logo
description: >-
  Create a logo or brand mark with colors and fonts via LOGO.com. Use when the
  user wants to create a logo, design a logo, make a logo, generate logo ideas,
  get logo colors and fonts, or create a logo for a website. Do not use for
  building or publishing a website, or exporting a full brand kit through MCP;
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

1. Use the business or brand name and style ideas already provided. Ask only for missing details needed for the brief.
2. Call `generate_logos` with that brief (up to 4 designs; uses AI credits on a premium plan).
3. If generation is pending, poll `get_logo_generation` with the returned generation ID. Report partial or failed results accurately and do not start paid replacements without the user's authorization.
4. Present the returned previews and editor links. For saved logos, resolve a real logo ID through `list_logos`, then use `get_logo` for colors, fonts, previews and downloads. Do not use an unsaved draft ID as a saved logo ID.
5. Tell the user new designs are drafts until they open one in the LOGO.com editor and save it. Brand-kit exports and website building happen on logo.com.

## Related account tools

The plugin also exposes `get_account` for account summaries, plan details, AI credit balances and product counts on free or premium plans. It summarizes business email setup but does not read messages.

For domain and website questions, use the read-only tools rather than generating a logo: resolve owned domains through `get_account`, inspect settings with `get_domain`, check live DNS health with `check_domain_health`, and list websites with `get_site` before inspecting a returned site ID. Domain and website detail tools require a premium plan (including an active trial) for the selected organization. An account's `is_premium` flag alone does not establish that eligibility. Respect unavailable fields and truncated lists.

These tools do not change DNS, edit or publish websites, change subscriptions or buy credits. Report access errors without claiming the requested inspection succeeded.

## Example prompts

- "Create a logo for my coffee shop, Bean There"
- "Design a brand logo for my startup and show the colors and fonts"
- "I need a logo for my new website. Design a few options"
