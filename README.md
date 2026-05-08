# VentureMind — AI Brain Behind Nomad Flow

> **Version 1.0** · May 2026 · Public

---

## What Is VentureMind?

VentureMind is the autonomous AI engine powering **Nomad Flow** — a global platform that takes founders from raw idea to live, running business in a single session.

It does this by coordinating **10 specialist AI swarms**, each responsible for one domain of building a global company:

| # | Swarm | What It Does |
|---|-------|-------------|
| 1 | **Central Swarm** | The executive — coordinates all other swarms |
| 2 | **Engineering Swarm** | Builds your product: web, mobile, infrastructure |
| 3 | **Legal Swarm** | Sets up your LLC, IBC, or holding structure |
| 4 | **Financial Swarm** | Tax strategy, bookkeeping, financial planning |
| 5 | **Capital Swarm** | Raises funding, manages runway and burn |
| 6 | **Growth Swarm** | SEO, content, paid acquisition |
| 7 | **Web3 Swarm** | Crypto wallets, DeFi, token strategy |
| 8 | **Wealth Swarm** | Personal investing, portfolio management |
| 9 | **Mobility Swarm** | Residency, banking, travel logistics |
| 10 | **Journey Swarm** | Health, lifestyle, nomad infrastructure |

---

## Architecture

VentureMind is built on three open-source frameworks:

