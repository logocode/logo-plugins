# LOGO.com OpenAI review materials

Status: package materials updated on 2026-10-30 for a Link in Bio-only website review scenario. These changes have not been uploaded or submitted. Direct generation checks now complete, but visual brief adherence and full host review remain unresolved.

The [Codex manifest](../plugins/logo-com/.codex-plugin/plugin.json) is the source of truth for listing copy, five positive cases, three negative cases, the commerce description, and release notes. Review metadata lives under `extensions.com.openai.review`; release notes live under `extensions.com.openai.publication`. The package retains the supported Codex manifest layout.

## Listing

| Field | Value |
| --- | --- |
| Plugin identifier | `logo-com` |
| Package version | `0.1.4` |
| Display name | LOGO.com |
| Subtitle | Logos, domains & account |
| Developer name | LOGO.com; confirm the matching verified identity in the portal |
| Category | Creativity; confirm the portal accepts it |
| Website | https://logo.com |
| Support | https://help.logo.com |
| Privacy policy | https://logo.com/privacy-policy |
| Terms | https://logo.com/terms-and-conditions |
| MCP endpoint | https://mcp.logo.com/mcp |
| Icon | `assets/logo.png`, 180 x 180 PNG |

The four public URLs were opened on 2026-10-29 and contain the corresponding LOGO.com pages. The privacy policy describes brand inputs, AI processing, service providers, and operational logs. Confirm that published policies cover the integration's actual practices before making the portal attestations.

## Capabilities and limits

- `get_account` reads the selected organization's account summary, plan, AI credit balances, domains, business email setup, websites and product counts. It works on free plans, respects list limits and does not read email messages.
- `get_domain` reads an owned domain's settings, DNS records, forwarding, linked-site and email status when available. Discover the domain through `get_account`; unavailable registrar fields can be null.
- `check_domain_health` checks an owned domain's live DNS health and reports problems and suggested fixes without changing configuration.
- `get_site` lists websites without an argument, or reads publication and custom-domain status for a returned `site_id`. It does not edit or publish sites.
- `list_logos` returns up to 50 recently created logos owned by the connected user in the organization selected during consent. It does not expose a next-page input.
- `get_logo` returns a saved logo's colors, fonts, previews, editor link, and available high-resolution PNG, transparent PNG, SVG, and PDF download links.
- `generate_logos` creates one to four draft designs from a business name and optional slogan, industry, style, colors, and icon idea. It consumes existing AI credits and can return a pending job.
- `get_logo_generation` checks the returned job and may wait up to 25 seconds. Continue polling a pending job instead of creating another one.
- Logo tools require premium user access. Domain and website detail tools require a premium plan (including an active trial) for the selected organization; the account summary's `is_premium` flag alone does not prove eligibility. Generation also requires creation permission and sufficient existing AI credits. Run generation cases sequentially.
- Editing and saving occur in the LOGO.com editor. Generated designs are not automatically saved logos.
- The plugin does not create standalone photos, retrieve another company's official logos, change DNS, edit or publish websites, read email messages, export a full brand kit, change subscriptions, or sell credits.

Expected annotations, to confirm against the server:

| Tool | `readOnlyHint` | `destructiveHint` | `openWorldHint` |
| --- | --- | --- | --- |
| `get_account` | true | false | false |
| `get_domain` | true | false | false |
| `check_domain_health` | true | false | false |
| `get_site` | true | false | false |
| `list_logos` | true | false | false |
| `get_logo` | true | false | false |
| `get_logo_generation` | true | false | false |
| `generate_logos` | false | false | false |

`generate_logos` creates drafts and spends existing AI credits, but deletes and overwrites nothing. Be ready to explain why it isn't marked destructive.

## Review case checklist

Use the exact prompts and expected results embedded in the manifest. All eight cases still need to be run through the intended host with the dedicated reviewer account. A direct tool smoke test does not prove prompt routing or completion of a review case.

P4 passes only when the generation completes with one design. P5 passes only when it completes with four designs. Correctly reporting a partial or failed generation is required error handling, but does not pass the successful-generation case.

| Case | Scenario | Expected tools | Reviewer-account status |
| --- | --- | --- | --- |
| P1 | Account summary, plan, credits and product inventory | `get_account` | Not run |
| P2 | Browse saved logos, inspect colors/fonts and verify four download formats | `list_logos`, `get_logo` | Not run |
| P3 | List and inspect an existing Link in Bio site | `get_site` | Not run |
| P4 | Generate one design from a detailed bakery brief | `generate_logos`; `get_logo_generation` if pending | Not run |
| P5 | Generate four cycling-shop options and follow progress | `generate_logos`; `get_logo_generation` if pending | Not run |
| N1 | Standalone photograph | No LOGO.com tools | Not run |
| N2 | Another company's official logo | No LOGO.com tools | Not run |
| N3 | Subscription upgrade or credit purchase | No LOGO.com tools | Not run |

Preparation checks on 2026-10-29 used the connected account, not a fresh host session with the dedicated reviewer account:

- Account summary, saved-logo list and detail calls succeeded. The four download formats, preview and editor URLs returned HTTP 200 with expected download formats.
- Domain and website detail calls returned `PREMIUM_REQUIRED`. Successful P3 coverage needs an eligible paid organization with sample domain and website fixtures.
- The one-design generation failed. The four-design generation returned two designs and two failures. Server logs reported `LOGO_RENDER_FAILED`. These outcomes do not pass P4 or P5. Resolve the server issue before authorized reruns.
- Fresh-host prompt routing, the three negative cases and interactive editor behavior remain unverified. Direct tool responses do not establish those results.

