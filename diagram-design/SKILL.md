---
name: nomad-flow-diagrams
description: VentureMind diagram-design skill — 14 editorial diagram types for Nomad Flow branding. Self-contained HTML+SVG. No shadows, no Mermaid-slop. | MIT
metadata:
  author: VentureMind × cathrynlavery
  homepage: https://houdinnie.zo.space
  repo: github.com/cathrynlavery/diagram-design
  brand: Nomad Flow | VentureMind
  tokens: references/style-guide.md
  version: 1.0.0
---

# VentureMind Diagram Design Skill

> *"Every node earns its place."*

14 editorial diagram types, VentureMind-branded. Brand tokens live in `references/style-guide.md` — update it once, all 14 diagrams inherit automatically.

---

## Brand Tokens

| Role | Value | Hex |
|---|---|---|
| `paper` | Deep navy background | `#0c0e1a` |
| `paper-2` | Card surface | `#13162d` |
| `ink` | Warm white | `#f1f5f9` |
| `muted` | Slate caption | `#64748b` |
| `accent` | Indigo | `#6366f1` |
| `highlight` | Amber | `#f59e0b` |

**Fonts**: Instrument Serif (titles), Geist (node names), Geist Mono (technical sublabels)

---

## The 14 Diagram Types

### 1. Architecture — System topology
Use for: microservices, platform layers, data pipelines

```
VentureMind Swarm Architecture:
  [User Interface] → [VentureMind Central Swarm] → [10 Domain Swarms]
                                               ↓
                        [Security Vault] [Agent Vault] [Paperclip Runtime]
```

**When to use**: Platform topology, agent communication topology, data flows
**When not to**: simple lists, single-process flows → use a paragraph instead

---

### 2. Flowchart — Decision logic
Use for: KYC decision trees, onboarding branching, compliance escalation

```
Founder Onboarding Flow:
  [Signup] → [Tier 0 Free Trial]
            → [KYC Check]
              → Score ≥ 0.80 → [Tier 3 Enterprise]
              → Score 0.60–0.79 → [Tier 2 Professional]
              → Score < 0.60 → [Human Review / Escalation]
            → [Compliance Audit] → [Approved / Rejected]
```

**When to use**: multi-branch decision logic, compliance workflows, process gates
**When not to**: linear sequences → use a timeline instead

---

### 3. Sequence — Events over time
Use for: OAuth handshake, API auth flows, onboarding steps, transaction lifecycle

```
Sovereign Account Creation Sequence:
  [Founder]    →  [Nomad Flow UI]
                →  [Central Swarm]
                         →  [Legal Swarm: Entity Check]
                         ←  [Entity Status: CLEAR]
                         →  [Capital Swarm: NeoBank API]
                         ←  [Account Provisioned]
                         →  [Security Vault: Store Keys]
                         ←  [Vault Receipt: ENCRYPTED]
                         →  [Founder] ← [Account Ready]
```

**When to use**: timed message exchanges, API auth, lifecycle events
**When not to**: parallel independent processes → use architecture instead

---

### 4. State Machine — Entity states + transitions
Use for: entity lifecycle, compliance status, subscription tiers, fund states

```
Entity State Machine:
  [DORMANT] → [INCORPORATING] → [ACTIVE]
             ← ← ← ← ← ← ← ← ← ←
             ↓
  [DISSOLVED] ← [ADMINISTRATIVE HOLD] ← [COMPLIANCE_FLAG]

  Triggers:
    DORMANT + Founding Docs → INCORPORATING
    INCORPORATING + Registry Approval → ACTIVE
    ACTIVE + Annual Filing Missed → ADMINISTRATIVE HOLD
    ADMINISTRATIVE HOLD + Remediation → ACTIVE
    ACTIVE + Court Order → DISSOLVED
```

**When to use**: entity lifecycle, status transitions, subscription tier changes
**When not to**: concurrent parallel processes → use swimlane instead

---

