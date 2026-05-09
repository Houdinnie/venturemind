# VentureMind API Credentials — Managed by Infisical Agent Vault
> DO NOT commit real keys. All credentials injected at runtime via brokered access.

## Quick Reference

| API Provider | Env Variable | Auth Type | Scope |
|-------------|--------------|-----------|-------|
| Marketstack | MARKETSTACK_API_KEY | apiKey | Financial, Capital |
| Aviationstack | AVIATIONSTACK_API_KEY | apiKey | Mobility |
| Etherscan | ETHERSCAN_API_KEY | apiKey | Web3 |
| Clearbit | CLEARBIT_API_KEY | apiKey | Capital, Growth |
| Hunter | HUNTER_API_KEY | apiKey | Growth |
| Traceseved | TRACESEVED_API_KEY | apiKey | Compliance |
| VirusTotal | VIRUSTOTAL_API_KEY | apiKey | Compliance |
| ipapi | IPAPI_API_KEY | apiKey | All Swarms |
| Fixer | FIXER_API_KEY | apiKey | Financial |
| Foursquare | FOURSQUARE_API_KEY | apiKey | Journey |
| IBAN Validate | IBAN_API_KEY | apiKey | Financial |
| VATlayer | VATLAYER_API_KEY | apiKey | Financial |
| MetalPriceAPI | METALPRICE_API_KEY | apiKey | Wealth |
| BreachDirectory | BREACHDIRECTORY_API_KEY | apiKey | Compliance |

## Setup Instructions

1. Get API keys from each provider's website
2. Go to [Settings > Advanced](/?t=settings&s=advanced) in Zo Computer
3. Add each key as a Secret with the matching Env Variable name
4. The Infisical Agent Vault broker will inject keys at runtime — agents NEVER see raw keys

## Testing

```bash
# Test Marketstack
curl https://api.marketstack.com/v1/eod? access_key=$MARKETSTACK_API_KEY&symbol=AAPL

# Test Aviationstack
curl "http://api.aviationstack.com/v1/flights?access_key=$AVIATIONSTACK_API_KEY&flight_iata=BA284"

# Test Etherscan
curl "https://api.etherscan.io/api?module=gwei&action=ethgasupdate&apikey=$ETHERSCAN_API_KEY"
```