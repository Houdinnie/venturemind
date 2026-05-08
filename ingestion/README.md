# VentureMind — Technical Ingestion System Summary
## Phase 1–5 Complete

---

## What Was Built

This document summarises the complete Structured Discovery Ingestion System for VentureMind / Nomad Flow.

---

## Architecture

```
USER ONBOARDING
     │
     ▼
┌─────────────────────────────┐
│  PHASE 1: DEEP DISCOVERY    │
│  Onboard Assistant UI        │
│  (7-stage adaptive form)    │
│                             │
│  Strategist Agent            │
│  (Branching interview logic) │
│                             │
│  Document OCR                │
│  (AWS Textract)              │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  PHASE 2: VERIFICATION      │
│  4 parallel research loops  │
│                             │
│  • Global Intelligence       │
│  • Citation & Source Layer   │
│  • Competitive Moat          │
│  • Compliance Cross-Check   │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  PHASE 3: SYNTHESIS         │
│  Orchestrator Agent         │
│  (Merges all loop outputs)  │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  PHASE 4: HUMAN REVIEW      │
│  (Conditional — if any     │
│   score < 0.70)            │
│  The Guild (human panel)    │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  PHASE 5: DELIVERY          │
│  PDF Generation             │
│  Secure transmission        │
│  Calendar sync (ICS)        │
└─────────────────────────────┘
```

---

## Files Produced

### `/home/workspace/VentureMind/ingestion/`

| File | Purpose |
|------|---------|
| `SPEC.md` | Complete ingestion system spec (5-phase pipeline, schemas, confidence rules, escalation triggers) |
| `prompts/strategist-agent.md` | Full prompt architecture for the Strategist Agent (branching logic, 7 stage gates, question library, disclaimer enforcement) |
| `ONBOARD-UI-SPEC.md` | Dark-terminal UI spec for the Onboard Assistant (all 7 screens, component library, document upload panel, escalation overlay) |
| `workflows/verification-loops.md` | Phase 2 verification engine (4 parallel loops, source classification, moat scoring, compliance cross-check) |

### `/home/workspace/VentureMind/personas/`

14 persona files covering all 10 Domain Leads + 3 Central sub-agents + architecture overview.

### `/home/workspace/VentureMind/SPEC.md`

Full technical spec for the entire VentureMind platform.

---

## Key Design Decisions

### 1. Zero-Loophole Data Gathering
Every unknown is a question, not a guess. The 7-stage gate system ensures no assumption is left unmapped before synthesis begins.

### 2. Confidence Scoring with Hard Blocks
Any claim below 0.70 confidence is flagged/blocked. No unverified "AI logic" reaches the final blueprint.

### 3. SafetyNet Guardrails
- Jurisdiction disclaimers on all Tax/Legal outputs
- Human escalation for capital raises > $500k
- Full stop for sanctions-listed counterparties or entities in sanctioned jurisdictions
- Non-negotiable legal consent required before any onboarding begins

### 4. Parallel Verification (not serial)
All 4 verification loops run simultaneously, dramatically reducing time-to-blueprint while ensuring comprehensive coverage.

### 5. Audit-Ready by Default
Every decision is logged with timestamp, confidence score, cited sources, and jurisdiction. The `AgentDecisionLog` creates a defensible record from day one.

---

## Deployment Notes

**Frontend**: React + Vite + Tailwind CSS (dark terminal aesthetic)  
**OCR**: AWS Textract (or alternative: Google Cloud Document AI)  
**Backend**: FastAPI + LangGraph (orchestrator)  
**Database**: PostgreSQL (append-only immutable audit log)  
**PDF Generation**: Puppeteer + react-pdf  
**Secure Delivery**: S3 pre-signed URLs + AES-256 encryption  
**Background Jobs**: Celery + Redis

---

## Compliance Notes for Houdinnie's Context

**Zimbabwe** is currently on FATF's monitored jurisdiction list. This means:
- Enhanced Due Diligence (EDD) is **required** for any Zimbabwean nationals
- Source of funds documentation must be collected
- KYC Tier 2+ applies (not eligible for Tier 0 free trial)
- This must be disclosed in the audit log with a flag

This does **not** prevent VentureMind from serving Zimbabwean clients — it requires a higher standard of documentation from the intake phase.

---

*VentureMind Ingestion System v1.0 — Complete*
*Owner: Houdinnie (houdinnie.zo.computer) — 2026*