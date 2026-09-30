# Gmail

Search, read, label and draft email through Google's own Gmail MCP server
(`https://gmailmcp.googleapis.com/mcp/v1`, Google Workspace Developer Preview).

## What it does

- **Apps:** one remote MCP server, `gmail`. Myndr signs in with your Google account through its Gmail
  connection. The package declares no credentials; Myndr matches the server's address to the connection.
  You can connect more than one account.
- **Skills:**
  - `gmail:inbox-triage` — sort new threads, apply labels, queue drafts.
  - `gmail:draft-replies` — draft replies in your voice.

  Both leave sending to you: the server has no send tool.

Tools that change your mailbox (labels, drafts, trash, spam) ask for approval by default in Myndr.

## Skill provenance

Both skills are written by Myndr for this plugin (MIT). They name only tools the Gmail server lists.

## Logo and trademark

`assets/icon.svg` and `assets/icon-dark.svg` are Google's official Gmail product icon, unmodified, from
https://fonts.gstatic.com/s/i/productlogos/gmail_2020q4/v6/192px.svg. The two files are identical: the
full-colour mark is used on light and dark backgrounds alike.

Trademark: Gmail and the Gmail logo are trademarks of Google LLC. They are used here only to identify the
service this plugin connects to; this plugin is not made, sponsored or endorsed by Google. Use follows Google's
brand guidelines (https://about.google/brand-resource-center/). The logo files are not covered by this
repository's MIT license.
