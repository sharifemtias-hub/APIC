# 🔒 APIC — AI-Powered Inventory Controller
## V1 FINAL MASTERPLAN — LOCKED

**Product**: APIC (scoped-down derivative of the original APBC plan)
**Company**: DIYANO TECH — Founder: Sharif Emtias
**AI**: Nexa only — Aira is entirely excluded from APIC
**Payment**: Manual only — no payment gateway, no Banking vertical
**Scope**: SME multi-branch business suite (Inventory + Sales are the core; HR/Payroll/Accounting/Finance are lightweight support modules)

---

## 0. CORE PHILOSOPHY (unchanged from APBC — still the foundation)

1. Backend is authoritative.
2. Database is source of truth.
3. AI is not authorization.
4. AI is not authoritative arithmetic.
5. Security is defense-in-depth.
6. Position ≠ Permission.
7. Human Power ≠ AI Power.
8. Parent Position ≠ automatic child power.
9. Company ≠ user.
10. Tenant boundary is absolute.
11. Modules are development-isolated, not data-isolated.
12. Shared entities have one source of truth.
13. Critical actions are server-side controlled.
14. Real-time visibility never bypasses authorization.
15. Every critical operation is auditable.

---

## 1. MODULE SCOPE & TIERING (LOCKED TODAY)

### Tier 1 — Core Operational (full vertical slice, all 24 checklist steps)
- **Inventory** — product, stock, batch, warehouse, inter-branch transfer
- **Sales/Billing** — customer, invoice, order; built-in AND import-capable

### Tier 2 — Owner Support (lightweight, mostly auto-derived from Tier 1)
- **HR** — employee profile, attendance, leave, salary-structure link only
- **Payroll** — monthly salary run, tax withholding, payslip
- **Accounting** — chart of accounts + AUTO-GENERATED journal entries from Sales/Inventory/Payroll
- **Finance** — cash/bank, AR/AP aging, profit/margin dashboards

### PHASE ORDER (LOCKED)
- Phase 0: Architecture Foundation ✅ COMPLETE
- Phase 1: Security + Authority Foundation 🔜 TODO
- Phase 2: Shared System Core 🔜 TODO
- Phase 3: User & Position Control Center 🔜 TODO
- Phase 4: Inventory (Tier 1) 🔜 TODO
- Phase 5: Sales / Billing (Tier 1) 🔜 TODO
- Phase 6: Accounting & Finance (Tier 2) 🔜 TODO
- Phase 7: HR (Tier 2) 🔜 TODO
- Phase 8: Payroll (Tier 2, depends on HR) 🔜 TODO
- Phase 9: Multi-Branch Consolidation 🔜 TODO
- Phase 10: Integration + Testing + Release 🔜 TODO

---

## 2. CANONICAL REPO STRUCTURE (LOCKED — same 8-folder pattern, trimmed contents)

```
01_APPS/            → customer-web, admin-web, dev-dashboard
02_SHARED_CORE/     → identity, tenant, position, permissions, access-control, audit, security, documents, workflow, ai-gateway, reporting, industry-templates, integrations
03_BUSINESS_MODULES/→ inventory, sales, hr, payroll, accounting, finance
04_AI/              → nexa/, orchestration/ (NO aira/)
05_ENGINES/         → authorization-engine, action-guard, calculation-engine (Formula Registry), audit-engine, reconciliation-engine
06_DATABASE/        → migrations, schemas, rls, seeds, test-data
07_INFRASTRUCTURE/  → cloudflare, docker, terraform, github-actions, secrets, backup, disaster-recovery
08_TESTS/           → unit, authorization, security, cross-module, etc.
```

No new top-level folder without an explicit architecture-change decision.

---

## 3. RULES KEPT FROM APBC (condensed — full detail in MASTER_RULES.md)

### S-001–S-012: AI Security Foundation
- Prompt-injection defense
- Instruction/data separation
- Least privilege
- Human approval for high-risk actions
- Applies to Nexa only

### S-013–S-023: Operational Security
- Cross-company isolation
- Immutable audit
- Soft delete/recovery

### S-024–S-040: Governance
- Rule-change authorization
- Rule hierarchy
- Adaptive authentication

### S-047: Employee + AI Positional Authority
### S-061: Temporary vs permanent access
### S-063: Protected internal information
### S-065: Hierarchical Custom Position (KEY for small teams with multi-module custom roles)
### S-066: Power Non-Transitivity (parent position never auto-grants AI power or child power)
### S-067: Segregation of Duties (requester ≠ approver ≠ executor)
### S-068: Transaction Authorization + step-up verification
### S-069: Financial Master Data Change Control
### S-072: Financial Reconciliation
### S-073: Duplicate/Replay/Double-payment prevention
### S-074: Approval Limit & Escalation

