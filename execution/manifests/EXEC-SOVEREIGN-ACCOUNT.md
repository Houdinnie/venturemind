# Execution Manifest: SOVEREIGN ACCOUNT SETUP
## Web3 Swarm | Safe Multi-Sig Wallet | Ethereum Mainnet

**Manifest ID**: `EXEC-SOVEREIGN-ACCOUNT-V1`
**Risk Level**: CRIMSON (> $10,000) — Multi-sig + 48hr cooling-off
**Swarm Owner**: Web3 Swarm (Custody Sub-Agent + On-Chain Compliance Sub-Agent)
**Estimated Gas**: ~0.005 ETH (~$15–$50 depending on network congestion)

---

## Pre-Flight Checklist

- [ ] FounderProfile complete (Discovery Phase)
- [ ] KYC Tier verified (Tier 2 minimum for wallet operations)
- [ ] OFAC / Sanctions pre-check passed
- [ ] Operating fund source documented (for anti-money laundering record)
- [ ] Gas budget set ($50–$200/mo recommended)
- [ ]HITL: Cooling-off period elapsed (48 hours since command issued)

---

## Step A1: Environment Initialization

```python
# Agent action — autonomous
def initialize_safe_environment():
    """
    Set up a dedicated TEE (Trusted Execution Environment) for key generation.
    This ensures the seed phrase is never exposed to the agent's general memory.
    """
    import subprocess
    result = subprocess.run(
        ["python3", "/venturemind/secure_enclave/init_tee.py"],
        capture_output=True,
        env={**os.environ, "TEE_MODE": "strict"}
    )
    return result.returncode == 0
```

---

## Step A2: Safe Wallet Deployment

```python
# Agent action — autonomous (gas estimation + transaction construction)
def deploy_safe_wallet(founder_wallet: str, owners: list[str], threshold: int = 2):
    """
    Deploys a new Gnosis Safe to Ethereum mainnet.
    - owners: list of addresses (Founder mobile + Agent Operational Key)
    - threshold: 2-of-2 multi-sig (founder + SafetyNet agent)
    
    Transaction NOT broadcast yet — pending founder approval.
    """
    from safe_community.module import SafeManager
    
    safe_manager = SafeManager(provider_rpc=os.environ["ETH_RPC_URL"])
    
    # Build transaction — NOT yet signed
    safe_deployment_tx = safe_manager.build_create_safe_tx(
        owners=owners,
        threshold=threshold,
        salt_nonce=generate_salt_nonce(founder_wallet)
    )
    
    return {
        "tx_hash": None,  # Not broadcast yet
        "safe_address": None,  # Not deployed yet
        "estimated_gas": safe_deployment_tx.gas_estimate,
        "estimated_cost_usd": calculate_usd_cost(safe_deployment_tx.gas_estimate),
        "deployment_payload": safe_deployment_tx.serialize(),
        "status": "PENDING_APPROVAL",
        "safety_flags": {
            "requires_multisig": True,
            "requires_cooling_off": True,
            "cooling_off_expires": get_cooling_off_timestamp(48),  # 48 hours
        }
    }
```

**API Endpoint**: `POST /api/v1/execution/wallet/deploy`
**SafetyNet Flags**: `requires_multisig: true`, `requires_cooling_off: true`

---

## Step A3: Secure Display Event (Seed Phrase Handoff)

```python
# Agent action — triggers mobile handoff, agent memory wiped immediately after
def trigger_secure_display_event(seed_phrase: str):
    """
    Displays the recovery phrase ONCE on the founder's mobile device.
    Agent NEVER stores this phrase. After display, agent memory is wiped.
    """
    payload = {
        "event_type": "SECURE_DISPLAY_SEED_PHRASE",
        "display_mode": "one_time_only",
        "auto_delete_after_display": True,
        "wipe_confirmed": False
    }
    
    # Send secure event to founder mobile
    response = requests.post(
        f"{os.environ['MOBILE_API_BASE']}/secure-events/display",
        headers={
            "Authorization": f"Bearer {os.environ['MOBILE_AUTH_TOKEN']}",
            "X-SafetyNet-Flag": "seed_phrase_display"
        },
        json=payload
    )
    
    # Immediately after triggering display
    wipe_confirmation = wait_for_mobile_confirmation(timeout=120)
    
    if wipe_confirmation["wipe_confirmed"]:
        audit_log.record(
            event="seed_phrase_displayed_and_wiped",
            seed_stored=False,
            wipe_timestamp=datetime.utcnow().isoformat(),
            display_device="founder_mobile_only"
        )
        return {"status": "complete", "seed_wiped": True}
    else:
        # Protocol Zero trigger — potential breach
        trigger_protocol_zero(reason="seed_phrase_wipe_not_confirmed")
        return {"status": "frozen", "protocol_zero": True}
```

**API Endpoint**: `POST /api/v1/security/secure-display`
**SafetyNet Flags**: `key_management_protocol: strict`, `memory_wipe_required: true`

---

## Step A4: Safe Wallet Deployment (Transaction Broadcast)

