# Google Docs

Read, summarize and write into documents through Google's own Docs MCP server
(`https://docsmcp.googleapis.com/mcp/v1`, Google Workspace Developer Preview).

## What it does

- **Apps:** one remote MCP server, `gdocs`. Myndr signs in with your Google account through its Google
  Docs connection. The package declares no credentials; Myndr matches the server's address to the
  connection. You can connect more than one account.
- **Skills:**
  - `google-docs:summarize-doc` — read a doc and summarize it, with open comments.
  - `google-docs:draft-doc` — write a draft into a doc you choose, directly or as suggestions.

The Docs server reads and edits existing documents; it cannot create one.

In Myndr, tools the Docs server marks read-only (`read_doc`) run without asking. Every other tool asks for
approval by default, including the one that changes a document (`update_doc`). You can set any tool to run
without asking for a particular agent.

## Skill provenance

Both skills are written by Myndr for this plugin (MIT). They name only tools the Docs server lists.

## Logo and trademark

`assets/icon.svg` and `assets/icon-dark.svg` are Google's official Google Docs product icon, unmodified, from
https://fonts.gstatic.com/s/i/productlogos/docs_2020q4/v6/192px.svg. The two files are identical: the
full-colour mark is used on light and dark backgrounds alike.

Trademark: Google Docs and its logo are trademarks of Google LLC, used here only to identify the service this
plugin connects to. Myndr packages this plugin and wrote its skills; Google does not sponsor or endorse it. Use
follows Google's brand guidelines (https://about.google/brand-resource-center/). The logo files are not covered by
this repository's MIT license.
