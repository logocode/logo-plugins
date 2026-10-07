# LOGO.com Claude directory materials

Status: draft materials prepared on 2026-10-07. Not submitted or ready for review yet.

Claude's directory takes two submissions from the developer portal at [claude.ai/directory/manage](https://claude.ai/directory/manage). Submit the MCP connector for `https://mcp.logo.com/mcp` first, then the plugin bundle from this repo. The bundle's `.mcp.json` already points at the same URL, so people who install both see one set of tools. [Process](https://claude.com/docs/directory/publish)

The submitter needs a Pro, Max, Team or Enterprise plan. On Team and Enterprise, an Owner submits. On Enterprise, an Owner can also give other members the Directory permission through a custom role.

## 1. MCP connector

[Form reference](https://claude.com/docs/connectors/building/submission)

### Listing

| Field | Limit | Value |
| --- | --- | --- |
| Server name | 100 characters | LOGO.com |
| One-liner | 200 characters | Browse your LOGO.com logos and create new logo designs from a chat. |
| Categories | 1 to 5 | Design. The allowed list isn't published, so pick the closest match in the portal. |
| Documentation URL | | https://github.com/logocode/logo-plugins/tree/main/plugins/logo-com |
| Privacy policy URL | | https://logo.com/privacy-policy |
| Support contact | | https://help.logo.com |
| Icon | | `plugins/logo-com/assets/logo.png`, 180 x 180 PNG |
| URL slug | Permanent once published | `logo-com`, to match the plugin name |

Description (up to 2,000 characters):

> Connect your LOGO.com account to look through your logos and create new logo designs without leaving the chat.
>
> Ask for your most recent logos to see previews and links to open each one in the LOGO.com editor. Open a logo to see its colors, fonts and download files. Describe your business, with an optional slogan, industry, style, colors and icon idea, and get up to four new logo designs.
>
> New designs are drafts until you open one in the LOGO.com editor and save it. Creating designs uses your account's AI credits.
>
> Requires a LOGO.com premium plan.

### Use cases

- **Primary use cases:** reviewing your existing logos and their brand details (colors, fonts, downloads), and creating new logo designs from a short brief.
- **Before connecting:** a LOGO.com account on a premium plan. The tools refuse requests from free accounts.
- **Reads or writes:** both.

| Tool | Reads or writes | Expected annotations |
| --- | --- | --- |
| `list_logos` | Reads the user's logos | `title`, `readOnlyHint: true` |
| `get_logo` | Reads one logo's details and download links | `title`, `readOnlyHint: true` |
| `get_logo_generation` | Reads the status of a design job | `title`, `readOnlyHint: true` |
| `generate_logos` | Creates draft designs and uses AI credits. Deletes and overwrites nothing. | `title`, `readOnlyHint: false`, `destructiveHint: false` |

The portal syncs tools from the server and flags any without a `title` or the applicable hint. Fix flags in the server, not here. [Review criteria](https://claude.com/docs/connectors/building/review-criteria)

### Example prompts

The directory policy asks for at least three working examples. [Policy 3E](https://support.claude.com/en/articles/13145358-anthropic-software-directory-policy)

1. Show me my most recent logos.
2. What colors and fonts does my latest logo use?
3. Design a logo for my coffee shop, Bean There.

Each prompt must work on the reviewer's account, which is a fresh premium account with sample logos.

### Company and authentication

- **Company:** LOGO.com, https://logo.com. Enter the primary contact for review updates only in the portal.
- **Authentication:** OAuth with dynamic client registration. The server answers unauthenticated requests with `401` and a `WWW-Authenticate` header naming its resource metadata and the `logos` scope. The authorization server at `https://logo.com` supports S256 PKCE.
- **Redirect URIs the authorization server must accept** ([authentication](https://claude.com/docs/connectors/building/authentication)):
  - `https://claude.ai/api/mcp/auth_callback` for claude.ai, the desktop and mobile apps, and Cowork.
  - `http://localhost/callback` and `http://127.0.0.1/callback` on any port, for Claude Code.
- Claude gives the discovery, registration and token endpoints 10 seconds to respond.

### Data handling

- **Underlying API:** LOGO.com's own.
- **Personal health data:** no.
- **Sponsored content:** no.

### Test account

Enter all of this only in the portal, never in this repo:

- credentials for a premium account populated with sample logos, with known colors, fonts and working downloads
- enough AI credits for the reviewer to run `generate_logos` several times
- every step needed to connect

You must also confirm in the form that you ran every tool. [Review criteria](https://claude.com/docs/connectors/building/review-criteria)

### Compliance acknowledgements

All seven are required. Notes on the ones that need thought:

- **AI media generation.** The policy permits "Design-focused software that uses AI models to create visual aids (such as slides, diagrams, charts, UI mockups, logos, or other design assets)", "provided the developer does not offer standalone image generation as a primary service." LOGO.com is a logo and brand design product, not a general image generator, and the tools only make logos. The review criteria page lists only "diagrams, charts, or UI mockups" as allowed design output, so be ready to point the reviewer at the policy wording. [Policy 4B](https://support.claude.com/en/articles/13145358-anthropic-software-directory-policy)
- **Financial transactions.** The tools don't move money or make purchases. `generate_logos` uses AI credits the account already has.
- **Prompt injection.** Tool descriptions come from the server. Check that none tell Claude to call tools the user didn't ask for, or promote LOGO.com products.
- **Conversation data.** The server must not log or keep conversation content beyond what the tools need. [Policy 1D](https://support.claude.com/en/articles/13145358-anthropic-software-directory-policy)
- **Public documentation.** This must be live by the publish date. The plugin README covers the tools, requirements, troubleshooting and support. [Policy 3C](https://support.claude.com/en/articles/13145358-anthropic-software-directory-policy)

## 2. Plugin bundle

Submit after the connector. [Form reference](https://claude.com/docs/plugins/submit)

| Field | Value |
| --- | --- |
| Repository | `logocode/logo-plugins` |
| Plugin path | `plugins/logo-com` |
| Branch or tag | `main`, or a release tag made with `claude plugin tag plugins/logo-com` |
| Update checks | GitHub push webhook |

The form reads the listing from `plugins/logo-com/.claude-plugin/plugin.json` and `plugins/logo-com/README.md`. To change the listing, edit those files and validate again.

Data handling answers:

- **Reads or stores personal data:** reads the user's own LOGO.com logos through the connector. The plugin itself stores nothing.
- **Sends data anywhere besides its declared connector:** no.
- **How long it keeps data:** the plugin keeps nothing. LOGO.com's retention is in its privacy policy.
- **Intended for people under 18:** no.

The form asks for four acknowledgements and a contact email. Confirm both in the portal.

### Pre-submission checklist

Checked on 2026-10-07 against the [pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist) and with `claude plugin validate --strict plugins/logo-com`:

| Item | Status |
| --- | --- |
| Name `logo-com`: lowercase, digits and hyphens, at most 64 characters, not reserved | Pass |
| README in the plugin folder with at least 40 words outside code blocks | Pass |
| `license` set in `plugin.json` (MIT) | Pass |
| `description`, `author` and `version` set | Pass |
| Remote server has `type: http` and an absolute `https://` URL | Pass |
| Only regular files: no symlinks, submodules or LFS pointers in the plugin folder | Pass |
| File types: JSON, Markdown and one PNG. No `.ico` | Pass |
| No secrets or credentials | Pass |

A plugin with only a connector is complete, but the docs say most product plugins also ship skills. [What to build](https://claude.com/docs/connectors/building/what-to-build) Decide on PR #3, which adds a `create-logo` skill, before submitting.

## Remaining before submission

- Confirm every tool's `title` and hints in the server. This repo can't see the tool definitions without signing in.
- Confirm the authorization server accepts the claude.ai and loopback redirect URIs above. Signing in once from claude.ai and once from Claude Code proves both.
- Run the three example prompts and every tool on the reviewer account.
- Prepare the reviewer account and enter its details only in the portal.
- Settle PR #3 so the README and `plugin.json` the form reads are final. Then copy the final listing copy into this file.
- Confirm the privacy policy covers this integration's data practices before making the acknowledgements.
- Choose the slug. It can't change after publishing.

## Review outcome

Not submitted yet. Record the outcome and the reviewer's feedback here.
