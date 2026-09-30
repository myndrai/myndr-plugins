---
name: summarize-doc
description: Read a Google Doc and summarize it — purpose, conclusions, key points by section, and open comments. Use when asked to summarize, review, or pull the decisions out of a Google Doc.
---

# Summarize a doc

Tools from this plugin's `gdocs` server, read-only here: `gdocs__read_doc`.
This skill changes nothing.

If the tools take a `myndr_account` argument, use the account that can open the doc.

## Steps

1. **Get the id.** From the link: the part after `/document/d/`. This server
   cannot search. Without a link, ask for one; the Google Drive plugin can find
   docs by name if it is installed.
2. **Read.** `gdocs__read_doc` with `documentId`. Add `commentsIncluded: true`
   when the user asks about feedback, open questions or review status.
3. **Walk the structure.** The result is the Docs API JSON:
   - text is in `body.content[].paragraph.elements[].textRun.content`
   - headings are paragraphs whose `paragraphStyle.namedStyleType` is
     `TITLE` or `HEADING_1`…`HEADING_6`
   - tables are `table.tableRows[].tableCells[].content[]`

   A document with tabs keeps each tab's content under `tabs[].documentTab.body`;
   read every tab and name them.
4. **Summarize** in the document's own section order. Keep decisions, owners,
   dates and numbers verbatim.
5. **Comments.** Group unresolved comments by section with who asked what.

## Rules

- Say what the document says, not what it should say. Opinions go in a separate
  "My notes" line, and only if asked.
- Quote short phrases for decisions and commitments; paraphrase the rest.
- Very long docs: the summary scales down (one line per section), never gets cut
  off mid-way.

## Output

```
<Title> — <link>

Purpose: <one sentence>
Bottom line: <decision or conclusion, or "none stated">

By section
- <Heading>: <gist>

Decisions / owners / dates
- …

Open comments (<n>)
- <section> — <person>: <question>
```
