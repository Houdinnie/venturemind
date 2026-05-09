# VentureMind on Understand Anything
> "Graphs that teach > graphs that impress." — Lum1104

---

## What It Is

**Understand Anything** (github.com/Lum1104/Understand-Anything) is a Claude Code plugin that turns any codebase, knowledge base, or docs into an interactive knowledge graph you can explore, search, and ask questions about. It has **13,500 GitHub stars**, **1,200 forks**, **6 releases** (latest v2.5.0, May 2026), and **60 closed PRs**.

The key phrase from the founder: **"You just joined a new team. The codebase is 200,000 lines of code. Where do you even start?"**

Understand Anything answers that by running a multi-agent pipeline that maps every file, function, class, and dependency — then gives you an interactive dashboard with plain-English explanations, guided tours, semantic search, and diff impact analysis.

---

## Why It Matters for VentureMind

VentureMind manages complex multi-swarm operations across 10 domains, dozens of agent personas, and hundreds of skill files. Understanding the full picture — which domains interact with which, what a change in the capital structure means for the legal swarm, what dependencies exist before the engineering swarm can ship — requires exactly the kind of graph-based comprehension that Understand Anything provides.

For Nomad Flow founders, the "codebase" is their business. The Domain Analyst uses `/understand-domain` to map every domain, flow, and process step from their pitch deck, business plan, and uploaded documents.

---

## Key Features

| Feature | VentureMind Application |
|---------|------------------------|
| Structural graph — click/search nodes with plain-English explanations | Navigate the full founder business model |
| Domain view — maps code to business processes | Founder business domain extraction |
| Guided tours — auto-generated architecture walkthroughs | Onboard new advisors, co-founders, investors |
| Semantic/fuzzy search — find by name or meaning | "Which swarm handles KYC?" → instantly |
| Diff impact analysis — ripple effects before committing | Pre-change impact reports for the Orchestrator |
| Layer visualization — color-coded by API/Service/Data/UI/Utility | Identify which layers need attention |
| Persona-adaptive UI — junior dev / PM / power user modes | Tailored views for founders vs. agents vs. advisors |
| Multi-platform support — Claude Code, Codex, Cursor, Copilot, Gemini CLI | Consistent experience across all AI tooling |

---

## 6-Agent Pipeline (for reference)

| Agent | Role |
|-------|------|
| `project-scanner` | Discover files, detect languages and frameworks |
| `file-analyzer` | Extract functions, classes, imports; produce graph nodes and edges |
| `architecture-analyzer` | Identify architectural layers |
| `tour-builder` | Generate guided learning tours |
| `graph-reviewer` | Validate graph completeness and referential integrity |
| `domain-analyzer` | Extract business domains, flows, and process steps |
| `article-analyzer` | Extract entities, claims, and implicit relationships from wikis |

---

## VentureMind Integration

Three agents use Understand Anything:

| Agent | Command | Purpose |
|-------|---------|---------|
| **Intake Agent** | `/understand-domain ~/ventures/<slug>/` | Map new founder's business into domain graph |
| **Engineering Swarm Agent** | `/understand-diff` | Pre-change impact analysis for engineering decisions |
| **Domain Analyst** | `/understand-chat`, `/understand-knowledge`, `/understand-onboard` | Conversational queries, wiki parsing, onboarding tours |

---

## Quick Start

```bash
# Install (Claude Code)
/plugin marketplace add Lum1104/Understand-Anything
/plugin install understand-anything

# Extract domain graph from a founder's venture directory
/understand-domain ~/ventures/nomad-flow-founder/

# Ask questions about the graph
/understand-chat "Which domains handle compliance?"

# Generate an onboarding tour for new advisors
/understand-onboard
```