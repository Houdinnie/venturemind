# Execution Manifest: NEOBANK & FIAT BRIDGE
## Mobility Swarm + Capital Swarm | Business Bank Account Setup

**Manifest ID**: `EXEC-NEOBANK-FIAT-V1`
**Risk Level**: RED ($1,000–$10,000) — Explicit multi-sig approval required
**Swarm Owner**: Mobility Swarm (Banking Navigator Sub-Agent) + Capital Swarm
**Prerequisites**: Entity Formation Complete (EXEC-ENTITY-FORMATION must be completed first)

---

## Pre-Flight Checklist

- [ ] Entity Formation complete (EIN/Tax ID received)
- [ ] FounderProfile KYC Tier 2 verified
- [ ] Documents ready: Articles of Organization, Operating Agreement, EIN letter, Founder ID
- [ ] Business address verification (virtual address or physical)
- [ ] Founder mobile device linked for biometric handoff
- [ ] HITL: Bank selection approved by founder

---

## Step C1: Bank Selection

```python
# Agent action — autonomous (research) + HITL (selection)
def recommend_neobanks(founder_profile: dict) -> dict:
    """
    The Banking Navigator Sub-Agent researches and recommends
    the best neobank(s) based on the founder's profile.
    """
    from venturemind.mobility.banking import BankingNavigator
    
    navigator = BankingNavigator(founder_profile)
    
    # Evaluate neobanks against criteria
    recommendations = navigator.evaluate_banks(
        criteria={
            "jurisdiction": founder_profile["target_jurisdiction"],
            "business_type": founder_profile["entity_type"],
            "monthly_volume_estimate": founder_profile.get("monthly_volume_usd", 50000),
            "international_wire_support": founder_profile.get("international_clients", True),
            "crypto_friendly": founder_profile.get("accepts_crypto", False),
            "employee_count": founder_profile.get("employee_count", 1),
            "industry_category": founder_profile.get("industry"]
        }
    )
    
    # Top 3 recommendations with reasoning
    return {
        "recommendations": recommendations[:3],
        "comparison_matrix": navigator.create_comparison_matrix(recommendations),
        "founder_action_required": "SELECT_BANK"
    }
```

---

## Step C2: Application Pre-Fill (Autonomous)

```python
# Agent action — autonomous (pre-fills from FounderProfile)
def prefill_bank_application(bank: str, entity_data: dict, founder_data: dict) -> dict:
    """
    Pre-fills the bank application using data from the Discovery Phase.
    The AI NEVER touches biometric data or government ID documents.
    """
    from venturemind.mobility.banking import ApplicationBuilder
    
    builder = ApplicationBuilder(bank=bank)
    
    prefill_payload = {
        "business_name": entity_data["legal_name"],
        "business_type": entity_data["entity_type"],
        "ein": entity_data["ein"],
        "formation_jurisdiction": entity_data["formation_jurisdiction"],
        "principal_address": entity_data["principal_address"],
        "business_phone": entity_data["business_phone"],
        "business_email": entity_data["business_email"],
        "website": entity_data.get("website"),
        "industry_naics_code": entity_data.get("naics_code"),
        "estimated_monthly_volume": entity_data.get("monthly_volume_usd", 50000),
        "source_of_funds": entity_data.get("source_of_funds"),
        # DO NOT include: SSN, passport number, ID scans
    }
    
    # Generate the pre-filled application form
    application_form = builder.generate_prefilled_form(bank, prefill_payload)
    
    audit_log.record(
        event="bank_application_prefilled",
        bank=bank,
        fields_prefilled=len(prefill_payload),
        pii_fields_excluded=["ssn", "passport", "id_scan"]
    )
    
    return {
        "status": "READY_FOR_REVIEW",
        "bank": bank,
        "application_form": application_form,
        "documents_required": builder.list_required_documents(bank),
        "founder_action_required": "REVIEW_AND_SIGN_APPLICATION"
    }
```

---

## Step C3: Document Vault Integration (KYC)

```python
# Agent action — autonomous (document preparation) + HITL (biometric)
def prepare_kyc_package(bank: str, entity_data: dict) -> dict:
    """
    Compiles the KYC package from the Document Vault.
    Biometric capture is delegated to the mobile SDK — AI never handles it.
    """
    from venturemind.security.document_vault import DocumentVault
    
    vault = DocumentVault(founder_id=founder_data["founder_id"])
    
    # Retrieve documents (AI can see metadata only, not content)
    kyc_documents = {
        "articles_of_organization": vault.retrieve(
            doc_type="entity_document",
            document_id=entity_data["articles_id"],
            return_type="secure_url"  # Signed URL — AI never stores content
        ),
        "operating_agreement": vault.retrieve(
            doc_type="entity_document",
            document_id=entity_data["operating_agreement_id"],
            return_type="secure_url"
        ),
        "ein_letter": vault.retrieve(
            doc_type="tax_document",
            document_id=entity_data["ein_letter_id"],
            return_type="secure_url"
        ),
        "founder_government_id": {
            # NEVER returned to AI — goes directly to KYC provider
            "status": "REQUIRES_MOBILE_HANDSHAKE",
            "provider": "sumsub",
            "handoff_required": True
        }
    }
    
    return {
        "kyc_package_status": "PARTIALLY_READY",
        "documents": kyc_documents,
        "pending_handoff": "founder_government_id",
        "handoff_trigger": "trigger_kyc_mobile_handoff"
    }
```

