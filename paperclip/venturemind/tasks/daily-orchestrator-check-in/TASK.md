---
name: daily-orchestrator-check-in
assignee: central-swarm-lead
schedule:
  timezone: Asia/Shanghai
  startsAt: 2026-05-09T08:00:00+08:00
  recurrence:
    frequency: daily
    interval: 1
    time:
      hour: 8
      minute: 0
---

Run the daily VentureMind orchestration check-in. Verify all 10 swarm health scores, flag any open escalations, review the week's pending execution manifests, and send a digest to Houdinnie via Telegram.

**Steps:**
1. Check swarm heartbeat statuses (all 10 swarms)
2. Review open escalations and SLA breaches
3. Flag any pending Green Button approvals
4. Report Watchdog Rule trigger count (last 24h)
5. Summarise the week's milestone targets
6. Alert immediately if any CRIMSON-level issues are open