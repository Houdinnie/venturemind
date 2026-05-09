# VentureMind on OpenSpace — Self-Evolving Agent Swarm
> 46% fewer tokens · 4.2× higher income · 🧬 Self-Evolving · 🌐 Collective Intelligence

---

## What Is OpenSpace?

**OpenSpace** (HKUDS, MIT) is a Python-based self-evolving agent engine. It turns any AI agent into a self-improving system through three mechanisms:

| Mechanism | What It Does |
|-----------|-------------|
| **🔧 FIX** | Repairs broken or outdated skills automatically |
| **🚀 DERIVED** | Creates specialised child skills from parent |
| **✨ CAPTURED** | Extracts novel reusable patterns from successful executions |

**Benchmarks**: 4.2× higher income, 46% fewer tokens vs baseline on 50 real-world professional tasks (GDPVal benchmark).

---

## VentureMind × OpenSpace

VentureMind's 10 domain swarms plug into OpenSpace as skills. Each swarm evolves continuously — failed patterns are repaired, successful executions become reusable skills, and new domain knowledge is captured and shared across the collective.

### Architecture

```
VentureMind (Nomad Flow)
├── 10 Domain Swarms (Paperclip)
├── SafetyNet + HITL (Compliance Auditor)
├── Hardened Sovereignty (Agent Vault / Infisical)
├── Research Layer (Feynman)
└── OpenSpace Evolution Engine ← HERE
    ├── venturemind-evolution-controller (SOUL.md)
    ├── venturemind-self-evolution (SKILL.md)
    └── evolution-manifests/ (skill lineage tracking)
```

### How It Works

**Cold Start**: Ventures from scratch using base agent capabilities + VentureMind domain knowledge.
**Warm Rerun**: Re-executes tasks with evolved skill database — dramatically lower cost, higher quality.

### Skill Categories (165 Expected)

| Category | Count | Example |
|----------|-------|---------|
| File Format I/O | 44 | PDF extraction, contract DOCX generation, ESOP xlsx |
| Execution Recovery | 29 | Sandbox fallback, error chain resolution |
| Document Generation | 26 | Compliance forms, legal agreements, KYC reports |
| Quality Assurance | 23 | KYC validation, entity formation checks |
| Task Orchestration | 17 | Multi-swarm tracking, sovereign execution pipeline |
| Domain Workflow | 13 | Entity formation, tax filing, crypto onboarding |
| Web & Research | 11 | Treaty research, jurisdiction analysis |

### Evolution Triggers

| Trigger | Mode | Frequency |
|---------|------|-----------|
| Post-execution analysis | FIX / DERIVED / CAPTURED | After every session |
| Tool degradation (>10% failure rate) | FIX | Per incident |
| Metric monitor (fallback rate >15%) | DERIVED | Hourly scan |
| 3+ consecutive successes | CAPTURED | Per pattern |

### Safety

- **Confirmation gate**: halts if confidence < 0.80
- **Anti-loop guard**: max 3 evolutions per skill per hour
- **Dangerous pattern detection**: prompt injection signatures, credential exfiltration
- **Validation gate**: evolved skill must pass 3-test suite before activation
- **HITL integration**: Compliance-critical skills (KYC, AML, tax) require human sign-off

---

## Quick Start

```bash
# Clone OpenSpace
git clone https://github.com/HKUDS/OpenSpace.git
cd OpenSpace

# Install
pip install -r requirements.txt

# Run VentureMind evolution controller
python -m openspace --config venturemind --evolution on
```

---

*Built on [HKUDS/OpenSpace](https://github.com/HKUDS/OpenSpace) · MIT · Python 3.12+*