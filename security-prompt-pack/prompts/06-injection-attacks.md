# Security Prompt Pack — Section 06
## Injection Attacks (SQL, XSS, CSRF)

**Vulnerability:** AI doesn't protect forms and endpoints from malicious input. Unsanitised user input = injection attacks.

**VentureMind risk:** All forms, URL parameters, MarkItDown document ingestion, KYC fields.

---

## PROMPT 1 — Check for SQL injection vulnerabilities

```
Review every database query in this project.
Are any of them built using string concatenation with user input?
For example: 'SELECT * FROM users WHERE id = ' + userId
If so, rewrite them using parameterised queries or prepared statements.
Show me every query you changed and explain why the original was vulnerable.
```

**Expected output:** All queries use parameterised statements. Zero string-concatenated queries.

---

## PROMPT 2 — Check for XSS vulnerabilities

```
Review every place in this app where user-generated content is displayed
on a page — comments, usernames, profile bios, search results,
any text a user typed that appears in the UI.
Is it being properly escaped or sanitised before rendering?
If any user-generated content is inserted as raw HTML
(using innerHTML, dangerouslySetInnerHTML, or equivalent),
flag it and fix it. User content should always be treated as text, never as HTML.
```

**Expected output:** All user content escaped on render. Zero raw HTML injection.

---

## PROMPT 3 — Add CSRF protection

```
Does this app have Cross-Site Request Forgery (CSRF) protection?
Check:
(1) Are state-changing requests (POST, PUT, DELETE) protected with CSRF tokens?
(2) Are authentication cookies set with sameSite=strict or sameSite=lax?
(3) Does the server verify the Origin or Referer header on sensitive requests?
If any of these are missing, add them.
Show me exactly what you changed and where.
```

**Expected output:** CSRF tokens on all state-changing requests. sameSite cookies set.

---

## PROMPT 4 — Full injection audit

```
Audit this entire application for injection vulnerabilities.
Check:
(1) All database queries for SQL injection — are they all using parameterised queries?
(2) All rendered content for XSS — is user input always escaped before display?
(3) All URL parameters for injection — are they validated and sanitised?
(4) All file uploads for path traversal —
    can a user upload a file with a malicious filename?
(5) All API endpoints for command injection —
    does any endpoint pass user input to a shell command?
List every vulnerability found and fix each one.
```

**Expected output:** Full injection audit complete. Zero vulnerabilities found.

---

## PROMPT 5 — Protect against DOM-based XSS

```
Review all client-side JavaScript for DOM-based XSS.
Check if any code reads from URL parameters (window.location, URLSearchParams),
document.referrer, or other user-controllable sources
and writes to the DOM using innerHTML, document.write, or similar.
Also check for any eval() calls with user input.
These don't involve the server at all — the attack happens entirely in the browser.
Fix every instance you find.
```

**Expected output:** Zero DOM-based XSS. No eval() with user input.

---

## PROMPT 6 — Protect against open redirects

```
Check every redirect in this app (redirects after login, after form submission,
in email links).
Can a user manipulate the redirect URL by changing a query parameter
like ?redirect=/dashboard to ?redirect=https://evil-site.com?
If so, add a whitelist of allowed redirect destinations
and reject any URL that doesn't match.
Open redirects are used in phishing attacks to make malicious links look legitimate.
```

**Expected output:** All redirects whitelisted. Manipulated redirect = rejected.

---

## Quick XSS test

Type `<script>alert('test')</script>` into any form field and submit.
If an alert box pops up for anyone who views that data = XSS vulnerability.