### 5. ER / Data Model — Entities + fields
Use for: data models, schema design, entity relationships

```
VentureMind Core Data Model:
  ┌─────────────┐       ┌──────────────┐       ┌─────────────────┐
  │   FOUNDER   │       │    ENTITY    │       │  SWARM_SESSION  │
  ├─────────────┤       ├──────────────┤       ├─────────────────┤
  │ id          │──┐    │ id           │       │ id              │
  │ kyc_tier    │  │    │ founder_id   │←──┐   │ founder_id      │←─┐
  │ risk_score  │  └───→│ jurisdiction │   │   │ swarm_domain    │  │
  │ trust_level │       │ status       │   │   │ confidence_score│  │
  │ nacl_key    │       │ incorporation │   │   │ hitl_triggered  │  │
  └─────────────┘       └──────────────┘   │   └─────────────────┘  │
                                           │          │             │
                     ┌────────────────────┘          │             │
                     │                               ↓             │
              ┌──────────────┐              ┌─────────────────┐   │
              │ TRANSACTION  │              │   EXECUTION_LOG  │   │
              ├──────────────┤              ├─────────────────┤   │
              │ id           │              │ id               │   │
              │ entity_id    │←─────────────│ session_id       │←─┘
              │ amount_usd   │              │ action           │
              │ counterparty │              │ approved         │
              │ aml_status   │              │ sovereign_key_used│
              └──────────────┘              └─────────────────┘
```

**When to use**: data model design, schema documentation, entity relationships
**When not to**: business process description → use flowchart or swimlane

---

### 6. Timeline — Events on an axis
Use for: founding journey, compliance milestones, capital raise stages

```
Nomad Flow Founder Journey — 90 Day Timeline:
  Q1  |— Feb —|— Mar —|
  Day 1        Day 30     Day 60        Day 90

  ● Onboarding  ● Entity      ● NeoBank    ● Capital
    Complete       Formation    Account       Raise
                ● KYC Tier 2  Provisioned  Prepared
                ● Tax ID      ● Vault       ● Investor
                             Setup         Deck
  [Strategy Agent]  [Legal Swarm]  [Mobility]  [Capital Swarm]
```

**When to use**: chronological journey, milestone tracking, project phases
**When not to**: decision branches → use flowchart instead

---

### 7. Swimlane — Cross-functional process flow
Use for: compliance audit, multi-swarm execution, Handoff protocols

```
Sovereign Account Execution — Swimlane:
  ┌─────────────────┬─────────────────┬─────────────────┬─────────────────┐
  │   Founder       │  Central Swarm   │  Legal Swarm     │ Capital Swarm   │
  ├─────────────────┼─────────────────┼─────────────────┼─────────────────┤
  │                 │                 │                 │                 │
  │  [Intent: Open  │                 │                 │                 │
  │   Sovereign    │                 │                 │                 │
  │   Account]     │                 │                 │                 │
  │                 │ → Create Session│                 │                 │
  │                 │                 │ → Validate Docs │                 │
  │                 │ ← Entity Clear  │                 │                 │
  │                 │                 │                 │ → NeoBank API   │
  │                 │                 │                 │ ← Account Ready │
  │                 │ → Vault Store   │                 │                 │
  │                 │                 │                 │                 │
  │  ← Account     │                 │                 │                 │
  │   Delivered    │                 │                 │                 │
  └─────────────────┴─────────────────┴─────────────────┴─────────────────┘
```

**When to use**: multi-agent handoff, compliance audit, cross-swarm coordination
**When not to**: simple linear flow → use timeline instead

---

### 8. Quadrant — Two-axis positioning
Use for: opportunity assessment, jurisdiction comparison, risk/effort matrix

