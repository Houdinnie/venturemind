# VentureMind on MarkItDown — Universal Document Ingestion
> 122k GitHub stars · MIT · Microsoft Autogen Team

---

## What Is MarkItDown?

**MarkItDown** is a Python tool from Microsoft that converts virtually any file into clean, structure-preserving Markdown — optimised for LLMs and text analysis pipelines.

**Why it matters for VentureMind:** Every founder uploads contracts, pitch decks, financial models, and legal documents. MarkItDown normalises all of them into token-efficient Markdown before they enter the agent brain — without losing headings, tables, lists, or links.

---

## Integration Architecture

```
Founder Upload (any format)
        │
        ▼
┌───────────────────────────────────────┐
│  venturemind-document-processor Agent │
│  SOUL: Zero-trust · Sanitised I/O     │
└─────────────────┬─────────────────────┘
                  │ 1. Validate file type + size
                  │ 2. Call markitdown.convert_local()
                  │ 3. Strip any extracted metadata
                  │ 4. Pass Markdown to Strategist Agent
                  ▼
┌───────────────────────────────────────┐
│  MarkItDown (Python 3.10+)             │
│  Supported inputs:                    │
│  ✅ PDF     ✅ PowerPoint             │
│  ✅ Word    ✅ Excel                  │
│  ✅ Images  ✅ Audio (transcription)  │
│  ✅ HTML    ✅ CSV / JSON / XML        │
│  ✅ ZIP     ✅ YouTube URLs           │
│  ✅ EPubs   ✅ + more                 │
└─────────────────┬─────────────────────┘
                  │ Clean Markdown output
                  ▼
┌───────────────────────────────────────┐
│  Strategist Agent                     │
│  Parses Markdown → structured context │
│  Triggers Confidence Score (CS ≥ 0.70)│
└───────────────────────────────────────┘
```

---

## Security Model

> *"Sanitize your inputs in untrusted environments."* — MarkItDown Security Considerations

MarkItDown runs with the privileges of the current process. VentureMind enforces:

| Control | Implementation |
|---------|---------------|
| Narrowest conversion API | `convert_local()` only — no remote URI fetching by default |
| File type allowlist | `.pdf .docx .xlsx .pptx .png .jpg .jpeg .zip .html .epub` |
| Size cap | 50 MB per file |
| MIME verification | Magic byte check before MarkItDown invocation |
| Metadata stripping | EXIF, author, timestamps — all removed before ingestion |
| Sandboxed subprocess | MarkItDown runs in isolated process — no filesystem access beyond temp dir |
| Prompt injection scan | All extracted text scanned for injection patterns before AI processing |

---

## VentureMind Document Pipeline (MarkItDown)

### Pipeline: `venturemind-document-pipeline`

| Stage | Agent | Action |
|-------|-------|---------|
| **1. Upload** | — | File received, type + size validated |
| **2. Sanitise** | `document-processor` | MIME check, magic bytes, size cap, EXIF strip |
| **3. Convert** | `markitdown` | `convert_local()` → clean Markdown |
| **4. Parse** | `document-processor` | Strip metadata, scan injection patterns |
| **5. Score** | `strategist-agent` | Confidence Score ≥ 0.70 → proceed; < 0.70 → Human Escalation |
| **6. Ingest** | `central-swarm-lead` | Context stored in Qdrant vector DB |

---

## Supported File Types

| Category | Extensions | MarkItDown Converter |
|----------|-----------|---------------------|
| **Documents** | `.pdf` | `PdfConverter` |
| **Office** | `.docx` `.pptx` `.xlsx` | `WordConverter` `PptxConverter` `ExcelConverter` |
| **Images** | `.png` `.jpg` `.jpeg` `.webp` | `ImageConverter` (EXIF + OCR) |
| **Audio** | `.wav` `.mp3` | `AudioConverter` (transcription + EXIF) |
| **Web** | `.html` | `HtmlConverter` |
| **Data** | `.csv` `.json` `.xml` | `TextConverter` |
| **Archives** | `.zip` | Iterates contents |
| **Video** | YouTube URL | `YoutubeConverter` (transcription) |
| **Ebooks** | `.epub` | `EpubConverter` |
| **Email** | `.msg` `.eml` | `OutlookConverter` (optional) |

---

## Skill: `venturemind-markitdown-ingestion`

```yaml
---
name: venturemind-markitdown-ingestion
description: >
  VentureMind MarkItDown document ingestion skill.
  Converts any supported file to Markdown for agent consumption.
  Always validates file before conversion. Never passes
  untrusted input to MarkItDown without sanitisation.
metadata:
  source: microsoft/markitdown
  version: 0.1.5
  license: MIT
  attribution: Microsoft Autogen Team
---
```

**Activation trigger:** Founder uploads a document (contract, pitch deck, financial model, legal filing) in any supported format.

**Agent actions:**
1. Validate file (type, size, MIME, magic bytes)
2. Run `markitdown document-processor --input <file> --output -`
3. Strip all metadata from output
4. Scan for prompt injection patterns
5. Pass clean Markdown to `strategist-agent` for Confidence Scoring

---

## VentureMind × MarkItDown — Capability Map

| VentureMind Need | MarkItDown Solution |
|-----------------|-------------------|
| Convert PDF contracts to AI-readable text | `PdfConverter` — preserves headings, tables, links |
| Extract text from founder pitch decks (PPTX) | `PptxConverter` — slide-by-slide Markdown |
| Parse financial models (Excel) | `ExcelConverter` — table format, cell values preserved |
| Ingest legal documents (DOCX) | `WordConverter` — formatting structure intact |
| OCR on scanned documents | `markitdown-ocr` plugin — LLM Vision on embedded images |
| YouTube founder interview transcription | `YoutubeConverter` — speech → Markdown |
| Process ZIP archives of mixed documents | `ZipConverter` — iterates and converts each file |
| Extract EXIF from uploaded images | `ImageConverter` — strips GPS/camera metadata |

---

## MCP Server (Optional)

MarkItDown includes an MCP server (`markitdown-mcp`) for integration with LLM apps. VentureMind can expose this as a tool to the Strategist Agent:

```bash
# Install
pip install markitdown[all] markitdown-mcp

# Run MCP server
markitdown-mcp --port 4321
```

**VentureMind usage:** The `document-processor` agent calls the MCP tool on-demand when a founder uploads a document in the Nomad Flow interface.

---

## Quick Reference

| Item | Detail |
|------|--------|
| **GitHub** | `microsoft/markitdown` |
| **Stars** | 122k ⭐ |
| **License** | MIT |
| **Language** | Python 3.10+ |
| **Latest** | v0.1.5 (Feb 2026) |
| **Install** | `pip install 'markitdown[all]'` |
| **CLI** | `markitdown file.pdf -o output.md` |
| **Security** | Always use `convert_local()`; validate + sanitise inputs first |
| **VentureMind Agent** | `venturemind-document-processor` (SOUL.md) |
| **VentureMind Skill** | `venturemind-markitdown-ingestion` (SKILL.md) |
| **Pipeline** | `venturemind-document-pipeline` (PROJECT.md) |

---

*Integrated by VentureMind SafetyNet · 2026-05-09*