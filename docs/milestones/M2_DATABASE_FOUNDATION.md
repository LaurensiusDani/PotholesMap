# PotholeMap — M2 Database Foundation

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M1](M1_APPLICATION_SHELL.md) · **Next:** [M3](M3_MAP_FOUNDATION.md).

## 1. Objective and boundaries

Create a reproducible isolated database with PostGIS, core schema, restrictive permissions and synthetic fixtures. Entry requires M1 exit/default-order signoff, M0 runtime/environment evidence and user activation.

**In:** Local Supabase tooling, SQL migrations, indexes/constraints, grants/RLS, HTTP Data API/RPC boundary assessment and integration tests. **Out:** Production migrations, public write API, real uploads, actual admin sessions, full moderation procedures and job infrastructure.

Use the master's `reports`, `admin_profiles` and required append-only `report_status_events` model. Decide enum/check representation and version column here; preserve pending/reported defaults. The audit table may exist now, but executable admin mutations belong to M10. Do not grant placeholder privileged functions just to make a demo work.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M2-01 | M1 exit, M0-09 | not_started | Isolated local DB tooling pending |
| M2-02 | M2-01 | not_started | Core schema pending |
| M2-03 | M2-02 | not_started | Spatial/index contract pending |
| M2-04 | M2-02, M2-03 | not_started | Permissions/RPC boundary pending |
| M2-05 | M2-04 | not_started | Fixtures/security integration pending |
| M2-06 | M2-01–M2-05 | not_started | Migration/recovery handoff pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M2-01 — Isolated development database

- **Objective/scope:** Pin the local Supabase toolchain, document required local container/runtime support and introduce `pnpm db:start` / `pnpm db:reset` wrappers with explicit local-target protection.
- **Excluded:** Installing system software without required approval, defaulting to production when local tooling is absent, paid staging.
- **Files:** Package scripts/lock as needed, local Supabase config, `.env.example`, `docs/runbooks/DATABASE.md`.
- **Acceptance:** Clean development machine prerequisites are documented; start/reset are repeatable; reset rejects hosted/unknown targets; no production credentials required. An unavailable local runtime is an honest blocker; any alternate isolated environment needs a documented decision.
- **Verification/security:** Start/reset a disposable verified local stack; negative target-guard test and environment/secret review.
- **PR/evidence:** Exact tool versions/commands, isolation evidence and recorded setup limitations.

### M2-02 — Core schema and constraints

- **Objective/scope:** Migrations for reports, admin profiles and status events; server timestamps, nullable reviewer fields, optimistic version and enum/check constraints. Define auth-user/report relationships and deliberate delete behavior.
- **Excluded:** User accounts for reporters, operational job/idempotency tables, new moderation states without requirement.
- **Files:** `supabase/migrations/`, schema tests and database runbook.
- **Acceptance:** Fresh reset builds the schema; pending/reported defaults hold; invalid/null required data fails; event history cannot be silently removed by an unintended cascade. Synthetic admin identities are linked to local Auth correctly.
- **Verification/security:** SQL constraint/FK/default tests, invalid-state inserts and timestamp/version behavior. No real reporter/admin identities in seeds.
- **PR/evidence:** Schema summary, migration/reset evidence and delete-policy rationale.

### M2-03 — Geospatial and query foundations

- **Objective/scope:** `geography(Point,4326)`, coordinate-validity constraints, GiST index and moderation/stable-order indexes; establish lon/lat conventions and public-query plan expectations.
- **Excluded:** Frontend map, live public endpoint or index proliferation without query need.
- **Files:** SQL migrations, geo fixtures/tests and query notes.
- **Acceptance:** Swapped/invalid coordinates are rejected where invalid; known points match an expected bbox; stable `(created_at,id)` ordering is defined; representative explain evidence exists. Do not claim a tiny fixture must always choose an index scan.
- **Verification/security:** Boundary-coordinate tests, bbox fixtures and explain on a representative synthetic dataset with recorded size.
- **PR/evidence:** Coordinate conventions, indexes, query-plan observations and limitations.

### M2-04 — Grants, RLS and data-access boundary

- **Objective/scope:** Restrict tables/functions, deny anonymous/ordinary-user writes and admin self-promotion; decide server-only HTTPS Data API/RPC adapter strategy. Public browser report reads remain through Workers, not a broad direct-table policy.
- **Excluded:** Shipping service-role keys to clients, implementing M9 sessions or M10 admin mutation functions.
- **Files:** Permission migrations, minimal server adapter/config only as needed for proof, security tests, ADR for data access if it changes assumptions.
- **Acceptance:** Direct public/ordinary-user calls cannot read private rows or write roles/reports/events; private app internals are not exposed through views/functions. Service-role bypass is documented and tested separately from RLS. Future RPC execute grants default closed.
- **Verification/security:** Direct Data API/SQL role matrix, forged actor/role payloads and bundle/secret checks; review elevated functions/search paths if any.
- **PR/evidence:** Role/action matrix, transport proof and least-privilege rationale; no claim of implemented admin authorization.

### M2-05 — Fixtures and DB regression harness

- **Objective/scope:** Deterministic approved/pending/rejected synthetic reports, stable timestamps and local identities; introduce finite `pnpm test:db` against isolated targets.
- **Excluded:** Production seeding, real photos/people, weakening grants to run tests.
- **Files:** Local seed/fixture scripts, DB integration tests, package scripts and DB runbook.
- **Acceptance:** Reset/seed/test repeats cleanly; constraints and direct-access denials have regressions; test credentials cannot target production. Synthetic photo paths are placeholders, never a public-media bypass.
- **Verification/security:** `pnpm db:reset`, `pnpm test:db`, intentional denied-access cases and shared quality gates where code changed.
- **PR/evidence:** Repeatability, role-matrix results and fixture lifecycle/cleanup instructions.

### M2-06 — Migration lifecycle and exit audit

- **Objective/scope:** Document local application, compatible evolution, backup/recovery requirements for future production changes and separation from app deploy. Reconcile schema with M4/M6/M9/M10 contracts.
- **Excluded:** Applying production schema or building the public API.
- **Files:** Database runbook, ADR/schema notes and this evidence ledger.
- **Acceptance:** Clean local reset passes; migration changes have forward-safe/recovery guidance; local reset is never described as production rollback; all M2 evidence reviewed.
- **Verification/security:** Fresh isolated setup, `pnpm test:db`, existing quality gates and permissions review.
- **PR/evidence:** Exit report, schema/transport decisions, future compatibility notes and maintainer signoff; stop before M3.

## 4. Exit gate

- [ ] Isolated start/reset guard and clean migrations verified.
- [ ] Core schema, audit model, geo conventions and representative indexes tested.
- [ ] Direct-access grant/RLS matrix passes; no client privileged key.
- [ ] Deterministic fixtures and `pnpm test:db` repeatable.
- [ ] Migration runbook and downstream contracts reconciled; signoff recorded.

**Evidence index:** Not recorded. **Result:** Not verified complete. No production schema is claimed.
