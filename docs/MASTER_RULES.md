# APIC Master Rules (Referenced from MASTERPLAN)

## Security Rules (S-001 to S-119)

### S-001–S-012: AI Security Foundation
- Prompt-injection defense
- Instruction/data separation
- Least privilege access
- Human approval for high-risk actions
- Applies to Nexa only (no Aira)

### S-013–S-023: Operational Security
- Cross-company isolation (absolute tenant boundary)
- Immutable audit logging
- Soft delete/recovery (never hard delete)

### S-024–S-040: Governance
- Rule-change authorization
- Rule hierarchy (enforced at backend)
- Adaptive authentication

### S-047: Employee + AI Positional Authority
- Position determines what user can do
- Position determines what AI can do (independently)

### S-061: Temporary vs Permanent Access
- Access can be time-limited
- Permanent access requires explicit approval

### S-063: Protected Internal Information
- Certain data marked RESTRICTED or PRIVATE
- Never exposed to frontend without explicit authorization

### S-065: Hierarchical Custom Position (KEY for APIC)
- Small teams can create custom positions
- One employee = one custom position spanning multiple modules
- Example: "Finance & HR Officer" with both Accounting + HR + Payroll access

### S-066: Power Non-Transitivity
- Parent position ≠ automatic child power
- Human Power ≠ AI Power (must be configured independently)
- S-105–S-106: Nexa never inherits authorization from human

### S-067: Segregation of Duties
- Requester ≠ Approver ≠ Executor
- Different people for each role (simplified for small teams)

### S-068: Transaction Authorization + Step-Up Verification
- High-value actions require step-up (re-authentication, OTP, etc.)
- Approval limits per position

### S-069: Financial Master Data Change Control
- Changes to GL accounts, tax rates, salary structures tracked
- Requires approval before posting

### S-072: Financial Reconciliation
- Stock count ≠ System count → reconciliation required
- Bank statement ≠ GL → reconciliation required
- Formula Registry validates matches

### S-073: Duplicate/Replay/Double-Payment Prevention
- Imported records checked for duplicates before commit
- Financial entries have unique transaction IDs
- Timestamp + sequence validation

### S-074: Approval Limit & Escalation
- Positions have approval limits (e.g., "can approve up to $5000")
- Higher amounts escalate to owner/manager