Latest direct checks on 2026-10-30 used the dedicated reviewer account:

- Account summary, saved-logo list/detail, and Link in Bio list/detail succeeded. The sample one-page site was published and had no custom domain. Its earlier `PREMIUM_REQUIRED` refusal no longer occurs.
- Both PNG variants, SVG and PDF completed downloads in the authenticated Work browser. File contents and editor interactions still need visual verification.
- The one-design and four-design jobs completed with exactly five total designs and no render failures. Both initial tool responses reported completed, so a pending-job polling path was not exercised.
- Visual inspection found a bull icon instead of the bakery brief's wheat stalk, and a dark icon with poor contrast in one cycling design. Successful rendering is not proof of brief adherence or visual quality. Keep these findings open rather than changing the prompts to conceal them.
- The account summary showed no change in image or text credit balances immediately after these calls. This is only the observed balance, not proof of billing behavior.
- Fresh-host prompt routing and all three negative cases remain unverified. Domain tools are still advertised server capabilities but are outside this recording scenario and were not exercised against an owned domain.

## Demo recording plan

The owner is preparing the recording. Show real interactions in the intended ChatGPT or Codex host with sample data, using the package version selected for submission.

1. Show LOGO.com connected without exposing credentials, OAuth tokens, or unrelated account data. Explain free account summaries, premium logo access, the premium organization requirement for domain/site details, including active trials, and existing credits used for generation.
2. Run P1 and show the account summary, plan, credits and product counts.
3. Run P2, compare the saved logo's colors and fonts with the editor, and open all four download links to verify the selected logo and formats.
4. Run P3 against the existing Link in Bio sample site. Show its URL, publication state and custom-domain status without making changes. Do not demonstrate Gen websites or domain tools.
5. Run P4. Show the actual preview and open its editor link. Explain that saving is a separate editor action.
6. After P4 finishes, run P5. If generation is pending, show polling of that same job. If it completes immediately, show the completed response without claiming the pending path was exercised. Show all returned designs and their corresponding editor links.
7. Run N1, N2, and N3. Show the unsupported-request explanations and absence of LOGO.com tool calls.
8. Review the recording for readable prompts, correct results, and exposed private data. Host it at a reviewer-accessible URL and verify playback without private access requests.

Add the verified recording URL as `extensions.com.openai.review.demo_recording_url` before rebuilding the final ZIP. It is currently omitted; a script or placeholder URL is not a completed recording.

## Remaining preparation

- The owner is preparing reviewer access. Use a dedicated reviewer user with premium logo access and a selected organization on a premium plan (including an active trial). Seed owned sample logos with known colors, fonts and working downloads, and an existing Link in Bio site with known publication and custom-domain status. No owned domain or Gen website is required for these cases. Provide sufficient existing credits for five designs and any separately authorized reruns.
- Verify sign-in from a fresh browser without email or SMS codes, magic links, MFA approval, or access to a private network. Enter credentials and private sign-in instructions only in the portal.
- Run all eight cases against that account and record pass/fail evidence privately. Resolve failures before submission.
- Add and verify the demo recording URL.
- Confirm the verified developer identity and country availability. `publication.countries` is omitted pending that decision; omission is not a declaration of worldwide availability.
- Confirm actual data practices and policy coverage. Complete legal and policy attestations in the portal.
- Confirm the server sets `readOnlyHint`, `destructiveHint` and `openWorldHint` explicitly on every tool, as in the table under Capabilities and limits.
- Confirm the message free accounts get doesn't promote an upgrade or link to checkout. Only a link to an informational plans page is allowed. [Guidelines](https://developers.openai.com/plugins/plugin-guidelines)
- After the server fix, recheck all eight live tool schemas, annotations, OAuth metadata, UI metadata and server instructions against this package. Package wording cannot override server-owned definitions.
- Version 0.1.4 contains the Link in Bio case update. The personal browser directory still showed cloud package 0.1.1 with a desktop-only launch option during preflight. Verify the portal draft and imported fields before upload; the directory entry does not establish the current submission draft state.

## Package and portal handoff

After the remaining preparation is complete, create a ZIP containing the single `logo-com` directory under `plugins/`, including hidden manifests, `.mcp.json`, the README, and the referenced asset. Do not archive the repository root or include this preparation file. Inspect the ZIP for missing files and private data.

The bundled Plugin Creator validator is older than the current submission schema: it rejects `interface.supportURL` and the `extensions` field used for review metadata. Preserve these documented fields. Check the current official field reference and the portal's validation instead of removing them to satisfy the stale validator.

Upload the ZIP as a draft under the intended organization and verified identity. Complete the portal's domain challenge for the MCP hostname or its allowed parent origin, connect OAuth, and inspect the discovered tools and required findings. Verify imported cases, release notes, commerce details, and country availability. Submission for review and publication after approval are separate actions.

## Official references

- [Upload and submit a plugin](https://developers.openai.com/plugins/deploy/submission)
- [Review and publication fields](https://developers.openai.com/plugins/deploy/submission#configure-onboarding-review-and-publication)
- [Plugin guidelines](https://developers.openai.com/plugins/plugin-guidelines)
- [Remote MCP server review requirements](https://developers.openai.com/plugins/deploy/app-review)
