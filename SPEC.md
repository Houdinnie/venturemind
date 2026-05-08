# VentureMind — Technical Specification v1.0

> **AI brain behind Nomad Flow.** A multi-agent autonomous platform taking global founders from idea to execution.

---

## 1. System Overview

### 1.1 Architecture Philosophy

```
User Input → Orchestrator → Agent Swarm → Output + Audit Log
                ↓
         Jurisdiction Engine
                ↓
         Human Escalation (if triggered)
```

- **Jurisdiction-first**: Every recommendation starts with the user's current country, goals, and capital profile
- **Advise then execute**: Agents recommend AND perform actions (form entities, book flights, send messages)
- **Trust by design**: Full audit trail, explainability layer, human escalation gates

### 1.2 Component Map

```
┌─────────────────────────────────────────────────────────┐
│                      FRONTEND (React)                    │
│  Dashboard │ AgentChat │ JurisdictionAdvisor │ Pillars  │
└────────────┬───────────────────────────────────────────┘
             │ HTTPS (REST + SSE)
┌────────────▼───────────────────────────────────────────┐
│                   API GATEWAY (FastAPI)                  │
│  Auth middleware │ Rate limiter │ Prompt injection guard│
└────┬─────────────────────┬──────────────────┬──────────┘
     │                     │                  │
┌────▼────┐         ┌─────▼─────┐      ┌────▼─────────┐
│Orchestr.│         │  Agent    │      │  Pillar      │
│(LangGraph│         │  Swarm    │      │  Services    │
│Supervis.│         │  8 agents │      │  (Juris.,    │
└────┬────┘         └─────┬─────┘      │  Crypto, LIC)│
     │                    │              └──────────────┘
     │  LangGraph        │
     │  State Graph      │
┌────▼────────┐   ┌─────▼─────┐
│ Vector Store │   │ Agent Mem │
│  (Qdrant)    │   │  (Zep)    │
└─────────────┘   └───────────┘
```

### 1.3 Tech Stack

| Layer | Technology | Notes |
|-------|------------|-------|
| Frontend | React + Vite + Tailwind + shadcn/ui | Dark terminal aesthetic |
| Backend | Python 3.11+ + FastAPI | Async-first |
| Agent Layer | LangGraph | Multi-agent orchestration with state machines |
| Vector Store | Qdrant | Semantic search for jurisdictions/services |
| Agent Memory | Zep Cloud | Per-agent persistent memory |
| Database | PostgreSQL | Supabase or Render managed |
| Cache | Redis | Sessions, rate limiting, Celery broker |
| Task Queue | Celery | Async heavy tasks (entity formation, KYC) |
| Primary LLM | Claude (Anthropic) | claude-sonnet-4-20250514 |
| Search | Claude web_search + Amadeus/Duffel | Real-time data |
| Auth | Supabase Auth (JWT) | + client-side session management |
| KYC | Sumsub (primary) + Veriff (fallback) | Tiered KYC/AML |
| Payments | Stripe | Subscriptions + percentage billing |
| Storage | Supabase Storage (S3-compatible) | Encrypted document storage |
| Monitoring | LangSmith + Sentry + Datadog | Agent tracing + error tracking |
| Hosting | Vercel (FE) + Render (BE) | → Paid post-MVP |

---

## 2. Agent Architecture

### 2.1 Core Agent Swarm (8 Agents)

| Agent | Memory Type | Tools | Rate Limit |
|-------|------------|-------|------------|
| **Tax Strategist** | Structured + Zep | Web search, calc engine, treaty DB | 20 req/min |
| **Entity Lawyer** | Structured + Zep | Web search, doc generator | 20 req/min |
| **Nomad Concierge** | Episodic + Zep | Calendar, messaging APIs, visa DB | 30 req/min |
| **Exit Engineer** | Structured + Zep | Financial calc, cap table modeler | 15 req/min |
| **Compliance Auditor** | Audit log (immutable) | Sanctions API, screening APIs | 50 req/min |
| **Wealth Architect** | Structured + Zep | Trust DB, treaty DB, calc engine | 15 req/min |
| **Developer** | Repo + Zep | Code execution, scaffolding | 20 req/min |
| **Accountant & Investment Officer** | Structured + Zep | Financial DB, broker APIs | 20 req/min |

### 2.2 Orchestrator (Supervisor)

LangGraph supervisor node routing to agents:

