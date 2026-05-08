# VentureMind Security Architecture — Zero-Knowledge Vault & Hardened Infrastructure
## Version 1.0 | Protocol Zero | Audit-Ready

---

## Overview

VentureMind handles sensitive founder data: passports, tax IDs, contracts, banking credentials. This document defines the **Zero-Knowledge Security Stack** — a hardened architecture where even if a server is fully compromised, sensitive information remains mathematically unreadable without the user's active cryptographic participation.

**Design Philosophy**: "Sovereign Data Isolation" — the system is architecturally incapable of misusing user data.

---

## 1. Zero-Knowledge Document Vault

### 1.1 Client-Side Encryption Flow

```
User File → Browser (AES-256-GCM) → Encrypted Blob → S3/Vault → NEVER plain text
                ↑
         User's derived key
         (never stored server-side)
```

**Encryption Spec**:
- Algorithm: **AES-256-GCM** (authenticated encryption)
- Key derivation: **PBKDF2** with 100,000 iterations from user login + per-document salt
- IV/Nonce: unique per document, stored alongside ciphertext
- Algorithm identifier stored as prefix: `vault:v1:aes256gcm:...`

### 1.2 Encrypted Vault Schema

```typescript
// Vault document record (plain text metadata only)
interface EncryptedVaultRecord {
  id: string;                    // UUID v4
  ownerId: string;               // User ID (indexable)
  documentType: DocumentType;    // PASSPORT | TAX_ID | CONTRACT | BANK_STATEMENT | OTHER
  encryptedChecksum: string;     // SHA-256 of plaintext (for integrity verification)
  vaultAlgorithm: string;       // "vault:v1:aes256gcm"
  keySalt: string;               // Base64 encoded per-document salt
  iv: string;                    // Base64 encoded 12-byte nonce
  ciphertext: string;            // Base64 encoded encrypted blob
  region: string;                // e.g., "us-east-1"
  bucket: string;                // e.g., "venturemind-vault-prod"
  s3Key: string;                 // S3 object key (UUID-named, no user data in path)
  createdAt: Date;
  accessedAt: Date | null;      // null until first access
  accessCount: number;
  expiresAt: Date | null;        // Optional auto-expiry for transient documents
  isCompromised: boolean;       // Set true if rotation triggered
}

interface UserVaultKeyRecord {
  // This table should ONLY contain the encrypted key envelope — never raw keys
  userId: string;
  encryptedKeyBlob: string;      // Master key encrypted with user's derived key
  keyAlgorithm: string;        // e.g., "rsassa-pss:sha256:4096"
  publicKeyPem: string;        // For verifying client-signed decryption requests
  keySalt: string;             // PBKDF2 salt for user's master key derivation
  keyIterations: number;       // PBKDF2 iterations (100,000+)
  rotatedAt: Date;
  compromised: boolean;
  compromisedReason: string | null;
}
```

### 1.3 Key Hierarchy

```
User Password
     ↓ (PBKDF2, 100k iterations, unique salt)
User Master Key (UMK) — stored encrypted in UserVaultKeyRecord
     ↓ (derived per session)
Session Key — held in memory only, never persisted
     ↓ (derived per document)
Document Key — unique per file, stored in document's ciphertext envelope
```

### 1.4 Document Types & Access Policies

| Document Type | Retention | Auto-Delete | Requires Re-Auth |
|--------------|-----------|-------------|-----------------|
| Passport / ID | Until account deletion | No | Yes — biometric every 30 days |
| Tax Documents | 7 years (legal requirement) | No | Yes — every access |
| Contracts | Until contract expiry + 3 years | Yes (on expiry) | Yes — every access |
| Bank Statements | 5 years | No | Yes — every access |
| Crypto Wallet Seeds | Never delete | Never | Yes — hardware key every time |

---

## 2. Ghost Infrastructure — API Key & Secret Isolation

### 2.1 The Problem

Standard `.env` files are vulnerable to:
1. **Prompt injection**: A compromised LLM reads `process.env.STRIPE_KEY` and exfils
2. **Log exposure**: Secrets appear in error logs, stack traces, or LangSmith traces
3. **Employee access**: DevOps / engineers can `echo $SECRET_KEY` on the server

