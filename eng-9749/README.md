# ENG-9749 evidence

Evidence for the Cursor plugin manifest and marketplace in logocode/logo-plugins, branch `harrison/eng-9749-cursor-plugin`. Collected on 2026-10-09 with Cursor 3.24.9 on macOS.

## No Cursor screenshots

`screencapture` failed with "could not create image from display", both inside and outside the command sandbox. `osascript` also has no accessibility access. So there are no screenshots of Cursor (`cursor-01`, `cursor-02` and `cursor-03` are missing). The Cursor evidence below comes from Cursor's own logs and state files and from a local stand-in server.

## Files

| File | What it shows |
| - | - |
| `logo-svg-vs-png.png` | `assets/logo.png` beside `assets/logo.svg` rendered at 180 px with `rsvg-convert`, plus their difference at 4x brightness |
| `validate-logo-svg-vs-png.txt` | The pixel comparison: a mean difference of 0.44/255 per channel, and 0.21% of pixels off by more than 64, all on anti-aliased edges |
| `validate-json-parse.txt` | Every manifest, marketplace and MCP file parses as JSON |
| `validate-cursor-template.txt` | Cursor's template validator, `scripts/validate-template.mjs` from cursor/plugin-template at `46216072`, passes. Its one warning is about `hooks/hooks.json`, which we don't use. |
| `validate-cursor-reference-rules.txt` | 44 passes and 0 failures from a throwaway checker (not committed) for reference rules the template validator skips: documented fields, `mcp.json` contents, SVG safety, skill frontmatter, README length and secrets |
| `validate-claude.txt` | Claude's strict validate passes for `./plugins/logo-com` and for the repo marketplace. The README advice in it also appears on `main`. |
| `cursor-install-commands.txt` | The shell trace of the local install, and of the cleanup afterwards |
| `cursor-logs-local-test.txt` | Excerpts from Cursor's logs. The plugin loads, the `logo-com` server is created from `mcp.json` over streamable HTTP, and it ends in `needsAuth` |
| `mock-mcp-requests.jsonl` | Every request Cursor sent to the local stand-in server, including the client registration it made on load |
| `cursor-oauth-attempt-mock.json` | The OAuth attempt Cursor stored for the stand-in, with secrets redacted |
| `cursor-agent-mcp-list.txt` | `cursor-agent mcp list` on build 2025.09.17. This build reads only `mcp.json` files, not plugins, so `logo-com` isn't listed. |

## Local Cursor test

The docs say to put the plugin in `~/.cursor/plugins/local/<name>`, and that Cursor skips a symlink pointing outside that folder. So the test used a copy, not a symlink.

Cursor 3.24.9 reads `.mcp.json` as well as `mcp.json`, and registers an OAuth client as soon as a remote server answers `401`, without any click. Loading the real `https://mcp.logo.com/mcp` would have created a production client, which ENG-9751 gates. So the test copy left out `.mcp.json`, and its `mcp.json` pointed at a stand-in on `127.0.0.1:47821`. The stand-in answers as the real server does before sign-in (`401`, protected resource metadata, and authorization server metadata with a registration endpoint). Nothing reached LOGO.com.

Steps (exact trace in `cursor-install-commands.txt`):

1. Start the stand-in on `127.0.0.1:47821`.
2. Create `~/.cursor/plugins/local/logo-com` and copy `plugins/logo-com` into it with `rsync` in archive mode, excluding `.mcp.json`.
3. Write the test `mcp.json` with the url `http://127.0.0.1:47821/mcp`.
4. Cursor loaded the plugin in both open windows within a second, without a reload. It created the `logo-com` server, called `initialize`, fetched both metadata documents, registered a client and stopped at `needsAuth`. Its state file `STATUS.md` read "The MCP server needs authentication."
5. Delete the test `mcp.json`. Cursor reloaded the plugin and removed the server.
6. Stop the stand-in.

Not observed: the plugin, logo and `create-logo` skill as shown in **Customize**. That needs the screen. The plugin loaded with 0 failures, and the skill passes the template validator's frontmatter check.

The copy left in place at `~/.cursor/plugins/local/logo-com` has no `.mcp.json` or `mcp.json`, so it can't contact LOGO.com. Remove it with:

```sh
rm -rf ~/.cursor/plugins/local/logo-com
```

Cursor also kept an OAuth attempt record for the stand-in at `~/Library/Application Support/Cursor/User/globalStorage/mcp-oauth-attempts/b5669554-c6ea-4ea5-b815-1b6e42ad9cbd.json`. It's harmless, and you can delete it.
