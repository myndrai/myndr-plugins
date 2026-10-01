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

In Myndr, tools the Calendar server marks read-only (listing calendars and events, suggesting times) run without
asking. Every other tool asks for approval by default, including every tool that changes your calendars:
creating, updating, deleting and answering events. Creating or updating an event that has attendees emails
them invitations from your account. You can set any tool to run without asking for a particular agent.

## Skill provenance

Both skills are written by Myndr for this plugin (MIT). They name only tools the Calendar server lists.

## Logo and trademark

`assets/icon.svg` and `assets/icon-dark.svg` are Google's official Google Calendar product icon, unmodified, from
https://fonts.gstatic.com/s/i/productlogos/calendar_2020q4/v11/192px.svg. The two files are identical: the
full-colour mark is used on light and dark backgrounds alike.

Trademark: Google Calendar and its logo are trademarks of Google LLC, used here only to identify the service this
plugin connects to. Myndr packages this plugin and wrote its skills; Google does not sponsor or endorse it. Use
follows Google's brand guidelines (https://about.google/brand-resource-center/). The logo files are not covered by
this repository's MIT license.
