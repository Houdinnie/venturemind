# VentureMind SafetyNet — Human-in-the-Loop (HITL) Triggers
## Version 1.0 | Protocol Zero | Audit-Ready

---

## Overview

The **SafetyNet HITL Framework** is the legal accountability layer between autonomous swarm execution and real-world action. It ensures that no irreversible legal standing or significant capital movement occurs without the founder's explicit, verified authorization.

**Design Principle**: "The AI advises, drafts, and prepares. The human signs, approves, and executes. Never the other way around."

---

## Risk Classification

| Risk Level | Threshold | Behaviour | Example |
|-------------|-----------|-----------|---------|
| **GREEN** | $0 / No legal standing | Fully autonomous — no HITL required | Data gathering, draft generation, research synthesis |
| **YELLOW** | < $1,000 / Draft filings | Pre-fills forms, prepares drafts — human reviews before submission | Operating Agreement draft, Tax calculation |
| **RED** | $1,000–$10,000 / Filings | Human must explicitly approve before API call executes | LLC filing fee payment, Business bank account application |
| **CRIMSON** | > $10,000 / Contracts | Multi-sig approval + 48-hour cooling-off period | Capital deployment, Multi-jurisdiction entity formation |
| **BLACK** | Protocol Zero | Immediate freeze — all API calls revoked | Anomaly, breach, or runaway execution suspected |

---

## Category 1: High-Stakes Legal & Financial Triggers

### 1.1 Entity Filing Authorization

**Trigger Condition**: API call initiated to any formation service (Stripe Atlas, Wyoming SOS, Dubai IFZA, Singapore ACRA)

**Flow**:
```
Legal Swarm (Formation Sub-Agent)
    │
    ├─ Drafts Articles of Organization / Incorporation
    ├─ Runs Jurisdiction Verification (Reg-Watch check)
    └─ Presents "Final Draft Review" UI
              │
              ▼
        HITL: Founder reviews draft
              │
         ┌────┴────┐
         │ APPROVE │──────── Submit API call
         │ REJECT  │──────── Agent revises draft
         └─────────┘
```

**API Endpoint**: `POST /api/v1/execution/entity/filing`
**SafetyNet Flag**: `requires_human_signoff: true`
**Audit Log**: `entity_filing_authorization`

---

### 1.2 Capital Movement Thresholds

| Amount | Action Required |
|--------|-----------------|
| < $100 | Autonomous — gas fee auto-payment only |
| $100–$1,000 | Pre-approval notification via Telegram — 24hr window to cancel |
| $1,000–$10,000 | Explicit multi-sig approval required via Secure Dashboard |
| > $10,000 | Multi-sig + 48-hour cooling-off period + OTP |

**Multi-Sig Logic** (Gnosis Safe):
```
Transaction Request
    │
    ├─ Agent proposes transaction (amount, destination, gas estimate)
    ├─ AI Auditor validates: OFAC check, sanctions screen, recipient risk
    └─ Threshold 2/3 signers required
            │
            ├─ Founder (mobile approval)
            └─ SafetyNet Agent (automated second signature)
```

**API Endpoint**: `POST /api/v1/execution/capital/move`
**SafetyNet Flag**: `requires_multisig: true`
**Audit Log**: `capital_movement_authorization`

---

### 1.3 Contractual Commitment

**Trigger Condition**: Any digital signature request on service provider agreements (lawyers, accountants, realtors, SaaS platforms)

**Flow**:
```
Orchestrator detects signing event
    │
    ├─ Highlights "Non-Negotiable Clauses" in red
    ├─ Presents full contract for review
    └─ Founder digitally signs via:
            │
            ├─ DocuSign API (legal contracts)
            └─ HelloSign API (general agreements)
```

**Non-Negotiable Clause Categories**:
- Indemnification clauses
- Liability caps
- Data retention / deletion rights
- Jurisdiction and governing law
- Auto-renewal terms

**API Endpoint**: `POST /api/v1/execution/contract/sign`
**SafetyNet Flag**: `non_negotiable_review_required: true`
**Audit Log**: `contractual_commitment`

---

## Category 2: Regulatory & Accuracy Triggers (The Auditor Layer)

### 2.1 Confidence Score Thresholds by Swarm