| Framework | Role |
|-----------|------|
| **[Paperclip](https://github.com/paperclipai/paperclip)** | Agent org chart — defines every agent's role, reports, skills, and schedule |
| **[Feynman](https://github.com/getcompanion-ai/feynman)** | Research layer — verifies claims, audits sources, reproduces data |
| **[agent-skills-eval](https://github.com/darkrishabh/agent-skills-eval)** | Validation — proves every skill actually works before shipping |
| **[gstack](https://github.com/garrytan/gstack)** | Engineering team — 23 opinionated slash commands for shipping velocity |

---

## Repository Structure

```
VentureMind/
├── personas/                    ← Human-readable agent descriptions (14 agents)
│   ├── README.md                ← Swarm of Swarms index
│   ├── central-swarm-lead.md
│   ├── engineering-swarm-lead.md
│   ├── legal-swarm-lead.md
│   ├── financial-swarm-lead.md
│   ├── capital-swarm-lead.md
│   ├── growth-swarm-lead.md
│   ├── web3-swarm-lead.md
│   ├── wealth-swarm-lead.md
│   ├── mobility-swarm-lead.md
│   ├── journey-swarm-lead.md
│   ├── strategy-agent.md
│   ├── operator-agent.md
│   └── talent-agent.md
│
├── paperclip/venturemind/       ← Paperclip company package (AI org chart)
│   ├── COMPANY.md               ← Root company definition
│   ├── agents/                  ← 12 agent definitions (AGENTS.md format)
│   │   ├── central-swarm/
│   │   ├── engineering-swarm/
│   │   ├── legal-swarm/
│   │   ├── financial-swarm/
│   │   ├── capital-swarm/
│   │   ├── growth-swarm/
│   │   ├── web3-swarm/
│   │   ├── wealth-swarm/
│   │   ├── mobility-swarm/
│   │   ├── journey-swarm/
│   │   ├── watchdog/
│   │   └── orchestrator/
│   ├── teams/                   ← 6 team groupings
│   ├── projects/                ← 5 active projects
│   ├── tasks/                   ← 4 recurring scheduled tasks
│   └── skills/                  ← 4 core skills (SKILL.md format)
│
├── feynman/                     ← Feynman research agent package
│   ├── agents/                  ← 4 specialist research agents
│   │   ├── researcher/
│   │   ├── reviewer/
│   │   ├── writer/
│   │   └── verifier/
│   ├── skills/                  ← 4 feynman-powered skills
│   │   ├── venturemind-deepresearch/
│   │   ├── venturemind-lit/
│   │   ├── venturemind-watch/
│   │   └── venturemind-recipe/
│   └── research/               ← 2 active research projects
│       ├── uae-dubai-ibc-vs-wyoming-llc/
│       └── treaty-network-analysis/
│
├── gstack/                     ← gstack engineering team integration
│   ├── CLAUDE.md               ← gstack configuration for Nomad Flow
│   └── skills/                 ← 4 VentureMind-specific gstack skills
│       ├── venturemind-office-hours/
│       ├── venturemind-security-audit/
│       ├── venturemind-review/
│       └── venturemind-ceo-review/
│
├── ingestion/                   ← Structured Discovery System
│   ├── SPEC.md                 ← Full ingestion system specification
│   ├── ONBOARD-UI-SPEC.md      ← Dark terminal UI design spec
│   ├── prompts/
│   │   └── strategist-agent.md ← Strategist Agent prompt architecture
│   ├── workflows/
│   │   └── verification-loops.md ← Phase 2: 4-parallel verification loops
│   └── templates/
│       └── intake-report.md     ← Audit-ready intake report template
│
├── execution/                  ← Green Button Autonomous Execution
│   ├── README.md               ← Execution system overview
│   ├── SafetyNet-HITL.md       ← Human-in-the-Loop trigger matrix
│   ├── manifests/              ← Execution manifests
│   │   ├── EXEC-SOVEREIGN-ACCOUNT.md
│   │   ├── EXEC-ENTITY-FORMATION.md
│   │   └── EXEC-NEOBANK-FIAT.md
│   └── agents/
│       └── watchdog-agent.md   ← Real-time threat detection
│
├── security/                   ← Hardened Security Architecture
│   ├── SECURITY-VAULT.md       ← Zero-Knowledge Vault & Ghost Protocol
│   └── agents/
│       └── watchdog-agent.md   ← Watchdog Agent (separate copy)
│
├── evals/                      ← agent-skills-eval validation suite
│   ├── README.md
│   ├── safety-net-protocols/   ← SafetyNet HITL skill eval
│   ├── orchestrator/           ← Orchestrator skill eval
│   ├── green-button/           ← Green Button execution eval
│   └── vault-access/          ← Zero-Knowledge Vault eval
│
├── SPEC.md                    ← Master technical specification
└── PRD.md                    ← Product Requirements Document (PDF attached)

```

---

## The 3 Modes: Advise → Authorise → Execute

### Mode 1: Advise (Discovery)
The Strategist Agent runs a 7-stage intake interview. Every answer is verified against 4 primary sources in parallel. An audit-ready intake report is generated with a confidence score.

### Mode 2: Authorise (SafetyNet)
The Green Button system presents every proposed action with full context, risk level, and legal implications. The user approves or rejects each action. CRIMSON-level actions trigger mandatory human review.

### Mode 3: Execute (Autonomous)
Approved actions run autonomously. Every step is logged to an immutable decision chain. The Watchdog Agent monitors every action in real time and can pause any execution instantly.

---

## Core Skills

| Skill | What It Does |
|-------|-------------|
| `safety-net-protocols` | Enforces HITL triggers, confidence thresholds, and escalation paths |
| `orchestrator` | Routes tasks to the correct swarm, manages inter-swarm dependencies |
| `green-button` | Manages the full execution manifest lifecycle |
| `vault-access` | Zero-knowledge document handling with ghost redaction |

---

## The 10 Pillar Services (Nomad Flow)

1. **Idea → Launch** — Product development with engineering swarm
2. **Legal Counsel** — Entity formation, contracts, compliance
3. **Nomad Navigator** — Visa, residency, travel logistics
4. **Luxury Optimizer** — Lifestyle infrastructure (flights, hotels, concierge)
5. **Health** — Health insurance, telemedicine, fitness
6. **Tax Optimisation** — International tax structure, treaty optimisation
7. **Crypto Services** — Wallets, exchanges, DeFi, tax reporting
8. **Entity Formation** — LLC, IBC, holding structures globally
9. **Capital Raise** — Investor matching, pitch deck, due diligence
10. **The Accountant** — Bookkeeping, CFO services, financial reporting

---

## Build Roadmap

| Phase | Focus | Status |
|-------|-------|--------|
| 1 | Core Agent Brain — intake, routing, orchestration | Complete |
| 2 | SafetyNet — HITL triggers, compliance layer | Complete |
| 3 | Green Button — execution manifests, real execution | Complete |
| 4 | Full Platform — UI, UX, payment, onboarding | In Progress |

---

## Stack

**Backend**: Python · FastAPI · LangGraph · PostgreSQL · Redis · Qdrant  
**Frontend**: React · TypeScript · Tailwind CSS  
**AI**: Claude · Groq · OpenAI  
**Agents**: Paperclip · Feynman · gstack  
**Infrastructure**: Docker · GCP · Cloudflare  

---

## Revenue Model

| Tier | Price | Includes |
|------|-------|---------|
| **Nomad** | $99/mo | 1 swarm, basic intake, 10 actions/mo |
| **Founder** | $299/mo | 3 swarms, full intake, 50 actions/mo, SafetyNet |
| **Sovereign** | $999/mo | All 10 swarms, unlimited actions, Watchdog, priority support |

Plus: % of entity formation fees, % of capital raised, affiliate commissions.

---

## Related Documents

- `SPEC.md` — Full technical architecture
- `PRD.md` — Product Requirements Document (plain English)
- `ingestion/SPEC.md` — Structured Discovery System
- `execution/SafetyNet-HITL.md` — Human-in-the-Loop framework
- `security/SECURITY-VAULT.md` — Zero-Knowledge security architecture

---

*VentureMind is the AI brain behind Nomad Flow. Built for founders who want to build globally, not just locally.*
