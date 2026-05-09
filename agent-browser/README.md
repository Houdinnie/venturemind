# VentureMind on agent-browser — Web Intelligence Layer
> 32.3k GitHub stars · Apache 2.0 · Rust · vercel-labs/agent-browser

---

## What Is agent-browser?

**agent-browser** (vercel-labs, Apache 2.0) is a fast native Rust CLI for browser automation — purpose-built for AI agents. 32.3k stars, 82 releases, 110 contributors, active push May 2026. Runs headless or headed, persists auth sessions, and exposes a full scripting interface for autonomous web operations.

### Key Stats

| Metric | Value |
|--------|-------|
| Stars | 32.3k |
| License | Apache 2.0 |
| Language | Rust (primary) |
| Releases | 82 |
| Contributors | 110 |
| Last push | May 2026 |

### Core Capabilities

- **Commands**: open, navigate, click, dblclick, focus, type, fill, press, hover, select, check/uncheck, scroll, drag, upload, screenshot, pdf, close
- **Selectors**: CSS, accessibility refs (@e1), semantic (role/label/text/testid/title)
- **Auth**: Chrome profile reuse, persistent profiles, session persistence, auth vault (encrypted credentials), state encryption (AES-256-GCM)
- **Sessions**: Multiple isolated browser instances, concurrent sessions
- **Data**: get text/html/value/attr/title/url/count/box/styles, accessibility snapshots
- **React**: Component tree, fiber inspection, render profiling, Suspense boundary analysis, Web Vitals
- **Security**: Content boundary markers, domain allowlisting, state encryption at rest
- **Batch**: Multi-step workflows in one CLI call (avoids per-command startup overhead)
- **Init scripts**: Pre-navigation setup for SSR flows, auth cookies, protected origins

---

## VentureMind × agent-browser

VentureMind's Browser Agent runs on agent-browser as the **web intelligence layer** — the "fingers" of the autonomous swarm. It extracts live data from any web surface, manages authentication state, captures evidence, and introspects Nomad Flow's own React UI.

### Architecture

```
VentureMind Browser Agent
├── agents/venturemind-browser-agent/SOUL.md
│   └── Full operational SOUL — responsibilities, safety, swarm integration, metrics
├── skills/venturemind-browser-autonomy/SKILL.md
│   └── SKILL.md — 20+ commands, auth vault, session isolation, React introspection
└── protocols/
    └── (protocol manifests per use-case: KYC, bank scraping, exchange extraction, etc.)
```

### Integration Map

| Swarm | Use Case | agent-browser Capability |
|-------|----------|--------------------------|
| Engineering | QA testing, React introspection, screenshot diffs | react tree, screenshot --annotate |
| Financial | Bank portal scraping, invoice capture | open + form fill + get text |
| Legal | Court docket monitoring, regulatory portals | wait --url + screenshot |
| Capital | Deal room access, investor platform login | --profile + state load |
| Growth | SEO scraping, competitor monitoring, lead enrichment | batch workflows |
| Mobility | Travel portal booking, airline check-in, visa status | click + wait + get text |
| Web3 | DEX scraping, on-chain data, wallet testing | --allowed-domains + get text |
| Compliance | KYC portal automation, document upload, verification | upload + wait --text + snapshot |

### Quick Start

```bash
# Install
npm install -g agent-browser
agent-browser install

# Open and explore
agent-browser open https://nomadflow.io
agent-browser snapshot
agent-browser screenshot dashboard.png

# Auth vault — encrypted credentials, LLM never sees passwords
echo "secret123" | agent-browser auth save deriv \
  --url https://deriv.com --username user@example.com --password-stdin
agent-browser auth login deriv

# Domain allowlisting — stay within trusted surfaces
agent-browser open \
  --allowed-domains "deriv.com,exness.com,nomadflow.io" \
  https://deriv.com

# React introspection — debug Nomad Flow's own UI
agent-browser open --enable react-devtools http://localhost:3000
agent-browser react tree
agent-browser react inspect <fiberId>
agent-browser react renders start
agent-browser react renders stop --json

# Batch workflow — one CLI call, no per-command startup overhead
agent-browser batch \
  '["open","https://deriv.com"]' \
  '["find","role","button","click","--name","Log In"]' \
  '["fill","@e3","user@example.com"]' \
  '["screenshot","login.png"]'

# Concurrent sessions (max 5)
agent-browser --session session1 open https://deriv.com &
agent-browser --session session2 open https://exness.com &
```

---

## Why agent-browser over Playwright or Puppeteer?

| Feature | agent-browser | Playwright | Puppeteer |
|---------|-------------|-----------|---------|
| Language | Rust | TypeScript | TypeScript |
| Startup time | ~200ms | ~2s | ~2s |
| CLI-first | Yes | No (code only) | No (code only) |
| Auth vault | Native | Manual | Manual |
| React introspection | Native | Partial | No |
| Batch mode | Native | Manual | Manual |
| State encryption | Native AES-256-GCM | Manual | Manual |
| Session isolation | Native | Manual | Manual |
| Domain allowlisting | Native | Manual | Manual |
| Stars | 32.3k | 65k | 82k |

agent-browser is purpose-built for AI agents. Playwright/Puppeteer are general browser automation libraries — agent-browser has agent ergonomics built in from the ground up.

---

*Built on [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) · Apache 2.0 · Rust*