| Swarm | Green (Autonomous) | Yellow (Review) | Red (Escalate) | Escalation Target |
|-------|-------------------|-----------------|----------------|-------------------|
| **Engineering** | 0.90–1.00 | 0.75–0.89 | < 0.75 | Human developer review |
| **Legal** | 0.95–1.00 | 0.85–0.94 | < 0.85 | The Guild ( vetted lawyers) |
| **Financial / Tax** | 0.95–1.00 | 0.85–0.94 | < 0.85 | The Guild (CPA network) |
| **Capital** | 0.90–1.00 | 0.80–0.89 | < 0.80 | Human financial advisor |
| **Growth / Marketing** | 0.85–1.00 | 0.70–0.84 | < 0.70 | Human marketing review |
| **Web3** | 0.95–1.00 | 0.90–0.94 | < 0.90 | Smart contract auditor |
| **Wealth** | 0.95–1.00 | 0.85–0.94 | < 0.85 | The Guild (wealth advisor) |
| **Mobility / Journey** | 0.90–1.00 | 0.80–0.89 | < 0.80 | Human concierge review |
| **Central** | 0.90–1.00 | 0.80–0.89 | < 0.80 | Founder direct review |

**Confidence Score Formula**:
```
Score = (
    0.30 × SourceVerificationRatio   # % of claims with primary sources
  + 0.25 × JurisdictionAccuracy     # % of recommendations matching current law
  + 0.20 × HistoricalValidation      # Past similar cases successfully executed
  + 0.15 × CrossSwarmAgreement       # Other swarms confirm same recommendation
  + 0.10 × UserDataCompleteness      # Profile completeness from discovery phase
)
```

---

### 2.2 Jurisdiction Conflict Alert

**Trigger Condition**: Two or more swarms produce recommendations that conflict based on the same current-year regulatory data

**Example Conflict**:
```
Tax Agent → Recommends Portugal NHR programme
Visa Agent → Detects new NHR restriction on remote worker visas (2026 update)
         ↓
Strategy Conflict Alert triggered
```

**Flow**:
```
Jurisdiction Conflict Detected
    │
    ├─ Audit Report generated with:
    │       - Conflicting recommendations
    │       - Data sources cited by each agent
    │       - Timestamp of regulatory change
    └─ Strategy Conflict Alert UI displayed
              │
              ▼
        HITL: Founder selects resolution strategy
              │
         ┌────┴────┐
         │ Override │ (Founder chooses one recommendation)
         │ Remand  │ (Return to both agents to resolve)
         └─────────┘
```

**API Endpoint**: `POST /api/v1/safety/conflict-alert`
**SafetyNet Flag**: `requires_conflict_resolution: true`

---

### 2.3 Hallucination Check

**Trigger Condition**: Compliance Auditor Agent cross-references agent output against primary sources and finds a mismatch

**Flow**:
```
Agent output produced
    │
    ├─ Auditor Agent runs:
    │       - Primary source validation (gov websites, tax codes, treaties)
    │       - Citation verification
    │       - Jurisdiction currency check
    └─ Mismatch detected
              │
              ▼
        Hallucination Flag: PROCESS_FROZEN
              │
         ┌────┴────┐
         │ View Mismatch Report │
         └─────────┘
              │
         ┌────┴──────────┐
         │ Agent re-research │
         │ Human expert    │ (if source unavailable)
         └─────────────────┘
```

**Audit Log**: `hallucination_check_failed`

---

## Category 3: Identity & Security Triggers

### 3.1 KYC Biometric Handoff

**Rule**: The AI may pre-fill KYC forms but may NEVER capture biometric data directly.

**Flow**:
```
KYC Application Required
    │
    ├─ AI pre-fills all text fields from FounderProfile
    ├─ AI prepares document package (passport scan, proof of address)
    └─ Biometric Capture
              │
              ▼
        Mobile Handoff Event triggered
              │
         ┌────┴────┐
         │ Send to │
         │ Founder │
         │ mobile  │
         └─────────┘
              │
         Founder completes liveness check
         on Sumsub / Onfido SDK
              │
         Results returned to Compliance Auditor
```

**Blocked from AI**: Fingerprint, facial scan, video liveness, government ID OCR (done by SDK)
**AI Permitted**: Pre-filling form fields, document sorting, deadline tracking

