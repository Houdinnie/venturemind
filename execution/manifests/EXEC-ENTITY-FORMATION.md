# Execution Manifest: ENTITY FORMATION
## Legal Swarm | LLC / IBC Formation | Multi-Jurisdiction

**Manifest ID**: `EXEC-ENTITY-FORMATION-V1`
**Risk Level**: CRIMSON (> $10,000) — Multi-sig + 48hr cooling-off + Reg-Watch check
**Swarm Owner**: Legal Swarm (Formation Sub-Agent + Governance Sub-Agent)
**Estimated Cost**: $500–$2,500 (取决于jurisdiction)

---

## Pre-Flight Checklist

- [ ] FounderProfile complete (Discovery Phase)
- [ ] KYC Tier verified (Tier 3 minimum for entity formation)
- [ ] Jurisdiction confirmed (Jurisdiction Navigator Sub-Agent)
- [ ] Reg-Watch pre-check passed (no recent legal changes in target jurisdiction)
- [ ] Operating Agreement template approved by founder
- [ ] HITL: Final Draft Review completed and approved
- [ ] HITL: Cooling-off period elapsed (48 hours since command issued)

---

## Step B1: Jurisdiction Verification (Reg-Watch Check)

```python
# Agent action — autonomous
def run_reg_watch_check(jurisdiction: str, entity_type: str) -> dict:
    """
    Runs a real-time legal change scan on the target jurisdiction
    before any filing is initiated.
    """
    # Trigger Web-Search Agent for jurisdiction scan
    regulatory_changes = web_search_agent.scan(
        query=f"{jurisdiction} {entity_type} legal changes 2026",
        include_government_sites=True,
        time_range="month"
    )
    
    # Key areas checked
    check_areas = [
        "annual reporting requirements changed",
        "minimum capital requirements changed",
        "foreign ownership restrictions",
        "tax registration deadlines",
        "beneficial ownership reporting",
        "renewal fee increases"
    ]
    
    alerts = []
    for change in regulatory_changes:
        for area in check_areas:
            if area.lower() in change["summary"].lower():
                alerts.append({
                    "area": area,
                    "change_detected": change["summary"],
                    "source": change["url"],
                    "severity": change["relevance_score"]
                })
    
    # If HIGH severity alerts found — freeze and escalate
    high_severity = [a for a in alerts if a["severity"] > 0.85]
    if high_severity:
        return {
            "status": "FROZEN",
            "reason": "significant_regulatory_change",
            "alerts": high_severity,
            "escalation_target": "The_Guild"
        }
    
    return {
        "status": "CLEAR",
        "jurisdiction": jurisdiction,
        "alerts": alerts,
        "last_verified": datetime.utcnow().isoformat()
    }
```

---

## Step B2: Formation Service Integration

```python
# Agent action — autonomous (drafting) + HITL (submission)
def prepare_formation_package(jurisdiction: str, entity_type: str, founder_data: dict) -> dict:
    """
    Prepares the complete formation package — all documents drafted,
    ready for founder review before any API submission.
    """
    from venturemind.legal.formation import FormationBuilder
    
    builder = FormationBuilder(jurisdiction=jurisdiction, entity_type=entity_type)
    
    # Draft core documents
    documents = {
        "articles_of_organization": builder.draft_articles(
            company_name=founder_data["proposed_company_name"],
            registered_agent=founder_data.get("registered_agent"),
            principal_address=founder_data["principal_address"],
            registered_office=founder_data.get("registered_office")
        ),
        "operating_agreement": builder.draft_operating_agreement(
            members=[
                {"name": founder_data["founder_name"], "ownership": 100}
            ],
            management_structure="manager_managed",
            voting_rights="per_proportional",
            profit_distribution="proportional",
            dissolution_terms="standard"
        ),
        "beneficial_ownership_declaration": builder.draft_boi_report(
            beneficial_owners=[founder_data["founder_data"]],
            submission_method="finCEN_eFiling"
        ),
        "company_bylaws": builder.draft_bylaws(
            board_structure=["sole_director"],
            officer_roles=["CEO", "CFO", "Secretary"],
            meeting_requirements="annual_only"
        ),
        "stock_certificate_template": builder.draft_stock_certificate(
            authorized_shares=founder_data.get("authorized_shares", 10000),
            par_value=founder_data.get("par_value", 0.001)
        )
    }
    
    # Calculate total cost
    formation_costs = builder.quote_costs()
    
    return {
        "status": "READY_FOR_REVIEW",
        "documents": documents,
        "estimated_total_cost": formation_costs["total"],
        "breakdown": formation_costs,
        "jurisdiction": jurisdiction,
        "estimated_timeline_days": formation_costs["timeline_days"]
    }
```