```python
class OrchestratorState(TypedDict):
    messages: Annotated[list, operator.add]
    user_context: UserContext          # country, capital, goals, business_type
    active_agents: list[str]
    pending_tasks: list[Task]
    escalation_flag: bool
    session_id: str
    user_id: str
    jurisdiction_recommendations: list[JurisdictionRecommendation] | None
```

**Routing logic:**
1. Parse user intent → identify required agent(s)
2. Check `ESCALATION_TRIGGERS` before each agent call
3. If triggered → halt agent, queue for human review, notify user
4. Aggregate agent outputs into coherent response
5. Append jurisdiction-aware disclaimer
6. Write to `AgentDecisionLog`

### 2.3 Agent Decision Log (Immutable Audit Trail)

Every agent decision is logged for legal defense:

```python
@dataclass
class AgentDecisionLog:
    log_id: str                     # UUID
    session_id: str
    user_id: str                     # Hashed
    agent_name: str
    input_summary: str               # Sanitized user query
    output_summary: str              # Agent recommendation (truncated)
    sources_cited: list[str]        # Tax code refs, treaty articles
    confidence_score: float          # 0.0–1.0 (agent-reported)
    disclaimer_shown: bool
    human_review_triggered: bool
    timestamp: datetime
    jurisdiction_context: str
    model_version: str
    token_usage: dict               # For cost tracking
```

**Retention**: 7 years minimum. Stored in append-only table with no DELETE permissions.

### 2.4 Human Escalation Triggers

```python
ESCALATION_TRIGGERS = {
    "capital_raise_amount": 500_000,       # USD, any currency converted
    "confidence_threshold": 0.70,
    "sanctioned_jurisdiction_request": True,
    "user_nationality_high_risk": True,
    "crypto_transaction": 10_000,           # USD equivalent
    "tax_liability_estimate": 100_000,
    "citizenship_renunciation_query": True,
    "conflicting_jurisdiction_obligations": True,
    "user_expresses_urgency_re_authorities": True,
    "failed_kyc_attempts": 3,
    "structuring_detected": True,
    "adverse_media_match": True,
}
```

On trigger: pause response → flag compliance officer → notify user (24hr SLA) → log to `HumanReviewQueue`.

### 2.5 Prompt Injection Protection

```
Input Sanitization Pipeline:
User Input → Heuristic Filters → Adversarial Prompt Detector → [BLOCK/FLAG/WARN] → Agent
```

- **Heuristic filters**: Block common jailbreak patterns (`ignore previous instructions`, `sudo`, etc.)
- **Adversarial detector**: Small classifier flagging suspicious rephrasing attempts
- **Output validation**: Agent outputs checked for leaked system prompt fragments before delivery
- **Rate limiting**: 5 jurisdiction queries per 10 minutes per user (fraud signal)

---

## 3. Data Models

### 3.1 Core Entities

```python
# User & Identity
class User:
    id: uuid
    email: str
    hashed_id: str                    # For audit logs (never plaintext)
    kyc_tier: int                    # 0–4
    kyc_status: enum[pending, approved, rejected, suspended]
    primary_jurisdiction: str        # ISO country code
    business_type: str
    risk_score: int                  # 0–100 composite
    created_at: datetime
    last_active: datetime

# Jurisdiction
class JurisdictionProfile:
    code: str                        # e.g. "AE-DXB"
    name: str
    tax_regime: TaxRegime
    visa_options: list[VisaOption]
    entity_types: list[EntityType]
    banking_access: enum[easy, moderate, restricted]
    substance_requirements: dict
    crs_status: str
    fatca_status: str
    last_updated: datetime
    regulatory_change_alerts: list[Alert]

# Agent Memory (Zep)
class AgentMemory:
    agent_id: str
    user_id: uuid
    session_id: str
    memory_type: enum[episodic, structured, semantic]
    content: dict
    embedding: list[float]
    created_at: datetime
    updated_at: datetime

# Task & Workflow
class Task:
    id: uuid
    user_id: uuid
    agent_name: str
    status: enum[pending, in_progress, completed, failed, escalated]
    input_payload: dict
    output_payload: dict | None
    error_message: str | None
    created_at: datetime
    completed_at: datetime | None
```

### 3.2 Multi-Tenancy Isolation

- **Row-level security (RLS)** enforced in PostgreSQL via Supabase policies
- Every query includes `user_id` filter derived from JWT claims
- KYC documents stored with `user_id` + `document_type` keys; access logged
- Agent memory namespace per `user_id` (Zep)
- Qdrant collections scoped to `user_id`

