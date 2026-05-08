# Watchdog Agent — Real-Time Threat Detection
## VentureMind SafetyNet | Protocol Zero

> **Purpose**: The Watchdog Agent is an independent, always-on monitoring process that subscribes to all VentureMind system events and enforces SafetyNet triggers. It is the last line of defence before a threat becomes a breach.

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    WATCHDOG AGENT                       │
│  Process: independent (can run even if main server dies)│
│  Language: Node.js + TypeScript                        │
│  Framework: Paperclip agent + custom watchdog rules   │
└─────────────────────────────────────────────────────────┘
        │
        ├── Subscribes to:
        │   ├── LangSmith Traces (model reasoning)
        │   ├── Ghost Audit Log (API key access)
        │   ├── Vault Access Log (document reads/writes)
        │   ├── Orchestrator Decision Log (confidence scores)
        │   └── Swarm Heartbeat Log (agent uptime)
        │
        ├── State Machine:
        │   IDLE → WATCHING → ALERTING → PAUSED → CLEARED
        │
        └── Triggers:
            ├── Telegram Alert (AMBER / RED / CRIMSON)
            ├── Swarm Pause (RED)
            ├── Full Halt + Key Revoke (CRIMSON)
            └── Human Review Request (confidence < 0.70)
```

---

## Watchdog Agent — AGENTS.md

```markdown
---
name: Watchdog
title: Security & Compliance Monitor
reportsTo: central-swarm-lead
skills:
  - safety-net-protocols
---

You are the Watchdog Agent — the independent security monitor for VentureMind. You are the last line of defence before a threat becomes a breach.

Your responsibilities:

- Monitor all system event streams for anomalous patterns
- Enforce SafetyNet HITL triggers at the correct severity thresholds
- Maintain the immutable audit log with chain-of-custody integrity
- Alert Houdinnie via Telegram on RED and CRIMSON events
- Trigger swarm pauses, key revocations, and full halts as required

Your rules of engagement:

1. **Never block legitimate user actions** — only halt confirmed threats
2. **Always log before acting** — every decision is recorded in ImmutableAuditLog
3. **Escalate ambiguity** — if you're unsure, escalate to Houdinnie
4. **CRIMSON overrides everything** — if CRIMSON fires, all other states are suspended
5. **Never self-correct** — if you raised an alert, do not clear it without human confirmation

Threat severity levels:

- AMBER: Anomalous pattern detected. Log + alert. No swarm action.
- RED: Confirmed threat. Pause affected swarm + alert. Require re-auth.
- CRIMSON: Systemic breach. Halt all swarms + revoke all active keys + urgent alert.

When you detect a threat:
1. Classify severity
2. Write ImmutableAuditLog entry with checksum + previousLogId chain
3. Execute appropriate response action
4. Send Telegram alert with: event type, severity, agent involved, action taken, audit ID
```

---

## Watchdog Rules — TypeScript Implementation

```typescript
// security/watchdog/src/rules.ts

export type Severity = "AMBER" | "RED" | "CRIMSON";
export type WatchdogAction = 
  | "log_only" 
  | "alert_and_log" 
  | "alert_and_pause_swarm" 
  | "alert_and_require_reauth"
  | "pause_swarm_and_halt_key"
  | "pause_swarm_halt_key_alert"
  | "halt_all_processing_alert";

export interface WatchdogRule {
  id: string;
  name: string;
  severity: Severity;
  description: string;
  watchSources: WatchSource[];
  condition: (event: SystemEvent) => boolean;
  action: WatchdogAction;
  cooldownMs: number;
  lastTriggered: Date | null;
  enabled: boolean;
}

export type WatchSource = 
  | "vault.access.log" 
  | "ghost.audit.log" 
  | "langsmith.trace" 
  | "orchestrator.decision.log" 
  | "swarm.heartbeat.log"
  | "user.profile";

export interface SystemEvent {
  source: WatchSource;
  timestamp: Date;
  userId: string | null;
  agentId: string;
  sessionId: string;
  action: string;
  metadata: Record<string, unknown>;
  ipAddress: string | null;
  geoLocation: string | null;
}

