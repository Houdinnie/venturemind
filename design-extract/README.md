# VentureMind on design-extract — Design Intelligence Layer
> 2,400 GitHub stars · v12.6.0 · MIT License · Manav Arya Singh

## What It Is

**design-extract** (branded as **designlang**) by Manavarya09 extracts a complete design system from any website in seconds using a headless browser. One command emits **17+ output files** including DTCG tokens, Tailwind config, Figma variables, shadcn/ui theme, motion tokens, component anatomy stubs, brand voice, and a paste-ready prompt pack for v0/Lovable/Cursor/Claude Artifacts.

## Why It Matters for VentureMind

Nomad Flow's UX/UI Swarm needs to:
- Clone competitor/inspiration designs for clients on demand
- Audit accessibility (WCAG) across multi-page interfaces
- Generate brand-aligned themes from client feedback URLs
- Keep Nomad Flow consistent as the platform scales

designlang solves all four in one pipeline.

---

## Key Capabilities

| Capability | Command | VentureMind Use |
|---|---|---|
| Full extraction | `designlang <url> --full` | Capture complete design systems |
| Design scoring | `designlang grade <url> --badge` | WCAG compliance reports |
| Clone to Next.js | `designlang clone <url>` | Generate starter projects |
| Multi-platform emit | `--platforms web,ios,android,flutter,wordpress` | iOS/Android/WordPress design tokens |
| Theme swap | `designlang theme-swap <url> --primary "#4F46E5"` | Apply VentureMind brand |
| Drift detection | `designlang drift <url> --tokens ./tokens.json` | Monitor brand consistency |
| Head-to-head battle | `designlang battle <urlA> <urlB>` | Competitive design analysis |
| Restyle | `designlang remix <url> --as cyberpunk` | Exploration of design variants |
| MCP server | `designlang mcp` | Cursor / Claude Code / Windsurf integration |

---

## Integration Architecture

```
Strategist Agent
    ↓ (design extraction request)
Design Intelligence Agent (SOUL.md)
    ↓ (runs designlang)
designlang <url> --full --platforms web,ios,android,flutter,wordpress
    ↓ (17+ output files)
┌─────────────────────────────────────┐
│  VentureMind Design Pipeline        │
├─────────────────────────────────────┤
│  → Tailwind config → Engineering    │
│  → shadcn theme → UX Swarm          │
│  → Figma vars → Design System       │
│  → WCAG report → Compliance Auditor │
│  → Brand voice → Copywriting        │
│  → Agent rules → paperclip/Cursor   │
└─────────────────────────────────────┘
```

---

## Designlang at a Glance

- **Stars**: 2,400+
- **Forks**: 216
- **License**: MIT
- **Language**: JavaScript (97.4%)
- **Runtime**: Node 20+, Playwright
- **Homepage**: designlang.app
- **Latest**: v12.6.0 (May 2026)

---

## VentureMind Integration Files

| File | Purpose |
|---|---|
| `skills/venturemind-design-extraction/SKILL.md` | Skill manifest with commands, flags, and pipeline |
| `agents/design-intelligence-agent/SOUL.md` | Design Intelligence Agent — extraction workflow, values, boundaries |
| `README.md` | This file |

## Quick Test

```bash
# Test designlang is working
npx designlang https://stripe.com --full

# Grade a competitor
npx designlang grade https://vercel.com --badge

# Extract + emit multi-platform
npx designlang https://linear.app --platforms web,ios,android --full
```

---