### S-105–S-106: Nexa (AI)
- Low-friction positional intelligence
- Silent authorization continuity (checks position, doesn't re-authenticate)
- Nexa generates reports, never approves or posts

### S-107: Deterministic Engine Integrity
- Formula Registry (not hardcoded formulas)
- Every calculation independent-checkable
- Mismatch handling: alert, never silently use one answer

### S-108–S-109: Tenant Security Policy Builder
- Non-Bypass mandatory floor (security rules cannot be disabled)
- Policy customization within locked boundaries

### S-110: Secrets/KMS
- Never commit secrets
- Use Doppler, Infisical, or GitHub Secrets
- Secrets rotated regularly

### S-111: Observability + Correlation ID
- Every request traced with unique ID
- Structured logging for all critical operations
- Can audit: which request caused which database change

### S-112: Incident Response
- Protocol documented and tested
- Backup/DR procedures reviewed

### S-113: Penetration Testing
- Cadence defined (at least before release)
- Results acted on before go-live

### S-114: Backup/DR (Tested)
- RPO (Recovery Point Objective) ≤ 15 minutes
- RTO (Recovery Time Objective) ≤ 1 hour
- Critical data restore ≤ 30 minutes
- Tested regularly (not assumed)

### S-115: Vertical Slice Completion Gate
- Every phase has exit criteria
- All criteria must pass before moving to next phase

### S-118–S-119: Position-Oriented Real-Time KPI + Dashboard Minimalism
- Dashboards scoped to position (Owner sees all, Manager sees branch only)
- Minimal KPIs (only what's actionable)
- Real-time data for operational decisions

---

## Rules Explicitly Dropped (Out of Scope for APIC)

✗ **S-077–S-104, S-093**: All Aira boundary/isolation rules
- Aira does not exist in APIC
- No multi-AI coordination needed

✗ **B-SEC-001–012**: Banking security
- Banking vertical dropped
- Manual payment only for V1

✗ **S-044–S-055**: Business Collaboration Network
- REPLACED by lightweight Inter-Company Collaboration (document exchange only)
- No live data projection between companies

✗ **S-070 (full form)**: Enterprise-grade double-entry ledger governance
- REPLACED by lightweight debit=credit validation in auto-journal

✗ **S-071 (full form)**: Elaborate period close/reopen workflow
- REPLACED by simple month-lock flag

✗ **S-076 (full form)**: Heavy financial export confidentiality apparatus
- REPLACED by existing field-level data projection

✗ **S-116**: AI Credit/billing metering
- Deferred (only needed if APIC becomes paid SaaS for others)

✗ **S-117 (founder-scale)**: Elaborate Founder Dashboard
- Deferred; Dev Dashboard covers small team needs

✗ **Forecasting/Optimization/Risk engines**
- Deferred (were Aira-dependent)

---

## New Rules Added for APIC (LOCKED)

### Industry Type Field Template Registry
- Industry-specific fields are DATA, never hardcoded
- Tenant selects industry at onboarding
- Adding new industry = data entry, zero code change

### Multi-Branch Dashboard Tiering
- Every transaction tagged with `branch_id`
- Owner: consolidated + branch drill-down
- Branch Manager: their branch only

### Custom Multi-Module Position for Small Teams
- One employee, one custom position
- Example: "Finance & HR Officer" (Accounting + Finance + HR + Payroll)
- Human Power spans modules; Nexa Power configured independently

### Draft → Approve → Post (No Auto-Posting, Ever)
- Auto-generated journal entries, payroll figures: always DRAFT
- Human review + approval required before POSTING
- Nexa may generate drafts, never approves or posts

### Sales/Data Import Pipeline
- Imported data: same validation/duplicate-check/authorization as manual entry
- Imported & manual records identical once committed (same tables)
- Staging → validation → authorization → deterministic recalculation → commit

### Nexa Channel Gateway (WhatsApp/Email)
- Nexa never accesses personal inboxes
- Only company-owned channels (WhatsApp Business API, company email)
- Employee messages company channel → verified → classified → authorized → report generated → filtered → sent back
- RESTRICTED/PRIVATE data never eligible for channel delivery
- Every message audited

### Report Integrity Registry
- Every report: content hash + metadata at generation time
- Verification reference travels with report
- Result: VERIFIED / MODIFIED / NOT FOUND
- Customization before hashing (doesn't break verifiability)

### Inter-Company Collaboration — Document Exchange
- NOT data sharing (distinct from dropped S-044–S-055)
- Company A & B never access each other's systems
- Exchange: discrete self-contained documents (PO, Report, etc.) like structured email
- Outbound docs pass through sender's field-level projection
- Inbound docs always DRAFT (never auto-create anything)
- Both sides audit independently
- Rate-limited per sender; recipient can block

---

## Execution Contract for Coding Agents (LOCKED)

**DO NOT INVENT ARCHITECTURE.** Follow MASTERPLAN + PRODUCTION_PLAN exactly.

### Forbidden Actions
- ✗ Move backend security rule to frontend-only logic
- ✗ Replace deterministic calculation with LLM arithmetic
- ✗ Let Nexa post, approve, or auto-execute financial entry
- ✗ Create undocumented permission bypass or admin bypass
- ✗ Create tenant bypass or database shortcut
- ✗ Create unrestricted AI tool
- ✗ Create new top-level folder outside 8 canonical structures

### Required Actions
- ✅ One Implementation Contract = one task = one PR
- ✅ Never build whole module as single task
- ✅ If rule conflict: stop and flag, don't silently fix
- ✅ Reference LOCKED rules explicitly in code comments

---

## Compliance Boundary (Restated)

APIC prepares audit-ready financial records. It does NOT auto-generate or auto-submit legal filings (RJSC Annual Return, NBR tax return). Licensed accountant/company secretary must review before filing.

---

**🔒 STATUS**: All rules LOCKED. Changes require founder approval.
