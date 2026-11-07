# PotholeMap — M11 Quality & Accessibility

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M10](M10_MODERATION_WORKFLOW.md) · **Next:** [M12](M12_PRODUCTION_READINESS.md).

## 1. Objective and boundaries

Verify the complete MVP on representative browsers/devices and failure conditions; fix behavior/accessibility/performance defects without extending product scope. Entry requires M10 exit, M5 explorer, existing security tests and user activation.

**In:** Isolated finite E2E harness, core/failure journeys, real mobile QA, accessibility, measured API/map/media performance and regression triage. **Out:** New features, production user traffic, broad redesign, analytics platform, security certification or public release.

M11-01 selects browser tooling compatible with repo/CI and documents setup. Proposed release targets, confirmed before measurement: core client text/controls meet WCAG 2.2 AA where applicable; no critical/serious automated accessibility findings left without documented manual disposition; usability at 200% zoom; bounded read/render counts; p95 warm read API target ≤1 s on a recorded modest pilot dataset, with cold/network behavior reported separately. Set concrete image/page/interaction budgets from the implemented runtime; never manufacture a passing benchmark.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M11-01 | M10 exit | not_started | E2E tool/environment setup pending |
| M11-02 | M11-01 | not_started | Core journey regression pending |
| M11-03 | M11-01, M11-02 | not_started | Failure/privacy regression pending |
| M11-04 | M11-02, M11-03 | not_started | Real-device/accessibility QA pending |
| M11-05 | M11-02, M11-03 | not_started | Performance budgets/evidence pending |
| M11-06 | M11-01–M11-05 | not_started | Defect triage/exit pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M11-01 — Finite isolated browser harness

- **Objective/scope:** Introduce `pnpm test:e2e`, pinned browser toolchain/setup, deterministic local/isolated DB/media fixtures, reset/cleanup guards and safe CI artifact handling.
- **Excluded:** Production fixtures/credentials, storing real session tokens in artifacts, globally disabling CAPTCHA/MFA to ease tests.
- **Files:** Browser config/tests, package/lock scripts, isolated fixture helpers, CI extension and testing runbook.
- **Acceptance:** Clean runner starts app/fixtures, runs finite tests and exits with meaningful status; no production target possible by default. Provider mocks/test keys are explicitly test-only and cannot be selected in production; retain separately labeled real integration evidence from M6/M9/M10.
- **Verification/security:** Positive/intentional-failure run, target guard tests, CI secrets/artifact redaction review and cleanup after failed run.
- **PR/evidence:** Exact setup/test commands, environment model, artifact retention and runner evidence.

### M11-02 — Core product journeys

- **Objective/scope:** Automate explorer/filter/detail, manual-pin/GPS denial, valid submission/pending, authorized admin review, approve→public and reject→hidden; independent browser contexts for roles.
- **Excluded:** Treating mock approval as proof of real DB transaction/auth, adding product features to satisfy brittle tests.
- **Files:** E2E specs/fixtures and narrow defects exposed by them.
- **Acceptance:** Assertions check persisted/visible behavior and role separation; no arbitrary long sleeps; stable event-based waiting; failures diagnose an actual broken journey. Photo output/privacy verified through relevant existing integration tests.
- **Verification/security:** `pnpm test:e2e`, applicable DB/API suites and targeted repeat only when flakiness or changes justify it.
- **PR/evidence:** Journey matrix, actual passing/failure output and integration-vs-mocked boundaries.

### M11-03 — Failure, race and privacy journeys

- **Objective/scope:** Automate expired admin session, 409 conflict, lost creation response/retry, 429/503, bot failure, offline/tile/media outage, invalid upload and takedown behavior.
- **Excluded:** Disabling security assertions, using real third-party outage/abuse or calling pending detail public for test convenience.
- **Files:** E2E/targeted integration tests and scoped fixes.
- **Acceptance:** One report per idempotent retry, honest error/maintenance states, no stale/private data, no endless client retry and no tokens/photos in captured artifacts. Old signed links expire within recorded policy.
- **Verification/security:** E2E plus DB/security suites, event/report/object counts and sensitive artifact inspection.
- **PR/evidence:** Failure/race matrix, privacy checks and defect links.

### M11-04 — Real mobile and accessibility audit

- **Objective/scope:** Real Android/iOS browser checks where available, desktop keyboard/screen-reader representative checks, camera/file picker, touch/manual pin, focus/dialog/list alternative, contrast/zoom and announcement review.
- **Excluded:** Claiming emulation is real-device testing or an automated scan proves complete accessibility.
- **Files:** QA evidence/runbook, scoped UI fixes and regressions.
- **Acceptance:** Record device/OS/browser/date and actual scenarios; no blockers in submit/review/explore/manual location paths; accessible list offers practical map alternative. Missing required real-device access is an explicit release blocker/maintainer decision, not an invented pass.
- **Verification/security:** Manual walkthrough plus recorded automated accessibility scan, 200% zoom/keyboard and representative assistive-tech checks. Screenshots use synthetic data.
- **PR/evidence:** Device/accessibility matrix, findings/dispositions and user-visible limitations.

### M11-05 — Measured performance and resource budgets

- **Objective/scope:** Measure warm/cold API behavior, spatial query cost, map summary/page/marker bounds, image output/transfer, client rendering and tile usage against a recorded modest dataset/traffic profile; optimize only demonstrated bottlenecks.
- **Excluded:** Unlimited production load, paid observability/cache service, performance claims without network/runtime context.
- **Files:** Reproducible bounded measurement scripts/fixtures, documented budgets and scoped performance changes.
- **Acceptance:** Final budgets recorded before evaluation; p95/read/render/resource observations reproducible; requests/results remain bounded. If targets fail, fix scope-relevant bottleneck or record an explicit release decision without quietly changing targets afterward.
- **Verification/security:** Target-runtime/representative-device measurements, query plans and current quota headroom; stop synthetic load before exceeding approved provider budgets.
- **PR/evidence:** Dataset/concurrency/network details, before/after values, cold-path limitations and quota implications for M12.

### M11-06 — Defect triage and quality exit

- **Objective/scope:** Close release-blocking quality defects, keep nonblocking follow-ups scoped, stabilize harness and hand off release-quality evidence.
- **Excluded:** One PR containing unrelated defect backlog or starting production readiness while required QA is still unverified.
- **Files:** This ledger, QA matrix and focused regression fixes; split independent defects into issues.
- **Acceptance:** Required journey/accessibility/performance evidence linked; critical security/data-loss defects resolved; accepted nonblocking limitations have owner/reason and release impact. No flaky test silently disabled.
- **Verification/security:** Shared quality gates, `pnpm test:db`, `pnpm test:e2e` and targeted checks for final changes.
- **PR/evidence:** Quality exit report/signoff and M12 release-risk list; stop here.

## 4. Exit gate

- [ ] Finite isolated E2E runner and safe artifacts operational.
- [ ] Core/failure/race/privacy journeys pass with real integration boundaries disclosed.
- [ ] Required real-device and accessibility evidence recorded.
- [ ] Performance/resource targets and measurements reviewed.
- [ ] Blockers resolved, limitations explicit and maintainer signoff recorded.

**Evidence index:** Not recorded. **Result:** Not verified complete. Public intake remains off.