```
Jurisdiction Opportunity Matrix:
                  HIGH TAX OPTIMISATION
                    │
                    │    [Dubai IBC]
                    │         [Wyoming LLC]
   LOW             │                   [Cayman CIMA]
EFFORT             │  [Estonia e-Residency]
                    │         [Portugal D8]
                    │              [Singapore EP]
                    │
  ────────────────────────────────────────────────────────
                  LOW              HIGH
                        INFRASTRUCTURE QUALITY

  Legend:
  • Circle = entity type
  • Position = tax optimisation vs infrastructure quality
  • Size = founder demand volume
```

| Use for | When not to |
|---|---|
| Opportunity matrix | Single-item positioning → just state it |
| Jurisdiction comparison | Multi-variable scoring → use table instead |
| Risk vs effort decisions | Complex scoring → use weighted model |

---

### 9. Nested — Hierarchy by containment
Use for: organisational structure, platform layers, service hierarchy

```
VentureMind Platform Hierarchy:
  ┌─────────────────────────────────────────────────────────────┐
  │  NOMAD FLOW PLATFORM                                        │
  │                                                             │
  │  ┌───────────────────────────────────────────────────────┐  │
  │  │  VENTUREMIND AI ENGINE                                │  │
  │  │                                                       │  │
  │  │  ┌──────────┐ ┌──────────┐ ┌──────────┐             │  │
  │  │  │ Strategy │ │ Operator │ │ Talent   │  ← Core Agents│  │
  │  │  │  Agent   │ │  Agent   │ │  Agent   │              │  │
  │  │  └──────────┘ └──────────┘ └──────────┘             │  │
  │  │                                                       │  │
  │  │  ┌──────────────────────────────────────────────┐    │  │
  │  │  │  10 DOMAIN SWARMS                            │    │  │
  │  │  │  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ │    │  │
  │  │  │  │ Lgl│ │ Fin│ │ Cap│ │ Grw│ │ Web│ │ Wlth│ │    │  │
  │  │  │  └────┘ └────┘ └────┘ └────┘ └────┘ └────┘ │    │  │
  │  │  │  ┌────┐ ┌────┐ ┌────┐ ┌────┐             │    │  │
  │  │  │  │ Mb│ │ Jrny│ │Eng │ │ Sec │             │    │  │
  │  │  │  └────┘ └────┘ └────┘ └────┘             │    │  │
  │  │  └──────────────────────────────────────────────┘    │  │
  │  └───────────────────────────────────────────────────────┘  │
  │                                                             │
  │  ┌──────────────┐  ┌──────────┐  ┌────────────────────┐    │
  │  │ SECURITY VAULT│  │Paperclip │  │ OpenFang AOS       │    │
  │  │ (Infisical)   │  │Runtime   │  │ (Autonomous Hands) │    │
  │  └──────────────┘  └──────────┘  └────────────────────────┘  │
  └─────────────────────────────────────────────────────────────┘
```

**When to use**: organisational hierarchy, platform layers, containment relationships
**When not to**: process flow → use flowchart instead

---

### 10. Tree — Parent → children
Use for: skill taxonomy, agent hierarchy, tool tree

```
VentureMind Agent Hierarchy:
                        ┌─────────────────────┐
                        │  CENTRAL SWARM LEAD │
                        └──────────┬──────────┘
                                   │
               ┌───────────────────┼───────────────────┐
               │                   │                   │
              [Strategy]        [Operator]          [Talent]
               Agent             Agent               Agent
               │                   │                   │
    ┌──────────┼──────────┐        │          ┌───────┴───────┐
    │          │          │        │          │               │
[Tax      [Entity    [Compliance [Execution [Capital   [Growth
Strategist] Lawyer]   Auditor]   Handler]  Raise]     Acquisition]
    │          │          │
    │          │          │
[9-Juris  [UAE/Dubai [AML Monitor
Tax Calc] IBC Form]  STRIDE Check]
```

**When to use**: skill taxonomy, agent inheritance, tool hierarchy
**When not to**: process flow with timing → use sequence instead

---

### 11. Layers — Stacked abstractions
Use for: technology stack, compliance layers, security model

