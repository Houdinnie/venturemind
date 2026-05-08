# Onboard Assistant — UI Specification
## Phase 1: Deep Discovery Interface

---

## Overview

The **Onboard Assistant** is the primary UI component for the Structured Discovery Phase. It replaces the generic chat input with a dynamic, multi-step onboarding flow that guides founders through all 7 Stage Gates.

**Design Philosophy**: "Form with intelligence, not form with bureaucracy." Each step should feel like a conversation, not a government form.

---

## Design System

**Aesthetic**: Dark terminal / monospace — matching the VentureMind brand DNA
**Primary Font**: `JetBrains Mono` or `Fira Code` (monospace fallback: `Courier New`)
**Color Palette**:
```
--bg-primary:      #0a0a0f   (near-black background)
--bg-secondary:    #12121a   (card/panel background)
--bg-tertiary:     #1a1a25   (input fields, hover states)
--text-primary:    #e8e8f0   (main text)
--text-muted:      #6b6b80   (secondary text, labels)
--accent-cyan:     #00d4ff   (primary CTA, active states)
--accent-amber:    #ffb800   (warnings, escalation flags)
--accent-red:      #ff3b5c   (critical alerts, full stops)
--accent-green:    #00e5a0   (success, completion)
--border:          #2a2a3a   (subtle borders)
```

---

## Screen Flow

### Screen 0: Legal Agreement (Consent Gate)

**Before ANY question is asked**, the user lands here.

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   🛡️  VENTUREMIND — SAFETY NET AGREEMENT                   │
│                                                             │
│   Before we begin, please review and acknowledge            │
│   the following non-negotiable terms:                      │
│                                                             │
│   ┌───────────────────────────────────────────────────────┐ │
│   │ ☐ I understand VentureMind provides general guidance  │ │
│   │   only and does NOT constitute legal, tax, or         │ │
│   │   financial advice.                                   │ │
│   └───────────────────────────────────────────────────────┘ │
│                                                             │
│   ┌───────────────────────────────────────────────────────┐ │
│   │ ☐ I consent to my data being processed to generate    │ │
│   │   a business strategy report.                         │ │
│   └───────────────────────────────────────────────────────┘ │
│                                                             │
│   ┌───────────────────────────────────────────────────────┐ │
│   │ ☐ I confirm I will seek licensed local professionals  │ │
│   │   before making any entity or tax decisions.           │ │
│   └───────────────────────────────────────────────────────┘ │
│                                                             │
│   ┌───────────────────────────────────────────────────────┐ │
│   │ ☐ I release VentureMind from liability for any        │ │
│   │   decisions made based on AI-generated outputs.       │ │
│   └───────────────────────────────────────────────────────┘ │
│                                                             │
│   [ UNABLE TO PROCEED UNLESS ALL BOXES ARE CHECKED ]       │
│                                                             │
│   Continue →                                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Behavior**:
- "Continue" button is disabled until all 4 checkboxes are checked
- On continue: Animate consent signature into audit log, store timestamp + IP hash
- Transition to Screen 1

---

