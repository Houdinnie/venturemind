# Security Prompt Pack — Section 01
## Frontend-Only Validation

**Vulnerability:** AI validates forms in the browser but skips the backend. Any technical person or bot can bypass the frontend and send raw data directly to your API — empty fields, fake emails, SQL commands disguised as usernames.

**VentureMind risk:** Intake forms, KYC submissions, founder data collection.

---

## PROMPT 1 — Check if backend validation exists

```
Review every form and user input in this project. For each one, tell me:
(1) Is there frontend validation?
(2) Is there backend validation?
(3) What specific checks does each one do?
List any inputs where the backend accepts data without validating it first.
Those are the vulnerabilities I need to fix.
```

**Expected output:** Table of inputs × validation coverage. Red = backend-missing.

---

## PROMPT 2 — Add backend validation to an existing feature

```
This feature has form validation on the frontend but not on the backend.
Add backend validation for every field:
- Check that required fields are present
- Data types are correct
- Strings don't exceed reasonable length limits
- Email fields are valid emails
- Numeric fields are within expected ranges
Reject anything that fails validation with a clear error response — don't just let it through.
```

**Expected output:** Backend validators added to every field. Invalid inputs return 400.

---

## PROMPT 3 — Full input sanitisation

```
For every place this app accepts user input — forms, search bars, URL parameters,
file uploads, query strings — make sure the input is sanitised before it's stored or displayed.
- All user input should be treated as plain text, never as executable code or database commands
- Use parameterised queries for all database operations
- Escape any HTML characters before displaying user-generated content
```

**Expected output:** Sanitisation layer on every input surface. No innerHTML with user content.

---

## PROMPT 4 — Validate data types and ranges

```
Review every API endpoint that accepts data. For each field, verify that the backend checks:
(1) The data type matches what's expected (string, number, boolean, date)
(2) Strings have maximum length limits to prevent abuse
(3) Numbers are within valid ranges
(4) Dates are valid and in expected format
(5) Enum fields only accept allowed values
(6) Arrays have a maximum length
Reject anything outside these bounds with a 400 error.
```

**Expected output:** Schemas enforced at every endpoint. Out-of-bounds = 400 reject.

---

## PROMPT 5 — Add rate-aware validation

```
Add validation that detects and blocks abuse patterns:
(1) If the same form is submitted more than 10 times in a minute from the same user or IP,
    temporarily block submissions.
(2) If a field receives data that looks like an attack (SQL keywords, script tags,
    extremely long strings), log the attempt and reject it.
(3) Add a honeypot field that's hidden from real users but filled by bots —
    reject any submission that includes it.
```

**Expected output:** Abuse pattern detection. Honeypot field added to all forms.

---

## Test yourself

Open DevTools → Network tab → find form submission → resend with modified/missing data.
If the backend accepts it, the validation isn't working.