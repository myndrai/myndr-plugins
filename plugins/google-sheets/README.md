# Google Sheets

Read and update spreadsheets through Google's own Sheets MCP server
(`https://sheetsmcp.googleapis.com/mcp/v1`, Google Workspace Developer Preview).

## What it does

- **Apps:** one remote MCP server, `gsheets`. Myndr signs in with your Google account through its Google
  Sheets connection. The package declares no credentials; Myndr matches the server's address to the
  connection. You can connect more than one account.
- **Skills:**
  - `google-sheets:analyze-sheet` — learn a spreadsheet's shape and answer questions with exact figures.
  - `google-sheets:update-sheet` — make range-precise edits, showing before and after.

Tools that change a spreadsheet (`update_values`, `update_formulas`, `insert_dimension` and
`update_spreadsheet`) ask for approval by default in Myndr.

## Skill provenance

Both skills are written by Myndr for this plugin (MIT). They name only tools the Sheets server lists.

## Logo and trademark

`assets/icon.svg` and `assets/icon-dark.svg` are Google's official Google Sheets product icon, unmodified, from
https://fonts.gstatic.com/s/i/productlogos/sheets_2020q4/v6/192px.svg. The two files are identical: the
full-colour mark is used on light and dark backgrounds alike.

Trademark: Google Sheets and the Google Sheets logo are trademarks of Google LLC. They are used here only to
identify the service this plugin connects to; this plugin is not made, sponsored or endorsed by Google. Use
follows Google's brand guidelines (https://about.google/brand-resource-center/). The logo files are not covered
by this repository's MIT license.
