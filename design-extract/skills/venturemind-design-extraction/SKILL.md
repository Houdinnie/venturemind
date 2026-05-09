---
name: nomad-flow-design-extraction
description: VentureMind — extract any website's design system into DTCG tokens, Tailwind config, Figma variables, shadcn/ui theme, and multi-platform emitters (iOS SwiftUI, Android Compose, Flutter, WordPress). Designed for the Nomad Flow design intelligence layer. MIT licensed, requires Node 20+.
metadata:
  source: https://github.com/Manavarya09/design-extract
  version: v12.6.0
  license: MIT
  stars: 2400
  author: Manavarya Arya Singh
  homepage: https://designlang.app
compatibility: Created for VentureMind · Node 20+ · Playwright · MCP-aware agents
metadata:
  author: houdinnie.zo.computer
---

# VentureMind — Design Extraction Skill

## Purpose

`designlang` extracts a complete design system from any URL in seconds. VentureMind uses this to:
1. Clone competitor/inspiration designs for Nomad Flow clients
2. Audit platform consistency across multi-page Nomad Flow interfaces
3. Generate brand-aligned Tailwind/Shadcn themes from live sites
4. Produce WCAG accessibility reports for legal compliance

## Quick Start

```bash
npx designlang <url>                    # extract everything
npx designlang <url> --full             # screenshots + responsive + interactions
npx designlang grade <url> --badge       # shareable design score badge
npx designlang clone <url>               # working Next.js starter
npx designlang pack <url>                # bundle every output into one directory
npx designlang battle <urlA> <urlB>      # head-to-head graded battle card
```

## Outputs (17+ files per extraction)

| File | What it is |
|---|---|
| `*-design-language.md` | 19-section markdown — feed any LLM to recreate the design |
| `*-design-tokens.json` | W3C DTCG tokens (primitive + semantic + composite layers) |
| `*-tailwind.config.js` | Drop-in Tailwind theme |
| `*-shadcn-theme.css` | shadcn/ui `globals.css` variables |
| `*-figma-variables.json` | Figma Variables import (light + dark) |
| `*-anatomy.tsx` | Typed React stubs for every detected component + variants |
| `*-motion-tokens.json` | Durations, easings, springs, scroll-linked flag |
| `*-voice.json` | Brand voice — tone, pronoun posture, CTA verb inventory |
| `*-prompts/` | Paste-ready prompts for v0, Lovable, Cursor, Claude Artifacts |
| `*-mcp.json` | Disk-backed MCP server payload |
| `*-grade.html` | Shareable Design Report Card (letter grade + evidence) |
| `*-battle.html` | Head-to-head graded battle card |
| `*-remix.<vocab>.html` | Site restyled in brutalist/swiss/art-deco/cyberpunk/soft-ui/editorial |

## Integration Points

### MCP Server (Cursor / Claude Code / Windsurf)
```bash
npx designlang mcp
```
Exposes tokens, regions, components, and contrast pairs as MCP resources.

### VentureMind Pipeline
```
User Request → Strategist Agent → designlang extract <target> --full
→ 17 output files → Domain Analyst → Tailwind/Shadcn theme applied to Nomad Flow
```

### Theme-Swap (recolour around VentureMind brand)
```bash
npx designlang theme-swap <target-url> --primary "#4F46E5"
```

### Drift Detection (monitor for design changes)
```bash
npx designlang drift https://nomadflow.io --tokens ./tokens.json
```

## Key Flags

| Flag | Use |
|---|---|
| `--full` | All captures — screenshots, responsive, interactions |
| `--platforms web,ios,android,flutter,wordpress` | Multi-platform emit |
| `--emit-agent-rules` | Cursor/Claude Code/CLAUDE.md agent rule files |
| `--cookie <name=value>` | Authenticated page extraction |
| `--json` | Print full extraction as JSON to stdout |
| `--score` | 7-category design quality score |

## Safety

- Sanitize URLs from untrusted input before passing to designlang
- Use narrow `convert_*` functions for untrusted environments
- Runs with the privileges of the current process

---