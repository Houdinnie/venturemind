---
name: venturemind-lit
description: VentureMind literature review — survey tax treaties, entity formation laws, and compliance frameworks across target jurisdictions.
metadata:
  source: https://github.com/Houdinnie/venturemind
  attribution: Houdinnie / VentureMind
  license: MIT
  usage: vendored
---

# VentureMind Literature Review

> Survey primary sources across tax law, entity structures, KYC/AML frameworks, and residency rules for global founder infrastructure.

## When to Use

Use this when building the jurisdiction knowledge base — e.g., "What treaties exist between UAE and these 12 countries?" or "What's the current state of Wyoming LLC vs Delaware C-Corp for non-resident founders?"

## Protocol

1. Search primary sources (gov portals, treaty databases, legal databases)
2. Identify consensus positions and disagreements
3. Flag open questions requiring human confirmation
4. Produce structured review with source URLs and last-verified dates

## Output

`outputs/<slug>/literature-review.md` — structured with:
- Consensus positions
- Disagreements and edge cases
- Open questions (flagged for human review)
- Full citation list with HTTP status