### 2.2 Ghost Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    UNENCRYPTED ZONE (memory only)           │
│  No secrets. No API keys. Pure business logic.              │
│  Can be deployed publicly. No damage if compromised.        │
└─────────────────────────────────────────────────────────────┘
                              ↑
              Ghost Protocol — in-process IPC only
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    GHOST PROCESS (isolated, no network)      │
│  Holds all API keys, secrets, encryption keys              │
│  Has NO external network access itself                     │
│  Only responds to IPC from main process                    │
│  Can be killed / restarted independently                    │
└─────────────────────────────────────────────────────────────┘
                              ↑
              Agent tools call ghost_get("STRIPE_SECRET")
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    HARDWARE KEYSTORE (not server-dependent)  │
│  AWS KMS / Cloud HSM for production                        │
│  Paperclip Secrets for local dev                           │
│  Keys are NEVER in environment variables                   │
└─────────────────────────────────────────────────────────────┘
```

### 2.3 Ghost Protocol Implementation

```typescript
// ghost-service/index.ts — runs as isolated child process
// NO network access. IPC only via stdin/stdout.
interface GhostCommand {
  action: "get" | "list" | "rotate" | "revoke";
  keyName: string;              // e.g., "STRIPE_SECRET_KEY"
  requestId: string;            // UUID for audit log
  requestingAgent: string;      // e.g., "capital-swarm-lead"
  requestingTool: string;       // e.g., "stripe-create-checkout"
  userId: string | null;        // null for system-level keys
  justification: string;        // Natural language why agent needs it
  ttlSeconds: number;           // Must be ≤ 60 for most operations
  signature: string;           // HMAC of payload using agent-specific secret
}

interface GhostResponse {
  requestId: string;
  granted: boolean;
  value?: string;               // Only on granted reads
  reason: string;
  expiresAt: string;            // ISO timestamp when this response's value is invalid
  auditId: string;             // Immutable log entry ID
}

// Ghost service NEVER logs the actual secret value
// Only: requestId, keyName, granted, requestingAgent, timestamp, ttl
```

### 2.4 Ghost Policy Matrix

| Action | Requires Human Auth | Requires Session Re-Verification | Max TTL |
|--------|--------------------|----------------------------------|---------|
| Read API key (low-risk: analytics, non-financial) | No | No | 60s |
| Read API key (medium-risk: email, calendar) | No | Yes — biometric | 30s |
| Read API key (high-risk: banking, Stripe, crypto) | **Yes** | **Yes — hardware key** | 10s |
| Rotate API key | **Yes** | **Yes** | N/A |
| Revoke API key | **Yes** | **Yes — supervisor approval** | N/A |
| Create new API key | **Yes** | **Yes** | N/A |
| Decrypt vault document | No | **Yes — biometric** | 30s |

---

## 3. Watchdog Agent — Real-Time Threat Detection

### 3.1 Architecture

```
┌──────────────────────────────────────────────────────┐
│              WATCHDOG AGENT (independent process)     │
│  Subscribes to: LangSmith traces, audit logs,        │
│                 ghost audit log, vault access logs  │
│                                                     │
│  State machine: IDLE → WATCHING → ALERTING → CLEARED │
└──────────────────────────────────────────────────────┘
        ↓
┌─────────────────────┐
│ Telegram Alert      │  ← Immediate escalation
│ to Houdinnie        │
└─────────────────────┘
        ↓
┌─────────────────────┐
│ Pause swarm if      │  ← CRIMSON trigger
│ CRIMSON threshold   │
└─────────────────────┘
```

### 3.2 Threat Detection Rules

```typescript
// watchdogs/anomaly-detector.ts

interface WatchdogRule {
  id: string;
  name: string;
  severity: "AMBER" | "RED" | "CRIMSON";
  condition: string;                    // Natural language for audit
  watchSources: WatchSource[];
  action: WatchdogAction;
  cooldownMs: number;                 // Don't re-alert within this window
  lastTriggered: Date | null;
}

