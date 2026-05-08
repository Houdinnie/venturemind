# Verification Engine — Parallel Research Loops
## Phase 2: Zero-Loophole Research System

---

## Overview

Once the `FounderProfile` is confirmed (Stage Gate 7), VentureMind's Orchestrator triggers **four simultaneous Verification Loops** that run in parallel, each owned by a dedicated agent. Results are merged before synthesis.

```
FOUNDER PROFILE CONFIRMED
         │
         ├──────────────────────────────────────────────────────┐
         │                                                      │
    ┌────▼────┐   ┌────▼────┐   ┌────▼────┐   ┌────────▼────┐
    │ Global  │   │Verification│   │Competitive│   │ Compliance │
    │Intelligence│ │& Citation │   │ Moat     │   │ Cross-Check │
    │  Loop   │   │  Layer   │   │ Analysis │   │            │
    └────┬────┘   └────┬────┘   └────┬────┘   └────────┬────┘
         │              │              │                  │
         └──────────────┴──────────────┴──────────────────┘
                              │
                    ┌─────────▼─────────┐
                    │  MERGE + SCORE    │
                    │  Confidence Check │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │  Any score <0.70?  │
                    │  → ESCALATE        │
                    │  → Guild Review    │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │  SYNTHESIS         │
                    │  Master Blueprint  │
                    └────────────────────┘
```

---

## Loop 1: Global Intelligence (Compliance Auditor Agent)

**Purpose**: Real-time regulatory scanning of all jurisdictions and industries mentioned in the FounderProfile.

### 1.1: Regulatory Change Monitor

For each country mentioned in `target_business_countries`, `current_tax_residence`, `proposed_entities`:

```
SCAN FREQUENCY: On ingestion + weekly thereafter
SCOPE:
  - Portugal NHR scheme (latest eligibility changes)
  - UAE Corporate Tax (Federal Decree-Law No. 47/2022 updates)
  - Georgia tax residency rules (Parliament of Georgia statutes)
  - Singapore withholding tax rates (IRAS publications)
  - Malta individual tax rules (MTA publications)
  - Any jurisdiction-specific Anti-BEPS rules

OUTPUT FORMAT:
  {
    "jurisdiction": "string",
    "regulatory_risk": "LOW | MEDIUM | HIGH | CRITICAL",
    "last_updated": "ISO 8601",
    "changes_detected": [
      {
        "description": "string",
        "effective_date": "ISO 8601",
        "impact_on_founder": "string",
        "confidence": 0.0-1.0,
        "source": "url"
      }
    ],
    "black_swan_alerts": []
  }
```

### 1.2: Black Swan Event Detection

**Triggered** when any of these keywords appear: "visa restriction", "banking sanctions", "capital control", "expulsion", "deportation", "AML directive", "tax treaty termination", "secondary sanctions":

```
SCAN SOURCES:
  - Reuters (regulatory section)
  - Financial Times (global markets)
  - EU Official Journal
  - OFAC SDN list (updates)
  - UN Security Council Resolutions
  - Country-specific government portals

BLACK SWAN THRESHOLD:
  - Regulatory reversal risk: HIGH if any treaty partner country is in geopolitical conflict
  - Banking access risk: HIGH if any target country is under FATF review
  - Visa risk: HIGH if any target country is changing visa-on-arrival rules

OUTPUT: alert with severity score and recommended action
```

### 1.3: News Triage Filter

**Triggered** on industry mentions. Example for a forex SaaS founder:

```
INDUSTRY: Foreign Exchange / Forex Trading
SCAN FOR:
  - FCA (UK) regulatory changes for CFD/fx platforms
  - CySEC (Cyprus) leverage restrictions
  - ASIC (Australia) product intervention orders
  - CFTC (US) regulations for retail forex
  - SEBI (India) forex trading restrictions
  - BaFin (Germany) product classification

FILTER CRITERIA:
  - Published in last 90 days
  - Affects B2B or B2C forex service providers
  - Mandatory compliance requirements
  - Licensing or registration changes
```

---

## Loop 2: Verification & Citation Layer

**Purpose**: Every recommendation must be backed by a citable primary source. No "AI logic" without a source.

### 2.1: Source Classification

| Source Type | Confidence | Examples |
|-------------|-----------|----------|
| Government Primary | 1.0 | OECD tax treaties, government statutes, official IRD/IRS publications |
| Government Secondary | 0.9 | Government agency websites, official gazettes |
| Authoritative Third-Party | 0.8 | Big 4 tax guides (PWC, Deloitte, KPMG, EY), law firm client briefings |
| Industry Standard | 0.7 | Nomad Capitalist, Flag Theory, widely-cited blogs with named sources |
| Agent Inference | ≤ 0.6 | Any reasoning without a cited source → BLOCK until cited |

