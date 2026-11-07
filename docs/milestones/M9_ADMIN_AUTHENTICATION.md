# PotholeMap — M9 Admin Authentication

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M8](M8_ABUSE_PREVENTION.md) · **Next:** [M10](M10_MODERATION_WORKFLOW.md).

## 1. Objective and boundaries

Authenticate admins with MFA and enforce authorization on the server/data boundary. Entry requires M8/default-order signoff, M2 role/grant foundation, M1 shell and user activation.

**In:** Auth/session model decision, controlled admin provisioning, login/MFA/logout UX, role/session guard, privileged RPC identity contract and auth-failure regressions. **Out:** Public signup, reporter accounts, approve/reject UI/API, production invites/messages without authorization and publicly enabling intake.

M9-01 chooses one session model compatible with actual Supabase/Workers deployment and current official guidance; do not mix cookie and bearer assumptions accidentally. Require verified admin membership and the configured MFA assurance for privileged actions, not merely an email match or client boolean. No default passwords/seeds for production.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M9-01 | M8 exit, M2-04 | not_started | Session/MFA/identity ADR pending |
| M9-02 | M9-01 | not_started | Controlled provisioning pending |
| M9-03 | M9-01, M9-02 | not_started | Login/MFA/logout pending |
| M9-04 | M9-01, M9-02, M9-03 | not_started | Server/data authorization pending |
| M9-05 | M9-03, M9-04 | not_started | Auth/CSRF/expiry regressions pending |
| M9-06 | M9-01–M9-05 | not_started | Exit audit pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M9-01 — Session, MFA and RPC identity decision

- **Objective/scope:** Verify current Supabase session/MFA verification mechanisms, choose cookie or bearer transport, refresh/logout semantics and trusted actor propagation to restricted DB transactions; document ADR and threat model.
- **Excluded:** Client-only JWT decode as verification, unreviewed auth framework/service and relying on a hidden admin route.
- **Files:** Auth ADR, security contract, nonsecret env/config notes.
- **Acceptance:** Trust chain from signed-in user through Worker to DB is explicit, including current role/MFA checks, role removal, expiry, refresh and CSRF/origin policy. Service-role calls never trust an unchecked actor argument. Actual free-plan MFA capability verified; missing mandatory capability is a blocker/architecture decision.
- **Verification/security:** Small isolated proof of verified session/role/assurance, token/issuer/audience/expiry behavior and DB-call context; use provider-supported verification rather than homemade crypto.
- **PR/evidence:** ADR, exact assumptions/source dates and M10 transaction-authorization interface.

### M9-02 — Controlled admin provisioning and recovery

- **Objective/scope:** Admin-only role provisioning/removal with least privilege, public signup restrictions, MFA enrollment/recovery and credential-rotation runbook using isolated accounts.
- **Excluded:** Public self-registration into admin role, committed passwords/recovery codes, unsolicited invitations or production-account changes.
- **Files:** `docs/runbooks/ADMIN_ACCESS.md`, narrow config/grants changes and local fixtures as needed.
- **Acceptance:** Ordinary Auth users cannot self-promote; authorization depends on role membership, not arbitrary profile metadata. Recovery is feasible without an insecure emergency bypass; removing a role denies subsequent privileged operations within the defined policy.
- **Verification/security:** Isolated create/remove/enroll/recover walkthrough, direct role-write denial and secret redaction review. Actual emails/invites require applicable explicit authorization.
- **PR/evidence:** Redacted provisioning/recovery steps, ownership model and role-revocation result.

### M9-03 — Accessible login, MFA and logout UX

- **Objective/scope:** Login/MFA challenge and enrollment guidance, session-expired/loading/error states, logout and admin-shell navigation using the selected transport.
- **Excluded:** Moderation controls, misleading login success before verification, exposing provider internals or persistent token logging.
- **Files:** Admin auth pages/hooks/client adapter and interaction tests.
- **Acceptance:** Wrong credentials/MFA failure yield useful neutral errors; successful admin session reaches an empty authorized shell; refresh/expiry/logout are deterministic. Focus/labels work on keyboard/mobile and secrets are never echoed.
- **Verification/security:** Success/failure/expired/challenge/logout cases with isolated accounts and test fixtures; inspect storage/cookies/bundle according to ADR.
- **PR/evidence:** Auth state matrix, accessibility check and redacted transport behavior.

### M9-04 — Server guard and transaction authorization boundary

- **Objective/scope:** Reusable verified-session/admin/MFA guard, trusted actor derivation and DB-side current-role/assurance checks ready for M10. Provide isolated test routes or harness only, not fake production moderation endpoints.
- **Excluded:** Browser role flags, ordinary-user RPC execute grants or service-role bypass described as admin verification.
- **Files:** Server security/data helpers, narrow restricted SQL functions if required and integration tests.
- **Acceptance:** No session → 401; non-admin/insufficient assurance → 403; forged/expired identity denied; verified actor reaches the DB authorization context without client substitution. Role removal cannot be bypassed through direct RPC access or cached UI state.
- **Verification/security:** Direct Worker/Data API/RPC access matrix, forged actor/role, revoked user, MFA assurance and expired/refreshed session cases; `pnpm test:db`.
- **PR/evidence:** Verified trust-chain results, grants matrix and M10 integration contract.

### M9-05 — Session abuse, CSRF and failure regressions

- **Objective/scope:** Apply selected cookie security/CSRF or bearer exposure controls, login/MFA rate-limit policy, safe redirect handling and provider-outage behavior.
- **Excluded:** Assuming CORS blocks nonbrowser auth attacks, broad token persistence or bypassing MFA in production to ease tests.
- **Files:** Auth/security middleware, limiter/header integration, tests and access runbook updates.
- **Acceptance:** Cross-site mutation/auth flows are protected according to transport; redirects cannot send credentials to arbitrary origins; logout/session expiry removes privileged ability; provider failure denies admin writes. Security headers still permit legitimate Auth/MFA flows.
- **Verification/security:** Cross-origin/CSRF where applicable, brute-force bounds, malicious redirect, provider outage, revoked/expired sessions and shared quality gates.
- **PR/evidence:** Failure/security matrix and residual token/session-revocation limitations.

### M9-06 — Auth exit and moderation handoff

- **Objective/scope:** Review identity/role/MFA/recovery evidence, document exact guards/actor contract and hand off to M10.
- **Excluded:** Admin decisions, public intake enablement and automatic production account changes.
- **Files:** This ledger, auth ADR/runbook and downstream contract updates.
- **Acceptance:** Real isolated MFA/admin flow plus direct-access denial evidence available; M10 can authorize a transaction without redesigning identity; no real credentials in artifacts.
- **Verification/security:** Auth/DB tests, existing quality gates and redacted access/recovery review.
- **PR/evidence:** Exit signoff and approved identity-to-transaction contract; stop here.

## 4. Exit gate

- [ ] Session transport/MFA/role/RPC trust-chain ADR verified.
- [ ] Controlled provisioning, role removal and recovery demonstrated.
- [ ] Accessible login/MFA/logout/expiry flows work.
- [ ] Server and direct data/RPC guards reject forged/non-admin/insufficient sessions.
- [ ] CSRF/origin/redirect/outage tests and signoff complete; public intake off.

**Evidence index:** Not recorded. **Result:** Not verified complete.
