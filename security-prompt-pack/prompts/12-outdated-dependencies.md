# Security Prompt Pack — Section 12
## Outdated & Vulnerable Dependencies

**Vulnerability:** Your app depends on dozens of open-source packages. Those packages sometimes have security vulnerabilities discovered after you installed them. If you never update, you're running code with known holes publicly documented and easy to exploit.

**VentureMind risk:** All npm/pip/bun packages across the full stack.

---

## PROMPT 1 — Run a security audit

```
Run a security audit on this project's dependencies.
Use the equivalent of:
  npm audit          (JavaScript/Node)
  pip audit         (Python)
  bundle audit      (Ruby)
  cargo audit       (Rust)
List every vulnerability found, its severity (low / medium / high / critical),
which package it's in, and what version fixes it.
Then apply all safe fixes that don't require major version changes.
For any that do require major version changes, tell me what might break
so I can decide.
```

**Expected output:** Vulnerability report. Critical/High items fixed. Medium/Low documented.

---

## PROMPT 2 — Check for abandoned packages

```
Review every dependency in this project. Flag any package where:
(1) The last update was more than 12 months ago
(2) The GitHub repository is archived or has no recent activity
(3) It has known vulnerabilities with no fix available
For each flagged package, suggest a well-maintained alternative I should switch to.
```

**Expected output:** Abandoned packages flagged. Recommended alternatives provided.

---

## PROMPT 3 — Safe update strategy

```
I want to update the dependencies in this project safely.
Create a plan that updates one package at a time,
starting with the ones that have known security vulnerabilities.
After each update, tell me what to test to make sure nothing broke.
If something breaks, tell me how to roll back that specific update.
Do not update everything at once.
```

**Expected output:** Incremental update plan. Rollback steps documented per package.

---

## PROMPT 4 — Lock down dependency versions

```
Review the package.json (or equivalent) for this project. Check:
(1) Are dependency versions pinned (exact versions like 2.1.3)
    or using ranges (^2.1.3 or ~2.1.3)?
(2) Is there a lock file (package-lock.json, yarn.lock, poetry.lock)
    and is it committed to Git?
(3) Are there any dependencies installed from Git URLs, tarballs,
    or other non-registry sources that could be tampered with?
Pin all versions and commit the lock file.
```

**Expected output:** All versions pinned. Lock file committed. Zero non-registry sources.

---

## PROMPT 5 — Enable automated vulnerability monitoring

```
Enable automated vulnerability monitoring for this project:
(1) GitHub Dependabot — enable on the repository
    (Settings → Security → Dependabot alerts)
(2) npm audit in CI/CD — fail builds on critical vulnerabilities
(3) Snyk or Renovate as an alternative for automated PRs on vulnerable deps
(4) Subscribe to security advisories for all major dependencies
Show me the setup steps for each.
```

**Expected output:** Dependabot enabled. CI/CD fails on critical vulnerabilities. Advisories subscribed.