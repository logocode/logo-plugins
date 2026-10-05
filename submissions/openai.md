# LOGO.com OpenAI review materials

Status: draft materials prepared on 2026-10-05. Not submitted or ready for review yet.

The [Codex manifest](../plugins/logo-com/.codex-plugin/plugin.json) is the source of truth for listing copy, five positive cases, three negative cases, the commerce description, and release notes. Review metadata lives under `extensions.com.openai.review`; release notes live under `extensions.com.openai.publication`. The package retains the supported Codex manifest layout.

## Listing

| Field | Value |
| --- | --- |
| Plugin identifier | `logo-com` |
| Package version | `0.1.0` |
| Display name | LOGO.com |
| Subtitle | Browse and create logos |
| Developer name | LOGO.com; confirm the matching verified identity in the portal |
| Category | Creativity; confirm the portal accepts it |
| Website | https://logo.com |
| Support | https://help.logo.com |
| Privacy policy | https://logo.com/privacy-policy |
| Terms | https://logo.com/terms-and-conditions |
| MCP endpoint | https://mcp.logo.com/mcp |
| Icon | `assets/logo.png`, 180 x 180 PNG |

The four public URLs were opened on 2026-10-05 and contain the corresponding LOGO.com pages. The privacy policy describes brand inputs, AI processing, service providers, and operational logs. Confirm that published policies cover the integration's actual practices before making the portal attestations.

## Capabilities and limits

- `list_logos` returns up to 50 recently created logos owned by the connected user in the organization selected during consent. It does not expose a next-page input.
- `get_logo` returns a saved logo's colors, fonts, previews, editor link, and available high-resolution PNG, transparent PNG, SVG, and PDF download links.
- `generate_logos` creates one to four draft designs from a business name and optional slogan, industry, style, colors, and icon idea. It consumes existing AI credits and can return a pending job.
- `get_logo_generation` checks the returned job and may wait up to 25 seconds. Continue polling a pending job instead of creating another one.
- A premium account is required. Its organization role must allow creation for generation cases. Run generation cases sequentially.
- Editing and saving occur in the LOGO.com editor. Generated designs are not automatically saved logos.
- The plugin does not create standalone photos, retrieve another company's official logos, change subscriptions, or sell credits.

## Review case checklist

Use the exact prompts and expected results embedded in the manifest. All eight cases still need to be run through the intended host with the dedicated reviewer account. A direct tool smoke test does not prove prompt routing or completion of a review case.

P4 passes only when the generation completes with one design. P5 passes only when it completes with four designs. Correctly reporting a partial or failed generation is required error handling, but does not pass the successful-generation case.

| Case | Scenario | Expected tools | Reviewer-account status |
| --- | --- | --- | --- |
| P1 | Browse recent saved logos | `list_logos` | Not run |
| P2 | Inspect colors and fonts | `list_logos`, `get_logo` | Not run |
| P3 | Download PNG, transparent PNG, SVG, and PDF | `list_logos`, `get_logo` | Not run |
| P4 | Generate one design from a detailed bakery brief | `generate_logos`; `get_logo_generation` if pending | Not run |
| P5 | Generate four cycling-shop options and follow progress | `generate_logos`; `get_logo_generation` if pending | Not run |
| N1 | Standalone photograph | No LOGO.com tools | Not run |
| N2 | Another company's official logo | No LOGO.com tools | Not run |
| N3 | Subscription upgrade or credit purchase | No LOGO.com tools | Not run |

Preparation checks completed on 2026-10-05:

- Authenticated `list_logos` and `get_logo` smoke calls succeeded on the existing development connection. The detail response matched the requested logo and returned colors, fonts, and four HTTPS download URLs. Customer content and identifiers are not included here.
- Download contents and editor interactions were not tested in that smoke check.
- The MCP, OAuth, discovery, access-control, and generation-job unit suites passed: 200 tests across eight files. These are local tests with mocked dependencies, not production or reviewer-account end-to-end results.
- No paid generation was run during preparation. The generation and negative prompt cases remain untested in the intended host.

## Demo recording plan

The owner is preparing the recording. Show real interactions in the intended ChatGPT or Codex host with sample data, using the package version selected for submission.

1. Show LOGO.com connected without exposing credentials, OAuth tokens, or unrelated account data. State the premium-plan requirement and that generation uses existing credits.
2. Run P1 and show the saved logo previews and editor links.
3. Run P2 and compare one logo's colors and fonts with the editor.
4. Run P3 and open the four returned download links. Verify the files contain the selected logo in the expected formats.
5. Run P4. Show the actual preview and open its editor link. Explain that saving is a separate editor action.
6. After P4 finishes, run P5. If generation is pending, show polling of that same job. If it completes immediately, show the completed response without claiming the pending path was exercised. Show all returned designs and their corresponding editor links.
7. Run N1, N2, and N3. Show the unsupported-request explanations and absence of LOGO.com tool calls.
8. Review the recording for readable prompts, correct results, and exposed private data. Host it at a reviewer-accessible URL and verify playback without private access requests.

Add the verified recording URL as `extensions.com.openai.review.demo_recording_url` before rebuilding the final ZIP. It is currently omitted; a script or placeholder URL is not a completed recording.

## Remaining preparation

- The owner is preparing reviewer access. Use sample saved logos with known colors, fonts, and working downloads. Provide sufficient existing credits for five designs plus generation preparation and any authorized reruns.
- Verify sign-in from a fresh browser without email or SMS codes, magic links, MFA approval, or access to a private network. Enter credentials and private sign-in instructions only in the portal.
- Run all eight cases against that account and record pass/fail evidence privately. Resolve failures before submission.
- Add and verify the demo recording URL.
- Confirm the verified developer identity and country availability. `publication.countries` is omitted pending that decision; omission is not a declaration of worldwide availability.
- Confirm actual data practices and policy coverage. Complete legal and policy attestations in the portal.
- Finish the separate server audit follow-up before submission.

## Package and portal handoff

After the remaining preparation is complete, create a ZIP containing the single `logo-com` directory under `plugins/`, including hidden manifests, `.mcp.json`, the README, and the referenced asset. Do not archive the repository root or include this preparation file. Inspect the ZIP for missing files and private data.

The bundled Plugin Creator validator is older than the current submission schema: it rejects `interface.supportURL` and the `extensions` field used for review metadata. Preserve these documented fields. Check the current official field reference and the portal's validation instead of removing them to satisfy the stale validator.

Upload the ZIP as a draft under the intended organization and verified identity. Complete the portal's domain challenge for the MCP hostname or its allowed parent origin, connect OAuth, and inspect the discovered tools and required findings. Verify imported cases, release notes, commerce details, and country availability. Submission for review and publication after approval are separate actions.

## Official references

- [Upload and submit a plugin](https://developers.openai.com/plugins/deploy/submission)
- [Review and publication fields](https://developers.openai.com/plugins/deploy/submission#configure-onboarding-review-and-publication)
- [Plugin guidelines](https://developers.openai.com/plugins/plugin-guidelines)
- [Remote MCP server review requirements](https://developers.openai.com/plugins/deploy/app-review)
