# Security Prompt Pack — Section 10
## Data Privacy & PII Handling

**Vulnerability:** AI logs personal data unnecessarily, stores it unencrypted, and provides no deletion mechanism. Under GDPR, fines can reach 4% of annual revenue. Data breaches expose personal information.

**VentureMind risk:** KYC SafetyNet tier data, founder personal information, financial records.

---

## PROMPT 1 — Map all personal data in this app

```
Create a complete inventory of every place this app collects, stores, or transmits
personal data (PII). For each piece of data, tell me:
(1) What is it (name, email, address, etc.)?
(2) Where is it collected (which form/endpoint)?
(3) Where is it stored (which database table/field)?
(4) Who can access it?
(5) Is it encrypted at rest?
(6) Is it transmitted securely?
(7) Is there a way to delete it if the user requests?
List everything — I need the complete picture.
```

**Expected output:** Complete PII inventory. All storage surfaces catalogued.

---

## PROMPT 2 — Minimise data collection

```
Review every form and data collection point in this app.
For each field, ask: do I actually need this data to provide the service?
Remove any fields that aren't strictly necessary.
For fields that are necessary, check if I can store a less sensitive version
(e.g., last four digits of phone number instead of the full number,
hashed email for analytics instead of plain text).
Apply the principle of data minimisation — collect only what you need.
```

**Expected output:** Non-essential fields removed. Sensitive fields minimised.

---

## PROMPT 3 — Encrypt sensitive data at rest

```
Review how sensitive data is stored in the database. Check:
(1) Are passwords hashed with bcrypt or argon2 (not MD5 or SHA-1)?
(2) Are other sensitive fields (SSN, payment info, health data) encrypted at rest
    using AES-256 or equivalent?
(3) Where are the encryption keys stored — are they separate from the database?
(4) Is the database itself encrypted at the storage level?
If not, enable it. List everything that needs encryption and add it.
```

**Expected output:** Passwords hashed bcrypt/argon2. Sensitive fields AES-256. Keys separate.

---

## PROMPT 4 — Add a user data deletion endpoint

```
Build a feature that lets users delete their account and all associated data.
This should:
(1) Delete all records in every table that reference this user
    (cascade through all relationships)
(2) Remove their files from storage
(3) Remove their data from any logs or analytics
(4) Send a confirmation email
(5) Actually delete the data — not just mark it as inactive
(6) Return confirmation that all data was removed
This is required by GDPR and similar laws.
```

**Expected output:** Deletion endpoint functional. All data removed, not just flagged.

---

## PROMPT 5 — Add a privacy-aware data export

```
Build a feature that lets users download all the data your app holds about them
(GDPR 'data portability'). Generate a JSON or CSV file containing:
- All their profile information
- All content they created
- All activity logs related to them
- All data stored in connected services
Make sure the export does not include other users' data,
internal system fields, or data about other users in shared contexts.
```

**Expected output:** Full data export available to users. Download in human-readable format.