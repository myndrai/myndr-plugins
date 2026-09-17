---
name: triage-issues
description: Group a repository's open issues and pull requests by what they need and propose the next action for each. Use when asked to triage, clean up, or summarize a repo's backlog.
---

# Triage issues

Triage is a decision per item, not a summary of the list. Every item leaves
with one next action and an owner-shaped hint.

Needs the GitHub tool connected (`gh_get_repo`, `gh_list_issues`,
`gh_list_prs`, `gh_search_code`). If it is not granted, say so and stop —
do not guess the backlog from the repository name.

## Steps

1. **Confirm the repository.** `gh_get_repo` on the `owner/repo` the user named.
   Never infer it from the working directory.
2. **Pull the backlog.** `gh_list_issues` and `gh_list_prs`, open only, unless
   the user asks for closed ones.
3. **Read before classifying.** Title plus body plus the last comment. A title
   describes the reporter's theory; the last comment often says it was already
   fixed.
4. **Classify each item** into exactly one bucket:
   - **Broken** — a user-visible defect with a reproduction. Highest priority.
   - **Broken, unclear** — a defect claim with no reproduction. Needs one
     question, not a fix.
   - **Wanted** — a feature or change request. Needs a decision, not work.
   - **Stale** — no activity and no longer matches the code. Candidate to close.
   - **Already done** — fixed or superseded. Close with the reference.
   - **Blocked** — waiting on a dependency, a decision, or another item. Name
     what it waits on.
5. **Check "already done" against the code**, with `gh_search_code` on the
   symbol or message in the report. An item nobody closed is not evidence it
   is still open.
6. **Propose the next action** per item, one line: fix, ask, decide, close,
   or unblock — and what specifically.

## Rules

- Do not write to the repository. Comments, labels, and closures are
  proposals the user approves; `gh_comment` and `gh_create_issue` only on an
  explicit instruction for that item.
- Priority follows user impact, not age. An old paper cut stays below a new
  crash.
- Two issues describing one defect: say which is the primary and that the
  other is a duplicate of it.
- Security reports never go in a public comment. Flag them to the user
  privately and stop.

## Output

```
Repo: <owner/repo> — <N> open issues, <M> open PRs

Broken (<n>)
- #123 <title> — <next action>

Broken, unclear (<n>)
- #124 <title> — ask: <the one question>

Wanted (<n>)
- #125 <title> — decide: <the decision>

Stale / already done (<n>)
- #126 <title> — close: <reason or reference>

Blocked (<n>)
- #127 <title> — waiting on <what>

Do first: <the two or three items that matter and why>
```
