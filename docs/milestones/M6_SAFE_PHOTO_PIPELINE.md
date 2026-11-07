# PotholeMap — M6 Safe Photo Pipeline

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M5](M5_REPORT_EXPLORER.md) · **Next:** [M7](M7_REPORT_SUBMISSION.md).

## 1. Objective and boundaries

Prove safe server-side sanitization/private storage within approved infrastructure and handle interrupted operations without exposing raw files. Entry requires M5/default-order signoff, M2 schema/permissions, M0-03 runtime assessment and user activation.

**In:** Media threat/limit contract, measured runtime go/no-go, actual decoding/re-encoding, private object adapter, authorized-test harness and safe reconciliation. **Out:** Public upload route, report form, admin UI, public signed URLs, AI detection, broker or paid image service.

**Hard stop:** If safe processing cannot fit approved free infrastructure, do not mark M6 complete or proceed to M7. Produce measured evidence and an ADR with alternatives for user decision. Browser compression, MIME headers, EXIF removal alone, human review or a queue are not substitutes for safe decoding/re-encoding. A candidate runtime changes architecture only through the documented decision process.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M6-01 | M5 exit, M0-03 | not_started | Media policy/runtime gate pending |
| M6-02 | M6-01 go decision | not_started | Sanitizer pending |
| M6-03 | M6-02, M2-04 | not_started | Private storage adapter pending |
| M6-04 | M6-03 | not_started | Compensation/reconciliation pending |
| M6-05 | M6-02, M6-03, M6-04 | not_started | Adversarial/runtime evidence pending |
| M6-06 | M6-01–M6-05 | not_started | Exit audit pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M6-01 — Media policy and measured runtime decision

- **Objective/scope:** Choose accepted formats/output, file/pixel/dimension caps, metadata policy, decode strategy, CPU/memory safety margin and per-upload resource budget; verify candidate codec licensing/runtime compatibility. Initial input ceiling is at most 5 MB, JPEG/PNG/WebP subset only; stricter limits are allowed with UX documentation.
- **Excluded:** Assuming desktop benchmarks prove edge feasibility, accepting SVG/HTML, new billed service or async architecture without approval.
- **Files:** Media-contract notes, disposable safe benchmark harness/fixtures and image-runtime ADR linked to ADR-001.
- **Acceptance:** Actual approved target runtime handles representative and hostile bounded inputs within documented limits; dynamic vendor quotas verified; go/no-go explicit. Animated/multipage images must be rejected or reduced via a documented safe rule, not accidentally expanded.
- **Verification/security:** Measured target CPU/memory/resource failures, oversized dimensions/truncation/malformed headers and output size/format inspection. Fixtures are synthetic/licensed and safe to retain.
- **PR/evidence:** Dated measured results, chosen caps/codec and architecture decision. No-go yields a blocker/ADR, not a fake passing milestone.

### M6-02 — Server-side image validation and sanitization

- **Objective/scope:** Sniff actual bytes, bound decoded dimensions/pixels, decode/re-encode safe output, strip metadata, reject unsupported/corrupt content and generate unpredictable object identifiers.
- **Excluded:** Trusting extension/Content-Type or publishing original bytes when decoding fails.
- **Files:** `src/server/modules/media/`, validators, bounded fixtures and sanitizer tests.
- **Acceptance:** Valid supported input yields a verified safe image without EXIF/location metadata; spoofed/corrupt/excessive input fails before unbounded allocation. Output obeys chosen dimensions/bytes/type and contains no user filename in keys.
- **Verification/security:** Boundary and malicious sample suite, independent output decode/metadata inspection and runtime failure checks; do not decompress intentionally huge samples outside enforced harness limits.
- **PR/evidence:** Input/output matrix, limits evidence and sanitization regressions.

### M6-03 — Private object storage adapter

