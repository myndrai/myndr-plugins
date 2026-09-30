---
name: daily-agenda
description: Summarize the user's day from Google Calendar — events in order, conflicts, gaps, and invitations still waiting for an answer. Use when asked what's on today, tomorrow, or a given day.
---

# Daily agenda

An agenda is what the day asks of the user, not a copy of the calendar. Lead
with what needs attention.

Tools from this plugin's `gcalendar` server:

- Read: `gcalendar__list_calendars`, `gcalendar__list_events`,
  `gcalendar__get_event`, `gcalendar__search_events`
- Changes calendars and notifies the organizer: `gcalendar__respond_to_event`,
  only when the user tells you how to answer a specific invitation

If the tools take a `myndr_account` argument, more than one account is
connected: include each account's day and label which is which.

## Steps

1. **Pick the day and zone.** Default to today in the primary calendar's time
   zone (`gcalendar__list_calendars`). "Tomorrow" and weekdays resolve in that
   zone.
2. **Choose calendars.** The primary calendar, plus any other calendar the user
   owns or has marked selected. Skip holiday and subscribed calendars unless
   asked.
3. **List events.** For each calendar, `gcalendar__list_events` with `calendarId`,
   `startTime` and `endTime` as the day's bounds (ISO 8601 with offset),
   `timeZone`, and `orderBy: "startTime"`.
4. **Read details only where needed.** `gcalendar__get_event` for an event whose
   location, link or attendee list matters and is missing from the list result.
5. **Find what needs attention:**
   - overlapping events
   - invitations whose response for the user is still `needsAction`
   - back-to-back runs longer than two hours with no break
   - events with no location or video link that have other attendees
   - early starts before the user's usual first event
6. **Answer invitations only on instruction.** "Accept the 3pm" →
   `gcalendar__respond_to_event` with `eventId`, `calendarId`, and
   `responseStatus` of `accepted`, `declined` or `tentative`. This emails the
   organizer; say so.

## Rules

- Read-only unless the user names an invitation and the answer.
- Private events of other people show as busy blocks; never guess their content.
- All-day events go first, separated from timed ones.

## Output

```
<Weekday, date> (<time zone>) — <n> events, first at <time>, last ends <time>

Needs attention
- <conflict / unanswered invitation / no link> — <event> <time>

Schedule
- 09:00–09:30 <title> — <where or link> — <key attendees>
- …

Free: <largest gaps>
```
