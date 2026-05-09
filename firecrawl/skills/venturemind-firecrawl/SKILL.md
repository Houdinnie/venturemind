---
name: venturemind-firecrawl-scrape
description: Use Firecrawl to search, scrape, crawl, map, and interact with any website. Integrate with VentureMind swarm agents for real-time web intelligence.
metadata:
  source: firecrawl/firecrawl
  license: AGPL-3.0 / MIT (SDKs)
  attribution: mendableai / firecrawl
  version: v2.9.0
---

# VentureMind Firecrawl Skill

> Fetch web data at scale. Scrape, crawl, map, search, and interact with any URL — output is LLM-ready markdown, JSON, or screenshots.

## Setup

Requires `FIRECRAWL_API_KEY` saved in [Settings > Advanced](/?t=settings&s=advanced) as `FIRECRAWL_API_KEY`.

## Core Commands

| Command | Description |
|---|---|
| `firecrawl search [query] --limit 5` | Web search with full content |
| `firecrawl scrape [url] --formats markdown` | Scrape single URL → LLM-ready |
| `firecrawl scrape [url] --formats json` | Scrape → structured JSON |
| `firecrawl crawl [url] --limit 50` | Crawl entire domain |
| `firecrawl map [url]` | Discover all URLs on site |
| `firecrawl batch [urls.txt] --formats markdown` | Thousands of URLs async |
| `firecrawl interact exec --prompt "[action]"` | Click, scroll, type on page |

## VentureMind Agent Usage

### Python SDK
```python
from firecrawl import Firecrawl

app = Firecrawl(api_key=os.environ["FIRECRAWL_API_KEY"])

# Autonomous research — describe what you need
result = app.agent(prompt="Find pricing for Stripe, Paddle, and Lemonsafe")
print(result.data)

# Scrape competitor
doc = app.scrape("https://stripe.com/pricing", formats=["markdown"])
print(doc.markdown)

# Map entire domain for competitor analysis
map_result = app.map_url("https://paddle.com")
print(f"Discovered {len(map_result)} URLs")

# Crawl for comprehensive research
docs = app.crawl("https://lemonsqueezy.com", limit=100)
for doc in docs.data:
    print(doc.metadata.source_url, doc.markdown[:200])
```

### Node.js SDK
```javascript
import Firecrawl from '@mendable/firecrawl-js';
const app = new Firecrawl({ apiKey: process.env.FIRECRAWL_API_KEY });

const result = await app.agent({ prompt: "Find the top 5 AI agent frameworks by GitHub stars" });
const doc = await app.scrape('https://github.com/trending', { formats: ['markdown'] });
const batch = await app.batchScrape(urls, { formats: ['markdown'] });
```

## Output Formats

- **`markdown`** — Clean, LLM-ready text. Best for ingestion into Mem0 or AnyLLM.
- **`json`** — Structured fields. Best for programmatic processing.
- **`screenshot`** — Visual capture. Best for UI comparison or design-extract.
- **`html`** — Raw HTML. Best for custom parsing.

## VentureMind Integration Points

| Swarm | Use Case |
|---|---|
| **Strategist** | Competitor pricing, market size, treaty network research |
| **Capital** | Investor news, M&A activity, funding rounds |
| **Legal** | Jurisdiction tax rules, regulatory updates, treaty text |
| **Growth** | Lead discovery, ICP profiling, market trend analysis |
| **Engineering** | Tech stack research, competitor feature comparison |

## Error Handling

| Error | Action |
|---|---|
| `403 Forbidden` | Domain blocks scraping — use `Map` instead, flag as blocked |
| `429 Rate Limited` | Back off 30s, retry with exponential cooldown |
| `Timeout` | Re-scrape with lower `limit`; use `Map` for discovery |
| `No content` | Try `agent` endpoint for autonomous fallback |

## Safety Rules

- Always cite source URLs in outputs
- Respect `robots.txt` (Firecrawl enforces by default)
- Never store scraped PII beyond the session
- Flag sources older than 6 months as stale
- Never attempt >3 scrapes on the same blocked URL
