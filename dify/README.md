---
name: venturemind-dify-workflows
description: VentureMind Dify integration — design, deploy, and operate Nomad Flow AI workflows on Dify Enterprise
metadata:
  author: houdinnie.zo.computer
  tags:
    - dify
    - llmops
    - rag
    - workflow
    - agentic
    - orchestration
sources:
  - kind: github
    repo: langgenius/dify
    attribution: LangGenius
    license: Dify Open Source License
---

# VentureMind on Dify

VentureMind uses **Dify Enterprise** as its primary workflow orchestration and LLMOps platform — the visual canvas where the Engineering Swarm designs, monitors, and iterates all Nomad Flow AI pipelines.

> **Status**: Planning target — Dify Enterprise deployment designed but not yet provisioned.

## Why Dify

Dify hits 5 of VentureMind's 7 critical requirements:

| Requirement | Dify Answer |
|---|---|
| Visual workflow design | Drag-and-drop canvas with testing per node |
| Multi-model support | 50+ providers; GPT, Claude, Llama, Mistral, Gemma all in one workflow |
| RAG pipeline | Out-of-box chunking + hybrid search + reranker |
| Agent tool calls | Function Calling + ReAct + 50+ built-in tools |
| LLMOps | Per-app logs, traces, cost analysis, dataset labelling |
| Backend-as-a-Service | All apps expose REST APIs — embed anywhere |
| Self-hostable | Docker Compose — full control, no vendor lock-in |

## Architecture

```
Strategist Agent
    ↓ (intake: user profile)
┌────────────────────────────────────────────┐
│         Dify Enterprise (self-hosted)      │
├────────────────────────────────────────────┤
│  Workflow: Deep Discovery Intake           │
│    → RAG KB: onboarding corpus            │
│    → Agent: ReAct (confidence scoring)      │
│    → confidence < 0.70 → HITL flag       │
├────────────────────────────────────────────┤
│  Workflow: Strategy Synthesis              │
│    → Multi-agent sequential:             │
│       Strategist → Planner →             │
│       Compliance → Wealth Architect      │
├────────────────────────────────────────────┤
│  Workflow: Legal Document Gen             │
│    → Agent: Function Calling             │
│    → Webhook → legal-tools API           │
│    → Compliance Auditor review           │
├────────────────────────────────────────────┤
│  Workflow: Tax Optimization               │
│    → ReAct Agent + Calculator            │
│    → RAG KB: treaty databases           │
├────────────────────────────────────────────┤
│  Workflow: Entity Formation               │
│    → Sequential: KYC → Docs →          │
│       Stripe → Zoneless → Concierge     │
├────────────────────────────────────────────┤
│  Workflow: Compliance Monitor           │
│    → LLMOps log → threshold alerts     │
│    → SafetyNet Watchdog Agent           │
└────────────────────────────────────────────┘
    ↓ (workflow outputs)
Operator Agent / Execution Manifests
    ↓
Nomad Flow Frontend (React)
```

## Dify Workflows in VentureMind

### 1. Deep Discovery Intake
- **Type**: Chatbot + RAG + ReAct Agent
- **Model**: Claude 4 Sonnet via OpenRouter
- **RAG KB**: VentureMind onboarding corpus + Nomad Flow FAQ
- **Escalation**: `confidence < 0.70` → Human-in-the-Loop flag
- **YAML marker**: `venturemind/workflows/deep-discovery.yaml`

### 2. Strategy Synthesis
- **Type**: Multi-agent sequential (4 nodes)
- **Models**: GPT-4o (orchestration) + Claude 4 Sonnet (per agent)
- **Outputs**: JSON roadmap, investment thesis, entity type recommendation
- **YAML marker**: `venturemind/workflows/strategy-synthesis.yaml`

### 3. Legal Document Generation
- **Type**: Agent (Function Calling) → Webhook
- **Webhook**: legal-tools API (internal)
- **Guardrail**: All docs pass Compliance Auditor before delivery
- **YAML marker**: `venturemind/workflows/legal-gen.yaml`

