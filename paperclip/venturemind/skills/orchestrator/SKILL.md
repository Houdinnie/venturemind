---
name: orchestrator
description: VentureMind orchestration skill — coordinates 10 domain swarms, runs 4 parallel verification loops, resolves inter-swarm conflicts, and produces the FounderBlueprint with confidence scores.
metadata:
  author: houdinnie.zo.computer
  version: 1.0
---

# Orchestrator Skill

The Orchestrator receives the FounderProfile and coordinates all 10 domain swarms to produce a unified, audit-ready FounderBlueprint.

## 4 Parallel Verification Loops

These run simultaneously and must all complete before synthesis:

### 1. Global Intelligence Loop
- **Owner**: Compliance Agent (Legal Swarm)
- **Scope**: Regulatory landscape for target jurisdictions
- **Outputs**: List of applicable treaties, regulatory deadlines, compliance gaps
- **Primary sources**: Government portals, official gazettes, treaty texts

### 2. Citation & Source Loop
- **Owner**: Strategy Agent (Central Swarm)
- **Scope**: All factual claims must cite primary sources
- **Enforcement**: No output accepted without at least one primary source citation
- **Sources**: Tax codes, treaty texts, legal precedents, official government documents

### 3. Competitive Moat Loop
- **Owner**: Capital Swarm Lead
- **Scope**: Market analysis, competitor positioning, TAM validation
- **Outputs**: Market map, competitive differentiation, risk factors
- **Primary sources**: Crunchbase, PitchBook, industry reports, primary interviews

### 4. Compliance Cross-Check Loop
- **Owner**: Legal Swarm Lead + Financial Swarm Lead (joint)
- **Scope**: Tax and legal positions must be consistent
- **Checks**: Tax treaty vs. legal entity structure consistency, regulatory filing deadlines alignment

## Conflict Resolution

When two swarms disagree:
1. Present both positions with primary source citations
2. Identify the factual basis of each position
3. Escalate to Central Swarm Lead with both positions
4. Central Swarm Lead routes to Houdinnie if unresolvable

## Confidence Scoring

| Score | Meaning | Action |
|-------|---------|--------|
| 0.90 - 1.00 | High confidence, well-sourced | Approved |
| 0.70 - 0.89 | Moderate confidence | Approved with source note |
| 0.50 - 0.69 | Low confidence | Needs human review |
| < 0.50 | Very low confidence | Escalate immediately |

## FounderBlueprint Output Schema

```yaml
FounderBlueprint:
  version: "1.0"
  createdAt: ISO8601
  founderId: UUID
  confidenceOverall: float
  sections:
    - id: string
      name: string
      confidence: float
      status: "approved" | "needs_review" | "escalated"
      primarySources: []
      warnings: []
      humanReviewTriggered: boolean
      output: string (markdown)
  executionManifests:
    - manifestId: string
      type: string
      riskLevel: "AMBER" | "RED" | "CRIMSON"
      requiredApprovals: string[]
      status: "pending" | "approved" | "rejected"
```