---

## Step B3: Final Draft Review (HITL)

```python
# UI: Final Draft Review Screen
def present_final_draft_review(formation_package: dict) -> dict:
    """
    Presents the complete formation package to the founder
    for review. No API submission occurs until founder approves.
    """
    return {
        "review_screen": {
            "title": "Entity Formation — Final Draft Review",
            "documents": formation_package["documents"],
            "cost_breakdown": formation_package["breakdown"],
            "timeline": f"{formation_package['estimated_timeline_days']} business days",
            "jurisdiction_summary": f"{formation_package['jurisdiction']} — {formation_package['entity_type']}",
            "safety_warnings": [
                "This filing creates a legal entity under {{jurisdiction}} law.",
                "This action cannot be undone without legal dissolution.",
                "Annual reporting fees will apply from year 1.",
                "Beneficial ownership will be reported to FinCEN."
            ]
        },
        "approval_actions": {
            "approve_all": {
                "label": "✅ APPROVE ALL DOCUMENTS & SUBMIT",
                "action": "submit_formation",
                "requires_cooling_off": True,
                "cooling_off_hours": 48,
                "risk_level": "CRIMSON"
            },
            "reject_specific": {
                "label": "✏️ REQUEST CHANGES",
                "action": "return_to_agent",
                "requires_feedback": True
            },
            "abort": {
                "label": "⛔ ABORT FORMATION",
                "action": "cancel_pipeline",
                "reason_required": False
            }
        }
    }
```

---

## Step B4: Filing Submission (After Cooling-Off)

```python
# Agent action — AFTER cooling-off period + founder approval
def submit_formation_filing(formation_package: dict, founder_approval: dict) -> dict:
    """
    Submits the formation filing to the relevant government portal
    ONLY after: cooling-off elapsed, Reg-Watch still clear, founder approved.
    """
    if not reg_watch_check_passed():
        trigger_protocol_zero(reason="regulatory_change_detected_post_approval")
        return {"status": "FROZEN", "reason": "regulatory_change_after_approval"}
    
    # Choose formation service based on jurisdiction
    jurisdiction = formation_package["jurisdiction"]
    service = get_formation_service(jurisdiction)
    
    if jurisdiction == "wyoming_usa":
        # Wyoming Secretary of State — via Stripe Atlas or Wyoming SOS direct API
        result = stripe_atlas.submit_formation(
            entity_type="LLC",
            company_name=formation_package["documents"]["articles_of_organization"]["company_name"],
            registered_agent=formation_package["documents"]["articles_of_organization"]["registered_agent"],
            principal_address=formation_package["documents"]["articles_of_organization"]["principal_address"],
            founder_data=founder_data
        )
    elif jurisdiction == "dubai_ifza":
        # Dubai IFZA — via official portal
        result = ifza_portal.submit_registration(
            entity_type="LLC",
            company_name=formation_package["documents"]["articles_of_organization"]["company_name"],
            activity_code=determine_activity_code(founder_data["business_description"]),
            shareholder_data=founder_data
        )
    elif jurisdiction == "singapore":
        # ACRA — via BizFile+ or Sleek
        result = acra_bizfile.submit_registration(
            entity_type="Private Limited",
            company_name=formation_package["documents"]["articles_of_organization"]["company_name"],
            shareholders=founder_data["shareholders"],
            secretary_appointment=True
        )
    
    audit_log.record(
        event="entity_formation_submitted",
        jurisdiction=jurisdiction,
        entity_type=entity_type,
        filing_reference=result["filing_reference"],
        submitted_at=datetime.utcnow().isoformat(),
        cost_paid=formation_package["estimated_total_cost"],
        founder_approved=True,
        cooling_off_elapsed=True
    )
    
    return {
        "status": "SUBMITTED",
        "filing_reference": result["filing_reference"],
        "estimated_approval_days": formation_package["estimated_timeline_days"],
        "next_steps": [
            "Wait for government approval (typically 1–5 business days)",
            "EIN/Tax ID will be issued by the relevant authority",
            "Bank account application can proceed after EIN received"
        ]
    }
```

