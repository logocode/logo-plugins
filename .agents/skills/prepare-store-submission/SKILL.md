---
name: prepare-store-submission
description: >-
  Prepare or update LOGO.com's listing in Claude's directory (connector and
  plugin) or OpenAI's plugin directory (ChatGPT and Codex). Gathers the listing
  copy, example prompts, test cases and URLs, checks the package against each
  store's rules, and hands off to the person who submits. Use when a listing is
  being created or changed.
---

# Prepare a store submission

Agents prepare and a person submits. Both portals need an organization owner's account, and only that person enters reviewer credentials.

## Where things go

- **In this repo:** listing copy, example prompts and test cases, in `submissions/claude-directory.md` and `submissions/openai.md`. They become public in the listing anyway.
- **Only in the portal:** reviewer credentials, reviewer contact details, and anything else from the portals' private fields.

## Claude

There are two submissions at [claude.ai/directory/manage](https://claude.ai/directory/manage). First submit an **MCP connector** for the server, then a **Plugin bundle** from this repo, which must be public to publish. [Process](https://claude.com/docs/directory/publish)

Prepare:
- a name (up to 100 characters), a one-liner (up to 200), a description (up to 2,000), 1 to 5 categories, and a URL slug, which is permanent
- documentation, privacy policy and support URLs
- at least three example prompts that work on a fresh premium account
- what users need before connecting: a LOGO.com premium plan
- which tools read and which write

Check before handing off:
- Every tool has a `title` and the right `readOnlyHint` or `destructiveHint`. [Review criteria](https://claude.com/docs/connectors/building/review-criteria)
- The plugin passes the [pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist): a README of at least 40 words, a license, an https MCP URL, and no secrets.
- The listing fits the AI media policy. Anthropic permits design tools that make logos, provided standalone image generation isn't the developer's primary service. [Policy](https://support.claude.com/en/articles/13145358-anthropic-software-directory-policy)

## OpenAI

One submission at [platform.openai.com/plugins](https://platform.openai.com/plugins) covers ChatGPT and Codex. Upload a ZIP of `plugins/logo-com`, connect the server, verify the domain, submit for review, then publish. [Process](https://developers.openai.com/plugins/deploy/submission)

Prepare:
- the manifest's `interface` fields (see `build-plugin`)
- exactly 5 positive and 3 negative test cases, each with a prompt, the tool you expect, and the expected outcome
- a video walkthrough URL and release notes
- website, support, privacy policy and terms of service URLs, all https

Check before handing off:
- Every tool sets `readOnlyHint`, `destructiveHint` and `openWorldHint` explicitly. [Guidelines](https://developers.openai.com/plugins/plugin-guidelines)
- No tool result shows plans, starts a subscription or links to checkout. A link to an informational plans page is allowed.
- The reviewer account signs in without email or SMS codes, magic links or MFA.
- The submitter knows about domain verification. The portal gives a token to serve at `https://mcp.logo.com/.well-known/openai-apps-challenge`, which is a server change in LOGO.com's application repo.

To draft test cases and annotation answers, OpenAI's `chatgpt-app-submission` skill reads the server code (`codex plugin add openai-developers@openai-curated`). Run it from the server's repo, then copy what's useful into `submissions/openai.md`. It targets an older form, so map its fields onto the current ones.

## Hand off

Give the submitter the `submissions/` file for the store, the commit to package, and a list of anything you couldn't confirm. After review, record the outcome and the reviewer's feedback in the same file.