---

## Step C4: KYC Biometric Handoff (HITL)

```python
# Agent action — triggers mobile handoff for KYC biometrics
def trigger_kyc_mobile_handoff(kyc_provider: str = "sumsub") -> dict:
    """
    Triggers a secure handoff to the founder's mobile device
    for liveness check and government ID capture.
    
    AI pre-fills text fields. AI NEVER captures:
    - Facial scan / liveness video
    - Passport / ID number
    - Fingerprint
    """
    if kyc_provider == "sumsub":
        # Create Sumsub SDK token
        sumsub_token = sumsub.create_sdk_token(
            user_id=founder_data["founder_id"],
            level_name="basic_kyc",
            expiration_interval=3600  # 1 hour
        )
        
        # Send deep link to founder mobile
        mobile_event = send_mobile_handoff(
            event_type="kyc_biometric_capture",
            provider="sumsub",
            sdk_token=sumsub_token,
            required_documents=["government_id", "selfie"],
            message="Your bank application requires identity verification. Tap to complete."
        )
        
        return {
            "status": "AWAITING_BIOMETRIC_COMPLETION",
            "handoff_id": mobile_event["event_id"],
            "estimated_completion_minutes": 5,
            "next_poll_after_seconds": 60
        }
    
    elif kyc_provider == "onfido":
        # Similar flow for Onfido
        onfido_token = onfido.create_web_sdk_token(
            applicant_id=founder_data["founder_id"]
        )
        
        mobile_event = send_mobile_handoff(
            event_type="kyc_biometric_capture",
            provider="onfido",
            sdk_token=onfido_token
        )
        
        return {
            "status": "AWAITING_BIOMETRIC_COMPLETION",
            "handoff_id": mobile_event["event_id"],
            "estimated_completion_minutes": 5
        }
```

---

## Step C5: Live Video Verification (Bank Requirement)

```python
# Agent action — schedules video call if bank requires it
def handle_video_verification_requirement(bank: str, bank_response: dict) -> dict:
    """
    If the bank requires a live video call (common for business accounts),
    the AI schedules and prepares the founder for the call.
    """
    if not bank_response.get("requires_video_call"):
        return {"status": "SKIPPED", "reason": "video_call_not_required"}
    
    # Schedule video call via Zoom API / Calendly
    availability = get_banking_partner_availability(bank)
    
    scheduled_call = {
        "provider": "zoom",
        "meeting_link": zoom.create_meeting_link(
            topic=f"Business Verification — {entity_data['legal_name']}",
            duration_minutes=15,
            host_email=get_banking_partner_email(bank)
        ),
        "scheduled_time": availability["earliest_slot"],
        "prep_checklist": [
            "Government-issued photo ID (passport or driver's license)",
            "Proof of address (utility bill or bank statement — dated within 60 days)",
            "Business documents if requested by the bank",
            "Confirm you are in a well-lit, quiet environment"
        ],
        "note": "The call will be with a bank compliance officer. AI will join as an observer to take notes."
    }
    
    return {
        "status": "VIDEO_CALL_REQUIRED",
        "call_details": scheduled_call,
        "founder_action_required": "SCHEDULE_CALL"
    }
```

---

## Step C6: Stripe Claimable Sandbox (Fiat Bridge)

```python
# Agent action — autonomous (Stripe Connect setup for fiat on/off ramp)
def setup_stripe_connect(entity_data: dict, bank_account_verified: dict) -> dict:
    """
    Sets up Stripe Connect to enable fiat deposits and withdrawals.
    This is the fiat bridge between the neobank and the Safe wallet.
    """
    import stripe
    
    stripe.api_key = os.environ["STRIPE_SECRET_KEY"]
    
    # Create Stripe Connect account
    connect_account = stripe.Account.create(
        type="express",
        country=entity_data["formation_jurisdiction"][:2].upper(),  # e.g., "US", "AE", "SG"
        email=entity_data["business_email"],
        business_profile={
            "mcc": entity_data.get("naics_code", "8999"),
            "url": entity_data.get("website", f"https://nomadflow.com/{entity_data['legal_name']}")
        },
        metadata={
            "venturemind_entity_id": entity_data["entity_id"],
            "venturemind_safe_address": entity_data["safe_address"]
        },
        capabilities={
            "transfers": {"requested": True},
            "card_payments": {"requested": True}
        }
    )
    
    # Generate onboarding link
    onboarding_link = stripe.AccountLink.create(
        account=connect_account.id,
        refresh_url=f"{os.environ['APP_URL']}/stripe/onboard/refresh",
        return_url=f"{os.environ['APP_URL']}/stripe/onboard/return",
        type="account_onboarding"
    )
    
    audit_log.record(
        event="stripe_connect_account_created",
        stripe_account_id=connect_account.id,
        entity_id=entity_data["entity_id"],
        country=entity_data["formation_jurisdiction"][:2].upper()
    )
    
    return {
        "status": "ONBOARDING_REQUIRED",
        "stripe_account_id": connect_account.id,
        "onboarding_link": onboarding_link.url,
        "founder_action_required": "COMPLETE_STRIPE_ONBOARDING"
    }
```

