# VentureMind — Structured Discovery Ingestion System
## Version 1.0 | Audit-Ready | Zero-Loophole

---

## Overview

The **Structured Discovery Phase** replaces the traditional "single text box" onboarding with a multi-agent, multi-modal intake engine that collects, validates, and enriches founder data before any recommendation is generated.

**Goal**: Produce a clean, verified, audit-ready `FounderProfile` that the Swarm of Swarms can act on without hallucination risk or trust gaps.

---

## The 5-Phase Pipeline

```
┌─────────────────────────────────────────────────────────────────────┐
│                        PHASE 1: DEEP DISCOVERY                       │
│                    Onboard Assistant + Strategist Agent               │
│                                                                     │
│  Adaptive Interview → Document OCR → Real-Time Enrichment            │
│                          ↓                                          │
│              Cleaned FounderProfile JSON (T0)                       │
└────────────────────────────┬────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    PHASE 2: VERIFICATION LOOPS                       │
│              Parallel processing across 4 Verification Swarms         │
│                                                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  Global     │  │ Verification│  │ Competitive │  │ Compliance  │ │
│  │ Intelligence│  │ & Citation  │  │ Moat       │  │ Cross-Check │ │
│  │ Loop       │  │ Layer       │  │ Analysis   │  │             │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │
│         │               │               │               │         │
│         └───────────────┴───────────────┴───────────────┘         │
│                             ↓                                      │
│              VerifiedFounderProfile (T0 + verification)             │
└────────────────────────────┬────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                     PHASE 3: SYNTHESIS                               │
│                       Orchestrator Agent                             │
│                                                                     │
│  Swarm Data Merge → Confidence Scoring → Blueprint Draft             │
│                          ↓                                          │
│                Draft International Business Blueprint                │
└────────────────────────────┬────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    PHASE 4: HUMAN REVIEW                             │
│                  (if confidence < 0.70 anywhere)                    │
│                                                                     │
│  Human Escalation → Guild Review → Resolution                       │
│                          ↓                                          │
│              Approved / Corrected FounderProfile                     │
└────────────────────────────┬────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    PHASE 5: DELIVERY                                │
│                  Developer Agent + LuxOps                           │
│                                                                     │
│  PDF Generation → Disclaimer Watermarking → Secure Delivery         │
│                                                                     │
│  Outputs: Downloadable PDF + Email + Calendar Sync                   │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Deep Discovery — Detailed Workflow

### Step 1.1: Adaptive Interview (Onboard Assistant + Strategist Agent)

The `Onboard Assistant` is a React component that presents a branching, multi-step questionnaire. The `Strategist Agent` drives the logic, triggering follow-up questions based on prior answers.

**Branching Logic Example:**

```
User input: "I'm building a Crypto SaaS"
→ Triggers: Custody sub-questions, token utility questions, KYC requirements
→ Triggers: Web3 Swarm onboarding block

User input: "I'm a freelancer earning $80k/year"
→ Triggers: Self-employed tax structure questions, country of residence
→ Triggers: Financial Swarm onboarding block

