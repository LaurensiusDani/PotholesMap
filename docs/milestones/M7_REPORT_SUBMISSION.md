# PotholeMap — M7 Report Submission

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M6](M6_SAFE_PHOTO_PIPELINE.md) · **Next:** [M8](M8_ABUSE_PREVENTION.md).

## 1. Objective and boundaries

Deliver the anonymous photo/location submission journey **in isolated test environments**, with pending receipt, server validation and duplicate-safe mobile retries. Entry requires M6 safe-runtime exit, M5 explorer/M3 location selector, M2 DB/security foundation and user activation.

**In:** Multipart contract, safe body limits, server Turnstile verification, conservative shared write limits, idempotency, media/report persistence, form and failure states. **Out:** Public rollout, admin moderation, signed public photos, semantic duplicate detection, reporter accounts, processing queues and status-polling API.

Public intake stays off through M12. M8 hardens the baseline controls built here; M7 does not expose an unprotected endpoint while waiting for M8. API remains synchronous: 201 only for durably persisted safe image + pending row, never 202/background promises under this baseline.

Proposed form limits to reconcile with M6: one supported photo no larger than M6's approved cap (at most 5 MB), total multipart body capped with explicit bounded overhead, finite lat/lon, optional plain description up to 1,000 characters. Creation time/state/actor/path come from server, never client.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M7-01 | M6 exit, M5 exit | not_started | Submit/receipt/body contract pending |
| M7-02 | M7-01, M2-04 | not_started | Write gate/Turnstile/shared baseline limits pending |
| M7-03 | M7-01, M7-02, M6-04 | not_started | Durable retry ownership pending |
| M7-04 | M7-02, M7-03, M6-03 | not_started | Pending persistence endpoint pending |
| M7-05 | M7-01, M7-04, M3-04 | not_started | Photo/location form pending |
| M7-06 | M7-04, M7-05 | not_started | Retry/failure integration pending |
| M7-07 | M7-01–M7-06 | not_started | Exit audit pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M7-01 — Submission and receipt contract

- **Objective/scope:** Define multipart fields, actual body/file/field-count limits, idempotency-key header, response/error schemas and create/receipt state machine. Explain that a receipt does not make a pending report publicly readable.
- **Excluded:** Client-chosen moderation/road state, paths/timestamps/actors, separate public upload endpoint or public pending-detail API.
- **Files:** Submission validators/types, API documentation and focused parsing tests.
- **Acceptance:** Unknown/duplicate/missing fields and multiple files have defined rejection; oversized actual streams terminate safely; proposed description/body limits finalized with M6 output/runtime evidence. Error codes allow useful form recovery without private details.
- **Verification/security:** Invalid coordinates/text/field counts, absent/lying Content-Length, malformed multipart and over-limit streaming tests.
- **PR/evidence:** Concrete request/201/error examples, final caps and client contract for later form work.

### M7-02 — Intake switch, bot verification and shared baseline limits

- **Objective/scope:** Default-off intake configuration, isolated-environment enablement, server Turnstile verification and a conservative shared atomic write limiter with bounded retention. M8 reviews/measures/refines this same limiter rather than building a second one.
- **Excluded:** Production enablement, memory-only authoritative counters, trusting client bot verdicts or adding Redis/broker.
- **Files:** Server security/operations helpers, narrow operational SQL if needed, env example and limiter/bot tests.
- **Acceptance:** Disabled intake returns 503 before expensive work; tokens validated with relevant hostname/action/environment expectations and provider failure handled closed. Multi-instance writes share limits; identifiers use trusted ingress context and minimize/expire personal data. Final initial thresholds documented for M8 measurement.
- **Verification/security:** Bad/expired/reused token, verifier outage, forged address headers, concurrent limit consumption and missing config cases. Use official test fixtures/keys only in isolated tests.
- **PR/evidence:** Switch/limiter policy, provider expectations, direct-grant review and negative-case results.

### M7-03 — Idempotency and operation ownership

