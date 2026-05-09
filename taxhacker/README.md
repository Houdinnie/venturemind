# VentureMind × TaxHacker
> Self-hosted AI accounting as the financial nervous system for globally mobile founders.

---

## What Is TaxHacker?

**TaxHacker** (github.com/vas3k/TaxHacker) is a self-hosted AI accounting app written in **TypeScript** (99%) with **Next.js 15+**, **Prisma**, and **PostgreSQL**. It has **5,600 GitHub stars**, **887 forks**, and **11 releases** (latest v0.7.0, April 2026).

**Core capability:** Upload a photo of a receipt or a PDF invoice — TaxHacker uses AI to extract dates, amounts, vendors, line items, currencies, and tax fields, then categorises and stores everything in a structured database. It handles multi-currency (including crypto) using historical exchange rates from the transaction date.

**Why it matters for VentureMind:** Nomad Flow founders operate across 5–15+ countries simultaneously. They generate thousands of financial documents per year — receipts, invoices, contracts, exchange payments — in multiple currencies and tax jurisdictions. TaxHacker is the tool that turns that chaos into structured, analysable financial data at machine speed.

---

## The VentureMind Tax Intelligence Stack

| Layer | Technology | Role |
|-------|-----------|------|
| **Document capture** | Nomad Flow frontend | Founder uploads receipts/invoices |
| **Secure storage** | Agent Vault + S3 | Encrypted; credentials never exposed to agents |
| **AI extraction & categorisation** | TaxHacker | LLM-driven receipt/invoice analysis |
| **LLM flexibility** | Groq + Claude | Fast high-volume (Groq) + complex analysis (Claude) |
| **Tax intelligence** | Tax Strategist agent | Multi-jurisdiction position synthesis |
| **Compliance** | Compliance Auditor | Pre-filing review, audit defence |
| **Credential security** | Agent Vault | TaxHacker API keys secured at network layer |

---

## TaxHacker Capability Map for VentureMind

### Document Types Supported

| Document | Extracted Fields | Tax Relevance |
|----------|-----------------|--------------|
| Receipt (photo/PDF) | Date, amount, vendor, category, currency | Deductible expense tracking |
| VAT Invoice | Invoice number, VAT number, rate, CGST/SGST/IGST or VAT amount | VAT recovery, filing accuracy |
| GST Invoice (India) | GSTIN, place of supply, HSN code, rate, tax breakdown | India's GST filing |
| UAE Tax Invoice | TRN, Emirates, tax invoice type, VAT amount | UAE VAT compliance |
| South African Tax Invoice | VAT number, SARS compulsory fields, 15% rate verification | South Africa VAT filing |
| Multi-currency invoice | Line items, original currency, converted amount, exchange rate | Transfer pricing, CFC reporting |
| Crypto transaction | Amount, token, date, historical USD rate | Capital gains calculation |

### Multi-Currency Handling

- TaxHacker applies **historical exchange rates** from the actual transaction date
- Supported base currencies: USD, EUR, GBP, AED, INR, ZAR, SGD, and major crypto (BTC, ETH, USDC)
- All amounts normalised to the founder's primary reporting currency for tax filing

### Custom AI Extraction Prompts

TaxHacker supports writing custom prompts per field — VentureMind uses this for jurisdiction-specific extraction:

```
India GST Invoice:
  → Invoice Number, GSTIN, Place of Supply, Rate,
    CGST/SGST/IGST amounts, HSN Code, Supplier GSTIN

UAE VAT Invoice:
  → TRN, Tax Invoice type (standard/reverse-charge),
    Emirates, VAT amount, Total including VAT

South Africa SARS:
  → VAT Number, Compulsory fields per s20(2),
    15% rate verification, Supplier VAT number
```

---

## Tax Strategist Agent — Role in VentureMind

The **Tax Strategist** is the primary operator of TaxHacker within VentureMind's swarm architecture. As the Financial Swarm's tax intelligence lead, it:

1. **Maintains the Founder Tax Profile** — live Tax Position Summary per founder covering all jurisdictions of tax residency, PE risk, treaty positions, and estimated effective tax rate
2. **Operates TaxHacker** — triggers document analysis, reviews extractions, maps transactions to deductions
3. **Produces Tax Structuring Memoranda** — with legal basis, confidence score, flagged uncertainties, and human review requirements
4. **Coordinates with 4 other swarms**:
   - **Entity Lawyer** → new entity formation tax analysis
   - **Capital Swarm** → pre-transaction tax briefs (fundraising, token issuance, equity events)
   - **Compliance Auditor** → pre-filing reviews and audit defence
   - **Nomad Flow Intake** → initial founder Tax Profile from uploaded documents

### Tax Certainty Score

Before any recommendation is issued, the Tax Strategist calculates a score (0.0–1.0):

| Score | Action |
|-------|--------|
| 0.90–1.00 | Proceed; document position |
| 0.75–0.89 | Proceed with written uncertainty disclosure |
| 0.60–0.74 | Human review required before proceeding |
| 0.50–0.59 | Escalate to senior tax counsel |
| < 0.50 | Full halt; do not proceed |

---

## Compliance Calendar (Per Founder)

| Jurisdiction | Filing | Deadline | Trigger |
|-------------|--------|----------|---------|
| USA | Form 5471 | March 15 (auto) | CFC >10% ownership |
| UK | CT600 | 9 months post-year-end | UK trading / PE |
| UAE | Corporate Tax CT | 9 months post-tax period | AED taxable revenues |
| India | Transfer Pricing Form 3CE | November 30 | Related-party transactions |
| South Africa | IT14 | 12 months post-year-end | SA tax resident |

---

## Deployment Architecture

```
Founder (Mobile/Web)
  → Uploads receipt/invoice to Nomad Flow
    → Encrypted → Stored in VentureMind S3 bucket
      → Tax Strategist triggers TaxHacker API
        → TaxHacker analyses document (Groq or Claude LLM)
          → Structured transaction → Written to PostgreSQL
            → Tax Strategist maps to Tax Position Summary
              → Compliance Auditor notified if score < 0.60

All TaxHacker API calls routed through Agent Vault:
  Agent Vault Proxy (port 14322)
    → Injects TaxHacker API key at network layer
    → Agent never sees the raw API key
```

---

## Files in This Directory

```
taxhacker/
├── README.md                              ← This file
└── agents/
    ├── tax-hacker-controller/
    │   └── SOUL.md                        ← TaxHacker Controller — manages TaxHacker operations
    └── tax-strategist/
        └── SOUL.md                        ← Tax Strategist — full agent SOUL with 4 sections
```

---

## External Resources

- **TaxHacker Repo**: https://github.com/vas3k/TaxHacker
- **Demo**: https://taxhacker.app
- **Latest Release**: v0.7.0 (April 2026)
- **License**: MIT