```
VentureMind Security Stack:
  ┌─────────────────────────────────────────────────────────────┐
  │  USER INTERFACE                                             │
  │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐   │
  │  │ Nomad Flow    │  │  Dashboard    │  │  Telegram     │   │
  │  │ Web App       │  │  (Analytics)  │  │  Bot Interface│   │
  │  └───────┬───────┘  └───────┬───────┘  └───────┬───────┘   │
  ├──────────┼──────────────────┼──────────────────┼──────────┤
  │  AGENT   │  Central Swarm   │  Domain Swarms    │  Watchdog │
  │  RUNTIME │  Orchestrator    │  (10 Swarms)      │  Agent    │
  ├──────────┼──────────────────┼──────────────────┼──────────┤
  │  ORCHEST │  LangGraph       │  Qdrant           │  Redis    │
  │  STRATOR │  State Machine   │  Vector Store     │  Queue    │
  ├──────────┼──────────────────┼──────────────────┼──────────┤
  │  SECURE  │  Infisical       │  Paperclip        │  OpenFang │
  │  LAYER   │  Agent Vault     │  Company Package   │  AOS      │
  │          │  (AES-256-GCM)   │  (MIT)            │  (Rust)   │
  ├──────────┼──────────────────┼──────────────────┼──────────┤
  │  INFRA   │  FastAPI          │  PostgreSQL       │  Docker   │
  │          │  (Python)         │  (Supabase)       │  (Linux)  │
  └──────────┴──────────────────┴──────────────────┴──────────┘
```

**When to use**: technology stack, security layers, abstraction hierarchy
**When not to**: organisational hierarchy → use nested instead

---

### 12. Venn — Set overlap
Use for: founder requirements, capability intersection, compliance overlap

```
Founder Requirements — Venn Analysis:
  ┌──────────────────────────────────────────────────────────┐
  │                                                          │
  │         ┌─────────────────┐    ┌─────────────────┐      │
  │         │   LEGAL STRUCTURE│    │   TAX           │      │
  │         │                 │    │   OPTIMISATION   │      │
  │         │  • UAE/Dubai IBC│    │                 │      │
  │         │  • Wyoming LLC  │    │  • 0% CIT       │      │
  │         │  • BVI offshore │    │  • No capital   │      │
  │         │                 │    │    gains tax    │      │
  │         └────────┬────────┘    └────────┬────────┘      │
  │                  │         │            │                │
  │                  │    ┌────┴────────────┴────┐          │
  │                  │    │   SOVEREIGN SWEET   │          │
  │                  │    │   SPOT — 0% CIT     │          │
  │                  │    │   + Zero capital    │          │
  │                  │    │   gains + UAE       │          │
  │                  │    │   residency         │          │
  │                  │    └─────────────────────┘          │
  │                  │                                     │
  │         ┌────────┴────────┐                            │
  │         │  BANKING ACCESS │                            │
  │         │                 │                            │
  │         │  • NeoBank USD  │                            │
  │         │  • Stablecoin   │                            │
  │         │    rails        │                            │
  │         │  • Multi-currency│                            │
  │         └─────────────────┘                            │
  │                                                           │
  └───────────────────────────────────────────────────────────┘
```

**When to use**: requirement overlap, capability intersection
**When not to**: exclusive categories → use nested instead

---

### 13. Pyramid — Ranked hierarchy or funnel
Use for: Maslow hierarchy of founder needs, priority tiers, service tiers

```
Founder Needs — Pyramid:
                          ▲
                         /│\
                        / │ \
                       /  │  \
                      /   │   \
                     /    │    \
                    /═════╪═════\
                   /      │      \
                  /   SOVEREIGNTY  \
                 /─────────────────\
                /  EXECUTION POWER  \
               /─────────────────────\
              /  LEGAL STRUCTURE      \
             /─────────────────────────\
            /    TAX OPTIMISATION       \
           /─────────────────────────────\
          /    BANKING INFRASTRUCTURE     \
         /────────────────────────────────\
        /      MOBILITY & RESIDENCY         \
       /────────────────────────────────────\
      /         NETWORK ACCESS               \
     /────────────────────────────────────────\
    │         INFORMATION & EDUCATION          │
    ▼                                          ▼

  Founder Journey: Education → Network → Mobility → Banking → Tax → Legal → Execution → Sovereignty
```

