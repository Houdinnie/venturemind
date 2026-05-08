---
name: safety-net-protocols
description: VentureMind SafetyNet HITL protocols — enforce human-in-the-loop checkpoints, manage escalation triggers (AMBER/RED/CRIMSON), and maintain immutable audit trail. Required skill for all domain leads and the Watchdog Agent.
metadata:
  author: houdinnie.zo.computer
  version: 1.0
---

# SafetyNet Protocols Skill

SafetyNet is VentureMind's human-in-the-loop accountability layer. Every agent must understand and enforce these protocols.

## Severity Levels

| Level | Meaning | Response |
|-------|---------|----------|
| **AMBER** | Anomalous pattern detected | Log + Telegram alert to Houdinnie |
| **RED** | Confirmed threat | Pause affected swarm + alert + require re-auth |
| **CRIMSON** | Systemic breach | Halt ALL swarms + revoke ALL keys + urgent Telegram alert |

## HITL Triggers (must pause and request authorisation)

- Entity formation filing → CRIMSON
- Capital raise > $500,000 → CRIMSON
- Multi-sig wallet deployment → CRIMSON
- Banking transaction > $10,000 → RED
- KYC tier violation attempt → CRIMSON
- Document checksum mismatch → CRIMSON

## Green Button Protocol

1. Orchestrator generates execution manifest
2. Central Swarm Lead reviews manifest risk level
3. If CRIMSON → alert Houdinnie via Telegram, wait for explicit approval
4. If RED → require hardware key verification
5. On approval → dispatch manifest to sub-agents
6. On rejection → log rejection reason, pause manifest

## Disclaimer Enforcement

All Tax and Legal outputs MUST append:

> "This is informational guidance only and does not constitute legal or tax advice. Consult a licensed attorney or CPA in your jurisdiction."

## Audit Log Requirements

Every agent decision must log:
- `timestamp`: ISO 8601 with microseconds
- `agentId`: Which agent made the decision
- `confidenceScore`: 0.0 - 1.0
- `primarySources`: Array of cited sources
- `escalationStatus`: "none" | "human_review" | "crimson_trigger"
- `decisionBasis`: "primary_sources" | "user_input" | "hallucination_fallback"

## Hallucination Recovery

When confidence < 0.70:
1. Do NOT guess — mark section as `needs_review`
2. Trigger human review escalation
3. Log the ambiguity with cited sources that conflict
4. Never produce confident output without primary sources