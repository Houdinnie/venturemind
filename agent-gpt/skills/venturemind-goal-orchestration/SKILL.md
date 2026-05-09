---
name: venturemind-goal-orchestration
description: VentureMind autonomous goal orchestration — decompose founder goals into multi-swarm task graphs, delegate to domain leads, verify outputs, and escalate via HITL. Built on AgentGPT architecture with LangChain, FastAPI, and Next.js 13.
metadata:
  sources:
    - kind: github-repo
      repo: reworkd/AgentGPT
      attribution: reworkd
      license: GPL-3.0
      usage: architecture-reference
version: 1.0.0
---

# VentureMind Goal Orchestration Skill

## Usage

Invoke when a founder states a business goal, lifestyle objective, legal need, or financial target. This skill decomposes the goal into executable tasks across 10 domain swarms, delegates with full context, tracks progress, and manages HITL escalation.

## Goal Decomposition Process

### Step 1: Parse Intent

Extract from the founder's message:

- **Objective type**: legal entity, financial optimization, capital raise, lifestyle setup, product build, growth
- **Jurisdictions**: which countries/cities are mentioned or implied
- **Timeline**: when does the founder want this achieved?
- **Budget**: any financial constraints?
- **Risk tolerance**: conservative, balanced, aggressive?

### Step 2: Map to Swarms

Route to relevant domain leads based on keywords:

| Keyword Pattern | Swarm |
|-----------------|-------|
| company, LLC, IBC, trademark, contract, legal | legal-swarm-lead |
| tax, invoice, accounting, receipt, bookkeeping | financial-swarm-lead |
| invest, raise, equity, investor, valuation, cap table | capital-swarm-lead |
| residency, visa, bank, flight, relocation, travel | mobility-swarm-lead |
| marketing, SEO, customer, growth, partnership | growth-swarm-lead |
| crypto, DeFi, wallet, exchange, token, blockchain | web3-swarm-lead |
| portfolio, wealth, invest, rebalance, financial plan | wealth-swarm-lead |
| build, code, deploy, infrastructure, product | engineering-swarm-lead |
| lifestyle, health, school, experience, travel | journey-swarm-lead |
| compliance, KYC, AML, audit, risk | compliance-auditor |

### Step 3: Generate Task Graph

Create ordered tasks with:

- `task_id`: Unique identifier (vm-task-YYYYMMDD-NNN)
- `description`: Plain-English task description
- `owner`: Domain swarm lead
- `dependencies`: Task IDs that must complete first
- `deadline`: ISO 8601 timestamp
- `confidence_threshold`: Minimum confidence score to proceed without HITL
- `hitl_required`: Boolean
- `escalate_to`: Who to notify if HITL fires

### Step 4: Run Ingestion Pipeline

For each task above confidence threshold:
1. Delegate to domain lead with full context
2. Track via heartbeat every 15 minutes
3. Verify output with Feynman verification
4. Log to AgentDecisionLog with evidence

Below confidence threshold:
1. Trigger Strategist Agent deep discovery
2. Fill information gaps
3. Recalculate confidence
4. Proceed or escalate

### Step 5: HITL Management

When HITL trigger fires:
1. Pause task execution
2. Notify founder via Telegram with task details, risk context, and approval buttons
3. Wait for approval or modification
4. Resume or cancel based on response
5. Log decision to AgentDecisionLog

---

## Swarm Communication

Send directives using SWARM_DIRECTIVE message format. Expect SWARM_RESPONSE within 5 minutes. If no response:
- Retry once with context reminder
- If still no response, escalate to Watchdog Agent for health check
- Mark task BLOCKED and notify founder

---

## Safety Constraints

- Never delegate raw credentials to any swarm — use Infisical Agent Vault
- Never execute legal tasks above threshold without human approval
- Never send financial transaction instructions without Compliance Auditor sign-off
- Always log every decision with timestamp, agent ID, model, confidence score, and evidence
- If Protocol Zero fires, pause all execution immediately

---

## Output Format

Return a structured task graph:

```markdown
## Goal Decomposition: [Goal Statement]

**Overall Confidence**: X.X/1.0

### Task Graph

| # | Task | Owner | Deadline | Confidence | HITL |
|---|------|-------|----------|------------|------|
| 1 | [Task description] | [Swarm] | [Date] | X.X | Yes/No |
| 2 | ... | ... | ... | ... | ... |

### Swarm Assignments

- **legal-swarm-lead**: Tasks #1, #5, #8
- **financial-swarm-lead**: Tasks #2, #3
...

### HITL Tasks Requiring Approval

- Task #5: [Explain why approval needed]
- Task #9: [Explain why approval needed]

### Dependencies

```
task1 → [task3, task4]
task2 → [task5]
...
```

### Next Action

[What happens next - either proceed or wait for human approval]
```