- **Objective/scope:** Private bucket configuration with restricted grants, sanitized-only object writes, generated keys, explicit storage error mapping and test-only preview under controlled authorization.
- **Excluded:** Anonymous bucket reads/writes, public bucket as shortcut, issuing public report photo links before M10.
- **Files:** Storage adapter/config/migration or runbook as supported, env examples and permission tests.
- **Acceptance:** No raw input written to publishable storage; direct anonymous/ordinary-user requests denied; uploaded object preserves verified output type; failures do not leak service secrets/paths. Test harness is local/isolated and not an unguarded HTTP upload endpoint.
- **Verification/security:** Direct storage API read/write denial, name collision/overwrite checks, failed upload and least-privilege review.
- **PR/evidence:** Bucket/grant matrix, sanitized object evidence and config/setup runbook.

### M6-04 — Upload staging, compensation and orphan cleanup

- **Objective/scope:** Stable operation/object ownership, failed-write compensation and bounded manual reconciliation procedure. Define active-upload protection, grace period, checkpoints and reference rechecks before object deletion.
- **Excluded:** Cron/queue job system, deleting on a DB timeout without reconciliation, sweeping an entire production bucket as a test.
- **Files:** Media/data operation helpers as needed, `docs/runbooks/MEDIA_CLEANUP.md`, failure-injection tests.
- **Acceptance:** Confirmed DB failure removes staged media or records a retryable cleanup failure; unknown commit outcome is reconciled before deletion. Concurrent submit/cleanup cannot delete active or referenced objects. Use a demonstrably safe shared ownership/lease mechanism if grace/recheck alone cannot prevent races; avoid inventing a generic jobs system.
- **Verification/security:** Crash/timeout/delete-failure scenarios, referenced/unreferenced/recent-object fixtures, bounded dry-run then verified isolated deletion and rerun tests.
- **PR/evidence:** State/failure matrix, safe cleanup commands/target guards and exact reconciliation/retention assumptions for M7/M12.

### M6-05 — Media adversarial and quota verification

- **Objective/scope:** Integrate real-runtime tests for supported input, malformed/spoofed content, dimension limits, EXIF stripping, private access and failure cleanup.
- **Excluded:** Unbounded stress attacks, real user images, claiming quota safety from a single happy-path image.
- **Files:** Targeted media/integration fixtures/tests and scoped fixes.
- **Acceptance:** Tests demonstrate fail-closed behavior; output remains private on all failures; representative max-allowed inputs fit budget with margin, or limits/runtime decision is revised before exit.
- **Verification/security:** Media suite via existing runner, isolated storage checks, target-runtime measurements and shared quality gates; record dataset/inputs/concurrency.
- **PR/evidence:** Negative-case matrix, resource measurements and residual limitations.

### M6-06 — Exit gate and submission handoff

- **Objective/scope:** Finalize the sanitizer/storage/reconciliation interface and document how M7 persists pending reports safely; ensure M10 owns public media access.
- **Excluded:** Creating the public multipart endpoint or bypassing unresolved runtime risks.
- **Files:** This ledger, media ADR/runbooks and affected downstream contracts.
- **Acceptance:** All media/runtime/private-storage criteria have evidence; receipt/persistence interface is implementable; no photo route publicly enabled. Any unresolved sanitization blocker prevents signoff.
- **Verification/security:** Applicable media/DB tests, existing quality gates and independent output/permission review.
- **PR/evidence:** Exit signoff, measured limits, cleanup interface and M7 integration handoff; stop here.

## 4. Exit gate

- [ ] Approved runtime sanitizes within measured limits; explicit go decision.
- [ ] Actual-byte, format, decoded-pixel, metadata and output checks pass.
- [ ] Private bucket permissions and generated-object policy proven.
- [ ] Crash/unknown-commit/cleanup races and safe reruns tested.
- [ ] Submission/media interface and evidence signed off; public intake/media still off.

**Evidence index:** Not recorded. **Result:** Not verified complete. Unresolved runtime safety is a blocking gate, not an accepted launch limitation.
