# Security Prompt Pack — Section 14
## The Master Security Review

**The one prompt to run after every single feature you build.**
Don't try to memorise everything in this pack — just build your feature, then paste this prompt.

The AI will catch what it missed the first time.
**Run it twice.** The second pass always finds things the first pass didn't.

---

## THE MASTER SECURITY REVIEW PROMPT

Run this after every feature:

```
I just finished building [describe the feature].
Review only the new code for security issues.
Check each of these specifically:
(1) Are there permission checks on every endpoint —
    both authentication (is the user logged in?)
    and authorisation (are they allowed to access this specific resource)?
(2) Are there any hardcoded secrets, API keys, or tokens?
(3) Is user input validated on the backend, not just the frontend?
(4) Are error messages safe — no stack traces, file paths,
    or database details exposed to the user?
(5) Is all user-generated content sanitised before being stored or displayed?
(6) Are database queries using parameterised queries, not string concatenation?
(7) If there are file uploads, are they validated and stored securely?
(8) Is there rate limiting on any endpoint that could be abused?
(9) Are there any CSRF vulnerabilities in state-changing operations?
(10) Is sensitive data encrypted and not over-exposed in API responses?

Flag everything you find, fix it, and then tell me what you changed and why.
```

**Expected output:** Issues found → fixed → list of changes with explanations.

---

## WHY RUN IT TWICE?

When the AI fixes a security issue, it sometimes introduces a new pattern that has its own vulnerability.
Or fixing one issue reveals another that was hidden behind it.
The second pass catches these cascading issues.

Professional security audits work the same way — nobody finds everything on the first pass.
Run it until the AI comes back with nothing to fix. **That's when you're done.**

---

## THE PRE-LAUNCH PROMPT

Run this before you go live:

```
I'm about to deploy this app to production.
Do a comprehensive security review of the entire codebase.
Check:
(1) All secrets are in environment variables, not in code
(2) All user input is validated on the backend
(3) All database queries use parameterised queries
(4) All API endpoints have authentication and authorisation checks
(5) Error messages don't expose internal details
(6) CORS is locked to my domain only
(7) Debug mode is off
(8) All cookies have secure, httpOnly, and sameSite flags
(9) HTTPS is enforced everywhere
(10) Rate limiting is in place on login and sensitive endpoints
(11) File uploads are validated and stored securely
(12) Dependencies have no known critical vulnerabilities
(13) No test credentials, dummy data, or development artifacts remain

Give me a pass/fail for each item and fix anything that fails.
```

---

## The Security Review Workflow

```
1. Build the feature. Get it working first — don't worry about security while iterating.
2. Paste the master security review prompt above.
3. Let the AI find and fix issues. It usually catches 2-3 on the first pass.
4. Run the same prompt again. It will find a second layer.
5. Repeat until the AI comes back clean. Then move on to the next feature.
```

---

## VentureMind Integration

Every agent in the VentureMind swarm runs these prompts at defined triggers:

| Trigger | Prompt Used |
|---------|-------------|
| After every feature build | Master Security Review |
| Before production deployment | Pre-Launch Prompt |
| Weekly (automated) | Full 14-section audit |
| After dependency install | Section 12 (Outdated Dependencies) |
| After adding new API key | Section 02 (Hardcoded Secrets) |
| After building forms | Section 01 (Frontend Validation) + Section 06 (Injection) |
| After building file uploads | Section 07 (File Upload Security) |
| Before launch | Pre-Launch Prompt |

---

*Security Prompt Pack — Section 14 · Master Security Review · VentureMind v1.0*