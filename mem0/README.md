# VentureMind on Mem0 — Long-Term Memory Layer
> 55.2k GitHub stars · Apache-2.0 · Self-hosted · Mem0 v3 (April 2026)

## What It Is

**mem0ai/mem0** is a universal memory layer for AI agents — giving the Nomad Flow swarm long-term, personalized, multi-level memory across all 10 Domain Swarms. Mem0 stores client preferences, session state, and agent performance data so that every agent interaction is informed by accumulated context, not a blank slate.

> **Key role in VentureMind**: Mem0 is the *shared brain* — every agent that touches a client retrieves and writes memory through Mem0, creating persistent, cross-session intelligence. It connects to EverythingLLM (documents), Dify (workflows), SafetyNet (compliance), and paperclip (orchestration).

## Why Mem0 for VentureMind

| VentureMind Need | Mem0 Answer |
|---|---|
| Cross-session client memory | Multi-level user/session/agent memory with 90%+ token savings |
| +26% accuracy over OpenAI Memory | LoCoMo benchmark: 91.6 (vs 71.4 old algorithm) |
| Sub-second memory writes | Single-pass ADD-only (1 LLM call, no update/delete) |
| Per-client data isolation | Application-layer hashing: `user_id = hashed_client_id` |
| Agent-generated facts as first-class | Strategy agent confirmations stored with equal weight to user preferences |
| Entity-linked memory clusters | UAE memories cluster together — richer retrieval over time |
| Self-hosted, no data leaves sandbox | Fully on-premise: Ollama + Qdrant + LanceDB |
| 90% lower token usage | Multi-signal retrieval: semantic (0.5) + BM25 (0.3) + entity (0.2) |

## Architecture: Mem0 × VentureMind Stack

```
┌─────────────────────────────────────────────────────────────┐
│                    VENTUREMIND SWARM                         │
│  Strategy │ Legal │ Finance │ Capital │ Mobility │ ...      │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼  (memory search / add)
┌─────────────────────────────────────────────────────────────┐
│           VentureMind Memory Controller Agent               │
│  SOUL.md: scope rules, compliance gating, escalation        │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                        Mem0 v3                               │
│  Multi-signal retrieval: semantic + BM25 + entity            │
│  ADD-only accumulation (no memory overwrite)                  │
│  Entity linking across memory clusters                       │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│             Self-Hosted Infrastructure                       │
│  Ollama (llama3.2) │ Qdrant (vector) │ LanceDB (storage)   │
│  DISABLE_TELEMETRY=true  │  No external API calls         │
└─────────────────────────────────────────────────────────────┘
```

## Three Memory Levels

| Level | Scope | Retention | Example |
|---|---|---|---|
| **User** | Per client | 90d–forever | "Client prefers UAE Golden Visa tax treatment" |
| **Session** | Per conversation | 24h + buffer | "User confirmed entity type: LLC, pending formation" |
| **Agent** | Per agent | 180d rolling | "Tax Strategist: treaty interpretation accuracy 91%" |

## 17 Integration Points

| Component | Role with Mem0 |
|---|---|
| `venturemind-memory-controller` (SOUL) | Scope enforcement, compliance gating, cross-agent sharing |
| `venturemind-mem0-memory` (SKILL) | Agent runtime: how to add, search, tag memories |
| **Deep Discovery Agent** | Writes: client goals, preferences, constraints |
| **Strategy Agent** | Reads: client context before planning |
| **Legal Swarm** | Writes: entity formations, jurisdiction selections |
| **Tax Strategist** | Writes: treaty confirmations, filing history |
| **Capital Swarm** | Writes: investor pipeline, term sheet status |
| **Mobility Swarm** | Writes: residency applications, banking hub selections |
| **AnythingLLM** | Reads: Mem0 context before answering document queries |
| **Dify** | Reads: client state between workflow steps |
| **SafetyNet** | Writes: escalation events (compliance_flag memories) |
| **paperclip** | Reads: memory before orchestrating task delegation |
| **OpenFang** | Writes: skill evolution tracking (agent scope) |
| **feynman** | Reads: accumulated research context for literature reviews |
| **agent-gpt** | Reads/writes: goal state, task memory |
| **Mem0 CLI** | Operators: debug, audit, backfill memories |
| **Evaluation** | Monthly: LoCoMo, LongMemEval, BEAM benchmarks |

