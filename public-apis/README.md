# VentureMind × public-apis — Curated API Catalog
> 433k stars · 47.4k forks · GPL-3.0
> Curated for 10 Domain Swarms · 60+ APIs

---

## What Is public-apis?

The internet's largest free API directory. 1,000+ hand-curated APIs across 50 categories — from finance and law to weather and blockchain. Used by millions of developers. Maintained by APILayer.

**Why it matters for VentureMind**: Every swarm needs real-time data — tax rates, legal entity data, market prices, flight schedules, crypto prices, compliance lists. Rather than building scrapers, VentureMind calls these APIs directly.

---

## Swarm API Mapping

| Swarm | APIs Used | Purpose |
|-------|-----------|---------|
| **Legal Swarm** | trademarks, trademarks-api, vatlayer,iban-validate | Trademark search, VAT validation |
| **Financial Swarm** | marketstack, fixer, exchangerate-api, stripe, tax-hacker | Stock data, currency exchange, invoicing |
| **Capital Swarm** | marketstack, ipapi, clearbit-logos | Investor signals, company intel |
| **Mobility Swarm** | aviationstack, icode, ipapi, foursquare | Flights, hotels, location intelligence |
| **Growth Swarm** | hunter, ipapi, clearbit | Lead generation, email finding, enrichment |
| **Web3 Swarm** | etherscan, bitquery, the-graph, covalent | On-chain data, DEX, smart contract verification |
| **Wealth Swarm** | marketstack, fixer, exchangerate-api, metalpriceapi | Portfolio, multi-currency, metals |
| **Journey Swarm** | foursquare, ipapi, aviationstack | Lifestyle, travel, experience discovery |
| **Engineering Swarm** | ipapi, clearbit-logos, github, gitlab | IP geolocation, company logos, code repos |
| **Compliance Swarm** | traceseved, virus-total, breach-directory | Security, breach checks, threat intel |

---

## Finance

