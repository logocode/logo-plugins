# LOGO.com Cursor Marketplace materials

Status: **NOT SUBMITTED.** Draft materials prepared on 2026-10-09. Submitting is gated on ENG-9753 and Harrison's OK. Don't submit at cursor.com/marketplace/publish before both.

Cursor lists plugins from public Git repositories, and its team reviews each plugin and each update by hand. You submit the repository link at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish). [Plugins](https://cursor.com/docs/plugins), [Reference](https://cursor.com/docs/reference/plugins)

Grok Bot installs plugins from the Cursor Marketplace and follows the team's Cursor connector policy, so this one listing covers Grok Bot too. Grok Bot has no documented way for a user to add a server by URL. [Grok Bot for teams](https://cursor.com/docs/grok-bot/teams), [Connect plugins](https://cursor.com/help/grok-bot/connect-plugins)

## Submission

| Field | Value |
| - | - |
| Submit at | https://cursor.com/marketplace/publish |
| Repository | https://github.com/logocode/logo-plugins (public) |
| Marketplace manifest | `.cursor-plugin/marketplace.json`, marketplace `logo-plugins` |
| Plugin folder | `plugins/logo-com` |
| Plugin name | `logo-com`, permanent once published |
| Branch | `main` |

The docs don't describe the publish form's fields. Record them here when you submit.

## Listing

Cursor merges `plugins/logo-com/.cursor-plugin/plugin.json` over the marketplace entry, with the manifest's values taking precedence. The checklist also asks for a README that documents usage, which is `plugins/logo-com/README.md`.

| Field | Value |
| - | - |
| `name` | `logo-com` |
| `displayName` | LOGO.com. The reference doesn't list this field, but Cursor's plugin template sets it and Cursor 3.24.9 reads it. |
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

### Dependencies

- ENG-9556: the redirect allowlist for dynamic client registration must accept Cursor's redirect URIs. If it rejects any URI in the request, the whole registration fails.
- ENG-9750: Google and email-code sign-in must return to the OAuth consent screen instead of the dashboard.
- ENG-9751: sign in and run `list_logos` from Cursor desktop, Cursor web and Grok Bot, and record the results here. It creates a production client, so it needs Harrison's OK.

## Submission checklist

Checked on 2026-10-09 against the [submission checklist](https://cursor.com/docs/reference/plugins) and with Cursor's template validator, `scripts/validate-template.mjs` from [cursor/plugin-template](https://github.com/cursor/plugin-template) at commit `46216072`.

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
| Tested locally | Partly. Cursor 3.24.9 loaded the plugin from `~/.cursor/plugins/local/logo-com`. It found the `logo-com` server and showed it as needing authentication. That test pointed `mcp.json` at a local stand-in, because loading the real URL registers a production client (ENG-9751). The skill and logo in **Customize** weren't checked by eye, because screenshots were blocked. |
| Repo has `.cursor-plugin/marketplace.json` at its root with unique plugin names | Pass |

No official JSON schema exists for `.cursor-plugin/plugin.json` or `marketplace.json`. The [Agent Plugins schemas](https://agent-plugins.org/schemas) cover the root `plugin.json` format, which this package doesn't use.

## Remaining before submission

- Land ENG-9556 and ENG-9750.
- With Harrison's OK, run ENG-9751. Sign in from Cursor desktop, Cursor web and Grok Bot with a premium account, run `list_logos`, and record the results here.
- Check in Cursor's **Customize** that the plugin shows as LOGO.com with its logo and the `create-logo` skill.
- Get design to approve `assets/logo.svg`, or to export the circular icon as an official SVG to replace it.
- Get Harrison's OK, then submit under ENG-9753.

## Review outcome

Not submitted. Record the outcome and the reviewer's feedback here.
