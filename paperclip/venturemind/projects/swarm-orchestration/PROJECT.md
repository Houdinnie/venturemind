---
name: swarm-orchestration
description: The core orchestration engine — receives the FounderProfile and coordinates all 10 domain swarms in parallel to produce the FounderBlueprint. Runs 4 verification loops, resolves inter-swarm conflicts, assigns confidence scores, and generates execution manifests.
owner: orchestrator
---

The Swarm Orchestration project is the brain of VentureMind. It takes the verified FounderProfile and turns it into a Master Blueprint that all swarms execute against.

**Workflow:**
1. Dispatch parallel research to all relevant swarms
2. Run 4 verification loops (Global Intelligence, Citation, Competitive Moat, Compliance Cross-Check)
3. Resolve conflicts between swarms
4. Synthesise into FounderBlueprint with confidence scores
5. Route through Human Review Gate if confidence < 0.70
6. Generate execution manifests on Green Button approval