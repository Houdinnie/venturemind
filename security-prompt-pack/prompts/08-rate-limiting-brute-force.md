# Security Prompt Pack — Section 08
## Rate Limiting & Brute Force

**Vulnerability:** AI builds login forms and API endpoints with no concept of "too many attempts." An attacker can try thousands of password combinations per second, create millions of fake accounts, spam your forms, or crash your server.

**VentureMind risk:** Login, signup, token generation, MarkItDown ingestion endpoint.

---

## PROMPT 1 — Add rate limiting to login

```
Add rate limiting to the login endpoint.
After 5 failed attempts from the same IP address,
block further login attempts from that IP for 15 minutes.
After 10 failed attempts for the same account (regardless of IP),
temporarily lock the account and require email verification to unlock.
Return a clear error message: 'Too many attempts. Please try again in 15 minutes.'
Log all rate-limiting events with timestamp and IP.
```

**Expected output:** Login rate limited. Account lockout after 10 failures.

---

## PROMPT 2 — Add rate limiting to all public endpoints

```
Add rate limiting to every public-facing endpoint in this app.
Use a sliding window algorithm:
- 100 requests per minute per IP for read endpoints (GET)
- 20 requests per minute per IP for write endpoints (POST, PUT, DELETE)
- 5 requests per minute per IP for auth-related endpoints (login, signup, password reset)
Return 429 Too Many Requests when the limit is exceeded.
Include rate limit headers in all responses:
X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset
```

**Expected output:** Sliding window rate limits. 429 responses. Headers present.

---

## PROMPT 3 — Prevent brute force on signup

```
Add rate limiting to the signup endpoint:
No more than 5 signup attempts per IP per hour.
No more than 3 accounts from the same IP per day.
Return a clear message when blocked.
Log all blocked attempts.
```

**Expected output:** Signup rate limited. Fake account creation blocked.

---

## PROMPT 4 — Add honeypot fields to all forms

```
Add a hidden honeypot field to every form in this app
(signup, login, contact, password reset, KYC submission).
The field should be: display:none or visibility:hidden in CSS,
not tabindex-able, and named something tempting to bots
(e.g., website_url, home_phone, fax).
Any form submission that includes the honeypot field = bot.
Reject it silently, return a success message to the bot,
but don't actually process the submission.
Log all honeypot triggers.
```

**Expected output:** Honeypot on all forms. Bot submissions silently rejected.

---

## PROMPT 5 — Detect anomalous request patterns

```
Add detection for anomalous request patterns that indicate abuse:
(1) Same endpoint called 100+ times in 1 minute from the same IP
(2) Sequential ID enumeration (e.g., /users/1, /users/2, /users/3...)
(3) Same form submitted with different values from the same IP rapidly
(4) Requests with missing or suspicious User-Agent headers
(5) Requests that bypass the frontend entirely and hit the API directly
Block or throttle suspicious patterns.
Log all detections with full request context.
```

**Expected output:** Anomaly detection active. Suspicious patterns blocked and logged.