### 4. Tax Optimization Engine
- **Type**: ReAct Agent + Calculator
- **KB**: IRS treaties, UAE corporate tax law, Cayman DTT, HK DTA
- **Outputs**: Tax savings calc, treaty matrix, filing timeline
- **YAML marker**: `venturemind/workflows/tax-optimization.yaml`

### 5. Entity Formation Pipeline
- **Type**: Sequential workflow (5 steps)
- **Steps**: KYC → documents → Stripe Connect → Zoneless → NomadConcierge
- **YAML marker**: `venturemind/workflows/entity-formation.yaml`

### 6. Compliance Monitoring
- **Type**: LLMOps log monitor → threshold alerts
- **Monitor**: message volume, SAR rate, EDD queue depth
- **Alert →**: SafetyNet Watchdog Agent
- **YAML marker**: `venturemind/workflows/compliance-monitor.yaml`

## Dify Setup (Self-Hosted)

```bash
# 1. Clone Dify
git clone https://github.com/langgenius/dify.git
cd dify/docker

# 2. Configure for self-hosted production
cp .env.example .env
# Edit .env:
#   CONSOLE_WEB=0
#   API_WEB=1
#   SERVER_ONLY=deployed
#   SECRET_KEY=<generate-32-byte-hex>
#   INITABLE_ADMIN_PASSWORD=<strong-password>

# 3. Start Dify
docker compose up -d

# 4. Access dashboard
# http://localhost/install → create admin account
# http://localhost → login

# 5. Add models (Settings → Model Providers)
# Add OpenRouter → Claude 4 Sonnet
# Add OpenAI → GPT-4o

# 6. Import workflows (Workspace → Import YAML)
# Import from venturemind/workflows/*.yaml
```

## Tenant Isolation

Each Nomad Flow client = one Dify **Organisation** with its own:
- Model quota and rate limits
- RAG knowledge bases
- App list and API keys
- LLMOps logs

Cross-tenant data access is architecturally impossible without Dify Enterprise admin credentials.

## Security

| Risk | Mitigation |
|---|---|
| API keys in Dify config | Use Infisical Agent Vault; inject via env vars at runtime |
| Workflow prompt injection | Input validation + output sanitisation on all tool nodes |
| RAG hallucinations | Hybrid search (vector + BM25) + reranker; confidence threshold |
| LLMOps PII leakage | Strip email/phone fields before log ingestion |
| Workflow failure masking | Non-retryable errors → SafetyNet webhook alert |

## Cost Model

Dify Enterprise self-hosted:
- **Infrastructure**: ~$80–200/month (VPS + PostgreSQL + Redis + vector DB)
- **Model costs**: Pay-per-token via OpenRouter / OpenAI / Groq
- **No per-seat licensing** for self-hosted

Nomad Flow tier cost recovery:
- Tier 1 ($99/mo) → $300/month model budget
- Tier 2 ($299/mo) → $600/month model budget
- Tier 3 ($999/mo) → $1,500/month model budget

## Integration With Other Stack Components

| Component | Integration Point |
|---|---|
| paperclip (orchestration) | paperclip agents delegate complex tasks → Dify workflow API |
| agent-vault (Infisical) | Dify credentials stored in vault; injected at runtime |
| openfang (AOS) | OpenFang hands monitor Dify workflow health |
| markitdown (document ingestion) | Ingest uploaded client PDFs into Dify RAG KB |
| feynman (research) | Feynman deepresearch → updates Dify RAG knowledge bases |
| public-apis (API catalog) | Dify tool nodes call public-apis for exchange rates, treaty data |
| zoneless (payouts) | Dify entity-formation workflow calls Zoneless API on payout step |
| fakecloud (testing) | Integration tests run against fakecloud-emulated AWS (DynamoDB, SQS) |

## Next Steps

1. Provision VPS with Docker + PostgreSQL + Redis + Qdrant
2. Deploy Dify Enterprise (docker-compose)
3. Configure model providers (OpenRouter for Claude)
4. Add VentureMind organisation + tenant isolation
5. Import first 3 workflows (deep discovery, strategy synthesis, tax)
6. Wire SafetyNet webhook → Compliance Auditor
7. Set up LLMOps dashboards in Grafana
8. Run integration tests against fakecloud
