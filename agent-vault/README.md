# VentureMind × Infisical Agent Vault
> The credential cornerstone of the Hardened Sovereignty security model.

---

## What Is Agent Vault?

**Agent Vault** (github.com/Infisical/agent-vault) is an open-source HTTP credential proxy and vault for AI agents, built by Infisical. It has **1,000+ GitHub stars**, is written in Go (74%) + TypeScript (24%), and is actively maintained with 21 releases.

**Core insight:** AI agents are non-deterministic — they can be tricked via prompt injection into outputting their environment variables, leaking every API key. Traditional secret managers return credentials to the caller, which means the agent *has* the credential and can be compelled to share it.

Agent Vault solves this with brokered access: **credentials are never returned to agents**. Instead, agents route HTTP traffic through a local proxy that injects the correct credential at the network layer.

---

## The VentureMind Security Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Runtime kernel** | OpenFang | 24/7 autonomous agents running Hands |
| **Company structure** | Paperclip | Org chart, agents, teams, projects |
| **Research intelligence** | Feynman | Deep research, fact-checking |
| **Engineering discipline** | gstack | Slash commands for code quality |
| **Credential vault** | Agent Vault | Zero-knowledge credential broker |
| **Skill validation** | agent-skills-eval | Empirical SKILL.md performance testing |

---

## How Agent Vault Works in VentureMind

```
┌──────────────────────────────────────────────────┐
│              VentureMind Agent                    │
│         (Strategist, Operator, etc.)              │
│                                                  │
│  fetch("https://api.stripe.com/charges")         │
│  ← agent thinks it's making a normal API call     │
└─────────────────────┬────────────────────────────┘
                      │ HTTPS_PROXY=localhost:14322
                      ▼
┌──────────────────────────────────────────────────┐
│         Agent Vault Proxy (port 14322)            │
│  TLS-encrypted MITM proxy                        │
│  Injects: Authorization: Bearer sk_live_xxx     │
│  Strict deny: unmatched hosts = 403              │
└─────────────────────┬────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────┐
│                  Stripe API                       │
│                                                  │
│  ← Stripe sees the real API key                   │
│  ← Agent never saw the key                       │
└──────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────┐
│       Agent Vault Server (port 14321)             │
│  Web UI · REST API · Vault management             │
│  AES-256-GCM at rest · Argon2id-wrapped master   │
└──────────────────────────────────────────────────┘
```

---

## VentureMind Vault Structure

```
VentureMind Root Vault
├── nomad-flow-production/      ← Live platform credentials
│   ├── stripe/
│   ├── twilio/
│   ├── aws/
│   ├── supabase/
│   ├── llm-providers/
│   ├── smtp/
│   └── telegram/
├── nomad-flow-staging/          ← Staging environment
├── founders/                    ← Per-founder credential isolation
│   └── {founder_id}/
│       ├── stripe-connect/
│       ├── exchange-api/
│       └── banking-api/
└── venturemind-internal/        ← Internal tooling
```

**Key guarantee:** Each founder has their own sub-vault. One founder's agents cannot access another founder's credentials.

---

## Vault Controller Agent (SOUL.md)

The Vault Controller is VentureMind's dedicated agent for all credential operations. It is the **only agent** that ever interacts with Agent Vault's API — all other agents use scoped sessions and proxy routes.

**Responsibilities:**
- Credential rotation (automated every 90 days + on-demand)
- New credential onboarding (human-gated)
- Founder sub-vault provisioning (triggered by intake-specialist)
- Compromised credential incident response (immediate revocation)
- Agent session lifecycle management
- Audit log generation for Compliance Auditor

**Reports to:** Watchdog Agent (compliance-watchdog)

---

## Security Model

| Threat | Protection |
|--------|-----------|
| Prompt injection leaking credentials | Credentials never in agent context |
| Credential exfiltration | Network-layer injection — agents never see raw keys |
| Session hijacking | HMAC-signed scoped sessions with TTL |
| Cross-founder credential access | Sub-vault RBAC — zero cross-access |
| Master password compromise | Argon2id-wrapped DEK; passwordless mode available |
| Unmatched host access | `unmatched_host_policy=deny` — unconfigured services = 403 |
| Audit log credential leakage | Request bodies/headers/queries never logged |

---

## Installation

```bash
# Script installer (macOS / Linux)
curl --proto '=https' --proto-redir '=https' --tlsv1.2 -fsSL https://get.agent-vault.dev | sh
agent-vault server -d

# Docker
docker run -d -p 14321:14321 -p 14322:14322 \
  -e AGENT_VAULT_MASTER_PASSWORD=your-master-password \
  -v agent-vault-data:/data \
  infisical/agent-vault
```

---

## Quick Reference

```bash
# Start server
agent-vault server -d

# Launch VentureMind agent with vault protection
agent-vault run -- openfang chat strategist

# Container isolation (strictest — physical network lockdown)
agent-vault run --isolation=container --share-agent-dir -- openfang chat operator

# Add credential (security admin only — never through agents)
agent-vault vault credential add --vault nomad-flow-production --service stripe --key sk_live_xxx

# Create scoped session for a founder's agent
agent-vault vault session create --vault founders/founder_123 --role strategist --ttl 4h

# Revoke compromised session immediately
agent-vault vault session revoke --session-id session_abc123

# View audit logs (no credentials, no request bodies)
agent-vault vault log --vault nomad-flow-production --last 24h
```

---

## Files in This Directory

```
agent-vault/
├── CLAUDE.md                              ← This file
└── agents/
    └── vault-controller/
        └── SOUL.md                        ← Vault Controller Agent — full SOUL.md
```

---

## External Resources

- **Agent Vault**: https://github.com/Infisical/agent-vault
- **Documentation**: https://docs.agent-vault.dev
- **Launch Blog**: https://infisical.com/blog/agent-vault-the-open-source-credential-proxy-and-vault-for-agents
- **Slack Community**: https://infisical.com/slack