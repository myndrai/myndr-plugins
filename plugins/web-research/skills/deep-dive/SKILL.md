---
name: deep-dive
description: Research a question on the live web and report findings with sources. Use when the answer depends on current facts, prices, releases, or anything that changes.
---

# Deep dive

Answer from sources you actually read, not from memory. Memory is a starting
point for queries, never the answer.

## Loop

1. **Write the question down** in one sentence, plus what a good answer must
   contain (a number, a date, a name, a comparison). If the request is vague,
   pick the narrowest reading that is still useful and say which reading you
   took.
2. **Search.** Call `search_web` with 2-4 differently worded queries. Reuse the
   vocabulary the source would use, not the user's paraphrase: a vendor's own
   release notes say "deprecated", a forum says "broken".
3. **Read the primary source.** Search snippets are a pointer, never the
   evidence. For the 2-5 results that matter, read the page: `crawl_url` for
   static pages, `browser_navigate` + `browser_observe` when the content only
   appears after the page runs. Quote the sentence that carries the fact.
4. **Stop when the question is answered**, not when the budget is gone. Three
   sources that agree on a specific number beat twelve that restate each other.
5. **Report.** Lead with the answer. Then the evidence, one line per source:
   claim, URL, publication date. Then what you could not confirm.

## Rules

- A date on every time-sensitive claim. "Current" without a date is a guess.
- Never present a search snippet as a read source.
- Sources disagreeing is a finding. Report both and say which is more
  authoritative and why (vendor docs over blog, primary over aggregator, dated
  over undated).
- No source found is a valid answer. Say what you searched and what came back
  empty instead of filling the gap.
- Vendor claims about their own product are evidence of the claim, not of the
  behavior. Say "the vendor states X" when nobody independent confirms it.

## Output

```
Answer: <one or two sentences>

Evidence
- <claim> — <url> (<publication date>)
- <claim> — <url> (<publication date>)

Not confirmed
- <what you could not establish, and what you tried>
```
