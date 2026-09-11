# 🔒 CRITICAL: LOCKED NOW - SECURITY & AUTHENTICATION

This document lists what MUST be correct from Phase 1 and CANNOT be easily changed later.

---

## 🔐 SECURITY (ABSOLUTELY LOCKED - NO CHANGES ALLOWED)

### Rules That Define Core Architecture
These are NOT negotiable. Changing ANY of these breaks the entire security model:

```
S-001–S-012: AI Security Foundation
✅ LOCKED NOW:
  ├── Nexa NEVER has write access (read-only always)
  ├── Nexa NEVER approves financial entries
  ├── Nexa NEVER posts GL entries
  ├── Nexa NEVER bypasses authorization
  ├── Prompt injection defense mandatory
  ├── Instruction/data separation in all AI calls
  └── Human approval required for high-risk actions

WHY LOCKED:
  If we build Nexa with posting ability, rearchitecting requires:
  └── All AI calls rewritten
  └── Permission tables redesigned
  └── Audit logs restructured
  └── Database constraints changed
  → 2-3 weeks of rework

S-013–S-023: Operational Security
✅ LOCKED NOW:
  ├── Cross-tenant isolation is ABSOLUTE (no company sees other's data)
  ├── Immutable audit (never deleted, never modified)
  ├── Soft delete only (hard delete forbidden)
  ├── Recovery always possible
  └── Row-Level Security (RLS) at database level (not application level)

WHY LOCKED:
  If we build without RLS:
  └── Have to add RLS policies to every query (risky)
  └── Could miss queries, leaving data exposed
  → Security breach risk

S-065: Hierarchical Custom Position (Multi-Module Roles)
✅ LOCKED NOW:
  ├── One employee = one custom position
  ├── Position can span multiple modules
  ├── Human Power ≠ AI Power (must configure separately)
  ├── No inheritance (parent position ≠ auto child power)
  └── Permission graph must support complex roles

WHY LOCKED:
  If we build without this architecture:
  └── Adding custom multi-module roles = redesign permission table
  └── Adding S-066 non-transitivity = add new columns + logic
  → 1-2 weeks rework

S-067: Segregation of Duties
✅ LOCKED NOW:
  ├── Requester ≠ Approver ≠ Executor (for high-value actions)
  ├── System tracks who did what
  ├── Approval chain enforced before posting
  └── Audit shows complete chain

WHY LOCKED:
  If we add later:
  └── Have to modify existing workflows
  └── Retroactive audit trail impossible
  → Financial integrity compromised
```

---

## 🔑 AUTHENTICATION (ABSOLUTELY LOCKED - NO CHANGES)

### Core Auth Requirements
These define the database schema. Changing means migrations:

```
✅ LOCKED NOW - CANNOT CHANGE WITHOUT MIGRATION:

1. Password-Based Auth
   ├── bcrypt hashing (salted, never plaintext)
   ├── Min 12 characters (enforced)
   ├── Uppercase + Numbers + Symbols (enforced)
   └── Rate limiting (5 failed attempts → lockout)
   
   WHY LOCKED: If we change hash algorithm, must re-hash all passwords

2. JWT Token Architecture
   ├── JWT payload: { user_id, tenant_id, position_id, expires_at }
   ├── Refresh tokens stored in DB (revocable)
   ├── Access tokens (15 min expiry)
   ├── Refresh tokens (30 day expiry)
   └── Session invalidation possible
   
   WHY LOCKED: Changing token structure = all clients must update

3. Session Management
   ├── One user = multiple active sessions (different devices)
   ├── Session stored in DB (can revoke individual sessions)
   ├── Logout destroys session token
   └── Auto-logout on 30 min inactivity
   
   WHY LOCKED: Client logic depends on session structure

4. Tenant Isolation in Auth
   ├── User tied to exactly ONE tenant (initially)
   ├── JWT includes tenant_id (checked on every request)
   ├── Cannot switch tenant in single session
   └── Must logout → login to different tenant
   
   WHY LOCKED: If we allow multi-tenant users, permission model breaks

5. Default Users (MUST CREATE IN PHASE 1)
   ├── Founder/Owner account (superuser for their tenant)
   ├── Admin account (can manage users, positions)
   ├── Demo account (read-only, shows features)
   └── Test accounts (for automated testing)
   
   WHY LOCKED: Phase 3 depends on these existing & working
```

---

## 👥 AUTHORIZATION (ABSOLUTELY LOCKED - NO CHANGES)

### Core Authorization Rules
These define permission checking logic. Cannot be changed per-customer:

