# Security Prompt Pack — Section 05
## Sensitive Error Messages & Data Leaks

**Vulnerability:** When things break, AI tools show technical details — stack traces, file paths, database errors — to users. Attackers use these to understand your system and craft exploits.

**VentureMind risk:** All API responses, founder error messages, KYC processing errors.

---

## PROMPT 1 — Audit error responses

```
Review every error response in this app — 400s, 500s, validation errors, catch blocks.
For each one, tell me:
(1) Does it expose internal details (file paths, function names, stack traces)?
(2) Does it reveal database structure or query details?
(3) Does it confirm whether a resource exists (e.g., 'user not found' vs generic)?
(4) Does it include developer-specific information useful for attacks?
Replace all internal details with generic user-safe messages.
Log the real error details server-side for debugging.
```

**Expected output:** All errors return generic messages to users. Real errors in server logs.

---

## PROMPT 2 — Add global error handler

```
Add a global error handler (middleware) for this application.
Every unhandled exception should:
(1) Return a generic 500 error to the client with no internal details
(2) Log the full error (stack trace, request details, user ID if known)
(3) Return a correlation ID that the user can report
(4) Not expose file paths, function names, or system information
```

**Expected output:** Global handler catches all unhandled errors. No internal details leaked.

---

## PROMPT 3 — Test error response security

```
Send intentionally malformed requests to every endpoint.
Try: invalid JSON, missing required fields, SQL injection in parameters,
extremely long strings, special characters.
Review the error responses.
If any error response contains: file paths, SQL query text, function names,
internal IPs, stack traces, or framework version info — flag it and fix it.
```

**Expected output:** All malformed inputs return safe error messages.