## Mem0 v3 Algorithm — April 2026

| Benchmark | Old Algorithm | New Mem0 v3 | VentureMind Target |
|---|---|---|---|
| LoCoMo | 71.4 | **91.6** | ≥ 88 |
| LongMemEval | 67.8 | **93.4** | ≥ 90 |
| BEAM (1M tokens) | — | **64.1** | ≥ 60 |
| Token efficiency | baseline | **-90%** | ≥ -85% |
| Latency (p50) | baseline | **-91%** | < 500ms |

**Key changes in v3**:
- Single-pass ADD-only: one LLM call, no overwrite, memories accumulate
- Agent-generated facts: equal weight to user-provided facts
- Entity linking: facts automatically connected across memory clusters
- Multi-signal retrieval: semantic (0.5) + BM25 (0.3) + entity matching (0.2)

## Privacy & Isolation

```python
# Client ID is one-way hashed — no PII stored in Mem0
def hash_client_id(client_id: str) -> str:
    return hashlib.sha256(client_id.encode()).hexdigest()[:32]

# user_id passed to Mem0 = hashed_client_id
memory.add(
    messages=[...],
    user_id=hash_client_id("cli_abc123"),  # Not the actual client ID
    ...
)
```

**Isolation rules**:
- user scope: Only `hashed_client_id` memories are returned
- Cross-client collision: Hard block, alert CISO, log `isolation_breach_attempt`
- Agent scope: Private to the agent that wrote it — other agents cannot read
- shared scope: Only pre-approved non-sensitive facts (completed entities, market insights)

## Deployment

```bash
# Self-hosted Mem0 server
git clone https://github.com/mem0ai/mem0.git
cd mem0/server
cp .env.example .env
# Set: ADMIN_API_KEY, AUTH_DISABLED=false, DISABLE_TELEMETRY=true

docker compose up -d
# API: http://localhost:3001
# Dashboard: http://localhost:3000
```

```python
# Python SDK connection
from mem0 import Memory

memory = Memory.from_llamaindex(
    dimension=1024,
    embedder_provider="ollama",
    embedder_model="nomic-embed-text",
    vector_store="qdrant",
    llm_provider="ollama",
    llm_model="llama3.2",
    enableNLP=True,
)

# VentureMind agent usage
memories = memory.search(
    query="client tax residency preference",
    filters={"user_id": hash_client_id(client_id)},
    top_k=5
)
```

## Comparison: Mem0 vs EverythingLLM vs Dify

| Dimension | Mem0 | EverythingLLM | Dify |
|---|---|---|---|
| **Primary role** | Memory/persistence | Document RAG | Workflow orchestration |
| **Type of data** | Preferences, facts, agent state | Documents, contracts, PDFs | Workflow definitions, prompts |
| **Recall type** | Semantic + BM25 + entity | Vector similarity | Tool calls + RAG |
| **Per-client isolation** | ✅ Hashed client_id | ✅ Workspace isolation | ⚠️ Requires config |
| **Agent-generated facts** | ✅ First-class | ❌ N/A | ❌ N/A |
| **Token efficiency** | ✅ 90% lower | ❌ Full context | ❌ Full context |
| **Self-hosted** | ✅ | ✅ | ✅ |
| **VentureMind usage** | **Shared brain** | **Document knowledge** | **Execution workflows** |

Mem0 handles *what we've learned about this client*. EverythingLLM handles *what's in this client's documents*. Dify handles *what to do next*. Together they form complete intelligence.