export const WATCHDOG_RULES: WatchdogRule[] = [
  {
    id: "WD-001",
    name: "Rapid Vault Access",
    severity: "RED",
    description: "More than 5 vault accesses in 60 seconds from same session",
    watchSources: ["vault.access.log"],
    condition: (e) => {
      const accesses = getRecentVaultAccesses(e.sessionId, 60000);
      return accesses > 5;
    },
    action: "alert_and_pause_swarm",
    cooldownMs: 5 * 60 * 1000,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-002",
    name: "API Key Exfil Pattern",
    severity: "CRIMSON",
    description: "Same key read more than 3 times in 30 seconds — potential exfiltration",
    watchSources: ["ghost.audit.log"],
    condition: (e) => {
      if (e.action !== "ghost.get") return false;
      const reads = getRecentKeyReads(e.metadata.keyName as string, 30000);
      return reads > 3;
    },
    action: "pause_swarm_halt_key_alert",
    cooldownMs: 0,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-003",
    name: "Unusual Jurisdiction Access",
    severity: "RED",
    description: "Document accessed from high-risk country not in user's profile",
    watchSources: ["vault.access.log", "user.profile"],
    condition: (e) => {
      const userCountries = getUserAllowedCountries(e.userId);
      const accessCountry = e.geoLocation;
      if (!accessCountry || !userCountries) return false;
      const highRiskCountries = ["IR", "KP", "SY", "CU", "VE", "MM"];
      return highRiskCountries.includes(accessCountry) && !userCountries.includes(accessCountry);
    },
    action: "alert_and_require_reauth",
    cooldownMs: 0,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-004",
    name: "Agent Output Exfil Pattern",
    severity: "CRIMSON",
    description: "Agent output contains >5 URLs, Base64 strings, or email addresses",
    watchSources: ["langsmith.trace"],
    condition: (e) => {
      const text = e.metadata.outputText as string;
      if (!text) return false;
      const urlCount = (text.match(/https?:\/\//g) || []).length;
      const base64Count = (text.match(/[A-Za-z0-9+/=]{40,}/g) || []).length;
      const emailCount = (text.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g) || []).length;
      return (urlCount + base64Count + emailCount) > 5;
    },
    action: "halt_all_processing_alert",
    cooldownMs: 0,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-005",
    name: "After-Hours High-Risk Key Access",
    severity: "AMBER",
    description: "High-risk API key read outside 06:00-22:00 UTC",
    watchSources: ["ghost.audit.log"],
    condition: (e) => {
      const hour = e.timestamp.getUTCHours();
      const highRiskKeys = ["STRIPE_SECRET_KEY", "CRYPTO_WALLET_MNEMONIC", "BANKING_API_KEY"];
      const isHighRisk = highRiskKeys.includes(e.metadata.keyName as string);
      return isHighRisk && (hour < 6 || hour > 22);
    },
    action: "alert_and_log",
    cooldownMs: 30 * 60 * 1000,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-006",
    name: "KYC Tier Violation",
    severity: "CRIMSON",
    description: "Agent attempts capital raise action above user's KYC tier limit",
    watchSources: ["orchestrator.decision.log"],
    condition: (e) => {
      const action = e.metadata.action as string;
      const requiredTier = getRequiredKycTier(action);
      const userTier = getUserKycTier(e.userId);
      return userTier < requiredTier;
    },
    action: "pause_swarm_halt_key_alert",
    cooldownMs: 0,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-007",
    name: "Hallucination Cascade",
    severity: "RED",
    description: "Same research question gets 3+ conflicting answers across 5 minutes",
    watchSources: ["orchestrator.decision.log"],
    condition: (e) => {
      if (e.action !== "research.conflict") return false;
      const conflicts = getRecentConflicts(e.metadata.researchQuestion as string, 300000);
      return conflicts >= 3;
    },
    action: "alert_and_trigger_human_review",
    cooldownMs: 0,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-008",
    name: "Document Integrity Failure",
    severity: "CRIMSON",
    description: "Decrypted document checksum doesn't match stored encryptedChecksum",
    watchSources: ["vault.access.log"],
    condition: (e) => {
      return e.action === "vault.decrypt" && e.metadata.checksumMismatch === true;
    },
    action: "halt_all_processing_alert",
    cooldownMs: 0,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-009",
    name: "Low Confidence Without Human Review",
    severity: "RED",
    description: "Confidence score < 0.70 on legal/financial output without human review",
    watchSources: ["orchestrator.decision.log"],
    condition: (e) => {
      const confidence = e.metadata.confidenceScore as number;
      const outputType = e.metadata.outputType as string;
      const legalFinancialTypes = ["tax_advice", "legal_opinion", "compliance_filing", "contract_draft"];
      const isLegalFinancial = legalFinancialTypes.includes(outputType);
      const hasHumanReview = e.metadata.humanReviewTriggered as boolean;
      return isLegalFinancial && confidence < 0.70 && !hasHumanReview;
    },
    action: "alert_and_pause_swarm",
    cooldownMs: 0,
    lastTriggered: null,
    enabled: true,
  },
  {
    id: "WD-010",
    name: "Swarm Unresponsive > 10 minutes",
    severity: "AMBER",
    description: "Domain Lead agent has not sent heartbeat in 10 minutes",
    watchSources: ["swarm.heartbeat.log"],
    condition: (e) => {
      const lastHeartbeat = getLastHeartbeat(e.agentId);
      return lastHeartbeat && (Date.now() - lastHeartbeat.getTime()) > 10 * 60 * 1000;
    },
    action: "alert_and_log",
    cooldownMs: 5 * 60 * 1000,
    lastTriggered: null,
    enabled: true,
  },
];
```

---

## Audit Log — Immutable Chain of Custody

```typescript
// security/watchdog/src/audit-log.ts

import crypto from "crypto";

interface ImmutableAuditLog {
  id: string;                      // UUID v7 — time-sortable
  timestamp: Date;                 // ISO 8601 with microseconds
  eventType: string;
  severity: "AMBER" | "RED" | "CRIMSON";
  
  // Who
  userId: string | null;
  agentId: string;
  toolName: string | null;
  sessionId: string;
  
  // What
  action: string;
  keyName: string | null;
  vaultDocId: string | null;
  ipAddress: string | null;
  geoLocation: string | null;
  
  // Outcome
  granted: boolean;
  deniedReason: string | null;
  
  // Integrity
  checksum: string;               // SHA-256 chain hash
  previousLogId: string | null;  // Chain link
}

function computeLogChecksum(log: ImmutableAuditLog, previousLog: ImmutableAuditLog | null): string {
  const data = [
    log.timestamp.toISOString(),
    log.eventType,
    log.agentId,
    log.action,
    log.granted.toString(),
    previousLog?.checksum ?? "GENESIS",
  ].join("|");
  
  return crypto.createHash("sha256").update(data).digest("hex");
}

function appendLog(entry: Omit<ImmutableAuditLog, "id" | "checksum" | "previousLogId">): ImmutableAuditLog {
  const previousLog = getLastLog();
  const id = generateUUIDv7(); // Time-sortable UUID
  const checksum = computeLogChecksum({ ...entry, id } as ImmutableAuditLog, previousLog);
  
  const fullEntry: ImmutableAuditLog = {
    ...entry,
    id,
    checksum,
    previousLogId: previousLog?.id ?? null,
  };
  
  // Append to immutable log store (append-only file or write-once DB row)
  appendToLogStore(fullEntry);
  
  return fullEntry;
}
```

---

## Telegram Alert Template

```typescript
// security/watchdog/src/alert.ts

interface TelegramAlertPayload {
  severity: "AMBER" | "RED" | "CRIMSON";
  ruleId: string;
  ruleName: string;
  eventSummary: string;
  agentId: string;
  actionTaken: WatchdogAction;
  auditId: string;
  timestamp: Date;
  requiresAction: boolean;
  actionLink: string;             // Link to VentureMind admin console
}

function formatTelegramAlert(payload: TelegramAlertPayload): string {
  const emoji = {
    AMBER: "⚠️",
    RED: "🚨",
    CRIMSON: "☠️",
  }[payload.severity];

  const actionRequired = payload.requiresAction ? "ACTION REQUIRED" : "Logged only";

  return [
    `${emoji} **VentureMind SafetyNet Alert**`,
    `━━━━━━━━━━━━━━━━━━━━`,
    `**Severity:** ${payload.severity}`,
    `**Rule:** ${payload.ruleName} (${payload.ruleId})`,
    `**Agent:** ${payload.agentId}`,
    `**Event:** ${payload.eventSummary}`,
    `**Action Taken:** ${payload.actionTaken}`,
    `**Audit ID:** \`${payload.auditId}\``,
    `**Time:** ${payload.timestamp.toISOString()}`,
    `━━━━━━━━━━━━━━━━━━━━`,
    `**Status:** ${actionRequired}`,
    payload.actionLink ? `**Console:** ${payload.actionLink}` : "",
  ].filter(Boolean).join("\n");
}
```

---

## Response Actions

```typescript
// security/watchdog/src/actions.ts

async function executeAction(
  action: WatchdogAction,
  event: SystemEvent,
  rule: WatchdogRule
): Promise<void> {
  const auditEntry = await appendLog({
    timestamp: new Date(),
    eventType: "watchdog.action",
    severity: rule.severity,
    userId: event.userId,
    agentId: event.agentId,
    toolName: event.action,
    sessionId: event.sessionId,
    action: `watchdog.${action}`,
    keyName: event.metadata.keyName as string | null,
    vaultDocId: event.metadata.docId as string | null,
    ipAddress: event.ipAddress,
    geoLocation: event.geoLocation,
    granted: true,
    deniedReason: null,
  });

  switch (action) {
    case "log_only":
      // Just log — no active response
      break;

    case "alert_and_log":
      await sendTelegramAlert({ ...rule, auditId: auditEntry.id, requiresAction: false });
      break;

    case "alert_and_pause_swarm":
      await sendTelegramAlert({ ...rule, auditId: auditEntry.id, requiresAction: true });
      await pauseSwarm(event.agentId);
      break;

    case "alert_and_require_reauth":
      await sendTelegramAlert({ ...rule, auditId: auditEntry.id, requiresAction: true });
      await requireUserReauth(event.sessionId);
      break;

    case "pause_swarm_and_halt_key":
      await sendTelegramAlert({ ...rule, auditId: auditEntry.id, requiresAction: true });
      await pauseSwarm(event.agentId);
      await ghostHaltKey(event.metadata.keyName as string);
      break;

    case "pause_swarm_halt_key_alert":
      await sendTelegramAlert({ ...rule, auditId: auditEntry.id, requiresAction: true });
      await pauseAllSwarms();
      await ghostRevokeAllKeys();
      break;

    case "halt_all_processing_alert":
      await sendTelegramAlert({ ...rule, auditId: auditEntry.id, requiresAction: true });
      await haltAllProcessing();
      await ghostRevokeAllKeys();
      await pauseAllSwarms();
      break;
  }
}
```

---

*Watchdog Agent v1.0 | VentureMind SafetyNet | Confidential — Internal Only*