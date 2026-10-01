# Google Drive

Find, read and summarize files through Google's own Drive MCP server
(`https://drivemcp.googleapis.com/mcp/v1`, Google Workspace Developer Preview).

## What it does

- **Apps:** one remote MCP server, `gdrive`. Myndr signs in with your Google account through its Google
  Drive connection. The package declares no credentials; Myndr matches the server's address to the
  connection. You can connect more than one account.
- **Skill:** `google-drive:find-and-summarize` — find a file by what you remember about it, read it, and
  summarize it with a link.

In Myndr, tools the Drive server marks read-only (searching and reading files) run without asking. Every other
tool asks for approval by default, including the ones that write to your Drive (`create_file` and
`copy_file`). You can set any tool to run without asking for a particular agent.

## Skill provenance

The skill is written by Myndr for this plugin (MIT). It names only tools the Drive server lists.

## Logo and trademark

`assets/icon.svg` and `assets/icon-dark.svg` are Google's official Google Drive product icon, unmodified, from
https://fonts.gstatic.com/s/i/productlogos/drive_2020q4/v10/192px.svg. The two files are identical: the
full-colour mark is used on light and dark backgrounds alike.

Trademark: Google Drive and its logo are trademarks of Google LLC, used here only to identify the service this
plugin connects to. Myndr packages this plugin and wrote its skills; Google does not sponsor or endorse it. Use
follows Google's brand guidelines (https://about.google/brand-resource-center/). The logo files are not covered by
this repository's MIT license.
