---
name: green-button
description: VentureMind Green Button execution skill — cryptographically signed execution manifests, multi-sig authorisation workflow, SafetyNet HITL enforcement, and manifest dispatch to sub-agents.
metadata:
  author: houdinnie.zo.computer
  version: 1.0
---

# Green Button Execution Skill

The Green Button is VentureMind's sovereign execution protocol — the moment the AI moves from "advise" to "execute." Every execution must be cryptographically signed, pass SafetyNet triggers, and receive explicit authorisation.

## Execution Flow

```
FounderBlueprint (approved)
    ↓
Orchestrator generates JSON Manifest
    ↓
Central Swarm Lead reviews risk level
    ↓
┌─────────────────────────────────────────┐
│  CRIMSON → Telegram alert → wait        │
│  RED → Hardware key verification        │
│  AMBER → Log and proceed                │
└─────────────────────────────────────────┘
    ↓
User issues Verified Command (cryptographically signed)
    ↓
Manifest dispatched to sub-agents
    ↓
Sub-agents execute with audit logging
    ↓
Completion reported to Central Swarm Lead
    ↓
Telegram notification to Houdinnie
```

## Verified Command Schema

```json
{
  "commandId": "uuid-v7",
  "type": "EXECUTE_PLAN_V1",
  "manifestId": "EXEC-ENTITY-FORMATION-V1",
  "founderId": "uuid",
  "actions": ["Formation(WY_LLC)", "Banking(Mercury)", "Wallet(Safe_Mainnet)"],
  "riskLevel": "CRIMSON",
  "signedAt": "ISO8601",
  "signature": "HMAC-SHA256(payload, userSessionKey)",
  "ttlSeconds": 300
}
```

## Manifest Types

| Manifest | Risk | Required Approvals |
|----------|------|-------------------|
| EXEC-ENTITY-FORMATION | CRIMSON | Green Button + KYC Tier 2+ |
| EXEC-NEOBANK-FIAT | RED | Green Button + Hardware Key |
| EXEC-SOVEREIGN-ACCOUNT | CRIMSON | Green Button + Multi-sig |
| EXEC-TAX-STRUCTURE | RED | Green Button + Human Review |
| EXEC-CAPITAL-RAISE | CRIMSON | Green Button + KYC Tier 3+ |

## Multi-Sig Authorization

For CRIMSON manifests:
1. Houdinnie approves via Telegram (+1 signature)
2. Hardware key presented (+1 signature)
3. Central Swarm Lead confirms (+1 signature)
4. All 3 signatures collected → manifest dispatched

## Post-Execution Audit

Every execution manifest logs:
- `manifestId`: Unique execution identifier
- `commandId`: Reference to the Verified Command
- `actionsExecuted`: Array of actions taken
- `completionTime`: ISO 8601
- `errors`: Any failures or partial completions
- `nextSteps`: Follow-up actions required