---
name: venturemind-anything-llm-ingestion
description: VentureMind document ingestion pipeline powered by AnythingLLM — private, multi-user RAG with workspace isolation. Each Nomad Flow client gets isolated AnythingLLM workspaces for legal, tax, KYC, HR, and finance documents. Used by the venturemind-document-intake-agent and venturemind-research-agent.
metadata:
  author: houdinnie.zo.computer
  tags:
    - rag
    - document-ingestion
    - privacy
    - anything-llm
    - multi-user
sources:
  - kind: github
    repo: Mintplex-Labs/anything-llm
    attribution: Mintplex Labs Inc
    license: MIT
    usage: referenced
---

# VentureMind on AnythingLLM — Ingestion Skill

## Overview

`venturemind-anything-llm-ingestion` defines the full document ingestion pipeline for VentureMind, powered by Mintplex-Labs/AnythingLLM (59.8k stars, MIT). It covers workspace setup, document classification, embedding, citation, privacy enforcement, and escalation.

> **Key differentiator from Dify**: AnythingLLM is the *per-client private document chat* layer. Dify is the *workflow orchestration* layer. They are complementary — Dify orchestrates the agent; AnythingLLM holds the documents.

## AnythingLLM Workspace Map

Each Nomad Flow client gets a set of isolated AnythingLLM workspaces:

| Workspace Slug | Content | Access Level |
|---|---|---|
| `legal-founders` | Founder agreements, shareholder docs, IP assignments | Client + Legal Swarm |
| `kyc-compliance` | Passports, IDs, proof of address, tax IDs | Compliance Auditor only |
| `tax-knowledge` | Tax filings, treaty texts, IRS / HMRC / UAE FTA docs | Tax Strategist + Compliance |
| `strategy-plans` | Business plans, pitch decks, roadmaps | Client + Strategy Agent |
| `hr-legal` | Employment contracts, HR policies, offer letters | HR Lead + Legal Swarm |
| `finance-captable` | Cap tables, ESOP agreements, financial models | Finance Swarm + Capital Swarm |

## Document Ingestion Flow

```
Nomad Flow Client Upload
    │
    ▼
venturemind-document-intake-agent
    │
    ├─► Classify document type
    │
    ├─► POST /api/v1/document/upload
    │     Body: { file: binary, workspace: slug, filename }
    │
    ├─► Poll GET /api/v1/workspace/{slug}/status until embedding_complete
    │
    ├─► Log to venturemind-intake-log.jsonl
    │
    └─► Confirm with client + store citation ID
```

## API Reference

### Upload Document
```
POST /api/v1/document/upload
Content-Type: multipart/form-data

Fields:
  - file: binary (PDF, DOCX, TXT, CSV, JSON)
  - workspace: string (workspace slug)
  - filename: string (original filename)

Response 200:
{
  "document_id": "uuid",
  "status": "queued",
  "workspace": "slug",
  "filename": "original.pdf",
  "embedding_tokens": null
}
```

### Check Embedding Status
```
GET /api/v1/workspace/{slug}/status?document_id={uuid}

Response 200:
{
  "document_id": "uuid",
  "status": "embedding_complete" | "processing" | "failed",
  "embedding_tokens": 1847,
  "chunks": 12,
  "error": null
}
```

### Query Workspace (Chat)
```
POST /api/v1/workspace/{slug}/chat
Content-Type: application/json

Body:
{
  "message": "What is the vesting schedule for the co-founder?",
  "mode": "chat" | "query",
  "session_id": "optional-uuid"
}

Response 200:
{
  "response": "The co-founder vesting schedule is...",
  "citations": [
    {
      "_chunk_id": "chunk-uuid",
      "document_name": "Founder Agreement.pdf",
      "page_number": 3,
      "text": "The co-founder shall vest over...",
      "relevance_score": 0.94
    }
  ],
  "latency_ms": 1247
}
```

### Delete Document
```
DELETE /api/v1/document/{document_id}
Response: 204 No Content
```

## Privacy Configuration

```bash
# In AnythingLLM server/.env
DISABLE_TELEMETRY=true          # CRITICAL — no data leaves the instance
ENABLE_MULTI_USER=true           # Multi-user mode for client isolation
JWT_SECRET=<32-byte-hex>         # Secure session tokens
STORAGE_DIR=./storage            # Local-only document storage
```

## Vector Database Options

AnythingLLM supports multiple backends. For VentureMind:

| Backend | Use Case | Recommended |
|---|---|---|
| Chroma | Dev / Small scale | ✅ Default |
| LanceDB | Production, high scale | ✅ Preferred |
| Pinecone | Cloud-managed | ⚠️ Data leaves instance |
| Qdrant | Self-hosted production | ✅ Supported |
| Weaviate | Enterprise | ✅ Supported |

**Recommendation**: Self-hosted LanceDB or Qdrant for full privacy.

## Telemetry Opt-Out (Mandatory)

```bash
# Always set in .env before deployment
DISABLE_TELEMETRY=true
```

Telemetry events that would be sent (when disabled = nothing):
- Document added/removed events (no content, just counts)
- LLM provider and model tag
- Chat sent events (no content)
- Vector database type

## Workspace Isolation

Each client is a separate AnythingLLM **organization** with its own:
- API keys
- Workspaces and documents
- User accounts
- Embedding models and LLM providers

Cross-client data access is architecturally impossible.

## Multi-Modal Support

AnythingLLM v1.12+ supports:
- PDFs (including EXIF, OCR via mdpdf fork)
- DOCX
- TXT / CSV / JSON
- Images (GPT-4V or Claude Haiku for descriptions)
- Audio (transcription via Whisper)
- YouTube URLs (transcription)
- Web pages (scraped and embedded)

## Deployment on VentureMind Infrastructure

```bash
# 1. Clone AnythingLLM
git clone https://github.com/Mintplex-Labs/anything-llm.git
cd anything-llm/docker

# 2. Configure .env for privacy-first
cp .env.example .env
# Set: DISABLE_TELEMETRY=true, JWT_SECRET, STORAGE_DIR, ENABLE_MULTI_USER=true

# 3. Use LanceDB as vector DB (self-hosted, no external calls)
echo "CHROMA_ENABLED=false" >> .env
echo "LANCEDB_ENABLED=true" >> .env
echo "LANCEDB_PERSISTENCE_PATH=/storage/lancedb" >> .env

# 4. Start
docker compose up -d

# 5. Create per-client organizations via Admin Panel
# http://localhost:3001/admin

# 6. Provision API keys per client
```

## Security Checklist

- [ ] DISABLE_TELEMETRY=true
- [ ] JWT_SECRET set to 32-byte hex
- [ ] STORAGE_DIR points to local volume (not NFS/cross-mount)
- [ ] Multi-user mode enabled (ENABLE_MULTI_USER=true)
- [ ] Chroma/LanceDB/Qdrant (NOT Pinecone/Weaviate cloud)
- [ ] LLM provider: self-hosted Ollama or OpenRouter (NOT OpenAI direct for sensitive docs)
- [ ] Document upload size limit: 50MB max
- [ ] Workspace API keys rotated every 90 days