- **Objective/scope:** Durable bounded submission-key records, canonical payload digest (excluding replaceable bot token), operation/report/object IDs, concurrency ownership/lease and retention/reconciliation rules aligned with M6.
- **Excluded:** Semantic duplicate pothole detection, generic event/job table, unbounded receipt lookup or treating a request ID as authentication.
- **Files:** Narrow migration/RPC, submission/data helpers, DB/concurrency tests and retry-contract documentation.
- **Acceptance:** Same-key/same-payload concurrent requests create at most one report/object ownership chain; changed payload conflicts. Completed replay returns only original minimal receipt and still passes request/abuse bounds without consuming the already-used CAPTCHA token. New or resumed processing needs a valid token and safe ownership; in-progress/expired-key behavior is explicit, with no blind automatic duplicate creation after unknown commits.
- **Verification/security:** Two-worker races, response loss after commit, lease expiry/slow operation, key abuse, changed payload and retention boundary tests. Internal records/RPC remain inaccessible to public direct calls.
- **PR/evidence:** Operation-state diagram/table, digest/expiry policy and failure/concurrency results.

### M7-04 — Synchronous pending creation endpoint

- **Objective/scope:** Wire guards → validation → operation ownership → M6 sanitize/private write → transactional pending report/idempotency completion. Add compensation/reconciliation for each partial failure.
- **Excluded:** Public approval/photo links, 201 before commit, unawaited critical persistence and production mutation.
- **Files:** `POST /api/reports`, report create service, restricted data/RPC and integration tests.
- **Acceptance:** Success persists pending/reported row and safe private object once; audit/admin ownership is not forged by input. Confirmed insert failure compensates; unknown commit reconciles before retry/delete; cleanup failure remains recoverable. Pending/rejected rows still invisible via M4 endpoints.
- **Verification/security:** Isolated HTTP/DB/storage end-to-end create, failure injection at sanitizer/upload/commit/response/delete stages and existing direct-access matrix.
- **PR/evidence:** Actual response/persistence evidence, rollback/reconciliation outcomes and final grants review.

### M7-05 — Anonymous mobile report form

- **Objective/scope:** Camera/file picker, local preview/validation, reuse optional GPS/manual pin, optional description, consent/notice, Turnstile integration, busy/error states and pending receipt.
- **Excluded:** Auto-submit, mandatory GPS/login, fake success or bypassing server validation through client compression.
- **Files:** Report feature form/components, client submit adapter and interaction tests.
- **Acceptance:** User can deny GPS, manually choose location and submit valid input in isolation; double tap is controlled; object URLs/resources cleaned up. Image preview uses safe supported types; compression if added preserves server enforcement and is measured. Success clearly says not public/no repair guarantee.
- **Verification/security:** Camera/file selection, unsupported/oversized input, location correction, keyboard/screen-reader labels and async failure/retry UX; no persistent photo/location storage without a product decision.
- **PR/evidence:** Form state captures, permission/copy review and privacy behavior.

### M7-06 — Mobile retry and failure integration

- **Objective/scope:** Exercise full form→API→storage/DB flow with lost response, double tap, token expiry, rate limit, disabled intake and unavailable dependencies; fix scoped defects.
- **Excluded:** Public rollout or admin approval journey before M10.
- **Files:** Submission integration tests/fixtures and client/server fixes.
- **Acceptance:** Lost success response followed by same-key retry returns one receipt/report; errors preserve useful user input without false success; explicit new attempts/key expiry follow documented behavior. Reads still hide the pending row; no raw object is publicly accessible.
- **Verification/security:** `pnpm test:db`, targeted existing-runner tests, manual mobile/emulated network checks with exact conditions; no real reporter data.
- **PR/evidence:** Retry/failure matrix, report/object counts and privacy assertions.

### M7-07 — Exit handoff to abuse hardening

- **Objective/scope:** Collect creation/form/retry evidence, reconcile limits and operation ownership with M6/M8/M10, document test-only enablement and production-off behavior.
- **Excluded:** Enabling public intake, implementing moderation or claiming beta readiness.
- **Files:** This ledger, submission/security notes and affected runbooks/contracts.
- **Acceptance:** M7 criteria verified in isolation; baseline limiter exists and has a measurable M8 handoff; intake remains default-off and no premature public photo path exists.
- **Verification/security:** Shared quality gates, DB/submission suites, built-config inspection and scoped diff review.
- **PR/evidence:** Exit signoff, exact limits/controls and outstanding hardening work; stop before M8.

## 4. Exit gate

- [ ] Body/file/coordinate/description checks and server bot verification proven.
- [ ] Shared baseline write limits, default-off switch and fail-closed behavior.
- [ ] Durable duplicate-safe receipt/persistence and reconciliation tested.
- [ ] Mobile/GPS-denial/manual-pin form with honest pending state.
- [ ] Private pending media/rows and production-off config verified; signoff recorded.

**Evidence index:** Not recorded. **Result:** Not verified complete.