```sql
-- Example RLS policy
CREATE POLICY user_own_data ON agent_decision_logs
  FOR ALL USING (auth.uid() = user_id);
```

---

## 4. API Design

### 4.1 API Structure

```
/api/v1/
├── auth/           # POST /login, /register, /refresh, /logout
├── users/          # GET /me, PATCH /me, GET /kyc-status
├── agents/         # POST /chat, GET /history/{session_id}, POST /escalate
├── jurisdictions/ # GET /recommend, GET /{code}, GET /compare
├── pillars/       # /tax, /legal, /crypto, /lic, /capital, /luxury
├── tasks/          # GET /{task_id}, GET /{task_id}/status
├── documents/      # POST /upload, GET /{doc_id}, DELETE /{doc_id}
├── kyc/           # POST /initiate, GET /status, POST /webhook
├── payments/       # POST /create-checkout, GET /subscriptions, POST /webhook
└── admin/          # GET /queue, POST /approve-escalation, GET /audit-logs
```

### 4.2 Agent Chat Endpoint

```typescript
// POST /api/v1/agents/chat
interface AgentChatRequest {
  message: string;
  session_id?: string;         // Continue session or new
  agent_preference?: string;   // "Tax Strategist" or null for orchestrator
  context?: {
    current_jurisdiction?: string;
    capital_available?: number;
    business_type?: string;
  };
}

interface AgentChatResponse {
  session_id: string;
  message: string;
  agent_used: string;
  confidence_score: number;
  sources_cited: string[];
  disclaimer: string;           // Full jurisdiction-aware disclaimer text
  escalation_triggered: boolean;
  task_id?: string;            // If async task spawned
}
```

### 4.3 Rate Limiting

| Endpoint Group | Limit | Window |
|--------------|-------|--------|
| Agent chat | 30 req | 1 min |
| Jurisdiction engine | 20 req | 1 min |
| Document upload | 10 req | 1 min |
| KYC initiation | 3 req | 1 min |
| Auth endpoints | 10 req | 1 min |

Rate limit headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`

### 4.4 Webhook Idempotency

All webhooks (Stripe, Sumsub, Amadeus) require idempotency keys:

```python
# Stripe webhook
idempotency_key = request.headers.get("Stripe-Signature") + event.id
if processed_events.contains(idempotency_key):
    return 200  # Already handled

# Sumsub KYC webhook
if kyc_approvals.contains(applicant_id + status):
    return 200  # Already processed
```

---

## 5. Security & Compliance

### 5.1 Compliance Architecture

```
┌──────────────────────────────────────────────────────┐
│                  USER ONBOARDING                      │
│  Email → KYC Tier 0 → Identity Verification          │
│           ↓                                          │
│  ┌─────────────────────────────────────────┐         │
│  │         SANCTIONS SCREENING             │         │
│  │  OFAC SDN │ UN │ EU │ HM Treasury │ PEP  │         │
│  └─────────────────────────────────────────┘         │
│           ↓ Pass                    ↓ Fail            │
│  ┌──────────────┐    ┌──────────────────────────┐   │
│  │ Risk Scoring │    │ Auto-deny + log attempt  │   │
│  │ 0–100       │    │ Alert compliance officer  │   │
│  └──────┬───────┘    └──────────────────────────┘   │
│         ↓ >70 = human review                         │
│  ┌─────────────────────────────────────────┐         │
│  │         KYC APPROVAL / ESCALATION        │         │
│  └─────────────────────────────────────────┘         │
└──────────────────────────────────────────────────────┘
```

### 5.2 KYC Tiers

| Tier | Price | KYC Level | Documents | Activation |
|------|-------|-----------|-----------|------------|
| 0 | Free | Email only | Email + reCAPTCHA | Instant |
| 1 | $99/mo | Basic | Gov ID + selfie liveness | < 5 min |
| 2 | $299/mo | Enhanced | + Proof of address + SoF | < 24 hrs |
| 3 | $999/mo | Full + EDD | + SoW + business docs | < 48 hrs |
| 4 | Custom | Full EDD + Corporate | + UBO + bank statements | < 72 hrs |

### 5.3 AML Monitoring

```python
class AMLMonitor:
    structuring_threshold = 9_500      # Flag below $10k reporting
    structuring_window = timedelta(days=7)
    max_jurisdictions_per_month = 3
    max_bank_accounts_per_month = 2

    suspicious_patterns = [
        "round_number_transactions",       # Always $10K, $50K exactly
        "rapid_jurisdiction_switching",   # UAE → Panama → Seychelles in 60 days
        "third_party_payments",
        "inconsistent_source_of_funds",
        "crypto_to_fiat_to_crypto",
    ]

    SAR_triggers = [
        "structuring_detected",
        "sanctions_match",
        "high_risk_country + transaction > $5000",
        "user_refuses_source_of_funds",
        "multiple_failed_KYC_attempts",
    ]