### 2.2: Citation Enforcement Rule

```
FOR EACH RECOMMENDATION IN THE BLUEPRINT:

IF no source is attached:
  → Confidence = 0.5
  → Flag: "[UNVERIFIED — needs primary source]"
  → DO NOT include in final report until source is attached

IF source is attached but is a secondary source:
  → Confidence = 0.7
  → Append disclaimer: "This information is derived from [source]. Verify independently."

IF source is a general blog or forum:
  → Confidence = 0.4
  → BLOCK from report
  → Suggest primary source alternatives
```

### 2.3: Primary Source Tracker

```
TEMPLATE FOR EVERY CITATION:
  {
    "claim": "string (exact quote of the claim being cited)",
    "source_type": "government_primary | government_secondary | authoritative_third_party | industry_standard | agent_inference",
    "source_name": "string",
    "source_url": "string",
    "publication_date": "ISO 8601",
    "cited_in_blueprint_section": "string",
    "verified_at": "ISO 8601",
    "verification_method": "manual | automated_web_search | api_lookup"
  }
```

---

## Loop 3: Competitive Moat Analysis

**Purpose**: Identify the founder's competitive position and ensure the strategy has a defined differentiator.

### 3.1: Competitor Identification

**Triggered** by `mentioned_competitors` in FounderProfile. If none listed, VentureMind auto-identifies top 5 competitors.

```
COMPETITOR UNIVERSE (for global nomad/business formation niche):
  - Nomad Capitalist (nomadcapitalist.com)
  - Flag Theory (flagtheory.com)
  - Herzen Path (herzen路径.com)
  - Global Banking Net (globalbanking.net)
  - Asset Protection Group (assetprotection.com)
  - Tax Free Today (taxfreetoday.com)
  - Offshore LP (offshore.lp)
  - Nomad Firm (nomadfirm.com)

FOR EACH COMPETITOR, ANALYZE:
  - Pricing model
  - Geographic focus
  - Service bundle
  - Target customer profile
  - Strengths
  - Weaknesses
  - How they compare to VentureMind's proposed strategy
```

### 3.2: Moat Scoring

```
MOAT FACTORS:
  1. Uniqueness Score (1–10): How unique is the proposed strategy?
  2. Defendability Score (1–10): Can the moat be copied by competitors?
  3. scalability Score (1–10): Can the strategy handle 10x more clients?

MOAT VERDICT:
  - 25–30: Strong moat — differentiation is clear and defensible
  - 18–24: Moderate moat — add more unique elements
  - 10–17: Weak moat — significant risk of commoditisation
  - < 10: No moat — recommend pivot or stronger positioning
```

### 3.3: TAM Validation

```
IF founder provided TAM:
  → Cross-reference with industry reports (IBISWorld, Statista, Grand View Research)
  → Flag discrepancies > 20% for review

IF founder did NOT provide TAM:
  → Researcher Agent estimates TAM based on:
      - Total addressable market for the niche (geographic + vertical)
      - Serviceable addressable market (countries VentureMind operates in)
      - Serviceable obtainable market (realistic client acquisition rate)
  → Report as: TAM / SAM / SOM with confidence score
```

---

## Loop 4: Compliance Cross-Check

**Purpose**: Ensure that the Tax Strategist and Entity Lawyer recommendations don't contradict each other, and that all CFC rules, transfer pricing rules, and treaty provisions are consistent.

### 4.1: Tax + Legal Consistency Matrix

```
CROSS-VALIDATE EVERY JURISDICTION PAIR:

Example: Founder proposes UAE Freezone (holding) + Portugal NHR (individual)

CHECKLIST:
  □ Does Portugal's NHR scheme tax UAE-sourced dividends at 0%?
     → Source: Portuguese Tax Authority (Autoridade Tributária)
  □ Does the UAE have CFC rules that would attribute UAE holding income to Portuguese tax residence?
     → Source: UAE Ministry of Finance
  □ Does the Portugal-UAE tax treaty exist and cover dividends?
     → Source: OECD Tax Treaties Database
  □ Is the "substance" in UAE sufficient to avoid Portuguese CFC attribution?
     → Source: Portuguese CFC Rules (IRC Art. 58-A)
  □ Will Portugal's NHR be affected by founder's days-per-year in Portugal?
     → Source: Portuguese NHR eligibility criteria

IF ANY CHECK FAILS:
  → Flag contradiction
  → Calculate tax leakage if contradiction is not resolved
  → Present to founder with: "Recommended fix: [option A] or [option B]"
```