const WATCHDOG_RULES: WatchdogRule[] = [
  {
    id: "WD-001",
    name: "Rapid Vault Access",
    severity: "RED",
    condition: "More than 5 vault accesses in 60 seconds from same session",
    watchSources: ["vault.access.log"],
    action: "alert_and_pause_swarm",
    cooldownMs: 5 * 60 * 1000,         // 5 min cooldown
  },
  {
    id: "WD-002",
    name: "API Key Read Frequency",
    severity: "AMBER",
    condition: "Same key read more than 3 times in 30 seconds — potential exfil",
    watchSources: ["ghost.audit.log"],
    action: "alert_and_log",
    cooldownMs: 60 * 1000,             // 1 min cooldown
  },
  {
    id: "WD-003",
    name: "Unusual Jurisdiction Access",
    severity: "RED",
    condition: "Document accessed from high-risk country not in user's profile",
    watchSources: ["vault.access.log", "user.profile"],
    action: "alert_and_require_reauth",
    cooldownMs: 0,                     // No cooldown — always alert
  },
  {
    id: "WD-004",
    name: "Agent Prompt Anomaly",
    severity: "CRIMSON",
    condition: "Agent output contains more than 5 URLs, Base64 strings, or email addresses — potential data exfil",
    watchSources: ["langsmith.traces"],
    action: "pause_swarm_and_halt_key",
    cooldownMs: 0,
  },
  {
    id: "WD-005",
    name: "Ghost Key Access Outside Business Hours",
    severity: "AMBER",
    condition: "High-risk API key read outside 06:00-22:00 UTC",
    watchSources: ["ghost.audit.log"],
    action: "alert_and_delay_response",
    cooldownMs: 30 * 60 * 1000,
  },
  {
    id: "WD-006",
    name: "KYC Tier Violation Attempt",
    severity: "CRIMSON",
    condition: "Agent attempts capital raise action above user's KYC tier limit",
    watchSources: ["orchestrator.decision.log"],
    action: "pause_swarm_halt_key_alert",
    cooldownMs: 0,
  },
  {
    id: "WD-007",
    name: "Hallucination Cascade Detection",
    severity: "RED",
    condition: "Same research question gets 3+ conflicting answers across 5 minutes",
    watchSources: ["orchestrator.decision.log"],
    action: "alert_and_trigger_human_review",
    cooldownMs: 0,
  },
  {
    id: "WD-008",
    name: "Document Integrity Check Failure",
    severity: "CRIMSON",
    condition: "Decrypted document checksum doesn't match stored encryptedChecksum",
    watchSources: ["vault.decrypt.log"],
    action: "halt_all_processing_alert",
    cooldownMs: 0,
  },
];
```

### 3.3 Severity & Response Matrix

| Severity | What Happens | Who Notified | Swarms Affected |
|----------|-------------|--------------|-----------------|
| **AMBER** | Log + Telegram alert | Houdinnie only | None (monitoring) |
| **RED** | Pause affected swarm + alert | Houdinnie + SafetyNet auditor | Domain Lead + sub-agents |
| **CRIMSON** | Halt all swarms + revoke all active keys + alert | Houdinnie + Telegram urgent | ALL swarms — full stop |

### 3.4 Audit Log Schema

```typescript
interface ImmutableAuditLog {
  id: string;                      // UUID v7 — time-sortable, unique
  timestamp: Date;                // ISO 8601 with microseconds
  eventType: AuditEventType;
  severity: "AMBER" | "RED" | "CRIMSON";
  
  // Who
  userId: string | null;
  agentId: string;                // e.g., "capital-swarm-lead"
  toolName: string;              // e.g., "stripe-create-checkout"
  sessionId: string;
  
  // What
  action: string;                 // e.g., "ghost.get"
  keyName: string | null;        // e.g., "STRIPE_SECRET_KEY"
  vaultDocId: string | null;
  ipAddress: string | null;
  geoLocation: string | null;     // e.g., "CN" (derived, not stored explicitly)
  
  // Context
  requestId: string;
  justification: string;
  confidenceScore: number;        // From orchestrator
  decisionBasis: string;          // "primary sources", "user input", "hallucination fallback"
  
  // Outcome
  granted: boolean;
  deniedReason: string | null;
  responseTtlSeconds: number;
  
