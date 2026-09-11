# 🔒 APIC — AI-Powered Inventory Controller

**Version**: V1 FINAL MASTERPLAN (LOCKED)  
**Company**: DIYANO TECH  
**Founder**: Sharif Emtias  
**AI**: Nexa only (Aira excluded)  
**Payment**: Manual only — no banking, no payment gateways

---

## What is APIC?

APIC is a secure, authority-driven business management system for **small-to-medium enterprises (SMEs)** with **multiple branches**. It handles inventory, sales, HR, payroll, accounting, and finance with **absolute security and auditability**.

### **Tier 1 — Core Operational** (Built first)
- **Inventory** — products, stock, batches, warehouses, inter-branch transfers
- **Sales/Billing** — customers, invoices, orders (manual + import-capable)

### **Tier 2 — Owner Support** (Built second, auto-derived from Tier 1)
- **HR** — employee profiles, attendance, leave requests
- **Payroll** — monthly salary runs, tax withholding, payslips
- **Accounting** — chart of accounts, auto-generated journal entries, trial balance, P&L
- **Finance** — cash, bank, AR/AP aging, profit dashboards

---

## 🔐 Core Philosophy (LOCKED)

1. **Backend is authoritative** — UI cannot override backend security
2. **Database is source of truth** — everything checks the DB
3. **AI is NOT authorization** — Nexa cannot approve or post financial entries
4. **Security is defense-in-depth** — layers of validation, not single gates
5. **Tenant boundary is absolute** — cross-company data leakage = FAIL
6. **Every critical action is auditable** — immutable audit trail forever
7. **Deterministic engines** — Formula Registry, never LLM arithmetic
8. **No auto-posting** — financial entries always require human approval

---

## 📁 Repository Structure

```
APIC/
├── 01_APPS/                 → Customer web, Admin web, Dev dashboard
├── 02_SHARED_CORE/          → Identity, tenant, permissions, audit, AI gateway
├── 03_BUSINESS_MODULES/     → Inventory, Sales, HR, Payroll, Accounting, Finance
├── 04_AI/                   → Nexa orchestration (NO Aira)
├── 05_ENGINES/              → Authorization, Action Guard, Formula Registry, Audit
├── 06_DATABASE/             → Migrations, schemas, RLS, seeds
├── 07_INFRASTRUCTURE/       → Cloudflare, Docker, Terraform, GitHub Actions
├── 08_TESTS/                → Unit, authorization, security, cross-module
├── docs/                    → Architecture, rules, implementation contracts
├── .github/workflows/       → Automated testing & CI/CD
└── package.json             → Monorepo configuration (pnpm workspaces)
```

---

## 🚀 Tech Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| **Frontend** | TypeScript + React | Web UI for customers & admins |
| **Backend** | TypeScript + Node.js | API servers, business logic |
| **Security Engines** | Rust | Formula Registry, Action Guard (high-security) |
| **Database** | PostgreSQL (Supabase) + RLS | Source of truth, row-level security |
| **Infrastructure** | Terraform + Docker | Deployment, infrastructure-as-code |
| **Testing** | Jest, Vitest, GitHub Actions | Automated quality assurance |
| **Secrets** | Doppler/Infisical | Environment secrets (never in code) |

---

## 📋 Phase Execution Plan (LOCKED ORDER)