```
✅ LOCKED NOW - IMMUTABLE ACROSS ALL TENANTS:

1. Permission Model
   ├── Position = set of permissions
   ├── User assigned to ONE position (per tenant)
   ├── Permission = { module, action, resource_type }
   │   Example: { inventory, create, product }
   └── Check: User.position.permissions contains required permission?

   WHY LOCKED: Changing permission model = rewrite all authorization checks

2. Authorization Function
   ├── authorize(user, action, resource) → ALLOW | DENY | APPROVAL_REQUIRED | STEP_UP_REQUIRED
   ├── NEVER returns boolean (always returns status)
   ├── Called on EVERY mutation (backend enforced)
   ├── Logged immutably in audit_log
   └── Frontend respects result (but backend is authoritative)
   
   WHY LOCKED: Every API endpoint depends on this signature

3. Custom Position Support (S-065)
   ├── Position can be: Standard OR Custom
   ├── Standard: predefined (Owner, Manager, Employee, etc.)
   ├── Custom: user-created, spans multiple modules
   ├── Custom position definition saved in `positions` table
   ├── Cannot be changed per-request (only via admin interface)
   └── Version tracked for audit
   
   WHY LOCKED: Changing position schema = modify permission table + all joins

4. Resource-Level Authorization
   ├── Check User.position → permission for action
   ├── THEN Check row ownership / tenant_id
   ├── THEN Check field-level projection (which fields can user see)
   └── All three MUST pass
   
   WHY LOCKED: Missing any layer = security breach

5. Authorization Approval Chain
   ├── Some actions require approval (e.g., GL posting)
   ├── Approval stored: approval_id, approved_by, approved_at
   ├── Approval must be from different person than requester
   ├── Cannot approve own request
   └── Audit trail shows complete chain
   
   WHY LOCKED: Adding approval chain later = redesign GL schema

6. Cross-Tenant Authorization
   ├── NO user from Company A can access Company B's data
   ├── Checked at:
   │   ├── JWT token validation (tenant_id)
   │   ├── RLS policy (tenant_id in WHERE clause)
   │   └── Domain service (explicit tenant check)
   ├── Three-layer defense (never just one place)
   └── No shortcuts, no exceptions
   
   WHY LOCKED: Single breach = entire system compromised
```

---

## 📊 FINANCIAL AUTHORIZATION (LOCKED - NO EXCEPTIONS)

### GL Entry Authorization (MUST be correct from Phase 6)
If we get this wrong initially, we cannot fix retroactively:

```
✅ LOCKED NOW:

1. GL Entry Lifecycle
   ├── DRAFT: Created (auto by system or manual by user)
   ├── APPROVAL_PENDING: Awaiting review
   ├── APPROVED: Human reviewed & approved
   ├── POSTED: Approved entry now counts toward Trial Balance
   ├── REJECTED: User decided not to post
   └── ARCHIVED: Old closed-period entries
   
   CRITICAL: Entry cannot skip DRAFT → APPROVED → POSTED
   (No auto-posting, no bypass)
   
   WHY LOCKED: Changing this architecture retroactively:
   └── Must audit all historical entries
   └── Must recalculate all GL reports
   └── Compliance nightmare

2. Posting Authorization
   ├── Only positions with "post_gl_entries" permission can approve
   ├── Different person from requester (S-067)
   ├── Approval recorded with timestamp + approver_id
   ├── Cannot un-post (only close period, which hides from TB)
   └── Audit shows: who posted, when, which entry
   
   WHY LOCKED: Removing audit trail requirement = regulatory violation

3. GL Formula Validation (before posting)
   ├── Debit ≠ Credit → Entry rejected with error
   ├── Account codes match CoA → Entry rejected if unknown account
   ├── Amount > 0 → Entry rejected if zero/negative
   ├── Currency matches tenant currency → Entry rejected if mismatched
   └── All checks BEFORE posting (post is final)
   
   WHY LOCKED: If we post invalid entries:
   └── Trial Balance becomes meaningless
   └── Cannot trust financial reports
   → Compliance failure
```

---

## 🔍 AUDIT & IMMUTABILITY (LOCKED - NO CHANGES)

### Audit Log Architecture
Cannot be changed - must be built correctly from day 1:

```
✅ LOCKED NOW:

1. Audit Log Table
   ├── audit_id (UUID, primary key)
   ├── tenant_id (UUID, who's affected)
   ├── user_id (UUID, who did it)
   ├── action (string: create, update, delete, approve, post)
   ├── entity_type (string: product, invoice, gl_entry, etc.)
   ├── entity_id (UUID, what was affected)
   ├── old_value (JSONB, before state)
   ├── new_value (JSONB, after state)
   ├── reason (string, optional, why they did it)
   ├── ip_address (VARCHAR, where from)
   ├── user_agent (VARCHAR, what device)
   ├── timestamp (TIMESTAMP, when)
   └── status (string: SUCCESS, DENIED, ERROR)
   
   CONSTRAINT: Never allow UPDATE or DELETE on audit_log
   (Only INSERT allowed)
   
   WHY LOCKED: If we allow deletion:
   └── Cannot prove what happened
   └── Compliance regulations violated
   → Criminal liability

2. Correlation ID (for tracing)
   ├── Every request gets unique correlation_id
   ├── All related audit logs have same correlation_id
   ├── Can trace: API call → DB changes → other modules affected
   └── Helps debug multi-module transactions
   
   WHY LOCKED: Adding later = hard to correlate historical requests

3. Immutability Guarantee
   ├── Audit entries created, NEVER modified
   ├── Queries: SELECT only (no UPDATE, DELETE, TRUNCATE)
   ├── Backups: Separate from main DB (offline, write-once)
   ├── Retention: 7 years minimum (compliance)
   └── Tamper detection: Content hash verification possible
   
   WHY LOCKED: If we allow modifications, audit is worthless
```

