---
name: venturemind-recipe
description: VentureMind execution recipe — generates step-by-step formation, banking, and compliance sequences for founder entity setup.
metadata:
  source: https://github.com/Houdinnie/venturemind
  attribution: Houdinnie / VentureMind
  license: MIT
  usage: vendored
---

# VentureMind Execution Recipe

> Generate ranked, implementable sequences for entity formation, banking setup, and compliance onboarding — with confidence scores and HITL checkpoints.

## When to Use

After a founder's Deep Discovery profile is complete and the system is ready to move from "Advise" to "Execute." Use this to generate the Green Button manifest for the specific jurisdiction stack the founder has chosen.

## Recipe Components

For each step in the sequence:
1. **Action** — what the sub-agent does
2. **Target** — which swarm/agent executes
3. **Prerequisites** — what must be complete first
4. **Confidence score** — based on source verification (0.0–1.0)
5. **HITL trigger** — if above threshold, block for human approval
6. **Estimated time** — realistic completion window
7. **Failure recovery** — what happens if this step fails

## Recipe Types

| Recipe | Use When |
|--------|----------|
| `entity-formation` | Founder needs LLC/IBC/C-Corp in target jurisdiction |
| `banking-setup` | Founder needs business bank account + Stripe Connect |
| `crypto-bridge` | Founder needs exchange account + multi-sig wallet |
| `residency-track` | Founder needs digital nomad visa or residency-by-investment |
| `capital-raise` | Founder is raising pre-seed or seed from international investors |

## Output

`outputs/<slug>/recipe.md` — ranked steps with:
- Confidence scores per step
- HITL trigger flags (GREEN / RED / CRIMSON)
- Prerequisite chain
- Failure recovery protocols
- Estimated total timeline

This feeds directly into the Green Button Execution Manifest.