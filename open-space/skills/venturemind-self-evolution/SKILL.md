---
name: venturemind-self-evolution
description: VentureMind self-evolution skill — FIX, DERIVED, CAPTURED modes for swarm skill lifecycle management | VentureMind × OpenSpace
metadata:
  author: houdinnie.zo.computer
  version: 1.0.0
  compatibility: VentureMind Swarm of Swarms v1.0 + OpenSpace engine
allowed-tools: Bash, Read, Write, Grep, WebSearch
---

# VentureMind Self-Evolution — OpenSpace Skill

## Purpose
Governs the self-evolution lifecycle of all 10 VentureMind domain swarms using OpenSpace's three evolution modes: **FIX**, **DERIVED**, and **CAPTURED**.

## When Triggered
- Post-execution analysis after every swarm session
- Tool degradation detection (success rate drops below 85%)
- Metric monitor detects skill underperformance (fallback rate > 15%)
- New execution pattern succeeds 3+ consecutive times

## Evolution Modes

### 🔧 FIX — Repair Broken Skills
Triggered when a skill produces degraded output or tool call failure rate exceeds 10%.
- Analyzes root cause in recording logs
- Produces minimal targeted diff
- Validates before replacing predecessor
- Stores in version DAG with full lineage

### 🚀 DERIVED — Specialise from Parent
Triggered when a skill handles a novel sub-domain or edge case.
- Creates child skill from parent baseline
- Coexists with parent; does not replace
- E.g., Tax Strategist → UAE Corporate Tax DERIVED skill

### ✨ CAPTURED — Extract Novel Patterns
Triggered when a successful execution contains a reusable pattern with no parent skill.
- Brand new skill, no parent
- Captured from successful swarm execution
- Highest value: these are production-proven patterns

## VentureMind Skill Taxonomy (165 Skills Expected)

| Category | Count | Examples |
|----------|-------|----------|
| File Format I/O | 44 | PDF extraction fallbacks, DOCX parsing, Excel merged-cell, contract generation |
| Execution Recovery | 29 | Layered fallback chains, sandbox recovery, error-chain resolution |
| Document Generation | 26 | End-to-end doc pipeline, compliance forms, legal documents |
| Quality Assurance | 23 | Post-write verification, KYC validation, entity formation checks |
| Task Orchestration | 17 | Multi-swarm tracking, sovereign execution pipeline, ZIP packaging |
| Domain Workflow | 13 | Entity formation, tax filing, bank account setup, crypto onboarding |
| Web & Research | 11 | Treaty research, jurisdiction analysis, compliance monitoring |

## Quality Monitoring Stack

### Skill Level
- `applied_rate`: how often skill was selected vs skipped
- `completion_rate`: task completed successfully using skill
- `effective_rate`: (completion_rate × quality_score) / tokens_used
- `fallback_rate`: how often skill fell back to parent or generic handler

### Tool Call Level
- `success_rate`: tool returned valid result
- `latency_p50`, `latency_p95`: performance buckets
- `flagged_issues`: injected failures, sandbox errors, prompt injection attempts

### Code Execution Level
- `execution_status`: SUCCESS / PARTIAL / FAILED
- `error_patterns`: categorised error fingerprints

## Cascade Evolution Rule
When any tool degrades, ALL skills that depend on that tool are batch-evolved simultaneously. A degraded tool in the Capital Swarm (e.g., brokerage API) cascades to update all dependent skills across Capital, Financial, and Engineering swarms.

## Safety Guardrails
1. Confirmation gate halts evolution if confidence < 0.80
2. Anti-loop guard: max 3 evolutions per skill per hour
3. Dangerous pattern detection: prompt injection signatures, credential exfiltration markers
4. Validation gate: evolved skill must pass 3-test suite before activation

## Integration with VentureMind SafetyNet
- Every evolved skill is audit-logged to AgentDecisionLog
- HITL triggers pause evolution if confidence < 0.70
- Compliance-critical skills (KYC, AML, tax) require human sign-off before activation