---

### 3.2 Key Management Protocol

**Rule**: No agent may ever store the Primary Admin Key for any wallet, exchange, or bank account.

**Key Hierarchy**:
```
Level 1: Cold Storage Key (Founder offline backup — never touches any system)
Level 2: Primary Admin Key (Founder mobile — for multi-sig transactions)
Level 3: Agent Operational Key (Stored in Vault — limited to pre-approved operations)
Level 4: Read-Only Key (No signing capability — audit and balance monitoring only)
```

**Flow for New Wallet Initialization**:
```
Safe Wallet initialization requested
    │
    ├─ Agent generates seed phrase in TEE (Trusted Execution Environment)
    ├─ Seed phrase displayed ONE TIME on Secure Display
    └─ Secure Display Event
              │
         ┌────┴────┐
         │ Founder  │
         │ records  │
         │ offline  │
         └─────────┘
              │
         Seed phrase wiped from agent memory
              │
         Primary Admin Key stored on founder mobile (non-exportable)
              │
         Agent receives only Operational Key (limited scope)
```

**API Endpoint**: `POST /api/v1/security/key-initialization`
**SafetyNet Flag**: `key_management_protocol: strict`

---

## Protocol Zero: Emergency Freeze

**Trigger Conditions**:
- Anomaly detection in API call patterns
- Unauthorized access attempt to Key Vault
- Regulatory sanction flag on a newly connected entity
- Multi-agent contradictory outputs without resolution after 3 cycles
- Founder emergency stop signal (voice, email, or SMS)

**Flow**:
```
Protocol Zero Activated
    │
    ├─ 1. FREEZE
    │       All outgoing API calls to banking, formation, and social platforms revoked
    │       Pending transactions suspended
    │       Agent execution queues paused
    │
    ├─ 2. NOTIFY
    │       Encrypted SMS + Telegram to founder:
    │       "Protocol Zero triggered. All actions suspended. Diagnostic report attached."
    │       Includes: trigger source, affected agents, last 10 API calls
    │
    ├─ 3. EXPORT
    │       Full LangGraph decision trace exported to Secure Vault
    │       Audit log packaged and timestamped
    │
    └─ 4. RESUME (Founder re-authorizes)
            Founder reviews diagnostic report
            Issues Resume Command: "VENTUREMIND RESUME"
            Optional: Schedule human expert review via The Guild
```

**API Endpoint**: `POST /api/v1/safety/protocol-zero`
**SafetyNet Flag**: `emergency_freeze: true`
**Audit Log**: `protocol_zero_activated`

---

## Handoff Matrix: Action Type → Required Human Action

| Action Type | Agent Does | Human Action Required |
|-------------|-----------|----------------------|
| **Research** | Autonomous data gathering | None — review final report |
| **Drafting** | Pre-fills forms/contracts | Review and "Approve for Submission" |
| **Filing** | Submits via API | Final 2FA / Biometric verification |
| **Banking** | Connects to Neobank API | Live video call (if required by bank) |
| **Funding** | Calculates gas/fees | Approve initial deposit to Operating Fund |
| **Contract Signing** | Highlights non-negotiables | Digital signature (DocuSign/HelloSign) |
| **Key Generation** | Generates in TEE, displays once | Record offline — agent memory wiped |
| **Wallet Initialization** | Sets up Safe multi-sig | Mobile handoff for admin key |
| **Multi-Jurisdiction Filing** | Prepares all documents | Multi-sig + 48-hour cooling-off |

---

## Verified Command Structure

When the founder issues "Go Ahead," it is a cryptographically signed instruction:

```json
{
  "command_id": "cmd_uuid_v4",
  "timestamp": "2026-05-09T08:00:00Z",
  "founder_id": "founder_uuid",
  "action": "EXECUTE_PLAN_V1",
  "pipeline": ["Formation(WY_LLC)", "Banking(Mercury)", "Wallet(Safe_Mainnet)", "Funding(Stripe_Connect)"],
  "risk_level": "RED",
  "multisig_required": true,
  "signature": "sig_base64_from_mobile_app",
  "biometric_verified": true,
  "cooling_off_expires": "2026-05-11T08:00:00Z"
}
```