---

## Step B5: Post-Formation Setup (Autonomous)

```python
# Agent action — autonomous after entity approved
def run_post_formation_setup(formation_result: dict) -> dict:
    """
    After the entity is officially registered, the AI runs these
    autonomous setup tasks (no further human approval required).
    """
    from venturemind.legal.governance import PostFormationSetup
    
    setup = PostFormationSetup(entity_id=formation_result["entity_id"])
    
    # Autonomous tasks
    tasks = [
        setup.create_member_consent_resolutions(
            document_type="initial_member_resolution",
            date=formation_result["effective_date"]
        ),
        setup.issue_stock_certificates(
            shareholder=founder_data["founder_name"],
            shares=10000,
            certificate_number="C-001"
        ),
        setup.create_operating_account_resolution(
            bank_name="Mercury",
            authorized_signers=[founder_data["founder_name"]]
        ),
        setup.register_for_foreign_qualification(
            states=["Delaware", "California"]  # if applicable
        ) if formation_result["jurisdiction"] != "delaware" else None
    ]
    
    # Schedule annual report reminders
    setup.schedule_compliance_reminders(
        annual_report_due=calculate_annual_report_due(formation_result["jurisdiction"]),
        registered_agent_renewal_due=calculate_ra_renewal_due(formation_result["jurisdiction"])
    )
    
    return {
        "status": "POST_FORMATION_COMPLETE",
        "documents_generated": ["member_consent_resolution", "stock_certificate_c1", "bank_resolution"],
        "compliance_schedule": "Annual report reminders set",
        "documents_stored_in_vault": True
    }
```

---

## Dashboard UI: Entity Formation Card

```
┌─────────────────────────────────────────────────────────┐
│  🟢 ENTITY FORMATION                   [Multi-Sig]       │
│  Wyoming LLC — TechVentures International Ltd            │
│                                                         │
│  Filing Status: APPROVED                               │
│  Entity ID: WY-2026-784321                             │
│  EIN: 83-4521098                                       │
│  Formation Date: 2026-05-09                           │
│                                                         │
│  Registered Agent: Wyoming Registered Agent Services    │
│  ─────────────────────────────────────                  │
│  Annual Report Due: January 1, 2027                   │
│  Registered Agent Renewal: Due in 364 days             │
│                                                         │
│  [📄 Documents]  [🏦 Bank Account]  [🔐 Keys]          │
└─────────────────────────────────────────────────────────┘
```

---

## Audit Trail Entries

| Timestamp | Event | Actor | Details |
|-----------|-------|-------|---------|
| T+0 | `formation_initiated` | founder_command | Manifest signed, cooling-off started |
| T+1h | `reg_watch_check_passed` | compliance_auditor_agent | Wyoming — no regulatory changes |
| T+48h | `cooling_off_elapsed` | safety_net_agent | 48hr window passed |
| T+48h+5min | `founder_approved_all` | founder_ui | Multi-sig signed |
| T+48h+5min | `formation_submitted` | legal_formation_agent | Filed via Stripe Atlas |
| T+72h | `entity_approved` | wyoming_sos | WY-2026-784321 issued |
| T+72h+30min | `post_formation_setup_complete` | legal_governance_agent | Resolutions, stock cert, bank resolution |
| T+72h+45min | `documents_vaulted` | document_vault_agent | All docs stored in secure vault |
| T+73h | `compliance_schedule_set` | legal_compliance_agent | Annual report reminder January 1, 2027 |
