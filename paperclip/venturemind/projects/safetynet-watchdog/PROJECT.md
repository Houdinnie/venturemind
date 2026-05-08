---
name: safetynet-watchdog
description: The independent security and compliance monitoring layer — Watchdog Agent runs continuously, enforcing all 10 SafetyNet triggers, maintaining immutable audit logs, and alerting Houdinnie via Telegram on RED and CRIMSON events.
owner: watchdog
---

The SafetyNet Watchdog project is VentureMind's last line of defence. The Watchdog Agent subscribes to all system event streams and enforces escalation triggers without exception.

**Watchdog Rules (WD-001 to WD-010):**
- Rapid vault access detection
- API key exfil pattern detection
- Unusual jurisdiction access detection
- Agent output exfil pattern detection
- After-hours high-risk key access alerts
- KYC tier violation detection
- Hallucination cascade detection
- Document integrity failure detection
- Low confidence without human review detection
- Swarm heartbeat failure detection