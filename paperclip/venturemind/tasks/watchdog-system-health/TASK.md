---
name: watchdog-system-health
assignee: watchdog
schedule:
  timezone: Asia/Shanghai
  startsAt: 2026-05-09T12:00:00+08:00
  recurrence:
    frequency: hourly
    interval: 1
---

Run the Watchdog system health check. Verify all event stream subscriptions are active, all 10 Watchdog Rules are enabled, and the immutable audit log chain is intact (verify last checksum matches previousLogId chain).

**Steps:**
1. Verify LangSmith trace subscription is active
2. Verify Ghost audit log subscription is active
3. Verify Vault access log subscription is active
4. Verify Orchestrator decision log subscription is active
5. Verify Swarm heartbeat log subscription is active
6. Verify audit log chain integrity (last 10 entries)
7. Report any anomalies immediately to Central Swarm Lead