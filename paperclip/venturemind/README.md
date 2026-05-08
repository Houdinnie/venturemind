# VentureMind on Paperclip — Autonomous Company Package

> **Status**: Ready for import into Paperclip. Deploy the AI company that runs itself.

---

## What Is This

This is the **VentureMind** autonomous company package for [Paperclip](https://github.com/paperclipai/paperclip) — the open-source orchestration platform for running AI-powered companies.

VentureMind runs on a **Swarm of Swarms** architecture: 10 autonomous domain swarms, each with a Domain Lead and specialist sub-agents, all coordinated by a central orchestrator. Every action is governed by SafetyNet protocols with human-in-the-loop checkpoints.

---

## Structure

```
venturemind/
├── COMPANY.md               ← Root entrypoint, org boundary, goals
├── agents/                  ← 14 agent definitions
│   ├── central-swarm/       ← Central Swarm Lead, Strategy, Operator, Talent
│   ├── orchestrator/        ← Core orchestration brain
│   ├── watchdog/            ← Security & compliance monitor
│   ├── engineering-swarm/   ← The Builder
│   ├── legal-swarm/         ← The Protector
│   ├── financial-swarm/     ← The Accountant
│   ├── capital-swarm/       ← The Investor
│   ├── growth-swarm/        ← The Voice
│   ├── web3-swarm/          ← The Innovator
│   ├── wealth-swarm/        ← The Preserver
│   ├── mobility-swarm/      ← The Navigator
│   └── journey-swarm/        ← The Logistics
├── teams/                   ← 6 team groupings
│   ├── central-swarm/        ← Orchestration & coordination
│   ├── legal-and-formation/ ← Legal + entity formation
│   ├── finance-and-tax/      ← Financial + wealth
│   ├── capital-and-growth/   ← Capital + marketing
│   ├── mobility-and-journey/ ← Mobility + travel
│   └── engineering-and-web3/ ← Engineering + crypto
├── projects/                ← 5 active projects
│   ├── deep-discovery-ingestion/
│   ├── swarm-orchestration/
│   ├── green-button-execution/
│   ├── safetynet-watchdog/
│   └── paperclip-integration/
├── tasks/                   ← 4 scheduled tasks
│   ├── daily-orchestrator-check-in/
│   ├── weekly-performance-review/
│   ├── watchdog-system-health/
│   └── safety-briefing-houdinnie/
└── skills/                  ← 4 reusable skills
    ├── safety-net-protocols/
    ├── orchestrator/
    ├── green-button/
    └── vault-access/
```

---

## Org Chart

```
HOUDINNIE (Owner)
    └── Central Swarm Lead
            ├── Strategy Agent
            ├── Operator Agent
            ├── Talent Agent
            ├── Orchestrator Agent
            ├── Watchdog Agent
            │
            ├── Engineering Swarm Lead
            ├── Legal Swarm Lead
            ├── Financial Swarm Lead
            ├── Capital Swarm Lead
            ├── Growth Swarm Lead
            ├── Web3 Swarm Lead
            ├── Wealth Swarm Lead
            ├── Mobility Swarm Lead
            └── Journey Swarm Lead
```

---

## Key Concepts

### 1. Swarm of Swarms
10 domain-specialised agent swarms, each autonomous within its domain but coordinated by Central Swarm Lead. No swarm acts independently on cross-domain decisions.

### 2. SafetyNet HITL Framework
Every CRIMSON action (entity formation, capital raise > $500k, multi-sig wallet deployment) requires Houdinnie's explicit approval via Telegram before execution.

### 3. Green Button Protocol
The sovereign execution mechanism — a cryptographically signed Verified Command that triggers the manifest dispatch pipeline with multi-sig authorisation.

### 4. Zero-Knowledge Document Vault
Client-side AES-256-GCM encryption. The server never sees plain text. Documents are decrypted only inside TEE enclaves, and only after biometric re-verification.

### 5. Watchdog Agent
Independent security monitor running 10 detection rules. CRIMSON triggers halt all swarms and revoke all active keys.

---

## Import into Paperclip

1. Copy the `venturemind/` directory to your Paperclip companies folder
2. Update `COMPANY.md` with your credentials (email, secrets references)
3. Import into Paperclip: `paperclip company import venturemind/`
4. Review and approve the org chart in Paperclip UI
5. Set monthly budgets per agent
6. Configure Ghost Protocol secrets (Paperclip Secrets for dev, AWS KMS for prod)
7. Schedule the daily orchestrator check-in task

---

## Scheduled Tasks

| Task | Schedule | Purpose |
|------|----------|---------|
| `daily-orchestrator-check-in` | Daily 08:00 CST | Morning digest, escalation review |
| `weekly-performance-review` | Weekly Monday 09:00 CST | Agent accuracy report |
| `watchdog-system-health` | Hourly | System health verification |
| `safety-briefing-houdinnie` | Weekly Friday 10:00 CST | Week in review, next week preview |

---

## Dependencies

- **Paperclip** v2026.416.0+
- **Node.js** 20+ (for Paperclip CLI)
- **Telegram** connected (for alerts and Green Button approvals)

---

*VentureMind v1.0.0 | Built for Nomad Flow | Powered by Paperclip*