### 4.2: High-Risk Jurisdiction Screening

```
AUTOMATICALLY FLAG IF FOUNDER PROPOSES:
  - Entity in Russia, Belarus, Iran, North Korea, Cuba → FULL STOP (sanctions)
  - Entity in secondary sanctions risk countries → CRITICAL ESCALATION
  - Entity in FATF "high-risk" or "monitored" jurisdictions → Compliance review required
  - Any combination that creates a结构性洗钱 (structured layering) pattern → FULL STOP

CURRENT FATF HIGH-RISK JURISDICTIONS:
  - North Korea
  - Iran
  - Myanmar

CURRENT FATF MONITORED JURISDICTIONS:
  - (Check current FATF list at each ingestion)
  - As of 2026: Bulgaria, Cameroon, Croatia, Haiti, Iraq, Kenya, Laos, Lebanon, Mongolia, Morocco, Mozambique, Namibia, Nigeria, Philippines, Senegal, South Africa, South Sudan, Syria, Tanzania, Uganda, Venezuela, Vietnam, Yemen, Zimbabwe

NOTE ON ZIMBABWE:
  - Zimbabwe is on FATF monitored list (as of available records)
  - This does NOT prevent service but REQUIRES enhanced due diligence (EDD)
  - Must trigger: Enhanced KYC check + source of funds documentation
  - Source: FATF Public Statement + Zimbabwe Financial Intelligence Unit requirements
```

### 4.3: Transfer Pricing Preview

```
IF founder has:
  - Multiple entities across jurisdictions
  - Cross-jurisdiction transactions > $10,000
  - Related-party transactions (even between personal and corporate)

THEN RUN:
  □ OECD Transfer Pricing Guidelines (Chapter I–IX) compliance check
  □ Arm's length principle verification
  □ Country-by-Country Report (CbCR) requirement check (revenue > €750M threshold — FYI only)
  □ Thin capitalisation rules check for each jurisdiction pair

OUTPUT:
  Transfer pricing risk score: LOW / MEDIUM / HIGH
  Recommended documentation level for Phase 1: Master file + Local file
```

---

## Verification Result Schema

```json
{
  "founder_profile_id": "uuid",
  "verification_run_at": "ISO 8601",
  "overall_confidence_score": 0.87,
  "passed": true,
  "loops": {
    "global_intelligence": {
      "status": "passed | flagged | escalation",
      "jurisdictions_scanned": ["Portugal", "UAE", "Singapore"],
      "regulatory_changes_found": [],
      "black_swan_alerts": [],
      "confidence_score": 0.92
    },
    "verification_citation": {
      "status": "passed | flagged | escalation",
      "claims_verified": 47,
      "claims_unverified": 2,
      "primary_sources_attached": 45,
      "confidence_score": 0.85
    },
    "competitive_moat": {
      "status": "passed | flagged | escalation",
      "competitors_analysed": ["Nomad Capitalist", "Flag Theory", "Herzen Path"],
      "moat_score": 22,
      "moat_verdict": "Moderate — add more unique elements",
      "tam_confirmed": true,
      "confidence_score": 0.78
    },
    "compliance_cross_check": {
      "status": "passed | flagged | escalation",
      "jurisdiction_pairs_checked": 6,
      "contradictions_found": [
        {
          "pair": "Portugal + UAE",
          "issue": "CFC risk — Portuguese NHR may attribute UAE holding income",
          "severity": "HIGH",
          "resolution_options": ["Remove Portugal NHR", "Establish UAE substance", "Use Georgia instead"]
        }
      ],
      "sanctions_screening": "cleared",
      "fatf_high_risk_flags": ["Zimbabwe — EDD required"],
      "confidence_score": 0.90
    }
  },
  "escalations_triggered": [
    {
      "loop": "compliance_cross_check",
      "field": "nationality",
      "reason": "Zimbabwe FATF monitored list — enhanced due diligence required",
      "action": "Request source of funds + enhanced KYC documentation",
      "severity": "HIGH"
    }
  ],
  "blocks_triggered": [],
  "next_action": "proceed_to_synthesis"
}
```

---

*Verification Engine v1.0 — VentureMind / Nomad Flow*
*Owner: Houdinnie (houdinnie.zo.computer)*