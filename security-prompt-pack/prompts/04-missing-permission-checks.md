# Security Prompt Pack — Section 04
## Missing Permission Checks

**Vulnerability:** #1 most common security vulnerability on the internet — OWASP Top 10 for years. AI builds features that work for the correct user. It almost never adds code to block the wrong user.

If someone changes the ID in the URL from `/orders/123` to `/orders/124`, they might see someone else's order. The frontend hides the button. The backend endpoint is wide open.

**VentureMind risk:** Swarm role assignments, founder data access, admin operations.

---

## PROMPT 1 — Full permission audit

```
Review every API endpoint and server action in this project. For each one, tell me:
(1) Does it check that the user is authenticated (logged in)?
(2) Does it check that the user is authorised to access this specific resource
    (not just any resource of this type)?
(3) What happens if an unauthenticated or unauthorised user sends a request
    directly to this endpoint?
Flag any endpoint that's missing either check.
```

**Expected output:** Every endpoint annotated. Missing auth = HIGH. Missing authz = CRITICAL.

---

## PROMPT 2 — Add ownership checks to a feature

```
This feature lets users view, edit, and delete their [orders/recipes/documents].
Add ownership verification to every endpoint:
before returning, updating, or deleting any record,
check that the requesting user's ID matches the owner ID on that record.
If it doesn't match, return a 403 Forbidden error.
Do not just hide buttons on the frontend — enforce this on the backend.
```

**Expected output:** Ownership check on every data operation. Unauthorized = 403.

---

## PROMPT 3 — Protect admin-only features

```
This feature should only be accessible to admin users.
Add a role check on the backend: before executing any admin action,
verify that the requesting user has an admin role in the database.
If they don't, return a 403 Forbidden.
Do not rely on frontend role checks — always verify on the server.
```

**Expected output:** Admin role verified server-side. Non-admin = 403.

---

## PROMPT 4 — Test IDOR vulnerabilities

```
Test every endpoint that takes an ID parameter (user ID, order ID, document ID).
Try accessing another user's resource by changing the ID.
List every endpoint where changing the ID gives you access to someone else's data.
Fix each one by adding ownership verification.
```

**Expected output:** IDOR test results. Any successful unauthorised access = fixed.

---

## PROMPT 5 — Row-level security for all data

```
For every database table that stores user data,
add row-level security (RLS) or equivalent access controls:
a user can only read and write their own rows.
Verify this is enforced at the database layer, not just the application layer.
If the database supports RLS (PostgreSQL, Supabase, etc.), enable it.
```

**Expected output:** RLS enabled on all user-data tables. Database-level enforcement.