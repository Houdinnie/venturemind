---
name: venturemind-mem0-memory
description: VentureMind Mem0 integration — long-term memory for the Nomad Flow AI engine. Self-hosted, per-client isolation, multi-level user/session/agent memory, Mem0 v3 algorithm.
metadata:
  source: mem0ai/mem0
  version: "3.0"
  license: Apache-2.0
  attribution: Mem0
---

# SKILL.md — VentureMind Mem0 Memory

> Use this skill whenever an agent needs to recall client context, store action outcomes, or retrieve accumulated facts across sessions.

**Trigger phrases**: "remember", "has the client mentioned", "what do we know about", "in previous sessions", "based on past interactions"

---

## Quick Reference

| Command | When to use |
|---|---|
| `mem0 init` | First-time Mem0 setup |
| `mem0 add` | Store memories from a conversation |
| `mem0 search` | Retrieve memories for a query |
| `mem0 list` | Show all memories for a user |
| `mem0 delete` | Remove specific memories |

---

## VentureMind Memory IDs

```
client_id  = hashed_client_id   # One-way hash, no PII stored in Mem0
session_id = {client_id}_{timestamp}
agent_id   = {swarm_slug}_{agent_slug}
```

**Scope rules**:
- `user` scope: ALL memories scoped to a client_id. Only the client can access.
- `session` scope: Memories from current conversation window. Auto-expires 24h after session end.
- `agent` scope: Agent-private performance and skill data. Not shared between agents.
- `shared` scope: ONLY non-sensitive facts explicitly approved for cross-agent sharing.

---

## Memory Tags — Required on Every Add

| Tag | Retention | Access |
|---|---|---|
| `kyc_sensitive` | 7 years | Compliance Auditor ONLY |
| `tax_filing` | 7 years | Tax Strategist + Compliance |
| `financial_data` | 5 years | Finance Swarm + Client |
| `legal_strategy` | Client-controlled | Legal Swarm + Client |
| `user_preference` | Per tier (90d–forever) | All Swarm Agents |
| `agent_fact` | 90 days | All Swarm Agents |
| `compliance_flag` | 7 years | Compliance Auditor ONLY |
| `session_only` | 24h | All Swarm Agents |

---

## Configuration

```bash
# .env
MEM0_ENABLED=true
MEM0_SELF_HOSTED=true
MEM0_VECTOR_STORE=qdrant          # self-hosted
MEM0_LLM_PROVIDER=ollama
MEM0_LLM_MODEL=llama3.2
MEM0_EMBEDDER_PROVIDER=ollama
MEM0_EMBEDDER_MODEL=nomic-embed-text
MEM0_API_KEY=<generated>
```

```python
from mem0 import Memory

memory = Memory.from_llamaindex(
    dimension=1024,
    embedder_provider="ollama",
    embedder_model="nomic-embed-text",
    vector_store="qdrant",
    llm_provider="ollama",
    llm_model="llama3.2",
    enableNLP=True,  # BM25 + entity extraction
)
```

---

## Usage Examples

### Store client preference (user scope)

```python
memory.add(
    messages=[
        {"role": "user", "content": "I want to set up my company in the UAE but pay as little tax as possible."},
        {"role": "assistant", "content": "Understood. UAE Golden Visa + IBC structure achieves zero corporate tax for mainland business."}
    ],
    user_id="cli_hashed_abc123",
    metadata={
        "action_tag": "tax_preference",
        "retention_tag": "user_preference",
        "shared": False,
        "sensitivity": "medium",
        "source_agent_id": "strategy-agent"
    },
    infer=True  # Auto-extract entities and facts
)
```

### Search for tax preference

```python
memories = memory.search(
    query="tax residency preference UAE client",
    filters={"user_id": "cli_hashed_abc123"},
    top_k=5,
    threshold=0.65
)

for m in memories["results"]:
    print(f"[{m['score']:.2f}] {m['memory']}")
```

### Store agent-generated fact (agent scope)

```python
memory.add(
    messages=[
        {"role": "assistant", "content": "Entity formation complete. UAE LLC registered with Al Tamimi & Co."}
    ],
    user_id="cli_hashed_abc123",
    metadata={
        "action_tag": "entity_formation_complete",
        "retention_tag": "agent_fact",
        "shared": True,
        "source_agent_id": "legal-agent"
    },
    infer=True
)
```

### Multi-client isolation check

```python
def safe_search(query, client_id, top_k=5):
    hashed = hash_client_id(client_id)  # one-way hash
    memories = memory.search(
        query=query,
        filters={"user_id": hashed},
        top_k=top_k
    )
    # Verify no cross-client contamination
    for m in memories["results"]:
        assert m["user_id"] == hashed, "ISOLATION_BREACH"
    return memories
```

---

## Mem0 v3 Algorithm — What's Happening Under the Hood

**ADD path** (single LLM call, no overwrite):
1. LLM extracts facts, entities, sentiment from messages
2. Facts stored as independent memories (accumulate, never replace)
3. Entity linker connects new facts to existing entity clusters
4. No delete or update in single pass

**SEARCH path** (multi-signal fusion, parallel):
1. Semantic: vector embedding similarity (weight 0.5)
2. BM25: keyword exact match (weight 0.3)
3. Entity matching: entity cluster membership (weight 0.2)
4. Fused score → top-k results returned

**Entity linking** means VentureMind memories get smarter over time:
- More memories about "UAE" → UAE entity cluster grows → future UAE queries return richer context
- Agent confirms a treaty benefit → entity linked to treaty cluster → next treaty query includes confirmation

---

## Integration Points

| Component | How it uses Mem0 |
|---|---|
| **Deep Discovery Agent** | Stores client goals, preferences, constraints |
| **Strategy Agent** | Retrieves client context before planning |
| **Legal Swarm** | Stores entity formation outcomes, jurisdiction selections |
| **Tax Strategist** | Stores treaty applications, filing history |
| **Capital Swarm** | Stores investor pipeline, term sheet status |
| **Mobility Swarm** | Stores residency applications, travel itineraries |
| **AnythingLLM** | Shared document RAG pipeline queries Mem0 for context |
| **Dify** | Workflow agents query Mem0 for client state between steps |
| **SafetyNet** | Escalation events stored as compliance_flag memories |
| **paperclip** | Orchestrator agent queries memory before task delegation |

---

## Benchmarking

Run the evaluation suite monthly:

```bash
git clone https://github.com/mem0ai/memory-benchmarks.git
cd memory-benchmarks
python evaluate.py --model llama3.2 --embedder nomic-embed-text
```

**VentureMind thresholds**:

| Benchmark | Min Acceptable | Target |
|---|---|---|
| LoCoMo | 80 | 88+ |
| LongMemEval | 85 | 90+ |
| BEAM (1M) | 55 | 60+ |

If scores drop below minimum for 3 consecutive cycles → Engineering Swarm review.
