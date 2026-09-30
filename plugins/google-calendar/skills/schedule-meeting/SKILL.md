---
name: schedule-meeting
description: Find a time that works for everyone with Google Calendar and create the event once the user picks a slot. Use when asked to schedule, book, or set up a meeting.
---

# Schedule a meeting

Finding a time is read-only. Creating the event is not: it lands on every
attendee's calendar and emails them an invitation. Nothing is created until the
user picks a slot and confirms the details.

Tools from this plugin's `gcalendar` server:

- Read: `gcalendar__list_calendars`, `gcalendar__suggest_time`,
  `gcalendar__list_events`
- Changes calendars and notifies attendees: `gcalendar__create_event`

If the tools take a `myndr_account` argument, use the account the user named. The
organizer's account decides whose calendar holds the event.

## Steps

1. **Collect the brief.** Attendee emails, duration, the window (for example
   "next week"), the user's time zone, and whether it needs a video link. Ask
   only for what is missing. Names without emails are missing.
2. **Know the user's calendar.** `gcalendar__list_calendars`; the primary
   calendar's id is the user's own email and its time zone is the default.
3. **Ask for slots.** `gcalendar__suggest_time` with:
   - `attendeeEmails`: the attendees plus the user
   - `startTime` / `endTime`: the window as ISO 8601 with offset
   - `durationMinutes`
   - `timeZone`: IANA id
   - `preferences`: `{startHour: "09:00", endHour: "17:00", excludeWeekends: true, pageSize: 5}`, unless the
     user said otherwise
4. **Offer two or three slots** in the user's time zone, plus the attendee's
   when it differs. Say that free/busy only covers calendars the user can see:
   an external attendee may show as free when they are not.
5. **Confirm the event.** Title, start and end, attendees, description, video
   link yes/no, and who gets notified. Wait for yes.
6. **Create.** `gcalendar__create_event` with:
   - `summary`, `startTime`, `endTime` (ISO 8601 with offset), `timeZone`
   - `attendees`: `[{email}]`
   - `description`
   - `addGoogleMeetUrl` when asked
   - `notificationLevel`: `ALL` by default; `NONE` only when the user asks for no invitation emails
7. **Report.** The event time, attendees, and Meet link if one was added.

## Rules

- Never create, move or delete an event the user has not confirmed in this
  conversation. `gcalendar__update_event` and `gcalendar__delete_event` are
  outside this skill.
- Never book over an existing event of the user's without saying so.
- Recurring meetings: confirm the recurrence in words ("every Tuesday until
  June") before creating.
- Times are always shown with their time zone.

## Output

```
Options for <title> (<duration>, <attendees>):
1. <Tue 14 Oct, 10:00–10:30 Europe/Berlin> (<attendee tz time>)
2. …
Which one? I'll send invitations to <n> people when you confirm.
```
