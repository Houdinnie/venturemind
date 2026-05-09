# VentureMind on AnythingLLM — Private RAG & Document Intelligence
> 59.8k GitHub stars · MIT license · Self-hosted · Multi-user

## What It Is

**Mintplex-Labs/AnythingLLM** is a private, on-device AI productivity platform that turns documents into chatble knowledge bases. It runs locally by default, supports multiple LLM backends, multi-user workspaces, and document ingestion (PDF, DOCX, TXT, CSV, images, audio, YouTube).

> **Key difference from Dify**: AnythingLLM is the *per-client private document chat* layer — each Nomad Flow client gets their own isolated AnythingLLM workspaces. Dify is the *workflow orchestration* layer that coordinates agents and tasks.

## Why AnythingLLM for VentureMind

| VentureMind Need | AnythingLLM Answer |
|---|---|
| Per-client document privacy | Multi-user mode with org-level isolation |
| Self-hosted, no data leaves | Runs 100% locally; telemetry can be disabled |
| Document RAG at scale | Built-in ingestion pipeline with chunking + embedding |
| Multi-format support | PDF, DOCX, TXT, CSV, images, audio, YouTube |
| Citations with answers | Per-chunk citations with relevance scores |
| Low-cost deployment | Single Docker container; no per-seat fees |
| Desktop + Cloud | macOS/Windows/Linux desktop + self-hosted Docker |

## Architecture: VentureMind × AnythingLLM

```
Nomad Flow Client
    │
    │  [Uploads KYC, contracts, tax docs]
    ▼
venturemind-document-intake-agent
    │
    ├─ Classify document type
    ├─ POST /api/v1/document/upload  →  AnythingLLM
    ├─ Poll embedding status
    └─ Log to venturemind-intake-log.jsonl
              │
              ▼
    AnythingLLM (Docker, self-hosted)
    ├─ Collector: parses PDF/DOCX/TXT
    ├─ Embedder: generates vector embeddings (self-hosted Ollama)
    ├─ Vector DB: LanceDB (local, no external calls)
    └─ Workspaces: legal | kyc | tax | strategy | hr | finance
              │
              ▼
    venturemind-document-intake-agent queries workspace
    └─► Returns answer + chunk-level citations
              │
              ▼
    SafetyNet Compliance Auditor reviews sensitive docs
    └─► Escalates or approves
```

## 6 Workspaces Per Client

| Workspace | Documents | Access |
|---|---|---|
| `legal-founders` | Founder agreements, shareholder docs, IP assignments | Client + Legal Swarm |
| `kyc-compliance` | Passports, national IDs, proof of address, tax IDs | Compliance Auditor only |
| `tax-knowledge` | Tax filings, treaty texts, IRS/HMRC/UAE FTA documents | Tax Strategist + Compliance |
| `strategy-plans` | Business plans, pitch decks, roadmaps | Client + Strategy Agent |
| `hr-legal` | Employment contracts, HR policies, offer letters | HR Lead + Legal Swarm |
| `finance-captable` | Cap tables, ESOP agreements, financial models | Finance Swarm + Capital Swarm |

## Supported File Types

| Type | Extension | Notes |
|---|---|---|
| PDF | `.pdf` | OCR via mdpdf fork; Korean/Chinese/Japanese support |
| Word | `.docx` | Full text extraction |
| Plain text | `.txt`, `.csv`, `.json` | Structured data supported |
| Images | `.png`, `.jpg`, `.webp` | GPT-4V or Claude Haiku descriptions |
| Audio | `.mp3`, `.wav` | Whisper transcription |
| Video | YouTube URL | Transcript extraction |
| Web | URL | Scraped and embedded |
| EPUB | `.epub` | Full text |

## LLM & Embedding Options

| Category | Options | Recommendation for VentureMind |
|---|---|---|
| Cloud | OpenAI, Azure, AWS Bedrock, Anthropic | ⚠️ Sensitive docs stay local |
| Self-hosted | Ollama (llama.cpp), LM Studio, vLLM | ✅ **Ollama + Llama 3.2** |
| Embedding | OpenAI Ada, self-hosted Nomic Embed | ✅ **Nomic Embed Text (self-hosted)** |
| Speech | Whisper (audio transcription) | ✅ Local Whisper |

## Privacy Configuration

```bash
# .env — disable ALL external calls
DISABLE_TELEMETRY=true
STORAGE_DIR=./storage  # local only
ENABLE_MULTI_USER=true
JWT_SECRET=<32-byte-hex>
```

When `DISABLE_TELEMETRY=true`, Nothing leaves the instance:
- No document content or filenames
- No chat messages
- No LLM prompts or responses
- Only anonymous event counts (can be disabled in-app)

## Integration With Other Stack Components

| Component | Integration |
|---|---|
| **Dify** (orchestration) | Dify agents query AnythingLLM via REST API for document grounding |
| **markitdown** (pre-processing) | MarkItDown converts complex docs to Markdown before AnythingLLM ingestion |
| **agent-vault** (Infisical) | API keys stored in vault; injected into AnythingLLM at runtime |
| **design-extract** (brand assets) | Extracted design tokens stored in `strategy-plans` workspace |
| **paperclip** (agent OS) | paperclip agents query AnythingLLM for client document context |

## Security Checklist

- [x] `DISABLE_TELEMETRY=true` — no data leaves the instance
- [x] Self-hosted LanceDB — no Pinecone/Weaviate cloud
- [x] Self-hosted Ollama — no OpenAI API for sensitive documents
- [x] Nomic Embed Text — self-hosted embeddings
- [x] JWT_SECRET set (32-byte hex)
- [x] Multi-user mode enabled — per-client org isolation
- [x] Document size limit: 50MB max upload
- [x] Workspace API keys rotated every 90 days
- [x] KYC workspace: Compliance Auditor access only
- [x] Intake log: `venturemind-intake-log.jsonl` — tamper-evident

## Deployment

```bash
git clone https://github.com/Mintplex-Labs/anything-llm.git
cd anything-llm/docker
cp .env.example .env
# Edit: DISABLE_TELEMETRY=true, ENABLE_MULTI_USER=true, JWT_SECRET, LANCEDB_ENABLED=true

docker compose up -d
# Dashboard: http://localhost:3000
# API: http://localhost:3001
```

## Comparison: AnythingLLM vs Dify in VentureMind

| Dimension | AnythingLLM | Dify |
|---|---|---|
| **Primary role** | Per-client private document chat | Workflow orchestration |
| **Document RAG** | ✅ Native | Via RAG pipeline node |
| **Multi-user isolation** | ✅ Built-in org/workspace model | ⚠️ Tenant config required |
| **Visual workflow canvas** | ❌ No | ✅ Yes |
| **Agent tool calls** | Basic | ✅ 50+ built-in tools |
| **LLMOps / observability** | Basic | ✅ Full |
| **Webhook support** | ✅ | ✅ |
| **Self-hosted** | ✅ Docker | ✅ Docker Compose |
| **VentureMind usage** | Document intake + private Q&A | Strategy workflows, tax engine, legal doc gen |

Both are deployed. AnythingLLM handles the document brain; Dify handles the execution workflows.