**When to use**: ranked priority, Maslow-style hierarchy, conversion funnel
**When not to**: equal-weight categories → use nested instead

---

### 14. Consultant 2×2 — Scenario matrix with named cells
Use for: market entry strategy, jurisdictional risk assessment, fund allocation

```
Jurisdiction Strategy — 2×2 Scenario Matrix:
                        HIGH COMPLIANCE COMPLEXITY
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        │  [VALIDATE]         │  [PRIORITISE]       │
        │                     │                     │
   LOW  │  Singapore EP       │  UAE/Dubai IBC      │
COMPLI │  • High infra        │  • Low CIT          │
ANCE   │  • Clear process     │  • Strong legal     │
RISK   │  • 6–8 week wait     │  • 4–6 week setup   │
        │  → Pilot here first  │  → Deploy in Q2     │
        │                     │                     │
        │─────────────────────│─────────────────────│
        │                     │                     │
   HIGH │  [DEFER]            │  [AVOID]             │
COMPLI │                     │                     │
ANCE   │  Cayman CIMA        │  San Marino         │
RISK   │  • Heavy admin       │  • High fees        │
        │  • Regulatory risk  │  • Limited banking  │
        │  → Re-evaluate 2027 │  → Reputational    │
        │                     │    risk too high    │
        │                     │                     │
        └─────────────────────┴─────────────────────┘
                              │
                    HIGH MARKET ENTRACTION POTENTIAL

  Labels: Validate / Prioritise / Defer / Avoid
  Axes: Compliance Complexity (x) / Market Opportunity (y)
```

**When to use**: strategic scenario planning, named decisions per quadrant
**When not to**: numeric scoring → use quadrant or table instead

---

## Annotations — Primitive

Italic Instrument Serif callout with dashed Bézier leader line.

```html
<svg>
  <path d="M 120,80 C 160,80 180,120 200,140"
        stroke="var(--accent)" stroke-width="1" stroke-dasharray="4,3" fill="none"/>
  <text x="80" y="76" font-family="Instrument Serif" font-style="italic" font-size="11"
        fill="var(--muted)">
    "Credentials never touch the agent."
  </text>
</svg>
```

---

## Primitive — Sketchy Filter

Hand-drawn SVG filter variant for essays. Not for technical docs.

```html
<svg>
  <filter id="sketchy">
    <feTurbulence type="turbulence" baseFrequency="0.03" numOctaves="3" seed="2" result="noise"/>
    <feDisplacementMap in="SourceGraphic" in2="noise" scale="2" xChannelSelector="R" yChannelSelector="G"/>
  </filter>
  <g filter="url(#sketchy)">
    <!-- diagram content -->
  </g>
</svg>
```

---

## Diagrams Gallery

Open [`assets/index.html`](assets/index.html) in a browser to browse all 14 diagrams in minimal light / minimal dark / full-editorial variants.

---

## When Not to Use Diagrams

| Use Instead | Reason |
|---|---|
| Lists | Unicode wire diagrams for tweets, terminal output → wiretext skill |
| Tables | Before/after comparisons, multi-item scoring |
| Paragraph | One-shape "diagrams" → just write the sentence |
| Timeline | Linear process without branching → timeline not flowchart |

---

## Installation

```bash
# Clone VentureMind's diagram-design package
git clone https://github.com/Houdinnie/venturemind
ln -s venturemind/diagram-design/skills/nomad-flow-diagrams ~/.claude/skills/diagram-design
# Restart Claude Code
```

---

*Built on [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) · MIT · Updated 2026-05-09*