### Screen 1: Welcome + Stage Gate 1 — Identity & Foundations

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE 1 OF 7 — IDENTITY & FOUNDATIONS                      │
│  ▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░ 14%                         │
│                                                             │
│  What is your full legal name?                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  What is your current country of tax residence?             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ [Start typing to search...              ▼]          │   │
│  │  ┌─────────────────────────────────────┐           │   │
│  │  │ 🇵🇹 Portugal                        │           │   │
│  │  │ 🇦🇪 United Arab Emirates             │           │   │
│  │  │ 🇬🇪 Georgia                          │           │   │
│  │  │ 🇸🇬 Singapore                        │           │   │
│  │  └─────────────────────────────────────┘           │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  What passport(s) do you hold?                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ [+ Add passport]                                    │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  What is your primary business type?                       │
│  [ SaaS ] [ Freelance ] [ E-commerce ] [ Crypto ]           │
│  [ Holding ] [ Agency ] [ Investment ] [ Other ]           │
│                                                             │
│                                        [ Next Stage → ]   │
└─────────────────────────────────────────────────────────────┘
```

**Field Types**:
- Text input (name)
- Searchable country dropdown with flags (tax residence)
- Multi-select chip buttons (business type)
- Add-another list (passports)

**Branching**: Selecting "Crypto" immediately reveals a nested question:
```
│  ⚡ CRYPTO DETECTED — ADDITIONAL QUESTIONS                  │
│                                                             │
│  Do you intend to issue a token?                           │
│  [ ] Yes — Utility Token   [ ] Yes — Security Token        │
│  [ ] Yes — Stablecoin     [ ] No — No token                │
│                                                             │
│  Have you consulted a crypto-specialised lawyer?            │
│  [ ] Yes   [ ] No — I need a referral                       │
```

---

### Screen 2: Stage Gate 2 — Financial Profile

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE 2 OF 7 — FINANCIAL PROFILE                          │
│  ▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░ 28%                         │
│                                                             │
│  What is your annual revenue (USD)?                        │
│  [ < $100k ] [ $100k–$500k ] [ $500k–$2M ] [ > $2M ]      │
│  [ Or type a specific amount: ___________ USD ]            │
│                                                             │
│  What is your personal net worth (USD)?                    │
│  [ < $100k ] [ $100k–$500k ] [ $500k–$2M ]                │
│  [ $2M–$10M ] [ > $10M ]                                   │
│                                                             │
│  Do you intend to raise external capital?                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ [ ] No — Bootstrapping                               │   │
│  │ [ ] Yes — Angels                                     │   │
│  │ [ ] Yes — VCs                                        │   │
│  │ [ ] Yes — Family Office                              │   │
│  │ [ ] Yes — Token Sale                                 │   │
│  └─────────────────────────────────────────────────────┘   │
│  Amount sought (if applicable): ___________ USD            │
│                                                             │
│  Do you hold cryptocurrency assets?                        │
│  [ ] Yes — approx. ___________ USD                          │
│  [ ] No                                                    │
│                                                             │
│                                       [ Next Stage → ]    │
└─────────────────────────────────────────────────────────────┘
```

---

### Screen 3: Stage Gate 3 — Mobility & Lifestyle

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE 3 OF 7 — MOBILITY & LIFESTYLE                       │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░ 43%                         │
│                                                             │
│  In which countries do you currently hold residency/visas?  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 🇵🇹 Portugal — Residency (NHR) — Expires: Mar 2027) │   │
│  │ [+ Add another country]                              │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  What is your preferred lifestyle?                         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ e.g., "Warm climate, good wifi, expat community,     │   │
│  │       $2,000–$3,000/month cost of living"            │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  How many days per year do you spend in each country?      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Portugal: [____] days                                │   │
│  │ UAE:      [____] days                                │   │
│  │ Georgia:  [____] days                                │   │
│  └─────────────────────────────────────────────────────┘   │
│  ⚠️ This is critical for tax residence determination        │
│                                                             │
│  What is your preferred mobility level?                     │
│  [ 🌍 Fully Nomadic ]                                      │
│  [ 🔄 Rotating Base (3–6 months) ]                         │
│  [ 📍 Single New Base ]                                   │
│  [ 🏠 Maintain Current Base ]                              │
│                                                             │
│                                       [ Next Stage → ]    │
└─────────────────────────────────────────────────────────────┘
```

---

### Screen 4: Stage Gate 4 — Entity & Legal Context

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE 4 OF 7 — ENTITY & LEGAL CONTEXT                     │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░ 57%                         │
│                                                             │
│  Do you have existing entities formed?                     │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ [ ] No entities yet                                  │   │
│  │ [ ] Yes — UAE Freezone LLC                          │   │
│  │ [ ] Yes — Singapore Pte. Ltd                        │   │
│  │ [ ] Yes — Delaware C-Corp                           │   │
│  │ [ ] Yes — Other (specify below)                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  Which jurisdictions are you considering for new entities? │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 🇦🇪 UAE (Dubai / Abu Dhabi Freezone)                │   │
│  │ 🇸🇬 Singapore                                       │   │
│  │ 🇵🇹 Portugal (NHR structure)                        │   │
│  │ 🇬🇪 Georgia                                          │   │
│  │ 🇲🇹 Malta                                            │   │
│  │ [+ Add another jurisdiction]                          │   │
│  └─────────────────────────────────────────────────────┘   │
│  🔍 Substance requirements apply. Select a jurisdiction   │
│     to see what's needed.                                  │
│                                                             │
│  Do you need corporate bank accounts?                     │
│  [ ] Yes — Multi-currency (USD, EUR, GBP)                 │
│  [ ] Yes — Crypto-friendly                                 │
│  [ ] No                                                   │
│                                                             │
│                                       [ Next Stage → ]    │
└─────────────────────────────────────────────────────────────┘
```

