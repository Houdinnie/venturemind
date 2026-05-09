# Security Prompt Pack — Section 11
## Insecure Configuration & Defaults

**Vulnerability:** Every tool, framework, and service comes with defaults designed for convenience during development — not for production security. Debug mode, open CORS, default passwords, publicly accessible admin panels.

**VentureMind risk:** Production deployment, all third-party service configurations.

---

## PROMPT 1 — Production readiness check

```
Is this project configured for production or still in development mode?
Check for:
(1) Debug mode — is it turned off?
(2) CORS settings — is the API accepting requests from everywhere ('*')
    or only from my app's domain?
(3) Database — is it publicly accessible or restricted to the app server?
(4) Default credentials — are there any default passwords, admin accounts,
    or test credentials still in place?
(5) Error verbosity — are detailed errors being sent to the client?
List everything that needs to change before this goes live.
```

**Expected output:** Production readiness scorecard. Every dev-mode setting flagged.

---

## PROMPT 2 — Remove development artifacts

```
Scan this entire project for anything that should not be in production:
(1) Test accounts or dummy data in the database
(2) Debug flags, verbose logging, or development-only code paths
(3) API documentation endpoints (like /api/docs or /swagger)
    that should be disabled or password-protected in production
(4) Any TODO comments mentioning security
(5) Test API keys or placeholder credentials
(6) Development-only middleware or routes
Remove or disable everything you find.
```

**Expected output:** Zero dev artifacts in production. Only production-ready code deployed.

---

## PROMPT 3 — Review third-party service configuration

```
Review every third-party service connected to this app
(database, auth provider, email service, payment processor, file storage, analytics).
For each one, check:
(1) Are you using the production configuration, not development/sandbox?
(2) Are API keys restricted to only the permissions they need
    (principle of least privilege)?
(3) Are webhook endpoints verified with a signature?
(4) Are any services using default or overly permissive settings?
List each service and what needs to change.
```

**Expected output:** All third-party services production-configured. Least privilege enforced.

---

## PROMPT 4 — Create a pre-deployment checklist

```
Create a pre-deployment checklist for this project.
It should cover:
(1) Environment variables set and verified
(2) Debug mode off
(3) HTTPS enforced
(4) CORS locked
(5) Error handling set to generic messages
(6) No test data or dev accounts in production database
(7) Rate limiting enabled
(8) Logging and monitoring active
(9) Dependencies audited for vulnerabilities
(10) All secrets in environment variables, not code
Have the CI/CD pipeline run this checklist automatically before any deployment.
```

**Expected output:** Pre-deployment checklist documented. Automated in CI/CD pipeline.