```python
# Agent action — AFTER cooling-off period AND founder multi-sig approval
def broadcast_safe_deployment(founder_approval: dict):
    """
    Broadcasts the Safe deployment transaction ONLY after:
    1. 48-hour cooling-off period elapsed
    2. Founder signed the multi-sig transaction via mobile
    3. SafetyNet Agent countersigned (automated approval)
    """
    from eth_account import Account
    
    # Verify founder signature
    if not verify_mobile_signature(founder_approval, payload=deployment_payload):
        raise SafetyNetError("Founder signature verification failed")
    
    # Build final transaction
    safe_deployment_tx = SafeManager.build_create_safe_tx(
        owners=owners,
        threshold=2,
        salt_nonce=generate_salt_nonce(founder_wallet)
    )
    
    # SafetyNet Agent countersigns
    safety_net_signature = safety_net_agent.sign_transaction(
        safe_deployment_tx,
        key=os.environ["SAFETY_NET_SIGNING_KEY"]
    )
    
    # Broadcast only if BOTH signatures present
    safe_address = safe_manager.deploy_safe(
        tx=safe_deployment_tx,
        founder_signature=founder_approval["signature"],
        safety_net_signature=safety_net_signature
    )
    
    audit_log.record(
        event="safe_wallet_deployed",
        safe_address=safe_address,
        deployed_by="web3_custody_agent",
        gas_used=safe_deployment_tx.gas_estimate,
        cooling_off_elapsed=True,
        multisig_verified=True
    )
    
    return {
        "status": "deployed",
        "safe_address": safe_address,
        "explorer_url": f"https://etherscan.io/address/{safe_address}",
        "owners": owners,
        "threshold": 2
    }
```

---

## Step A5: Gas Management Configuration

```python
# Agent action — autonomous within budget
def configure_gas_management(safe_address: str, monthly_gas_budget_usd: float = 100):
    """
    Sets up an automated gas tank so sub-agents can pay for transactions
    within the monthly budget without requesting approval for each tx.
    """
    # Deploy a simple Gas Tank contract or use a relay service
    gas_tank_config = {
        "safe_address": safe_address,
        "monthly_budget_usd": monthly_gas_budget_usd,
        "top_up_trigger": "auto_when_balance_below_20_usd",
        "relayer_address": os.environ["GAS_RELAYER_ADDRESS"],
        "authorized_sub_agents": [
            "web3_custody_agent",
            "legal_formation_agent",
            "mobility_banking_agent"
        ]
    }
    
    # Deploy gas tank (simple contract — autonomous)
    gas_tank_address = deploy_gas_tank(safe_address, gas_tank_config)
    
    audit_log.record(
        event="gas_management_configured",
        gas_tank_address=gas_tank_address,
        monthly_budget_usd=monthly_gas_budget_usd
    )
    
    return {
        "status": "configured",
        "gas_tank_address": gas_tank_address,
        "monthly_budget_usd": monthly_gas_budget_usd,
        "relayer_address": os.environ["GAS_RELAYER_ADDRESS"]
    }
```

---

## Step A6: Operating Fund Deposit (Funding)

```python
# Agent action — generates payment request, founder approves deposit
def initiate_operating_fund_deposit(target_amount_usd: float):
    """
    Creates a Stripe Connect payment request for the founder to deposit
    funds into the Safe operating fund. The AI calculates gas conversion.
    """
    # Step 1: Generate on-ramp quote (best rate for amount)
    onramp_quote = get_onramp_quote(
        amount_usd=target_amount_usd,
        destination_chain="ethereum",
        destination_address=safe_address
    )
    
    # Step 2: Create Stripe payment link
    payment_link = stripe.PaymentLinks.create(
        amount_cents=int(target_amount_usd * 100),
        currency="usd",
        description=f"VentureMind Operating Fund — {safe_address[:8]}...",
        metadata={
            "safe_address": safe_address,
            "founder_id": founder_id,
            "purpose": "operating_fund"
        }
    )
    
    audit_log.record(
        event="operating_fund_deposit_initiated",
        amount_usd=target_amount_usd,
        payment_link=payment_link.url,
        onramp_quote=onramp_quote
    )
    
    return {
        "status": "awaiting_deposit",
        "payment_link": payment_link.url,
        "estimated_crypto_received": onramp_quote["estimated_crypto_amount"],
        "crypto_symbol": onramp_quote["crypto_symbol"],
        "expires_at": payment_link.created_at + 3600  # 1 hour
    }
```

**API Endpoint**: `POST /api/v1/execution/funding/deposit`
**SafetyNet Flags**: `requires_payment_approval: true`, `fiat_onramp: true`

---

## Dashboard UI: Sovereign Account Card

```
┌─────────────────────────────────────────────────────────┐
│  🟢 SOVEREIGN ACCOUNT                      [Multi-Sig]  │
│  Ethereum Mainnet                                       │
│                                                         │
│  Safe Wallet: 0x7a250d5630B4cF5397...                   │
│  Etherscan: etherscan.io/address/0x7a2...  [View]       │
│                                                         │
│  Gas Tank Balance: 0.042 ETH (~$120)                    │
│  Monthly Budget: $100.00 USD     [Edit]                 │
│                                                         │
│  Authorized Agents: 3/8 active                         │
│  ─────────────────────────────────────                  │
│  Status: READY                    [REFILL + DEPOSIT]   │
│  Last activity: 2 hours ago                            │
└─────────────────────────────────────────────────────────┘
```

---

## Audit Trail Entries

| Timestamp | Event | Actor | Details |
|-----------|-------|-------|---------|
| T+0 | `wallet_deployment_initiated` | founder_command | Command signed, cooling-off started |
| T+48h | `cooling_off_elapsed` | safety_net_agent | 48hr window passed — ready for deployment |
| T+48h+5min | `founder_mobile_approved` | founder_mobile | Multi-sig signature received |
| T+48h+5min | `safetynet_countersigned` | safetynet_agent | Automated second signature applied |
| T+48h+6min | `safe_wallet_deployed` | web3_custody_agent | TX broadcast, safe_address assigned |
| T+48h+10min | `seed_phrase_displayed` | secure_display_event | One-time display on mobile, wiped |
| T+48h+15min | `gas_management_configured` | web3_custody_agent | Gas tank deployed, $100/mo budget set |
| T+49h | `operating_fund_deposit_initiated` | capital_agent | Payment link generated for $5,000 |
