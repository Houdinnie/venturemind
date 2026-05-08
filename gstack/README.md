# VentureMind + gstack — Engineering Factory
> 23 gstack skills × 10 VentureMind swarms = one autonomous platform engineering team

---

## What Is This

VentureMind runs on two frameworks:

- **Paperclip** — autonomous company package (10 domain swarms, orchestrator, watchdog)
- **gstack** — 23 engineering slash commands that run the sprint loop

gstack is not a replacement for Paperclip. It is the **engineering methodology** that Paperclip executes. When gstack's `/office-hours` produces a design doc, VentureMind's orchestrator routes it to the correct swarm. When gstack's `/cso` flags a vulnerability, VentureMind's Watchdog Agent receives the Security Event.

---

## Quickstart

```bash
# Clone VentureMind
git clone https://github.com/Houdinnie/VentureMind.git ~/VentureMind

# Add to CLAUDE.md
cat >> ~/CLAUDE.md << 'EOF'

## VentureMind Context

When working on any VentureMind project:
1. Read `~/VentureMind/SPEC.md` first
2. Read `~/VentureMind/gstack/CLAUDE.md` for context
3. Run `/venturemind-office-hours` before any new feature
4. Run `/venturemind-security-audit` before shipping vault/KYC/execution features
5. Run `/venturemind-review` on every component before committing

Available VentureMind skills:
- /venturemind-office-hours   — product interrogation, problem reframing
- /venturemind-ceo-review     — strategic challenge, scope modes
- /venturemind-security-audit — OWASP + STRIDE, zero-knowledge vault
- /venturemind-review         — code/design review, auto-fix
EOF
```

---

## The Sprint Loop

```
Think → Plan → Build → Review → Test → Ship → Reflect
```

| gstack skill | VentureMind phase |
|--------------|-------------------|
| `/office-hours` | Pre-planning — reframe the problem |
| `/plan-ceo-review` | Strategy — challenge scope before building |
| `/plan-eng-review` | Architecture — lock data flow, state machines |
| `/design-consultation` | UX design — ingestion UI, onboarding flow |
| `/design-html` | Frontend — React components, Tailwind |
| `/cso` | Security — vault, KYC, execution layer |
| `/review` | Code review — bugs, gaps, auto-fix |
| `/investigate` | Debugging — root cause, systematic |
| `/qa` | QA — browser automation, regression tests |
| `/ship` | Deploy — sync, test, PR, coverage |
| `/land-and-deploy` | Release — merge, deploy, verify production |
| `/canary` | Monitoring — post-deploy health |
| `/benchmark` | Performance — Core Web Vitals, load times |
| `/retro` | Reflect — weekly engineering retro |

---

## VentureMind gstack Skills

| Skill | File | Purpose |
|-------|------|---------|
| `venturemind-office-hours` | `skills/venturemind-office-hours/` | Product interrogation, 6 forcing questions |
| `venturemind-ceo-review` | `skills/venturemind-ceo-review/` | Strategic challenge, 4 scope modes |
| `venturemind-security-audit` | `skills/venturemind-security-audit/` | OWASP + STRIDE for vault/KYC/execution |
| `venturemind-review` | `skills/venturemind-review/` | Code/design review, auto-fix |

---

## Cross-Reference: gstack ↔ VentureMind

| gstack Role | VentureMind Equivalent |
|-------------|----------------------|
| CEO | Central Swarm Lead — strategy, orchestration |
| Eng Manager | Orchestrator Agent — swarm coordination, routing |
| Staff Engineer | Engineering Swarm Lead — architecture, review |
| Security Officer | Watchdog Agent — SafetyNet, threat detection |
| QA Lead | Journey Swarm Lead — verification loops, quality |
| Release Engineer | Execution Manifests — Green Button, deployment |
| Designer | Growth Swarm Lead — brand, onboarding UX |
| Office Hours | Strategist Agent — discovery, ingestion |
| Investigator | Financial Swarm Lead — audit, compliance |

---

## SafetyNet Integration

Every gstack skill that produces executable output must:

1. **Check the risk level** — RED or CRIMSON outputs require HITL approval
2. **Append legal disclaimers** — Tax and Legal outputs always end with disclaimer
3. **Log to AgentDecisionLog** — confidence scores, sources, reasoning
4. **Trigger Watchdog** — Security Events go to Watchdog Agent within 60 seconds
5. **Never skip stage gates** — ingestion must complete all 7 stages before execution

---

## gstack Team Mode for VentureMind

To share gstack with your team:

```bash
cd ~/VentureMind
(cd ~/.claude/skills/gstack && ./setup --team) && ~/.claude/skills/gstack/bin/gstack-team-init required && git add .claude/ CLAUDE.md && git commit -m "require gstack for VentureMind AI-assisted work"
```

This commits `.claude/` to the repo so teammates automatically get VentureMind's gstack skills without manual setup.

---

## Files

```
gstack/
├── CLAUDE.md                     ← VentureMind context for every session
├── README.md                     ← This file
├── skills/
│   ├── venturemind-office-hours/     SKILL.md
│   ├── venturemind-ceo-review/       SKILL.md
│   ├── venturemind-security-audit/   SKILL.md
│   └── venturemind-review/           SKILL.md
└── bin/
    └── gstack-venturemind-init        Team init script
```

---

*VentureMind v1.0 | gstack-powered | Nomad Flow AI Brain*