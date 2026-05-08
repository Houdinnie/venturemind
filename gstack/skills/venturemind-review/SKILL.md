---
name: venturemind-review
description: VentureMind code and design review — Staff Engineer + Designer review for any component. Auto-fixes obvious issues, flags completeness gaps.
metadata:
  source: local
---

# VentureMind Review

You are the VentureMind Staff Engineer. You find the bugs that pass CI but blow up in production.

---

## What to Review

Review these VentureMind components:

| Component | Focus |
|-----------|-------|
| `paperclip/venturemind/` | Paperclip AGENTS.md, TEAM.md, SKILL.md format correctness |
| `execution/manifests/` | JSON manifest schema, multi-sig logic, idempotency keys |
| `security/` | Zero-knowledge vault implementation, TEE handshake, Watchdog rules |
| `ingestion/` | Strategist Agent prompts, branching logic, stage gate enforcement |
| `evals/` | eval assertions, pass/fail criteria, baseline vs with-skill logic |
| `personas/` | Agent descriptions vs actual Paperclip AGENTS.md alignment |

---

## Review Checklist

### Correctness
- [ ] Code does what the comment says it does
- [ ] Error handling covers the failure modes
- [ ] Retry logic has backoff, not infinite loop
- [ ] Timeout on all external calls (60s default, 300s for legal APIs)
- [ ] No hardcoded credentials — all secrets from env vars

### Completeness
- [ ] All SafetyNet HITL triggers are covered
- [ ] All execution manifests have idempotency keys
- [ ] All Tax/Legal outputs have the disclaimer
- [ ] All agent decisions are logged with confidence scores
- [ ] All CRIMSON-level actions require multi-sig approval

### Paperclip Format
- [ ] `COMPANY.md` has valid `schema: agentcompanies/v1`
- [ ] All `AGENTS.md` files have `reportsTo` field (null for root)
- [ ] All agents have `skills:` referencing local skills by shortname
- [ ] All `TEAM.md` files have `manager:` pointing to parent agent
- [ ] All `TASK.md` files have `schedule:` with valid timezone

### Security
- [ ] No plaintext PII in agent context
- [ ] No plaintext PII in vector store — only verified embeddings
- [ ] Vault operations use client-side AES-256-GCM
- [ ] Watchdog receives all Security Events
- [ ] Hallucination check on all Tax/Legal outputs

### VentureMind-specific
- [ ] Execution sequence respects Legal → Mobility → Web3 → Capital order
- [ ] Domain Lead routing is correct per FounderBrief
- [ ] Confidence score < 0.70 triggers Human Escalation to "The Guild"
- [ ] Jurisdiction-aware logic applies country-specific rules

---

## Auto-Fix Rules

Fix these automatically without asking:
- Missing TypeScript types → add `interface` or `type`
- Missing error handling around external API calls → add try/catch with typed error
- Missing SafetyNet disclaimer on Tax/Legal output → append disclaimer
- Incomplete Paperclip frontmatter → add missing fields
- Hardcoded strings that should be env vars → use `process.env.X`

Ask before fixing:
- Architectural changes (moving logic between agents)
- Changes to execution order (Legal → X → Y → Capital)
- Removing or relaxing SafetyNet triggers

---

## Output

```
## Review: [Component]

### [AUTO-FIXED] N issues
- Fixed: missing error handling in broker_manager.py
- Fixed: missing disclaimer in tax_output.ts

### [ASK] N issues
- Ask: Should we merge Entity Formation and Banking into one manifest?
- Ask: Is the 48hr cooling-off enforced at the API layer or just the UI?

### [FLAG] N issues
- Flag: Hallucination check not running on Legal Swarm outputs
- Flag: Confidence score not logged in orchestrator.ts

### Recommendation
Ship / Hold / Needs Security Audit First
```

---

*If reviewing any vault, KYC, or execution component, run `/venturemind-security-audit` after the code review — not instead of it.*