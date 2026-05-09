# VentureMind on OpenFang — AOS Integration
> OpenFang is VentureMind's runtime kernel — 4 autonomous hands running 24/7

---

## What Is OpenFang?

**OpenFang** (github.com/RightNow-AI/openfang) is an open-source Agent Operating System written in Rust. It compiles to a single ~32MB binary and runs autonomous "Hands" (pre-built capability packages) on schedules without human prompting.

Key characteristics:
- **17,300 GitHub stars**, Apache-2.0/MIT licensed
- **137,728 lines of Rust** across 14 crates
- **1,767+ tests**, zero clippy warnings
- **40 channel adapters** (Telegram, WhatsApp, Signal, Slack, Discord, etc.)
- **27 LLM providers**, 123+ models, automatic fallback
- **16-layer security model** including subprocess sandbox, prompt injection scanner, GCRA rate limiter
- **OpenAI-compatible API** — drop-in for existing tools
- **OpenClaw migration engine** — one-command migration from OpenClaw

### The 7 Bundled Hands

| Hand | What It Does |
|------|-------------|
| **Clip** | YouTube → vertical shorts with captions, thumbnails, AI voice-over → Telegram/WhatsApp |
| **Lead** | Daily prospect discovery, enrichment, 0–100 scoring, ICP profiling |
| **Collector** | OSINT monitoring, change detection, sentiment tracking, knowledge graph |
| **Predictor** | Superforecasting with confidence intervals, contrarian mode, Brier score tracking |
| **Researcher** | Deep cited research, CRAAP credibility evaluation, APA formatting, multilingual |
| **Twitter** | Autonomous tweet thread creation, scheduling, engagement, approval queue |
| **Browser** | Web automation, form filling, purchase approval gate (will never spend money without OK) |

---

## VentureMind × OpenFang — Integration

VentureMind runs 4 custom Hands on the OpenFang kernel:

### 1. `venturemind-intake` Hand
**What it does**: The autonomous founder onboarding specialist.
- Phased adaptive discovery interview (identity → goals → jurisdictions → capital → timeline)
- Zero-Knowledge Vault activation for document storage
- Sanctions screening (OFAC, EU, UN, UK HMT)
- Confidence scoring across 6 dimensions (Goal Clarity 20%, Jurisdictional Feasibility 20%, Capital Adequacy 20%, Timeline Realism 15%, Legal Eligibility 15%, Documentation Completeness 10%)
- Routing to appropriate execution tier (Green Button / Amber / Orange / Red)

**Schedule**: On-demand per new founder intake

**Guardrails**: KYC tier enforcement; no execution below 0.70 confidence without human review

---

### 2. `venturemind-watchdog` Hand
**What it does**: The always-on SafetyNet compliance enforcer.
- Real-time AML transaction monitoring (structuring detection, velocity checks, SAR triggers)
- Agent decision auditing (every decision logged to immutable AgentDecisionLog)
- Protocol Zero enforcement (System Kill-Switch triggers)
- Cross-swarm conflict detection (legal vs. tax advice inconsistency flagging)
- KYC/EDD tier management per founder

**Schedule**: Continuous (24/7 monitoring)

**Guardrails**: Mandatory human approval for transactions >$50,000; PEP flags; any watchlist match

---

### 3. `venturemind-strategist` Hand
**What it does**: The Sovereign Blueprint builder.
- Generates 5-layer strategic plans: Entity Architecture, Tax Optimisation, Capital Strategy, Compliance Roadmap, Timeline & Milestones
- Sovereign Score calculation for each option (Tax Efficiency 30%, Legal Protection 25%, Operational Ease 20%, Reputation 15%, Cost 10%)
- Treaty network analysis (DTA, TIEA, EU Directives)
- Capital raise modelling (SAFE → Series A → bridge → exit)
- Option-level risk-adjusted scenario planning

**Schedule**: On-demand per founder per quarter (or triggered by milestone)

**Guardrails**: Recommendations below Sovereign Score 0.65 require explicit founder approval and SafetyNet review

---