User input: "I want to raise $2M from VCs"
→ Triggers: Capital Swarm questions, entity structure for investment
→ Triggers: Legal Swarm entity formation block
```

**Core Intake Fields (Required):**
- Full legal name + nationality
- Current country of tax residence
- Target countries for business operations
- Business type (SaaS / Freelance / E-commerce / Crypto / Agency / Holding)
- Annual revenue range (or target)
- Entity status (none / existing / in formation)
- Capital raise intent (Yes/No + amount)
- Personal assets / net worth range
- Risk tolerance / privacy preference
- Timeline (immediate / 3 months / 6 months / 12 months)

**Optional Enhancement Fields:**
- Target lifestyle (city hints: Lisbon, Dubai, Tbilisi, Bangkok, etc.)
- Family situation (solo / partner / dependents)
- Existing visas / citizenships
- Current banking relationships

### Step 1.2: Document Ingestion (OCR)

Upload zone accepts:
- Passport (all pages)
- Existing entity certificates (Certificate of Incorporation, Articles)
- Previous tax returns (last 2 years)
- Existing visa / residency permits
- Proof of address (< 3 months old)

**Processing:**
- AWS Textract (or equivalent) extracts text from uploaded PDFs/images
- Data is parsed into structured fields and cross-referenced with interview answers
- Conflicts are flagged for user verification (e.g., "You said you are tax resident in Portugal, but your passport shows UAE")

### Step 1.3: Real-Time Data Enrichment (Researcher Agent)

While the user types, the `Researcher Agent` performs background lookups:
- Competitor mentions → TAM validation
- Country mentions → Regulatory update scan
- Industry mentions → Market size estimate
- Competitor names → Moat analysis pre-check

---

## Phase 2: Verification Loops

### Loop 1: Global Intelligence (Compliance Auditor)

- Real-time scan for regulatory changes affecting any mentioned jurisdiction
- Black Swan event detection (visa restriction changes, tax law amendments, banking sanctions)
- Triggered whenever user mentions a country or industry

### Loop 2: Verification & Citation Layer

- Every claim must have a primary source attached
- Confidence score calculated per section
- Score < 0.70 → Human Escalation to "The Guild" (review board)

### Loop 3: Competitive Moat Analysis

- Identifies direct competitors (Nomad Capitalist, Flag Theory, Herzen Path, etc.)
- Maps the user's unique differentiators
- Ensures no duplicate strategy without clear differentiation

### Loop 4: Compliance Cross-Check

- Tax Strategist + Entity Lawyer cross-validate every jurisdiction recommendation
- Flag contradictions: e.g., "You want Dubai holding + Portugal NHR — here's the CFC risk"

---

## Phase 3: Synthesis — The Master Blueprint

The `Orchestrator Agent` merges outputs from all Verification Loops into:

**International Business & Life Blueprint** containing:
1. Executive Summary (founder's situation + top 3 recommendations)
2. Entity Structure Diagram (LLC/Holding/Trust hierarchy)
3. Jurisdiction Ranking (Top 3 with explainability layer — why each was ranked)
4. Tax Optimisation Map (effective rates, treaty benefits, CFC rules)
5. Visa & Mobility Plan (steps, timelines, costs)
6. 15-Step Execution Checklist (prioritised, with owners/swarm assignments)
7. Risk Register (identified risks with mitigation plans)
8. Audit Trail (every decision with confidence score + cited sources)

---

## Phase 4: Human Review (Conditional)

Only triggered if:
- Any section has confidence < 0.70
- Capital raise > $500,000
- CBI programme involvement
- Sanctions-adjacent jurisdiction
- Unresolvable data conflicts

**Guild Review**: A panel of 2+ human experts reviews flagged sections, provides corrections, and logs the resolution.

---

## Phase 5: Delivery

### PDF Generation (Developer Agent)
- Uses sub-agent to generate a formatted PDF
- All mandatory disclaimers permanently watermarked on every page
- Footer on each page: "Generated by VentureMind — for general guidance only. Not legal or tax advice."

### Secure Transmission
- Download: Direct link (authenticated, single-use expiry option)
- Email: Encrypted attachment via secure channel
- Calendar Sync: ICS file with tax deadlines, visa renewal dates, filing cutoffs

---

## Legal Safety: Non-Negotiable Clauses

Before AI initiates any research, user MUST sign:
1. **Limitation of Liability**: "VentureMind provides general guidance only. No liability for decisions made based on this output."
2. **AI Disclaimer**: "I understand this is AI-generated and does not constitute legal, tax, or financial advice."
3. **Data Consent**: "I consent to my data being processed for the purpose of generating a business strategy report."
4. **Jurisdiction Acknowledgement**: "I confirm I will seek licensed local professionals before making any entity or tax decisions."

**Enforcement**: These clauses are checkbox-mandatory before Phase 1 begins. No data ingestion begins without signed acknowledgement stored in the audit log.

---

## Technical Stack (Ingestion Module)

| Component | Technology |
|-----------|------------|
| Frontend (Onboard Assistant) | React + Vite + Tailwind CSS |
| Document OCR | AWS Textract |
| Real-time enrichment | Anthropic web search + company research APIs |
| PDF generation | puppeteer / react-pdf |
| Secure delivery | S3 pre-signed URLs + encryption |
| Audit log | PostgreSQL (append-only, immutable) |
| Background processing | Celery + Redis |
| Hosting | Vercel (frontend) + FastAPI (backend) |

---

## Output Schemas

### FounderProfile JSON

```json
{
  "id": "uuid",
  "created_at": "ISO 8601",
  "consent_signed": true,
  "basic_info": {
    "full_name": "string",
    "nationality": ["string"],
    "current_tax_residence": "string",
    "target_business_countries": ["string"],
    "business_type": "enum",
    "annual_revenue_usd": "range",
    "entity_status": "none | existing | in_formation",
    "capital_raise_intent": { "yes": false, "amount_usd": 0 }
  },
  "profile_enrichment": {
    "lifestyle_preferences": ["string"],
    "family_status": "string",
    "existing_visas": ["string"],
    "banking Relationships": ["string"],
    "mentioned_competitors": ["string"]
  },
  "documents": [
    {
      "type": "passport | certificate | tax_return | visa",
      "parsed": { "fields": {} },
      "verified": true,
      "source": "S3 path"
    }
  ],
  "confidence_score": 0.85,
  "verification_status": {
    "global_intelligence": "passed | flagged | escalation",
    "citation_layer": "passed | flagged | escalation",
    "competitive_moat": "passed | flagged | escalation",
    "compliance_cross_check": "passed | flagged | escalation"
  },
  "escalations": []
}
```

---

## Confidence Scoring Rules

| Score | Meaning | Action |
|-------|---------|--------|
| 1.0 | Government primary source (statute, treaty, official regulation) | Use freely |
| 0.9 | Official secondary source (government website, published ruling) | Use freely |
| 0.8 | Authoritative third-party (Big 4 analysis, law firm briefing) | Use freely |
| 0.7 | General knowledge (widely accepted practice) | Use — append disclaimer |
| 0.6 | Informed judgement (agent expertise, no primary source) | Flag + escalate |
| ≤ 0.5 | Low confidence / speculative | Block + escalate |

---

## Escalation Triggers

| Condition | Severity | Action |
|-----------|----------|--------|
| Any section score < 0.70 | HIGH | Pause blueprint, alert user |
| Capital raise > $500k | CRITICAL | Full stop, human review required |
| Transaction > $10k in plan | HIGH | Flag in report, user sign-off required |
| CBI programme involvement | CRITICAL | Full disclosure, legal review |
| Sanctions-adjacent jurisdiction | CRITICAL | Block, do not recommend |
| Unresolvable data conflict | MEDIUM | Flag, request user clarification |
| Black Swan event detected | HIGH | Alert user, update affected sections |

---

*Document version: 1.0 — Generated for VentureMind / Nomad Flow*
*Owner: Houdinnie (houdinnie.zo.computer) — 2026*