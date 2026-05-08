---
name: weekly-performance-review
assignee: talent-agent
schedule:
  timezone: Asia/Shanghai
  startsAt: 2026-05-11T09:00:00+08:00
  recurrence:
    frequency: weekly
    interval: 1
    weekdays:
      - monday
    time:
      hour: 9
      minute: 0
---

Run the weekly VentureMind agent performance review. Evaluate each domain swarm's decision accuracy, source citation rate, and confidence calibration. Produce AgentPerformanceReport and flag any agent with accuracy below 80%.

**Steps:**
1. Query ImmutableAuditLog for all agent decisions (last 7 days)
2. Calculate decision accuracy (% reversed by human review)
3. Calculate source citation rate (% citing primary sources)
4. Detect hallucination patterns (confidence > 0.95 with no sources)
5. Flag agents below 80% accuracy for Central Swarm Lead review
6. Send AgentPerformanceReport to Central Swarm Lead