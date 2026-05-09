# VentureMind — Pre-Launch Security Checklist
## Run before every production deployment

---

## Section 1 — Secrets & Configuration

- [ ] All secrets in environment variables, not in code
- [ ] `.env` file NOT committed to Git
- [ ] `.env.example` exists with all variable names, no values
- [ ] All API keys rotated after any accidental exposure
- [ ] No hardcoded credentials in frontend files
- [ ] Git history audited: `git log --all --full-history -- .env` → clean
- [ ] Debug mode OFF
- [ ] Verbose error logging OFF in production

---

## Section 2 — Authentication & Sessions

- [ ] Passwords hashed with bcrypt or argon2
- [ ] Access tokens expire (15-30 min)
- [ ] Refresh tokens expire (7-30 days)
- [ ] Tokens in httpOnly, secure, sameSite cookies — NOT in localStorage
- [ ] Logout invalidates token server-side
- [ ] Password reset tokens single-use, time-limited (15-30 min)
- [ ] Passwords checked against breached password list
- [ ] Login rate limited (5 attempts → 15 min block)
- [ ] Account lockout after 10 failed attempts

---

## Section 3 — Permissions & Access Control

- [ ] Every API endpoint checks authentication
- [ ] Every API endpoint checks authorisation (ownership)
- [ ] Admin routes verify admin role server-side
- [ ] Row-level security enabled on all user-data tables
- [ ] IDOR test passed: changing IDs in URLs doesn't bypass access control

---

## Section 4 — Input Validation & Sanitisation

- [ ] Backend validation on all forms and endpoints
- [ ] All user input sanitised before storage and display
- [ ] Parameterised queries on all database operations
- [ ] All user content escaped on render (no raw innerHTML)
- [ ] CSRF tokens on all state-changing requests
- [ ] URL parameters validated and sanitised
- [ ] Honeypot fields on all public forms

---

## Section 5 — File Uploads

- [ ] File type validated by magic bytes, not extension
- [ ] Max file size enforced at server level
- [ ] Double extensions rejected (e.g., file.php.jpg)
- [ ] Metadata stripped from uploaded images
- [ ] Random filenames generated, original name not used
- [ ] Files stored outside web root, not publicly accessible
- [ ] Cloud storage with access controls (S3/Supabase), not local disk
- [ ] Files served through authenticated proxy, not direct URL
- [ ] Upload rate limiting active
- [ ] Async processing for large files

---

## Section 6 — Network & Transport

- [ ] HTTPS enforced on all routes
- [ ] HSTS header set (max-age=31536000; includeSubDomains; preload)
- [ ] TLS 1.2 or 1.3 only, no TLS 1.0/1.1
- [ ] Strong cipher suites configured
- [ ] Certificate valid and not expired
- [ ] CORS locked to specific domains, not `*` with credentials
- [ ] No mixed content (all resources loaded over HTTPS)
- [ ] All cookies: Secure + httpOnly + sameSite flags

---

## Section 7 — Data Privacy

- [ ] Complete PII inventory documented
- [ ] Non-essential data fields removed
- [ ] Sensitive fields encrypted at rest (AES-256)
- [ ] Encryption keys stored separately from database
- [ ] User data deletion endpoint functional (GDPR)
- [ ] User data export endpoint functional (GDPR)
- [ ] Logs reviewed — no passwords, tokens, or full card numbers in logs
- [ ] Log files not publicly accessible

---

## Section 8 — Dependencies

- [ ] `npm audit` / `pip audit` run — no critical vulnerabilities
- [ ] All dependencies pinned to exact versions
- [ ] Lock file committed to Git
- [ ] No dependencies from non-registry sources
- [ ] No abandoned packages (last update > 12 months = replaced)
- [ ] GitHub Dependabot enabled
- [ ] CI/CD fails on critical vulnerabilities

---

## Section 9 — Logging & Monitoring

- [ ] Security events logged (login, auth failures, admin actions, data access)
- [ ] Logs in append-only store, not modifiable by app
- [ ] Alerts active: 10+ failed logins / 5 min / account
- [ ] Alerts active: single IP hitting 50+ accounts
- [ ] Alerts active: bulk data access detected
- [ ] Alerts active: new country / unusual IP per user
- [ ] Application health monitoring active
- [ ] Error rate alerting active
- [ ] Uptime monitoring active

---

## Section 10 — Infrastructure & Config

- [ ] Database not publicly accessible from internet
- [ ] No default credentials remaining
- [ ] API docs endpoints disabled or password-protected
- [ ] No test data or dev accounts in production DB
- [ ] Backup storage encrypted
- [ ] Pre-deployment checklist automated in CI/CD

---

## Sign-Off

| Role | Name | Date |
|------|------|------|
| Builder | | |
| Security Review | | |
| Compliance Check | | |

**All items must be CHECKED before deployment. Any unchecked item = deployment blocked.**

---

*VentureMind Pre-Launch Security Checklist v1.0 · 2026-05-09*