```

### 5.4 Data Privacy

| Requirement | Implementation |
|------------|----------------|
| Encryption at rest | AES-256 (Supabase Storage) |
| Encryption in transit | TLS 1.3 |
| KYC document access | Role-based — compliance officer only |
| KYC document retention | 5 years post account closure |
| GDPR deletion | 30-day erasure requests (AML docs exempt) |
| Data residency | Regional Supabase instances for UAE, EU, China |
| Cross-border transfers | Standard Contractual Clauses |
| Audit log | Every KYC document access logged |

### 5.5 High-Risk Country Handling

```python
BLOCKED_COUNTRIES = ["North Korea", "Iran", "Syria", "Cuba", "Crimea"]

HIGH_RISK_COUNTRIES = [
    "Nigeria", "Pakistan", "Philippines", "Kenya",
    "Vietnam", "South Africa",          # FATF grey-listed
]

# Zimbabwe: Not blacklisted → Tier 2+ KYC with SoF required
```

---

## 6. Infrastructure & Observability

### 6.1 CI/CD Pipeline

```
Push → GitHub Actions → Test → Build → Deploy
                                    ├── Frontend → Vercel
                                    └── Backend  → Render (auto-restart)
```

- **Staging environment**: `staging.nomadflow.com` on Render preview deploys
- **Environment secrets**: All via Render environment variables (no hardcoding)
- **DB migrations**: Alembic migrations run on deploy startup
- **Health checks**: `/health` endpoint hits DB + Redis + LLM connectivity

### 6.2 Observability Stack

| Tool | Purpose |
|------|---------|
| LangSmith | Agent decision tracing, prompt evaluation |
| Sentry | Error tracking, performance monitoring |
| Datadog | APM, infrastructure metrics, dashboards |
| Loki | Log aggregation (all services) |
| PagerDuty | On-call alerting for production incidents |

### 6.3 Backup & Disaster Recovery

- **Database**: Render PostgreSQL automated daily backups, 7-day retention
- **Vector store**: Qdrant Cloud automated snapshots
- **RTO** (Recovery Time Objective): 4 hours
- **RPO** (Recovery Point Objective): 24 hours
- **Document storage**: Supabase Storage with versioned uploads

---

## 7. Key Gaps Addressed

### 7.1 Gap: No Rate Limiting → FIXED

LangGraph supervisor + FastAPI middleware enforce per-user, per-agent rate limits. Token budgets cap monthly Claude spend per user tier.

### 7.2 Gap: No Hallucination Recovery → FIXED

- Confidence score required from every agent (0.0–1.0)
- `confidence < 0.70` → automatic human escalation
- All outputs require `sources_cited` linking to primary sources (tax code, treaty text)
- Citation chain verifiable by user

### 7.3 Gap: No Multi-Tenancy Isolation → FIXED

RLS on all PostgreSQL tables + Zep namespace isolation + Qdrant collection scoping per user.

### 7.4 Gap: No Offline Mode → FIXED (Phase 2)

- Async task queue via Celery for entity formation workflows
- Email/Telegram notifications on task completion
- Mobile-responsive PWA for status checking

### 7.5 Gap: No Document OCR → FIXED (Phase 2)

AWS Textract integration for:
- Passport/Tax document parsing
- Entity certificate OCR
- Automated data extraction into user profile

### 7.6 Gap: No API Versioning → FIXED

All routes under `/api/v1/` prefix. Deprecation policy: 12-month notice before sunset.

---

## 8. Deployment Configuration

### 8.1 Environment Variables

```bash
# Required
ANTHROPIC_API_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_KEY=
QDRANT_URL=
QDRANT_API_KEY=
REDIS_URL=
ZEP_API_KEY=

# Optional (fallback)
OPENAI_API_KEY=           # For fallback LLM
SENTRY_DSN=