  // Integrity
  checksum: string;               // SHA-256 of (timestamp + eventType + agentId + action + granted)
  previousLogId: string | null;  // Chain of custody — links to previous log
}
```

---

## 4. TEE Handshake — Trusted Execution Environment Integration

### 4.1 When TEE Is Required

| Operation | TEE Required? | Reason |
|-----------|--------------|--------|
| Decrypting vault documents for agent review | **Yes** | Document leaves server memory only inside enclave |
| Signing legal documents with HSM key | **Yes** | Private key never leaves hardware |
| Processing passport OCR for KYC | **Yes** | PII handled inside isolated environment |
| Multi-sig transaction signing | **Yes** | Crypto operation must be in secure enclave |
| Normal agent reasoning (non-sensitive) | No | Standard compute is fine |

### 4.2 TEE Handshake Protocol

```
1. Agent calls vault.getDocument(docId)
2. Ghost Service verifies: 
   - User biometric re-verified within 30s window?
   - Document type requires TEE?
   - Agent is on allow-list for this document type?
3. If ALL pass → Ghost signals TEE Provider (AWS Nitro / Cloudflare Workers)
4. TEE spins up isolated enclave, loads document
5. Agent receives: encrypted reference + TEE session token
6. Agent reasoning happens inside TEE — raw document never leaves enclave
7. On TEE session end → enclave is wiped, attestation report logged
8. Agent output goes to audit log, document reference is invalidated
```

### 4.3 TEE Attestation Report

```typescript
interface TEESessionRecord {
  sessionId: string;
  documentId: string;
  enclaveImageHash: string;       // Verifies correct TEE image loaded
  enclavePCRQuotes: string[];     // Platform attestation quotes
  startedAt: Date;
  endedAt: Date;
  agentId: string;
  actionsPerformed: string[];    // e.g., ["ocr", "redaction", "field_extraction"]
  outputHash: string;            // Hash of what agent could see (not raw document)
  attestationSignedBy: string;   // e.g., "AWS Nitro TPM"
}
```

---

## 5. Paperclip Secrets Integration

For development and staging, Paperclip's secrets system provides a secure alternative to `.env` files:

```yaml
# .paperclip/secrets VentureMind
# Stored encrypted in Paperclip's secrets vault, never in filesystem

STRIPE_SECRET_KEY:    # Stripe production key — Production Ghost
STRIPE_TEST_KEY:      # Stripe test key — Dev Ghost
SENDGRID_API_KEY:     # Transactional email
AWS_KMS_KEY_ID:       # Production encryption key reference
GOOGLE_PLACES_API:   # Location services for Mobility Swarm
DERIV_API_TOKEN:      # Trading signals for Trinity-AI integration
SAFE_WALLET_MNEMONIC: # Encrypted wallet seed (never full seed in memory)
```

**Paperclip Budget Integration**: Each agent in the Paperclip org chart has a monthly budget. If an agent's cost exceeds its limit, Paperclip halts that agent automatically — a built-in financial SafetyNet.

---

## 6. Compliance Mapping

| Regulation | Requirement | VentureMind Implementation |
|-----------|-------------|---------------------------|
| **GDPR** | Right to deletion | Client-side encryption makes deletion mathematically verifiable — delete key, data is gone |
| **CCPA** | Data minimization | Vault only stores encrypted blobs + metadata; no PII in plain text |
| **FINRA** | 6-year record retention | Vault enforces 7-year retention for financial documents |
| **FATF** | Travel Rule compliance | On-Chain Compliance Sub-Agent flags transfers > $3,000 |
| **BSA/AML** | SAR filing | AML Monitor triggers SAR workflow on structuring patterns |
| **SOC 2 Type II** | Access auditing | Immutable audit log with checksums + chain of custody |
| **ISO 27001** | Encryption at rest + in transit | AES-256-GCM at rest, TLS 1.3 in transit |

---

## 7. Implementation Sequence

| Phase | Action | Priority |
|-------|--------|----------|
| **Phase 0** | Paperclip Ghost Protocol for dev secrets | P0 — immediate |
| **Phase 1** | Vault encryption schema + S3 integration | P0 — before any user data |
| **Phase 2** | Watchdog Agent + audit log chain of custody | P0 — before go-live |
| **Phase 3** | TEE integration for KYC document processing | P1 — before production |
| **Phase 4** | Hardware key integration for financial operations | P1 — before payment features |
| **Phase 5** | AWS KMS migration for production key management | P2 — post-MVP |

---

*Document version: 1.0 | Last updated: 2026-05-09 | Classification: CONFIDENTIAL — INTERNAL ONLY*