### S-105–S-106: Nexa
- Low-friction positional intelligence
- Silent authorization continuity

### S-107: Deterministic Engine integrity (Formula Registry)
### S-108–S-109: Tenant Security Policy Builder + Non-Bypass mandatory floor
### S-110: Secrets/KMS
### S-111: Observability + correlation ID
### S-112: Incident Response
### S-113: Penetration Testing cadence
### S-114: Backup/DR (RPO ≤15min, RTO ≤1hr, critical restore ≤30min)
### S-115: Vertical Slice Completion Gate
### S-118–S-119: Position-Oriented Real-Time KPI + Dashboard Minimalism

---

## 4. RULES DROPPED FROM APBC (explicitly out of scope for APIC)

✗ S-077–S-104, S-093   All Aira boundary/isolation rules
✗ B-SEC-001–012        Banking security
✗ S-044–S-055          Business Collaboration Network (replaced with lighter inter-company doc exchange)
✗ S-070 (full form)    Enterprise-grade double-entry ledger governance (replaced by lightweight debit=credit validation)
✗ S-071 (full form)    Elaborate period close/reopen workflow (replaced by simple month-lock)
✗ S-076 (full form)    Heavy financial export confidentiality (replaced by existing field-level projection)
✗ S-116                AI Credit/billing metering
✗ S-117 (founder-scale)Elaborate Founder Dashboard
✗ Forecasting/Optimization/Risk engines (deferred)

---

## 5. NEW RULES ADDED TODAY (APIC-specific — LOCKED)

### RULE: Industry Type Field Template Registry
Industry-specific product fields are DATA (not hardcoded). Tenant selects one or more industry types at onboarding. Adding a new industry or field = data entry, never schema/code change.

### RULE: Multi-Branch Dashboard Tiering
Every transaction tagged with branch_id. Owner gets consolidated + branch-drilldown; Branch Manager gets their branch only.

### RULE: Custom Multi-Module Position for Small Teams
One employee can hold a single custom position (e.g. "Finance & HR Officer") spanning multiple Tier-2 modules.

### RULE: Draft → Approve → Post (no auto-posting, ever)
Every auto-generated journal entry, payroll figure is created as DRAFT. A human must review and approve before POSTING.

### RULE: Sales/Data Import Pipeline
Imported data passes through the same validation, duplicate detection, authorization, and recalculation as manual entry.

### RULE: Nexa Channel Gateway (WhatsApp/Email)
Nexa never accesses personal inboxes. Only company-owned channels. RESTRICTED/PRIVATE data never eligible for channel delivery.

### RULE: Report Integrity Registry
Every report registered at generation time with content hash + metadata. Reports can be verified as VERIFIED / MODIFIED / NOT FOUND.

### RULE: Inter-Company Collaboration — Document Exchange, NOT Data Sharing
Company A and Company B NEVER access each other's systems. Only discrete, self-contained documents exchanged (like structured email).

---

## 6. FORMULA REGISTRY — CATEGORIES ACTIVE FOR APIC V1

✓ Arithmetic (core primitives)
✓ Accounting (journal balance, trial balance)
✓ Financial formulas (basic — depreciation if Asset tracking added)
✓ Inventory formulas (reorder point, EOQ, stock valuation)
✓ Tax/VAT (versioned, configurable)
✓ Costing (landed cost, COGS)
✓ Reconciliation (stock count vs system, bank reconciliation)
✓ KPI calculation (generic ratio/aggregate)

✗ Forecasting primitives (deferred)
✗ Scenario calculations (deferred)
✗ Risk calculations (deferred)

---

## 7. COMPLIANCE BOUNDARY (unchanged principle, restated)

APIC prepares accurate, audit-ready financial records as a readiness architecture. It does NOT auto-generate or auto-submit legal filings — a licensed accountant/company secretary must review before any official filing.

---

## 8. EXECUTION CONTRACT FOR ANY CODING AGENT (LOCKED)

**DO NOT INVENT ARCHITECTURE.** Follow this MASTERPLAN and its referenced rules.

- Never move backend security rule into frontend-only logic
- Never replace deterministic calculation with LLM-generated arithmetic
- Never let Nexa post, approve, or auto-execute financial entry
- Never create undocumented permission bypass, admin bypass, tenant bypass
- Never create a new top-level folder outside the 8 canonical structures
- One Implementation Contract = one task = one PR

🔒 **STATUS**: APIC V1 Masterplan — LOCKED as of today's session.
