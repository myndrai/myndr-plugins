---
name: inbox-triage
description: Sort new Gmail threads by what they need, apply labels, and queue draft replies for the user to review. Use when asked to triage, clean up, or catch up on an inbox.
---

# Inbox triage

Triage is one decision per thread: reply, act, read later, or ignore. The user
sends; you never do. This server has no send tool, so a reply waits as a draft.

Tools from this plugin's `gmail` server:

- Read: `gmail__search_threads`, `gmail__get_thread`, `gmail__list_labels`,
  `gmail__list_drafts`
- Changes the mailbox: `gmail__label_thread`, `gmail__create_label`,
  `gmail__create_draft`
- Not used by this skill unless the user names the thread and the action:
  `gmail__unlabel_thread` (removing `INBOX` archives), `gmail__trash_thread`,
  `gmail__mark_thread_spam`, `gmail__apply_sensitive_thread_label`

If the tools take a `myndr_account` argument, more than one Gmail account is
connected and this agent has none pinned. Use the one the user named; ask when they did not.

## Steps

1. **Scope.** Default to `in:inbox is:unread newer_than:3d`. Use the user's
   words as Gmail search operators (`from:`, `label:`, `older_than:`,
   `-category:promotions`) rather than filtering results yourself.
2. **List.** `gmail__search_threads` with that `query` and `pageSize` 25. Follow
   `pageToken` up to 100 threads; if there are more, say so and triage the
   newest 100. `{}` means no threads, not an error.
3. **Read what the snippet cannot decide.** `gmail__get_thread` with
   `messageFormat: "PLAIN_TEXT"`. Newsletters and notifications rarely need it;
   a thread where the user was asked something always does.
4. **Classify each thread** into exactly one bucket:
   - **Reply needed** — someone is waiting on the user.
   - **Action** — the user must do something outside email (pay, sign, book).
   - **Read later** — worth reading, nothing owed.
   - **Waiting** — the user asked and is waiting on someone else.
   - **Noise** — promotions, automated notices with nothing to do.
5. **Propose before touching anything.** Show the plan (Output below) and wait
   for the user's yes, or their edits.
6. **Label on approval.** `gmail__list_labels` for label ids: system labels use
   their names (`STARRED`, `IMPORTANT`); user labels use `Label_…` ids. A
   missing label is proposed first, then created with `gmail__create_label`
   (`displayName`) only after a yes. Apply with `gmail__label_thread`
   (`threadId`, `labelIds`).
7. **Queue drafts on approval** for the "Reply needed" threads the user picked,
   following the `gmail:draft-replies` skill: one `gmail__create_draft` per
   thread with `replyToMessageId` set to the last message's id. Report each
   draft's `viewUrl`.

## Rules

- Nothing changes before the user approves. Labels and drafts both change the
  mailbox.
- Never archive, trash or mark spam in bulk. These actions are one thread at a
  time, and only when the user names the thread.
- A suspected phishing or security message is flagged, never answered. Do not
  open its links and do not draft a reply.
- Never copy one-time codes, passwords or account numbers into the summary.
- Priority follows who is waiting and on what, not arrival order.

## Output

```
Inbox: <query> — <N> threads

Reply needed (<n>)
- <sender> — <subject> — <what they need> — draft? yes/no

Action (<n>)
- <sender> — <subject> — <the action, with any deadline>

Waiting (<n>) / Read later (<n>) / Noise (<n>)
- <sender> — <subject>

Proposed: label <n> threads (<label names>), draft <m> replies. Go ahead?
```
