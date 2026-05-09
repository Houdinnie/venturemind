# Security Prompt Pack — Section 03
## Authentication & Session Security

**Vulnerability:** AI builds a login page that works. It almost never adds: token expiry, secure cookie settings, proper logout, password requirements, or protection against session hijacking.

**VentureMind risk:** KYC intake, founder onboarding, admin swarm access.

---

## PROMPT 1 — Full authentication audit

```
Review the entire authentication system in this app. Check:
(1) How are passwords stored — are they hashed with bcrypt or argon2, not stored in plain text?
(2) Do tokens have an expiration time? What is it?
(3) Are tokens stored securely (httpOnly cookies, not localStorage)?
(4) Does the logout function actually invalidate the session/token on the server,
    or just delete it from the browser?
(5) Is there protection against brute force login attempts?
(6) Are password reset tokens single-use and time-limited?
List every issue you find.
```

**Expected output:** Auth system rated pass/fail on all 6 points. Red = vulnerability.

---

## PROMPT 2 — Fix token and session management

```
Review the session/token configuration in this app. Make sure:
(1) Access tokens expire after a short period (15-30 minutes)
(2) Refresh tokens expire after a reasonable period (7-30 days)
(3) Tokens are stored in httpOnly, secure, sameSite cookies —
    not in localStorage or sessionStorage
    (those are accessible to any JavaScript on the page)
(4) The logout endpoint invalidates the token on the server side, not just the client
(5) When a user changes their password, all existing sessions are invalidated
```

**Expected output:** Token lifecycle hardened. Logout invalidates server-side.

---

## PROMPT 3 — Add password security requirements

```
Review the signup and password change forms. Add these requirements:
(1) Minimum 8 characters
(2) Check passwords against a list of commonly breached passwords
    (use the haveibeenpwned API or a local list of the top 10,000 common passwords)
(3) Don't enforce arbitrary complexity rules (like requiring a symbol) — length matters more
(4) Rate-limit password attempts to 5 per minute
(5) Never log or display passwords in plain text anywhere —
    check console.log statements and error messages
```

**Expected output:** Password policy enforced. Breached password check active.

---

## PROMPT 4 — Secure the password reset flow

```
Review the password reset flow end to end. Check:
(1) The reset token is random, long, and unpredictable
(2) The token expires after 15-30 minutes
(3) The token can only be used once
(4) After a successful reset, all other sessions for that user are invalidated
(5) The reset email does not reveal whether an account exists —
    always show 'If an account exists, we sent a reset link' regardless
(6) The reset page validates the token before showing the form
```

**Expected output:** Reset flow secured. No account enumeration possible.

---

## PROMPT 5 — Protect against session fixation and hijacking

```
Add these session security measures:
(1) Generate a new session ID after every successful login (prevents session fixation)
(2) Bind sessions to the user's IP or user agent and flag or invalidate sessions
    that suddenly change
(3) Add a 'log out everywhere' feature that lets users invalidate all active sessions
(4) Set the secure flag on all cookies so they're only sent over HTTPS
(5) Set sameSite=strict or sameSite=lax on authentication cookies
    to prevent CSRF attacks
```

**Expected output:** Sessions bound to user/IP. 'Log out everywhere' available.

---

## Test yourself

Open DevTools → Application → Cookies.
If auth cookies don't have httpOnly, secure, and sameSite flags = vulnerable.
If tokens are in localStorage = vulnerable.