### 4. `venturemind-monitor` Hand
**What it does**: The regulatory intelligence layer for all founders.
- Daily regulatory radar (tax law changes, treaty ratifications, AML updates)
- Quarterly market intelligence (competitor pricing, founder sentiment, demand signals)
- Per-founder portfolio monitoring (industry news, jurisdictional developments, filing deadlines)
- Active opportunity discovery (new banking infrastructure, SEZ benefits, startup visa programmes)

**Schedule**: Daily at 08:00 in each founder's timezone; weekly swarm intelligence report; monthly regulatory radar

**Guardrails**: Alert urgency classification (Red/Orange/Amber/Yellow/Green) with per-urgency delivery channels

---

## How OpenFang Fits in the VentureMind Stack

```
┌─────────────────────────────────────────────────────┐
│                   NOMAD FLOW PLATFORM               │
│                                                     │
│  ┌─────────────────────────────────────────────┐   │
│  │  Paperclip (Company Package)                 │   │
│  │  Defines org chart, agents, teams, projects  │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│  ┌─────────────────────────────────────────────┐   │
│  │  Feynman (Research Intelligence)             │   │
│  │  Deep research, literature review, verified  │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│  ┌─────────────────────────────────────────────┐   │
│  │  gstack (Engineering Discipline)            │   │
│  │  CEO/Review/QA slash commands                │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│  ┌─────────────────────────────────────────────┐   │
│  │  OpenFang (Runtime Kernel)                  │   │
│  │  24/7 autonomous Hands execution             │   │
│  │  4 VentureMind Hands running continuously     │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│  ┌─────────────────────────────────────────────┐   │
│  │  agent-skills-eval (Validation)               │   │
│  │  Empirical SKILL.md performance testing       │   │
│  └─────────────────────────────────────────────┘   │
│                                                     │
│  10 DOMAIN SWARMS (Legal, Tax, Capital, Growth,      │
│  Web3, Wealth, Mobility, Journey, Engineering,      │
│  Financial — each with Sub-Agents and SOUL.md)     │
│                                                     │
│  SafetyNet: HITL Triggers, Protocol Zero,          │
│  Zero-Knowledge Vault, Compliance Auditor           │
└─────────────────────────────────────────────────────┘
```

---

## Why OpenFang Was Chosen

| Requirement | OpenFang Delivers |
|-------------|-------------------|
| Always-on operation | Agents run on schedules, not just on-demand prompts |
| Single binary deployment | No Python environments, no dependency hell |
| Hardened security | 16-layer model: sandbox, prompt injection scanner, GCRA rate limiter |
| Multi-channel delivery | 40 adapters — Telegram, WhatsApp, Email for founder alerts |
| Agent-to-agent protocol | OFP P2P with HMAC-SHA256 mutual authentication |
| Audit trail | Immutable decision logging with citations and confidence scores |
| OpenClaw compatibility | One-command migration from existing OpenClaw setups |
| OpenAI-compatible API | Drop-in for existing tooling and integrations |

---

## Quick Start

```bash
# Install OpenFang
curl -fsSL https://openfang.sh/install | sh
openfang init
openfang start

# Dashboard at http://localhost:4200

# Chat with any agent
openfang chat intake-specialist
openfang chat compliance-watchdog

# Check hand status
openfang hand status
```

---

## Files in This Directory

```
openfang/
├── CLAUDE.md          ← Architecture overview
├── README.md          ← This file
└── agents/
    ├── intake-specialist/AGENTS.md     ← Founder onboarding
    ├── compliance-watchdog/AGENTS.md   ← SafetyNet enforcement
    ├── strategy-architect/AGENTS.md    ← Sovereign Blueprint builder
    └── market-monitor/AGENTS.md        ← Regulatory intelligence
```

---

## Relevant Links

- **OpenFang**: https://github.com/RightNow-AI/openfang
- **Docs**: https://openfang.sh/docs
- **Discord**: https://discord.gg/sSJqgNnq6X
- **VentureMind**: https://github.com/Houdinnie/venturemind