| Phase | Name | Status | Purpose |
|-------|------|--------|---------|
| **0** | Architecture Foundation | 🔄 IN PROGRESS | Folder structure, docs, CI/CD, testing setup |
| **1** | Security + Authority Foundation | 🔜 TODO | Tenant, User, Auth, Permissions, Audit |
| **2** | Shared System Core | 🔜 TODO | AI Gateway, Formula Registry, Field Classification |
| **3** | User & Position Control Center | 🔜 TODO | Create users, assign roles, configure permissions |
| **4** | Inventory | 🔜 TODO | Products, stock, batches, transfers, reconciliation |
| **5** | Sales / Billing | 🔜 TODO | Customers, invoices, orders, import pipeline |
| **6** | Accounting & Finance | 🔜 TODO | GL, auto-journals, trial balance, P&L, reports |
| **7** | HR | 🔜 TODO | Employees, attendance, leave, salary structure |
| **8** | Payroll | 🔜 TODO | Salary runs, tax withholding, payslips, audit trail |
| **9** | Multi-Branch Consolidation | 🔜 TODO | Owner dashboards, branch scoping, channel delivery |
| **10** | Integration + Testing + Release | 🔜 TODO | Security gate, regression, backup/DR, go-live |

---

## 🧪 Testing System (Automated, You Don't Write Tests)

### How It Works
- **I write all tests** automatically as I write code
- **GitHub Actions runs tests automatically** on every PR
- **You only check**: ✅ PASS or ❌ FAIL in GitHub
- **If FAIL**: I fix it and push again
- **If PASS**: Safe to merge

### Test Types Built-In
- ✅ **Unit Tests** — individual functions work correctly
- ✅ **Authorization Tests** — security rules are enforced
- ✅ **Integration Tests** — modules work together
- ✅ **Security Tests** — no vulnerabilities
- ✅ **Code Quality** — ESLint + Prettier enforce standards

### How to Check Test Results
1. Create a Pull Request
2. Scroll down to **"Checks"** section
3. See green ✅ or red ❌ status
4. Click to see detailed test output

---

## 📚 Key Documentation

All documentation is in `docs/` folder:
- **[APIC_V1_MASTERPLAN.md](./docs/APIC_V1_MASTERPLAN.md)** — Complete architecture, rules, principles
- **[APIC_V1_PRODUCTION_PLAN.md](./docs/APIC_V1_PRODUCTION_PLAN.md)** — Build sequence, exit criteria per phase
- **[MASTER_RULES.md](./docs/MASTER_RULES.md)** — All security & operational rules
- **[ARCHITECTURE.md](./docs/ARCHITECTURE.md)** — Technical architecture overview

---

## 🛡️ Security First (LOCKED PRINCIPLES)

- ✅ **No Aira** — only Nexa, read-only role
- ✅ **No auto-posting** — financial entries need human approval
- ✅ **Immutable audit** — every action logged forever
- ✅ **Soft deletes only** — data recovery always possible
- ✅ **Deterministic engines** — Formula Registry, never LLM arithmetic
- ✅ **Field-level projection** — share only authorized data
- ✅ **Tenant isolation** — absolute, cross-company leakage = FAIL

---

## 🤝 How We Build Together

1. **You command**: "Build [feature]"
2. **I code**: Write feature + automatic tests + documentation
3. **I push**: Create Pull Request with everything
4. **Tests run**: GitHub Actions automatically checks quality
5. **You review**: Check ✅ PASS or ❌ FAIL
6. **You merge**: If all looks good, merge to main

**You never need to write tests or debug code** — I handle that for you.

---

## 📞 Questions?

- **"How does [feature] work?"** → Check docs/
- **"Why is [rule] designed this way?"** → Check APIC_V1_MASTERPLAN.md
- **"Can we add [feature]?"** → Check MASTER_RULES.md (locked constraints)
- **"What do I do with test failures?"** → Let me know, I'll fix and re-push

---

## 🚀 Next Steps

Phase 0 is currently setting up:
- ✅ Project structure (8 canonical folders)
- ✅ Testing infrastructure (Jest, GitHub Actions)
- ✅ CI/CD pipeline (auto-runs on every PR)
- ✅ Documentation templates

Then we move to **Phase 1: Security + Authority Foundation** → Database, Authentication, Permissions.

---

**🔒 STATUS**: APIC V1 Masterplan — LOCKED. No architecture changes without founder approval.  
**👤 Lead**: Sharif Emtias (sharifemtias-hub)  
**📅 Started**: 2026-09-11