| API | Auth | HTTPS | CORS | VentureMind Use |
|-----|------|-------|------|-----------------|
| [Marketstack](https://marketstack.com/) | apiKey | Yes | Yes | Capital & wealth swarm — stock prices, market data |
| [Fixer](https://fixer.io/) | apiKey | Yes | Yes | Financial & wealth — real-time FX rates, EUR/USD, GBP/USD |
| [ exchangerate-api](https://www.exchangerate-api.com/) | No | Yes | Yes | Multi-currency conversion for international founders |
| [ metalpriceapi](https://metalpriceapi.com/) | apiKey | Yes | Yes | Wealth swarm — gold, silver, platinum spot prices |
| [ Stripe](https://stripe.com/docs) | apiKey | Yes | Yes | Payment processing, subscription billing |

---

## Identity & Compliance

| API | Auth | HTTPS | CORS | VentureMind Use |
|-----|------|-------|------|-----------------|
| [Trademarks](https://developer.uspto.gov/ibd-docs/) | apiKey | Yes | Yes | Legal swarm — US trademark search & registration status |
| [Trademarks API](https://www.trademarks.org/developers) | apiKey | Yes | Yes | Legal swarm — global trademark lookup |
| [IBAN validate](https://ibanapi.com/) | No | Yes | Yes | Financial swarm — validate IBAN, prevent payment errors |
| [VAT layer](https://vatlayer.com/) | apiKey | Yes | Yes | Financial swarm — EU VAT number validation |
| [Breach Directory](https://breachdirectory.com/) | apiKey | Yes | Yes | Compliance swarm — check if emails appeared in breaches |
| [VirusTotal](https://developers.virustotal.com/reference) | apiKey | Yes | Yes | Compliance swarm — URL/file/hash security scanning |
| [Traceseved](https://traceseved.com/) | apiKey | Yes | Yes | Compliance swarm — domain reputation, phishing detection |

---

## Travel & Geolocation

| API | Auth | HTTPS | CORS | VentureMind Use |
|-----|------|-------|------|-----------------|
| [Aviationstack](https://aviationstack.com/) | apiKey | Yes | Yes | Mobility swarm — real-time flight tracking, booking data |
| [Foursquare](https://foursquare.com/docs) | apiKey | Yes | Yes | Journey swarm — venue search, lifestyle recommendations |
| [IPapi](https://ipapi.co/) | apiKey | Yes | Yes | All swarms — IP geolocation, timezone, currency detection |
| [iCode](https://icode.flights/) | No | Yes | Yes | Mobility swarm — IATA codes, airport data |
| [Google Flights](https://developers.google.com/qpx-express) | apiKey | Yes | Yes | Mobility swarm — flight search, price tracking |
| [Google Hotels](https://developers.google.com/hotels) | apiKey | Yes | Yes | Mobility swarm — accommodation booking |

---

## Business Intelligence

| API | Auth | HTTPS | CORS | VentureMind Use |
|-----|------|-------|------|-----------------|
| [Clearbit Logo](https://clearbit.com/docs#logo-api) | apiKey | Yes | Yes | Capital & growth — company logos, enrichment |
| [Hunter](https://hunter.io/api) | apiKey | Yes | Yes | Growth swarm — find email addresses, domain search |
| [Tomba email finder](https://tomba.io/api) | apiKey | Yes | Yes | Growth swarm — B2B email discovery |
| [GitHub](https://docs.github.com/en/rest) | OAuth | Yes | Yes | Engineering swarm — codebase analysis, contributor data |
| [GitLab](https://docs.gitlab.com/ee/api/) | apiKey | Yes | Yes | Engineering swarm — project intelligence |

---

## Blockchain & Crypto

| API | Auth | HTTPS | CORS | VentureMind Use |
|-----|------|-------|------|-----------------|
| [Etherscan](https://etherscan.io/apis) | apiKey | Yes | Yes | Web3 swarm — Ethereum explorer, contract verification |
| [Bitquery](https://graphql.bitquery.io/ide) | apiKey | Yes | Yes | Web3 swarm — on-chain GraphQL, DEX data |
| [The Graph](https://thegraph.com) | apiKey | Yes | Yes | Web3 swarm — indexed blockchain data |
| [Covalent](https://www.covalenthq.com/docs/api/) | apiKey | Yes | Yes | Web3 swarm — multi-blockchain aggregated data |
| [Chainlink](https://docs.chain.link) | No | Yes | Unknown | Web3 swarm — oracle data, price feeds |

---

## Data & Text

| API | Auth | HTTPS | CORS | VentureMind Use |
|-----|------|-------|------|-----------------|
| [OpenLibrary](https://openlibrary.org/developers/api) | No | Yes | No | All swarms — knowledge base, book data |
| [Quran API](https://github.com/fawazahmed0/quran-api) | No | Yes | Yes | Journey swarm — cultural/religious data |
| [Cat Facts](https://catfact.ninja/) | No | Yes | Yes | Engineering — test data, mock responses |
| [Bored](https://www.boredapi.com/) | No | Yes | Yes | Journey swarm — activity recommendations |
| [xkcd](https://xkcd.com/json.html) | No | Yes | Yes | Engineering — test data, creative inspiration |

---

## How VentureMind Uses public-apis

### Legal Swarm — Trademark Search
```
GET https://api.tmdnd.com/v1/search?query=NOMAD+FLOW
headers: Authorization: Bearer {VM_API_KEY}
```

### Financial Swarm — Currency Conversion
```
GET https://api.exchangerate-api.com/v4/latest/USD
→ Returns all FX rates for multi-currency billing
```

### Mobility Swarm — Flight Tracking
```
GET http://api.aviationstack.com/v1/flights?access_key={KEY}&flight_iata=BA284
→ Real-time position for capital raise investor visits
```

### Capital Swarm — Company Enrichment
```
GET https://company.clearbit.com/v2/companies/find?domain=stripe.com
→ Logo, founding date, employees, geography for investor targeting
```

### Compliance Swarm — Breach Check
```
GET https://breachdirectory.com/api/send?func=check&value=user@nomad.com
→ Did this email appear in any known data breaches?
```

---

## VentureMind API Rate Limits & Quotas

| Tier | Monthly Calls | Use Case |
|------|--------------|---------|
| Free | 1,000 | Testing & discovery |
| Starter | 50,000 | MVP operations |
| Growth | 250,000 | Production scale |
| Enterprise | Unlimited | Full platform |

---

## Adding New APIs to VentureMind

1. Create PR to `public-apis/public-apis` with the API entry
2. Once merged, add to `VentureMind/public-apis/curated/NAME.md`
3. Document auth requirements in `swarm-mappings/`
4. Add to Infisical Agent Vault under `api/<provider>/key`
5. Add to Paperclip skill under `venturemind-api-layer/`

---

*Source: [public-apis/public-apis](https://github.com/public-apis/public-apis) · 433k stars · GPL-3.0 · Curated for VentureMind v1.0*