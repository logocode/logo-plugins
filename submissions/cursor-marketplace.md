# LOGO.com Cursor Marketplace materials

Status: **NOT SUBMITTED.** Updated on 2026-10-19. ENG-9556 and ENG-9750 are done. Authenticated tool-use tests passed in Cursor web, Cursor desktop, and Grok Bot. Desktop and Grok results are user-reported. Grok reused existing authentication; fresh Grok OAuth, consent, and callback evidence were not captured. Harrison has authorized finishing submission under ENG-9753. The publisher application and final Publisher Terms acceptance remain.

Cursor lists plugins from public Git repositories, and its team reviews each plugin and each update by hand. You submit the repository link at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish). [Plugins](https://cursor.com/docs/plugins), [Reference](https://cursor.com/docs/reference/plugins)

Grok Bot installs plugins from the Cursor Marketplace and follows the team's Cursor connector policy, so this one listing covers Grok Bot too. Cursor staff also describes adding a custom MCP URL through a bot conversation. That provides a possible OAuth test before publication; it does not verify marketplace installation. Two LOGO.com setup attempts on October 14 failed before OAuth, according to the bot's responses. A later Cursor web dashboard setup saved the intended test connection. On October 19, Harrison reported that Grok used that already-authenticated connection and successfully called `list_logos`. [Grok Bot for teams](https://cursor.com/docs/grok-bot/teams), [Connect plugins](https://cursor.com/help/grok-bot/connect-plugins), [Cursor staff guidance](https://forum.cursor.com/t/grokbot-custom-connectors/169965/5)

## Submission

| Field | Value |
| - | - |
| Submit at | https://cursor.com/marketplace/publish |
| Repository | https://github.com/logocode/logo-plugins (public) |
| Marketplace manifest | `.cursor-plugin/marketplace.json`, marketplace `logo-plugins` |
| Plugin folder | `plugins/logo-com` |
| Plugin name | `logo-com`, permanent once published |
| Branch | `main` |

The signed-in publish form was inspected without editing or submitting it on 2026-10-13. It is titled **Become a plugin publisher** and exposes these fields. Values below are a draft, not a submitted application.

| Form field | Draft value |
| - | - |
| Organization name | LOGO.com |
| Organization handle | `logo-com`, proposed to meet the form's kebab-case guidance. The form currently prefills `logo.com`; namespace availability is unverified. |
| Contact email | Use the submitter's contact address in the portal. Keep it out of this public repository. |
| Logotype URL | https://raw.githubusercontent.com/logocode/logo-plugins/main/plugins/logo-com/assets/logo.svg. The file exists on `main`. The form asks for a 1:1 SVG or PNG with a background plate. |
| Description | Create logos with LOGO.com, browse your existing designs, and get brand colors and fonts from chat. Requires a LOGO.com premium plan. |
| GitHub repository | https://github.com/logocode/logo-plugins |
| Owner | Team · LOGO.com, shown as a disabled field |
| Website URL | https://logo.com |

The final **Submit Application** action accepts Cursor's Publisher Terms. It was not clicked. [Publish form](https://cursor.com/marketplace/publish)

## Listing

Cursor merges `plugins/logo-com/.cursor-plugin/plugin.json` over the marketplace entry, with the manifest's values taking precedence. The checklist also asks for a README that documents usage, which is `plugins/logo-com/README.md`.

| Field | Value |
| - | - |
| `name` | `logo-com` |
| `displayName` | `LOGO.com` in the manifest. Cursor 3.24.12 Customize displayed the local package as `Logo Com` on October 13. This remains a recorded discrepancy; the heading has not been rechecked in the currently installed Cursor 3.25.6. |
| `description` | Create a logo, get brand basics (colors, fonts), or make a logo for a website. Browse your LOGO.com logos and generate new designs from a chat. Use for: create a logo, design a logo, make a logo, logo maker, brand mark, logo for my business, logo for my website. Requires a LOGO.com premium plan. |
| `version` | 0.1.1, the same as the Claude and Codex manifests |
| `author` | LOGO.com |
| `homepage` | https://logo.com |
| `repository` | https://github.com/logocode/logo-plugins |
| `license` | MIT |
| `keywords` | logo, branding, brand, design, logo maker, visual identity, website logo |
| `logo` | `assets/logo.svg`. A 180 x 180 SVG of the mark in logo.com's header, in white on a `#1890FF` circle, laid out to match `assets/logo.png`. |

Cursor turns the relative `logo` path into a raw.githubusercontent.com URL at the listed commit.

What the plugin installs:

- The `logo-com` MCP server at `https://mcp.logo.com/mcp`, from `mcp.json`. Its tools are `list_logos`, `get_logo`, `generate_logos` and `get_logo_generation`.
- The `create-logo` skill, from `skills/create-logo/SKILL.md`.

### Example prompts

1. Show me my most recent logos.
2. What colors and fonts does my latest logo use?
3. Create a logo for my coffee shop, Bean There.
4. I need a logo for my new website. Design a few options.

Each one needs a LOGO.com premium plan. Prompts 3 and 4 use AI credits.

## Sign-in

The server signs users in with OAuth and dynamic client registration. Unauthenticated requests get `401` with a `WWW-Authenticate` header that names the resource metadata and the `logos account:read` scope. The authorization server at `https://logo.com` advertises a registration endpoint and S256 PKCE. Add no headers, `auth` block or client ID to `mcp.json`. [MCP in Cursor](https://cursor.com/docs/mcp)

### Redirect URIs

The docs list two fixed redirect URIs:

- `https://www.cursor.com/agents/mcp/oauth/callback` for the web and Cursor Agents
- `http://localhost:8787/callback` for the desktop app

ENG-9625 also lists `cursor://anysphere.cursor-mcp/oauth/callback`. The docs don't mention it.

On 2026-10-09, Cursor 3.24.9 on macOS loaded the plugin against a local stand-in for the server. It sent this registration request on its own (`logo_uri` left out here):

```json
{
  "redirect_uris": ["https://www.cursor.com/agents/mcp/oauth/callback", "http://localhost:8787/callback"],
  "token_endpoint_auth_method": "none",
  "grant_types": ["authorization_code", "refresh_token"],
  "response_types": ["code"],
  "client_name": "Cursor",
  "scope": "logos account:read"
}
```

It didn't include the `cursor://` URI. The same app also contains these redirect URIs, which the docs don't mention:

- `cursor://anysphere.cursor-mcp/oauth/callback` and `cursor://anysphere.cursor-mcp/oauth/return`
- `https://www.cursor.com/bot/mcp/oauth/callback` and `grokbot://mcp/oauth/callback`, probably for Grok Bot
- `http://localhost:18787/callback`, `http://localhost:28787/callback` and `http://localhost:31787/callback`

Grok Bot's sign-in hasn't been observed, so its redirect URIs are unconfirmed. Check what each client actually registers before setting the allowlist.

Cursor would skip registration and use its client ID metadata document, `https://cursor.com/oauth/mcp-client.json`, but only if the authorization server advertised `client_id_metadata_document_supported`. `https://logo.com` doesn't.

Cursor registers a client as soon as it loads the plugin, before anyone clicks to sign in. So installing the plugin creates a client registration at `https://logo.com` even if the user never signs in.

### Dependencies and production verification

- ENG-9556: done. The deployed allowlist accepts the documented Cursor web callback and desktop loopback callback. The desktop Cloud Login callback was not captured, so `http://localhost:8787/callback` has deployed-code coverage but no observed production round trip. If the allowlist rejects any URI in a registration request, the whole registration fails.
- ENG-9750: done. Production deployment evidence passed for the change that resumes OAuth consent after sign-in. A fresh Google/email-code sign-in returning to consent remains unconfirmed at runtime.
- ENG-9751 verification: authenticated tool-use checks passed in all three clients. Cursor desktop and Grok results are user-reported, with the evidence limits below. Fresh Grok OAuth was not observed because the connection was already signed in.

On 2026-10-13, Cursor web added a personal `logo-com` HTTP MCP connection to `https://mcp.logo.com/mcp`. Its Login action reached LOGO.com's production consent screen with `https://www.cursor.com/agents/mcp/oauth/callback`, S256 PKCE, and the `logos account:read offline_access` scopes. On 2026-10-14, Harrison confirmed signing into the reviewer account. Cursor web showed the connection Online, and a dedicated verification agent successfully called `list_logos` through `logo-com`, returning zero logos. The reviewer identity and active Brand trial were subsequently confirmed in the account UI. Current production source treats that trial as premium for `list_logos`; a paid subscription was not verified. The sign-in-to-consent transition was not independently observed.

Later on October 14, Chrome at `cursor.com/dashboard/plugins` → **Personal** → **Add** → **Custom MCP** saved `logo-com-grok-review` with URL `https://mcp.logo.com/mcp`, no headers, and no client credential overrides. After reloading, selecting Personal, and searching for `logo`, both `logo-com` and `logo-com-grok-review` showed **Configured**. That observation verified saved configuration only. Native Grok selection then failed with ScreenCaptureKit `-3811` before the dedicated test bot could be selected. The first bot setup had timed out after about 14 minutes, and the authorized retry had been cut off after about four minutes. Neither setup attempt reached OAuth. These historical setup and UI failures preceded Harrison's successful tool test.

On October 19, Harrison was asked whether Cloud Login inside Cursor desktop included successful `list_logos` or tool use rather than sign-in alone. He confirmed: "yes, tools worked". Desktop Cloud Login and authenticated tool use are recorded as **Pass (user-reported)**. No independent desktop tool transcript, screenshot, exact output, result count, or callback was captured. The currently installed Cursor version is 3.25.6. The installed local manifest, both MCP configuration files, `create-logo` skill, and SVG were checked on October 19 and matched this branch.

On October 19, Harrison reported that Grok's existing `logo-com-grok-review` connection was already signed in. `list_logos` returned four logos with nothing further to load. Grok authenticated tool use is recorded as **Pass (user-reported)**. Logo names and other returned account content are omitted. Fresh Grok OAuth, consent, and callback details were not captured, and no independent Grok tool transcript or screenshot was captured.

| Client | OAuth result | `list_logos` result |
| - | - | - |
| Cursor desktop (3.25.6 currently installed) | **Pass (user-reported):** Harrison confirmed Cloud Login and working tools on October 19. Earlier automation capture failures are historical. The desktop callback was not captured. | **Pass (user-reported):** "tools worked" in response to the `list_logos`/tool-use check. No independent tool transcript, exact output, screenshot, or count was captured. |
| Cursor web | Authenticated connection Online after Harrison's reviewer sign-in. Tokens were not inspected. | Pass: `list_logos` through `logo-com` returned zero logos on 2026-10-14. |
| Grok Bot | **Authenticated (user-reported):** existing `logo-com-grok-review` was already signed in on October 19. Fresh OAuth, consent, and callback were not captured. | **Pass (user-reported):** `list_logos` returned four logos with nothing further to load. No independent tool transcript or screenshot was captured. |

Cursor web supports personal HTTP MCP connections from its MCP menu. This lets the server authentication be tested before the marketplace package is published. [Cloud Agent capabilities](https://cursor.com/docs/cloud-agent/capabilities#MCP-tools)

## Submission checklist

Package validation rerun on 2026-10-13. Both `claude plugin validate --strict ./plugins/logo-com` and `claude plugin validate --strict .` passed. Cursor's template validator, `scripts/validate-template.mjs` from [cursor/plugin-template](https://github.com/cursor/plugin-template) at commit `46216072ac5750f782f95bb325b4d12b7c3ae9c9`, passed with the expected missing-hooks warning. The Claude plugin validator also advised adding a one-step marketplace installation line to the README; that advisory did not fail validation. [Submission checklist](https://cursor.com/docs/reference/plugins)

| Item | Status |
| - | - |
| Valid `.cursor-plugin/plugin.json` manifest | Pass |
| `name` is unique, lowercase and kebab-case | Pass. `logo-com` is the only plugin in this marketplace. Clashes with other public plugins weren't checked. |
| `description` explains the plugin's purpose | Pass |
| Every component has valid files and frontmatter | Pass. `create-logo` has `name` and `description`, and `mcp.json` parses. |
| Logo committed and referenced by relative path | Pass. `assets/logo.svg` |
| `README.md` documents usage and configuration | Pass. `plugins/logo-com/README.md` |
| Agent Plugins conform to the Agent Plugins schemas | Not applicable. This is a Cursor Plugin, not an Agent Plugin with a root `plugin.json`. |
| Every `${VAR}` in `mcp.json` is declared in the manifest | Not applicable. `mcp.json` has no variables. |
| Manifest paths are relative, with no `..` or absolute paths | Pass |
| Tested locally | Desktop Cloud Login and tool use passed per Harrison's October 19 confirmation; no independent tool transcript was captured. Cursor 3.25.6 is installed, and the local manifest, both MCP configurations, skill, and SVG matched this branch on October 19. The October 13 Customize screenshot from Cursor 3.24.12 shows the logo and `create-logo` skill, with the historical `Logo Com` heading discrepancy. The heading has not been rechecked in 3.25.6. |
| Authenticated tool use in Cursor desktop, Cursor web, and Grok Bot | Pass. Web `list_logos` was observed directly. Desktop tool use and Grok `list_logos` success were reported by Harrison on October 19. This does not establish a fresh Grok OAuth round trip or marketplace installation. |
| Repo has `.cursor-plugin/marketplace.json` at its root with unique plugin names | Pass |

No official JSON schema exists for `.cursor-plugin/plugin.json` or `marketplace.json`. The [Agent Plugins schemas](https://agent-plugins.org/schemas) cover the root `plugin.json` format, which this package doesn't use.

## Remaining before submission

- Complete the publisher application, confirming the publisher handle and contact details.
- Complete final Publisher Terms acceptance and submission. Harrison has authorized finishing submission, but the final portal action has not been taken. No Linear comments, PR creation, or merges are authorized.

Evidence limits remain recorded: fresh sign-in returning to consent was not observed at runtime, although ENG-9750's production deployment evidence passed. The desktop loopback callback has deployed allowlist coverage without an observed production round trip. Grok reused existing authentication. None of these direct tool tests verifies marketplace installation.

Nonblocking notes: the historical `Logo Com` display-name discrepancy remains recorded, and `assets/logo.svg` still needs design approval. Harrison requested continuation after the naming finding. The icon has no obvious visual defect.

## Review outcome

Not submitted. Record the outcome and the reviewer's feedback here.
