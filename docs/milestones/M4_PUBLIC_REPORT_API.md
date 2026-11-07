# PotholeMap — M4 Public Report API

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M3](M3_MAP_FOUNDATION.md) · **Next:** [M5](M5_REPORT_EXPLORER.md).

## 1. Objective and boundaries

Deliver indexed, bounded, approved-only read endpoints against isolated synthetic data. Entry requires M3/default-order signoff, M2 DB/security evidence and user activation.

**In:** Read schemas/DTOs, HTTP/data/geo modules, bbox/status/cursor validation, list/detail endpoints, safe errors and read-load bounds. **Out:** Anonymous writes, real photos, private storage access, admin APIs and production data migration.

Proposed initial contract to confirm in M4-01: bbox is west,south,east,north; finite valid geographic coordinates with west < east and south < north; unsupported antimeridian boxes return 400; maximum span 1 degree per axis; default limit 100 and hard cap 200; stable descending `(created_at,id)` paging. Oversized/invalid explicit limits return 400. `status` filters approved road status, never moderation state. Tune these bounds with recorded fixture/provider evidence, not unbounded fallback.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M4-01 | M3 exit, M2-03 | not_started | Read contract pending |
| M4-02 | M4-01, M2-04, M2-05 | not_started | Indexed repository/RPC pending |
| M4-03 | M4-02 | not_started | List endpoint pending |
| M4-04 | M4-02, M4-03 | not_started | Detail/privacy behavior pending |
| M4-05 | M4-03, M4-04 | not_started | Boundary/security integration pending |
| M4-06 | M4-01–M4-05 | not_started | Exit handoff pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M4-01 — Public read schema and error contract

- **Objective/scope:** Define bbox/status/limit/cursor schemas, response DTOs and error mapping. List returns ID, point and road-status summary, pagination/truncation metadata; detail adds bounded description/date and optional safe media field.
- **Excluded:** Exposing row-shaped responses, photo paths, reviewer IDs or audit data; building future write endpoints.
- **Files:** Shared API types/validators, geo validation tests and read-contract notes in this file.
- **Acceptance:** Defaults/caps and stable order are explicit; optional image is null/omitted until M10; malformed/unknown filters fail predictably. Cursor validates shape/types/order and is bound to the active query/filter semantics; no dynamic SQL interpolation.
- **Verification/security:** Invalid/NaN/infinite/reversed/out-of-range/antimeridian bbox tests; limit/status/cursor fuzz cases; DTO allowlist review.
- **PR/evidence:** Final request/response examples, boundary table and downstream M3/M5 alignment.

### M4-02 — Approved-only spatial repository

- **Objective/scope:** Parameterized indexed query/RPC via chosen server adapter; approved predicate is mandatory in list/detail paths; stable paging and constrained projections.
- **Excluded:** Direct browser DB reads, service-role access treated as RLS proof, broad callable RPC grants.
- **Files:** `src/server/modules/reports/`, `geo/`, data adapters and narrowly scoped migration if needed.
- **Acceptance:** Approved fixture points inside bbox match expected results; pending/rejected never match; equal timestamps paginate without duplicates/missing rows on a static dataset. Query work is bounded; concurrent data changes have documented paging semantics.
- **Verification/security:** `pnpm test:db`, role/grant checks for new functions and representative explain evidence; test empty/boundary/tied-order fixtures.
- **PR/evidence:** Query/RPC contract, grant changes, results and plan observations.

### M4-03 — List endpoint and constrained reads

- **Objective/scope:** Implement `GET /api/reports` with validated inputs, request ID, consistent errors, upstream timeout and capped serialized response; apply available safe read throttling and record its limitations for M8.
- **Excluded:** Public report creation, unbounded scans, caching private responses or per-instance limiter presented as global protection.
- **Files:** HTTP route/middleware, report read service, integration tests.
- **Acceptance:** Expected 200/400/429/503 behavior; response contains only allowed fields; has-more/cursor behavior is truthful; too-wide views request zoom rather than returning all rows. No debug/SQL/secret detail in errors.
- **Verification/security:** Local HTTP + isolated DB checks, slow/unavailable DB simulation, repeated reads under bounded load and payload inspection. If shared read throttling is not yet implemented, keep exposure isolated and record M8 gate.
- **PR/evidence:** Route examples, failure matrix, bounded-load observation and residual exposure controls.

### M4-04 — Detail endpoint and private-record indistinguishability

- **Objective/scope:** Implement `GET /api/reports/:id`; valid approved details only; null/placeholder media pending M10 integration.
- **Excluded:** Signed storage URLs before authorization/publication policy, report-submitter identity and pending-status tracking API.
- **Files:** Detail route/service, DTO mapping and targeted integration tests.
- **Acceptance:** Unknown/pending/rejected valid IDs return the same public 404 shape; malformed IDs follow documented validation; approved details have only allowed fields. No raw path or preview token leaks through nested metadata.
- **Verification/security:** Guessed-ID and direct-data-access matrix, DTO snapshot/allowlist tests and upstream error handling.
- **PR/evidence:** Detail examples and privacy regression results for M5/M10.

### M4-05 — Public-read security and query regression

- **Objective/scope:** Integrate list/detail tests covering forged filters/cursors, direct RPC grants, mixed moderation states, response caps and DB failure.
- **Excluded:** Full production load test or claims of launch readiness.
- **Files:** API/DB integration fixtures/tests and scoped fixes.
- **Acceptance:** No parameter combination removes approved-only filtering; large queries terminate at limits; errors do not leak internals. Existing health/SPA routing and DB permission tests remain green.
- **Verification/security:** `pnpm test:db`, relevant API suite via configured `pnpm test`, shared quality gates; modest synthetic load with dataset/concurrency recorded.
- **PR/evidence:** Test matrix, payload/latency observations and actionable M8/M11 follow-ups.

### M4-06 — Exit audit and explorer handoff

- **Objective/scope:** Reconcile API types, defaults and pagination with M3/M5; document media placeholder and remaining public-exposure gates.
- **Excluded:** Wiring live explorer or changing production data.
- **Files:** This ledger/evidence, API documentation at the established location and downstream contract corrections if needed.
- **Acceptance:** All read criteria verified; M5 can consume the concrete API without guessing fields; M10 owns later safe media integration.
- **Verification/security:** Read/DB tests, existing quality gates and contract review.
- **PR/evidence:** Exit signoff, real examples and selected next issue proposal; stop here.

## 4. Exit gate

- [ ] Typed bounded bbox/status/cursor contract and indexed queries proven.
- [ ] Approved-only list/detail; private/unknown IDs do not reveal private state.
- [ ] No private metadata/media exposure; truthful paging and graceful failure.
- [ ] DB/API security tests and quality gates pass with evidence.
- [ ] Explorer handoff/signoff complete; public-intake gate remains closed.

**Evidence index:** Not recorded. **Result:** Not verified complete.
