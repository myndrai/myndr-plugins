---
name: source-check
description: Judge how much a web page can be trusted for a specific claim. Use before repeating a number, a benchmark, a security advisory, or anything a user will act on.
---

# Source check

Trust is per claim, not per site. A vendor page is authoritative about its own
pricing and worthless about a competitor's latency.

## Check, in order

1. **Who published it.** The organization, not the domain's reputation. An
   engineering blog on a corporate domain is still one engineer's opinion.
2. **When.** Publication date and last-modified. No date is itself a finding —
   report the claim as undated rather than current.
3. **Is it primary for this claim?** Primary: the vendor's own docs for its
   pricing and API, the standards body for the spec, the filing for the
   financials, the repository for the code. Everything else is reporting on a
   primary source, which can be stale or wrong.
4. **Does it show its work?** A number with a method you could repeat beats the
   same number with no method. Benchmarks without hardware, version, and
   configuration are anecdotes.
5. **Who benefits.** Note the incentive when the publisher sells the thing
   being measured. It does not void the claim; it sets how much independent
   confirmation you need.
6. **Corroboration.** One independent source that measured it separately is
   worth more than five that cite the same original.

## Report

```
Claim: <the exact claim>
Source: <publisher> — <url> (<date or "undated">)
Primary for this claim: yes | no (<what the primary source would be>)
Method shown: yes | no
Corroboration: <independent sources, or none>
Verdict: solid | usable with caveats | not enough to act on
Caveat: <what would change the verdict>
```

Never round a verdict up to be helpful. "Not enough to act on" is a useful
answer and the user can ask for more searching.