---

### Screen 5: Stage Gate 5 — Competitive Context

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE 5 OF 7 — COMPETITIVE LANDSCAPE                       │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░ 71%                         │
│                                                             │
│  Who are your direct competitors?                           │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ e.g., "Nomad Capitalist, Flag Theory, Herzen Path"   │   │
│  └─────────────────────────────────────────────────────┘   │
│  🔍 We're running a competitive moat analysis in parallel   │
│                                                             │
│  What is your unique differentiator or 'unfair advantage'?  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  What is your target market size?                           │
│  [ ] TAM: ___________   SAM: ___________   SOM: _________  │
│  [ ] Not sure — help me estimate                           │
│                                                             │
│  What is your estimated time to market?                     │
│  [ ] Already live   [ ] < 3 months   [ ] 3–6 months       │
│  [ ] 6–12 months   [ ] > 12 months                         │
│                                                             │
│                                       [ Next Stage → ]    │
└─────────────────────────────────────────────────────────────┘
```

---

### Screen 6: Stage Gate 6 — Risk & Compliance Self-Assessment

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE 6 OF 7 — RISK & COMPLIANCE                          │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░ 85%                         │
│                                                             │
│  ⚠️ CRITICAL SECTION — Answers affect your eligibility     │
│                                                             │
│  Have you ever been sanctioned by OFAC, UN, EU, or any       │
│  national regulator?                                        │
│  [ ] No (required to proceed)                              │
│  [ ] Yes — Explain: _______________                        │
│                                                             │
│  Have you been involved in any of the following?            │
│  [ ] AML investigation                                     │
│  [ ] Tax fraud or evasion                                   │
│  [ ] Sanctions evasion                                      │
│  [ ] Financial fraud                                        │
│  [ ] None of the above                                     │
│                                                             │
│  Do you have PEP (Politically Exposed Person) status?       │
│  [ ] No   [ ] Yes — self   [ ] Yes — immediate family      │
│                                                             │
│  What is your risk tolerance for this strategy?             │
│  [ Conservative — full compliance, zero ambiguity ]         │
│  [ Moderate — calculated grey zones, always legal ]         │
│  [ Aggressive — tax efficiency first ]                      │
│  ⚠️ Moderate/Aggressive selections require additional       │
│     liability waiver before proceeding                     │
│                                                             │
│                                       [ Next Stage → ]    │
└─────────────────────────────────────────────────────────────┘
```

---

### Screen 7: Stage Gate 7 — Review & Confirm

```
┌─────────────────────────────────────────────────────────────┐
│  STAGE 7 OF 7 — REVIEW & CONFIRM                           │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ 100%                         │
│                                                             │
│  ✅ Verification in progress...                             │
│  🔍 Running: Global Intelligence Loop                      │
│  🔍 Running: Citation & Source Validation                   │
│  🔍 Running: Competitive Moat Analysis                      │
│  🔍 Running: Compliance Cross-Check                         │
│                                                             │
│  Your FounderProfile is being compiled.                    │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Profile Confidence: ████████████░░░░░ 82%            │   │
│  │                                                     │   │
│  │ 🟡 Fields flagged for review (confidence < 0.70):   │   │
│  │   • Competitive moat analysis                       │   │
│  │   • Financial details (needs documentation)         │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  Review your profile below:                                 │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                                                       │   │
│  │  Identity:      Ryan P. — Zimbabwean                  │   │
│  │  Residence:    China (Tax) / UAE (Policy)           │   │
│  │  Business:     Forex Trading SaaS — B2B              │   │
│  │  Revenue:      $100k–$500k                           │   │
│  │  Capital:      Raising $500k (VC)                    │   │
│  │  Entities:     Considering UAE + Singapore           │   │
│  │  Mobility:     Rotating base — warm climates          │   │
│  │  Risk:         Moderate                              │   │
│  │                                                       │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  [ Edit anything that looks incorrect ]                     │
│                                                             │
│  [ ✅ CONFIRM & GENERATE MY BLUEPRINT → ]                  │
│  ─────────────────────────────────────────────────────────  │
│  ⏱ Estimated generation time: 3–5 minutes                 │
└─────────────────────────────────────────────────────────────┘
```