# Derived from user context
JWT_SECRET=                # Supabase JWT secret
```

### 8.2 Build Configuration

```yaml
# Render blueprint (render.yaml)
services:
  - type: web
    name: backend
    env: python
    buildCommand: pip install -r requirements.txt && alembic upgrade head
    startCommand: uvicorn app.main:app --host 0.0.0.0 --port $PORT
    healthCheckPath: /health
    autoDeploy: true

  - type: worker
    name: celery-worker
    env: python
    buildCommand: pip install -r requirements.txt
    startCommand: celery -A app.celery_app worker --loglevel=info
    autoDeploy: true
```

---

## 9. Testing Strategy

### 9.1 Test Types

| Type | Coverage Target | Tool |
|------|----------------|------|
| Unit | Agent logic, data models | pytest |
| Integration | API endpoints, DB queries | pytest + httpx |
| Agent eval | Tax accuracy, legal accuracy | Golden dataset + human eval |
| E2E | Full user flows | Playwright |
| Security | KYC bypass, prompt injection | Burp Suite + custom fuzzer |
| Load | 100 concurrent users | k6 |

### 9.2 Agent Evaluation Framework

```python
class AgentEval:
    """
    Benchmark dataset for each agent persona.
    Human experts label correct outputs.
    Run monthly to detect model degradation.
    """
    tax_strategist_cases: list[EvalCase]  # 50 cases
    entity_lawyer_cases: list[EvalCase]   # 50 cases
    # ...
```

---

## 10. Regulatory Considerations

### 10.1 Licenses Required (Pre-Launch)

| Activity | Jurisdiction | License |
|----------|-------------|---------|
| Tax advice | Most jurisdictions | Not legal advice — platform disclaimer sufficient |
| Capital raise facilitation | SEC (US), FCA (UK) | Likely requires broker-dealer license |
| Investment advice | Multiple | Not providing investment advice — advisory only |
| Money transmission | US (FinCEN) | Not handling funds directly |
| Entity formation | Varies | Partner with licensed service providers |

### 10.2 Jurisdiction for Legal Entity

**Recommendation**: Register legal entity in **Dubai DIFC** or **Singapore** for optimal protection, favorable dispute resolution (DIFC courts, Singapore courts), and broad treaty network.

### 10.3 Insurance (Pre-Launch)

- **Professional Indemnity (E&O)**: $1M minimum — covers AI advice errors
- **Cyber Liability**: $2M minimum — data breach response + regulatory fines
- **D&O**: Personal liability protection for founders

---

## 11. File Structure

```
nomadflow/
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Dashboard.tsx
│   │   │   ├── AgentChat.tsx
│   │   │   ├── JurisdictionAdvisor.tsx
│   │   │   ├── Onboarding.tsx
│   │   │   └── Pillars/
│   │   ├── components/
│   │   │   ├── AgentSwarm/
│   │   │   ├── KYCFlow/
│   │   │   └── JurisdictionMap/
│   │   ├── lib/
│   │   │   ├── api.ts           # Typed API client
│   │   │   └── auth.ts
│   │   ├── stores/
│   │   └── App.tsx
│   ├── vite.config.ts
│   └── package.json
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   └── v1/
│   │   │       ├── agents.py
│   │   │       ├── jurisdictions.py
│   │   │       ├── kyc.py
│   │   │       ├── payments.py
│   │   │       └── auth.py
│   │   ├── agents/
│   │   │   ├── tax_strategist.py
│   │   │   ├── entity_lawyer.py
│   │   │   ├── nomad_concierge.py
│   │   │   ├── exit_engineer.py
│   │   │   ├── compliance_auditor.py
│   │   │   ├── wealth_architect.py
│   │   │   ├── developer_agent.py
│   │   │   ├── accountant_agent.py
│   │   │   └── orchestrator.py
│   │   ├── core/
│   │   │   ├── config.py
│   │   │   ├── security.py
│   │   │   └── rate_limiter.py
│   │   ├── db/
│   │   │   ├── models.py
│   │   │   └── migrations/
│   │   ├── services/
│   │   │   ├── kyc_service.py
│   │   │   ├── aml_monitor.py
│   │   │   └── escalation_service.py
│   │   ├── tasks/
│   │   │   ├── celery_app.py
│   │   │   └── tasks.py
│   │   └── main.py
│   ├── alembic/
│   ├── requirements.txt
│   └── Dockerfile
├── docker-compose.yml
├── render.yaml
└── README.md
```

---

*Document version: 1.0 | Last updated: 2026-05-09 | Author: VentureMind Technical Team*
---