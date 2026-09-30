# Google Calendar

See, plan and schedule events through Google's own Calendar MCP server
(`https://calendarmcp.googleapis.com/mcp/v1`, Google Workspace Developer Preview).

## What it does

- **Apps:** one remote MCP server, `gcalendar`. Myndr signs in with your Google account through its Google
  Calendar connection. The package declares no credentials; Myndr matches the server's address to the
  connection. You can connect more than one account.
- **Skills:**
  - `google-calendar:schedule-meeting` — find a time that works for everyone, then create the event.
  - `google-calendar:daily-agenda` — summarize a day, with conflicts, gaps and unanswered invitations.

Tools that change your calendars (creating, updating, deleting or answering events) ask for approval by
default in Myndr.

## Skill provenance

Both skills are written by Myndr for this plugin (MIT). They name only tools the Calendar server lists.

## Logo and trademark

`assets/icon.svg` and `assets/icon-dark.svg` are Google's official Google Calendar product icon, unmodified, from
https://fonts.gstatic.com/s/i/productlogos/calendar_2020q4/v11/192px.svg. The two files are identical: the
full-colour mark is used on light and dark backgrounds alike.

Trademark: Google Calendar and the Google Calendar logo are trademarks of Google LLC. They are used here only to
identify the service this plugin connects to; this plugin is not made, sponsored or endorsed by Google. Use
follows Google's brand guidelines (https://about.google/brand-resource-center/). The logo files are not covered
by this repository's MIT license.