---

## 🚫 WHAT CANNOT CHANGE (Even If Customer Asks)

**NO customer/tenant can customize:**

```
❌ FORBIDDEN:
├── Authentication method (always password + JWT)
├── Authorization model (always role-based + position-based)
├── Audit logging (always immutable, always on)
├── Cross-tenant isolation (always enforced)
├── GL posting workflow (always DRAFT→APPROVE→POST)
├── Segregation of duties (always requester ≠ approver)
├── Password policy (always 12 chars + complexity)
├── Session management (always revocable)
├── Token expiry (always 15 min access, 30 day refresh)
├── Soft delete (always, never hard delete)
├── RLS enforcement (always, never bypass)
└── Nexa restrictions (always read-only, never write)

If customer asks to change any of these:
→ Answer: "This is locked by security rules. Cannot change."
→ Escalate: "Founder decision only, and only if major business case."
```

---

## ✅ WHAT WILL BE BUILT IN PHASE 1

With these rules LOCKED, Phase 1 will implement:

```
PHASE 1 DELIVERABLES (All security-locked):

1. Database Schema (06_DATABASE/migrations)
   ├── users table (tenant_id, email, password_hash, position_id)
   ├── tenants table (name, industry_type, currency, created_at)
   ├── positions table (standard + custom positions)
   ├── permissions table (what each position can do)
   ├── sessions table (active login sessions)
   ├── audit_log table (immutable, append-only)
   └── RLS policies (PostgreSQL row-level security)

2. Authentication Service (02_SHARED_CORE/identity)
   ├── register() - Create new user
   ├── login() - Authenticate user, return JWT
   ├── logout() - Invalidate session
   ├── refreshToken() - Get new access token
   ├── validateToken() - Check JWT is valid
   └── resetPassword() - Secure password change

3. Authorization Engine (05_ENGINES/authorization-engine + 02_SHARED_CORE/access-control)
   ├── authorize(user, action, resource) → status
   ├── checkPermission(position, permission) → boolean
   ├── checkTenantBoundary(user, tenant_id) → boolean
   ├── checkFieldAccess(user, field_name) → boolean
   └── checkApprovalChain(requester, approver) → boolean

4. Position Management (02_SHARED_CORE/position)
   ├── Standard positions (Owner, Manager, Employee, Accountant, etc.)
   ├── Custom position creation (admin only)
   ├── Permission assignment (which position → which permissions)
   ├── Custom position can span multiple modules
   └── S-066 enforcement (Human Power ≠ AI Power)

5. Action Guard (05_ENGINES/action-guard)
   ├── Wraps all mutations
   ├── Validates authorization BEFORE action
   ├── Logs to audit_log AFTER successful action
   ├── Returns error if denied
   └── Never proceeds if authorization fails

6. Immutable Audit Log (02_SHARED_CORE/audit)
   ├── Log every CRUD operation
   ├── Include: who, what, when, where, why
   ├── Immutable (never update/delete)
   ├── Queryable (can search by user, action, entity, time)
   └── Retention policy enforced

EXIT CRITERIA (ALL must pass):
  ✅ Cross-tenant isolation tested (adversarially)
  ✅ Custom position with multiple module permissions works
  ✅ Every mutation has audit log entry
  ✅ Authorization returns correct status (not boolean)
  ✅ Cannot change locked rules via API
  ✅ Cannot bypass RLS at database level
  ✅ Nexa cannot post/approve/write (read-only enforced)
```

---

## 📋 Summary: What's LOCKED vs FLEXIBLE

```
🔒 LOCKED NOW (Build correctly, never change):
├── Security architecture (defense-in-depth)
├── Authentication (JWT, sessions, password policy)
├── Authorization model (role + position based)
├── Audit logging (immutable, complete)
├── Cross-tenant isolation (RLS mandatory)
├── GL posting workflow (DRAFT→APPROVE→POST)
├── Segregation of duties (requester ≠ approver)
├── Nexa restrictions (read-only always)
└── Password policy (12 char + complexity)

✅ FLEXIBLE LATER (Easy to add/change):
├── Industry templates (data-driven, zero code changes)
├── Custom product fields (stored separately)
├── Custom CoA accounts (user-created)
├── Custom positions (admin interface)
├── Dashboard KPIs (user-selected)
├── Report templates (user-customized)
├── Currency conversion rates (updated in admin)
├── Approval limits (changed per position)
└── MFA methods (can add later)
```

---

**🔒 FINAL CONFIRMATION**

**These security rules are ABSOLUTELY LOCKED.**
**No negotiation, no exceptions, no shortcuts.**
**Building these correctly now = months saved in compliance later.**

Ready to proceed with Phase 1 with these locked? ✅