---

## Dashboard UI: Neobank Card

```
┌─────────────────────────────────────────────────────────┐
│  🟡 NEOBANK ACCOUNT                    [Pending Review]  │
│  Mercury — TechVentures International Ltd                │
│                                                         │
│  Application Status: READY FOR SUBMISSION                │
│  Pre-filled Fields: 12/14 complete                      │
│  Documents Attached: 4/6                                │
│                                                         │
│  ⏳ Awaiting:                                          │
│    • Founder biometric capture (Sumsub) — tap to start   │
│    • Operating Agreement upload — auto-attached         │
│    • EIN Letter upload — auto-attached                 │
│                                                         │
│  Estimated Approval: 1–3 business days                 │
│  ─────────────────────────────────────                  │
│  [📋 REVIEW APPLICATION]  [📱 CAPTURE BIOMETRIC]        │
└─────────────────────────────────────────────────────────┘
```

---

## Step C7: Fiat Deposit & Operating Fund Transfer

```python
# Agent action — generates deposit request, founder approves
def initiate_fiat_deposit(target_amount_usd: float, destination: str) -> dict:
    """
    Initiates a fiat deposit from the founder's personal account
    to the newly opened business bank account.
    """
    deposit_request = {
        "amount_usd": target_amount_usd,
        "source_account": "founder_personal_account",  # Linked externally
        "destination": destination,  # Business neobank account
        "purpose": "Initial operating capital",
        "estimated_arrival": "1–2 business days (ACH) / Same day (Wire)",
        "fees_estimate": calculate_transfer_fee(target_amount_usd, method="ach"),
        "founder_approval_required": True
    }
    
    return {
        "status": "AWAITING_FOUNDER_APPROVAL",
        "deposit_request": deposit_request,
        "founder_action_required": "APPROVE_DEPOSIT"
    }
```

---

## Dashboard UI: Fiat Bridge Card

```
┌─────────────────────────────────────────────────────────┐
│  💰 FIAT BRIDGE                        [Ready to Fund]   │
│  Stripe Connect — Business Account                        │
│                                                         │
│  Stripe Account: acct_1OmRs2K...    [Dashboard →]        │
│  Business Balance: $0.00 USD                           │
│  Pending Deposits: $5,000.00 USD (1 business day)     │
│                                                         │
│  [DEPOSIT FUNDS]                                        │
│  Amount: [$____________] USD                            │
│  Source: Personal Bank (Chase ****4521)                │
│  ─────────────────────────────────────                  │
│  🏦 Bank Account (Mercury): $0.00                     │
│  🔐 Safe Wallet (ETH): $0.00                          │
│  ⚡ Stripe Balance: $0.00                              │
└─────────────────────────────────────────────────────────┘
```

---

## Audit Trail Entries

| Timestamp | Event | Actor | Details |
|-----------|-------|-------|---------|
| T+0 | `banking_navigation_started` | mobility_banking_agent | Mercury recommended |
| T+30min | `application_prefilled` | mobility_banking_agent | 12/14 fields auto-filled from FounderProfile |
| T+30min | `kyc_package_compiled` | compliance_auditor_agent | 4 docs from vault, 1 mobile handoff required |
| T+35min | `kyc_mobile_handoff_triggered` | kyc_agent | Sumsub token generated, deep link sent to mobile |
| T+2h | `biometric_capture_complete` | sumsub | Founder completed on mobile |
| T+2h+10min | `kyc_results_received` | compliance_auditor_agent | Sumsub result: APPROVED |
| T+24h | `bank_application_submitted` | mobility_banking_agent | Mercury application filed |
| T+72h | `bank_account_approved` | mercury | Account ****7821 opened |
| T+72h+30min | `stripe_connect_created` | capital_agent | acct_1OmRs2K... created |
| T+72h+45min | `deposit_initiated` | capital_agent | $50,000 deposit request created |
| T+73h | `founder_deposit_approved` | founder_mobile | Multi-sig approved |
| T+73h+5min | `deposit_processing` | stripe | ACH transfer initiated |