---

### Document Upload Panel (Overlay)

Accessible via "Upload Documents" button on any screen:

```
┌─────────────────────────────────────────────────────────────┐
│  📄 DOCUMENT UPLOAD                                          │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                                                       │   │
│  │   Drag & drop or click to upload                     │   │
│  │   PDF, JPG, PNG — Max 10MB each                       │   │
│  │                                                       │   │
│  │   Accepted:                                          │   │
│  │   🛂 Passport (all pages)                           │   │
│  │   📜 Entity certificates                            │   │
│  │   📋 Tax returns (last 2 years)                     │   │
│  │   🏠 Proof of address (< 3 months)                  │   │
│  │   🌍 Residency / visa documents                     │   │
│  │                                                       │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  Uploaded:                                                   │
│  📄 passport_ryan.pdf — ✅ OCR Complete                     │
│  📄 certificate_uae_llc.pdf — ✅ Verified                  │
│  📄 tax_return_2024.pdf — ⏳ Processing...                 │
│                                                             │
│                                           [ Done ]          │
└─────────────────────────────────────────────────────────────┘
```

---

### Escalation Overlay (Full Stop Screen)

When a critical escalation trigger is hit:

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  🔴 FULL STOP — COMPLIANCE REVIEW REQUIRED                  │
│                                                             │
│  Your responses have triggered a compliance review.        │
│  Our Compliance Auditor agent has flagged the following:   │
│                                                             │
│  ┌───────────────────────────────────────────────────────┐ │
│  │ ⚠️ Adverse legal history declared                    │ │
│  │    Confidence: 1.0 (user self-reported)              │ │
│  │    Action: Guild Review required before proceeding    │ │
│  └───────────────────────────────────────────────────────┘ │
│                                                             │
│  Your intake is PAUSED. A member of The Guild will         │
│  review your case within 24 hours and contact you via:     │
│                                                             │
│  📧 Email: panamurang@gmail.com                             │
│  📱 Telegram: @Houddinie                                   │
│                                                             │
│  Reference ID: VM-2026-XXXXXX                              │
│                                                             │
│  We take compliance seriously. No data will be processed    │
│  until the review is complete.                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Component Library

| Component | States | Notes |
|-----------|--------|-------|
| `CheckboxItem` | unchecked, checked, disabled | Animated check on toggle |
| `CountryDropdown` | idle, open, searching, selected | Flag + country name + region |
| `ChipSelect` | idle, selected, disabled | Multi-select enabled |
| `RangeSlider` | default, dragging | For financial ranges |
| `ProgressBar` | stages 0–7 | Shows current stage + completion % |
| `FileUpload` | idle, drag-over, uploading, complete, error | OCR status shown per file |
| `EscalationBanner` | warning, critical | Full stop overlay on critical |
| `DisclaimerFooter` | visible, hidden | Always visible during intake |

---

## Accessibility

- All interactive elements have visible focus states
- Color is never the only indicator (icons + text + color for status)
- Minimum contrast ratio: 4.5:1
- Keyboard navigable throughout (Tab + Enter to advance)
- Screen reader labels on all form elements

---

*Onboard Assistant UI Spec v1.0 — VentureMind / Nomad Flow*
*Owner: Houdinnie (houdinnie.zo.computer)*