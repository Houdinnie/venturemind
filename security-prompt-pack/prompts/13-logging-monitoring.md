# Security Prompt Pack — Section 13
## Logging, Monitoring & Audit Trails

**Vulnerability:** AI builds features but never adds monitoring. If someone is breaking into accounts, exploiting a vulnerability, or accessing data they shouldn't — you have no way of knowing. Good logging tells you what happened, when, and who did it.

**VentureMind risk:** Watchdog Agent, Compliance Auditor, all production operations.

---

## PROMPT 1 — Add security event logging

```
Add logging for these security-relevant events:
(1) All login attempts (successful and failed) with timestamp, IP, and user agent
(2) All failed authorisation attempts
    (someone trying to access a resource they don't own)
(3) All account changes (password change, email change, role change)
(4) All admin actions
(5) All data exports or bulk data access
(6) All API errors above a certain rate
Store these logs in a secure location that can't be modified
by an attacker who compromises the app.
Log format: timestamp | event_type | user_id | ip | user_agent | details
```

**Expected output:** Security event logging active. Logs in append-only secure store.

---

## PROMPT 2 — Set up alerts for suspicious activity

```
Add monitoring alerts for:
(1) More than 10 failed login attempts for a single account in 5 minutes
(2) A single IP making requests to more than 50 different user accounts
(3) Bulk data access (more than 100 records in a single query
    or rapid sequential requests)
(4) Login from a new country or unusual IP for an existing user
(5) Multiple password reset requests in a short period
(6) Any 403/401 errors above baseline
Set up email or Slack notifications for each alert.
Log every alert trigger with full context for post-incident review.
```

**Expected output:** Alerting active on all 6 patterns. Notifications configured.

---

## PROMPT 3 — Create an audit trail for sensitive data

```
For every piece of sensitive data in this app
(user profiles, payment info, admin settings, user content),
add an audit trail that records:
(1) Who accessed or modified the data
(2) When they accessed or modified it
(3) What the previous value was (for modifications)
(4) The IP address and session ID
(5) Whether the access was through the UI or directly via API
Store audit logs in a separate, append-only table that can't be modified or deleted.
```

**Expected output:** Audit trail on all sensitive data. Append-only log table.

---

## PROMPT 4 — Add health monitoring and error tracking

```
Set up application health monitoring:
(1) Track error rates — alert if errors spike above normal levels
(2) Monitor response times — alert if endpoints suddenly get slower
    (could indicate a DDoS or resource exhaustion attack)
(3) Track uptime and set up a status page
(4) Monitor disk space and database connection count
(5) Set up structured logging with severity levels (info, warn, error, critical)
Recommend free or low-cost monitoring tools:
  - Sentry (error tracking)
  - UptimeRobot (uptime monitoring)
  - Loki + Grafana (log aggregation and alerting)
  - Your hosting platform's built-in monitoring
```

**Expected output:** Health monitoring active. Error tracking configured. Alerts set.

---

## PROMPT 5 — Ensure logs don't leak sensitive data

```
Review all logging in this application. Check that logs never contain:
(1) Passwords or password hashes
(2) Full API keys or tokens
(3) Credit card numbers or full SSNs
(4) Session tokens or authentication cookies
(5) Full request bodies that include personal data
Replace sensitive values in logs with redacted versions
(e.g., 'sk_live_****1234').
Verify that log files are not publicly accessible
and have appropriate access controls.
```

**Expected output:** Logs reviewed. Sensitive data redacted. Access controls verified.