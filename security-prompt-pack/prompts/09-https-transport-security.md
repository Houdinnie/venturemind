# Security Prompt Pack — Section 09
## HTTPS & Transport Security

**Vulnerability:** AI sends data in plain text or mixes secure and insecure content. Unencrypted traffic can be intercepted, session cookies stolen, and data modified in transit.

**VentureMind risk:** All public endpoints, payment processing, KYC data transmission.

---

## PROMPT 1 — Enforce HTTPS everywhere

```
Check if HTTPS is enforced on all routes in this application.
Add HTTP to HTTPS redirects.
Set HSTS (HTTP Strict Transport Security) header on all responses:
max-age=31536000; includeSubDomains; preload
Ensure all cookies have the Secure flag set.
Verify that all external resources (images, fonts, scripts) are loaded over HTTPS.
```

**Expected output:** HTTPS enforced. HSTS header set. No HTTP resources loaded.

---

## PROMPT 2 — Lock down CORS

```
Review the CORS configuration for this API.
Which domains are currently allowed to make requests?
If it's set to '*' (allow all origins), change it to only allow requests
from my app's domain: [your-domain.com].
Also check that only the necessary HTTP methods are allowed — not every method by default.
And verify that credentials are not being allowed with wildcard origins.
```

**Expected output:** CORS locked to specific domains. No wildcard origins with credentials.

---

## PROMPT 3 — Verify TLS configuration

```
For the production deployment, verify:
(1) TLS 1.2 or 1.3 is enforced — no TLS 1.0 or 1.1
(2) Strong cipher suites are configured — no weak or export ciphers
(3) Certificate is valid and not expired
(4) Certificate chain is complete
(5) Redirect from HTTP to HTTPS is in place
(6) External API calls from the server also use HTTPS
```

**Expected output:** TLS 1.2+. Strong ciphers. Valid certificate. All HTTPS.

---

## PROMPT 4 — Audit mixed content

```
Audit the entire application for mixed content issues:
(1) Are any resources (images, CSS, JS, fonts, iframes) loaded over HTTP?
(2) Are any third-party scripts or analytics loaded over HTTP?
(3) Are any API calls made over HTTP instead of HTTPS?
Replace every HTTP resource with HTTPS equivalents.
If a third-party resource doesn't support HTTPS, find an alternative.
```

**Expected output:** Zero HTTP resources. All content served over HTTPS.