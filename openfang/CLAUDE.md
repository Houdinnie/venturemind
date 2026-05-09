# VentureMind — OpenFang Agent Operating System
## Autonomous Agent Infrastructure for the Nomad Flow Platform

---

## Overview

OpenFang is VentureMind's **runtime kernel** — a battle-tested, Rust-based Agent Operating System that compiles to a single ~32MB binary and runs 24/7 without human prompting.

VentureMind runs 4 OpenFang agents (Hands) that orchestrate the entire Nomad Flow lifecycle.

---

## The 4 VentureMind Hands

| Hand | What It Does | Schedule |
|------|-------------|----------|
| **`intake-specialist`** | Conducts structured founder discovery, builds Sovereign Blueprint, scores confidence | On-demand (per intake) |
| **`compliance-watchdog`** | Monitors all swarms for AML, KYC, HITL, Protocol Zero violations | Continuous (24/7) |
| **`strategy-architect`** | Builds multi-layer sovereign blueprints (entity, tax, capital, compliance, timeline) | On-demand (per founder) |
| **`market-monitor`** | Watches global regulatory, market, and opportunity intelligence for all founders | Daily at 08:00 founder timezone |

---

## Architecture

```
Nomad Flow Platform
        │
        ▼
┌─────────────────────────────┐
│   OpenFang Kernel (Rust)    │
│   Single 32MB Binary        │
│   Dashboard: localhost:4200 │
└─────────────────────────────┘
        │
        ├── Agent: intake-specialist   (Founder onboarding)
        ├── Agent: compliance-watchdog  (SafetyNet enforcement)
        ├── Agent: strategy-architect   (Sovereign Blueprint builder)
        └── Agent: market-monitor       (Regulatory intelligence)
```

---

## Quick Start

```bash
# Install OpenFang
curl -fsSL https://openfang.sh/install | sh

# Initialize
openfang init

# Start the kernel
openfang start

# Dashboard
openfang chat compliance-watchdog
openfang hand status
```

---

## Key Features Used

- **Always-on agents**: VentureMind agents run on schedules, not just when prompted
- **HAND.toml manifest**: Each hand declares its tools, guardrails, and dashboard metrics
- **Guardrails (HITL)**: Sensitive actions require explicit human approval before execution
- **Immutable audit log**: Every decision logged with citations, timestamps, confidence scores
- **40 channel adapters**: Telegram, WhatsApp, Email — founders receive alerts on their preferred platform
- **16-layer security**: Subprocess sandbox, prompt injection scanner, GCRA rate limiter, path traversal prevention
- **27 LLM providers**: Anthropic, Gemini, Groq, DeepSeek, OpenRouter, Ollama — with automatic fallback

---

## Security Model

OpenFang provides the hardened runtime for VentureMind's SafetyNet:

| Layer | Protection |
|-------|-----------|
| Subprocess sandbox | `env_clear()` + selective variable passthrough |
| Prompt injection scanner | Detects override attempts, data exfiltration patterns |
| GCRA rate limiter | Cost-aware token bucket with per-IP tracking |
| Path traversal prevention | Canonicalization with symlink escape prevention |
| Loop guard | SHA256 tool call loop detection with circuit breaker |
| Session repair | 7-phase message history validation and automatic recovery |
| OFP mutual authentication | HMAC-SHA256 nonce-based P2P verification |

---

## Comparison: VentureMind Stack

| Component | Purpose | Role |
|-----------|---------|------|
| **Paperclip** | Company structure | Defines org chart, agents, teams, projects |
| **Feynman** | Research intelligence | Deep research, literature review, fact-checking |
| **gstack** | Engineering discipline | CEO/Eng Manager/QA/Review slash commands |
| **OpenFang** | Runtime execution | 24/7 autonomous agents, always-on monitoring |
| **agent-skills-eval** | Skill validation | Empirical testing of SKILL.md effectiveness |
| **VentureMind** | Domain logic | The 10 swarms, SafetyNet, HITL, Sovereign Blueprint |

---

## File Structure

```
openfang/
├── CLAUDE.md                          ← This file
├── agents/
│   ├── intake-specialist/AGENTS.md    ← Founder onboarding agent
│   ├── compliance-watchdog/AGENTS.md  ← Protocol Zero enforcer
│   ├── strategy-architect/AGENTS.md   ← Sovereign Blueprint builder
│   └── market-monitor/AGENTS.md       ← Regulatory intelligence agent
└── README.md                          ← Overview document
```

---

## Key Principles

1. **No hallucination**: Every claim must cite a primary source
2. **Confidence scoring**: All decisions have a score; below 0.70 triggers human review
3. **Audit everything**: Immutable AgentDecisionLog for every action
4. **Fail closed**: When in doubt, escalate — never execute
5. **Founder first**: Every agent exists to serve the founder, not the platform