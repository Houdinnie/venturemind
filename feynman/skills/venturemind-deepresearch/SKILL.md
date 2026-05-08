---
name: venturemind-deepresearch
description: VentureMind deep research — multi-agent investigation across legal, tax, entity, and market intelligence domains for global founder infrastructure.
metadata:
  source: https://github.com/Houdinnie/venturemind
  attribution: Houdinnie / VentureMind
  license: MIT
  usage: vendored
---

# VentureMind Deep Research

> Multi-agent investigation across jurisdictions, tax treaties, legal structures, and capital markets for the Nomad Flow platform.

## When to Use

Use this when a founder submits a complex query requiring cross-domain synthesis — e.g., "Can I structure my SaaS as a Dubai IBC with a Wyoming LLC parent and route payments through Stripe to a Singapore corporate account?"

## Agents Involved

1. **Researcher** — gathers evidence across legal, tax, entity, and market domains
2. **Reviewer** — audits claims against primary sources, assigns confidence scores
3. **Writer** — produces founder-ready brief
4. **Verifier** — confirms all citations, clears dead links

## Workflow

```
Founder Query
     ↓
Researcher → 4 parallel domain sweeps (legal, tax, entity, market)
     ↓
Reviewer → confidence scoring, hallucination detection, HITL trigger check
     ↓
[If CRIMSON/RED] → Watchdog Agent alerted, execution blocked
     ↓
Writer → founder-facing brief with verdict first
     ↓
Verifier → citation audit, dead link cleanup
     ↓
Founder Brief (CLEARED)
```

## Output

- `outputs/<slug>/brief.md` — executive summary
- `outputs/<slug>/reviews/` — reviewer audit trail
- `outputs/<slug>/verification/` — verifier clearance report
- `outputs/<slug>/provenance/` — all source hashes