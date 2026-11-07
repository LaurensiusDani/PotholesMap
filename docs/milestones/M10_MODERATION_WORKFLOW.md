# PotholeMap — M10 Moderation Workflow

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M9](M9_ADMIN_AUTHENTICATION.md) · **Next:** [M11](M11_QUALITY_ACCESSIBILITY.md).

## 1. Objective and boundaries

Complete the isolated submit → private review → approve/reject → public-read journey with safe media, atomic audit and concurrency protection. Entry requires M9 exit, M6 media, M7 creation, M8 controls and user activation.

**In:** Transition policy, admin queue/detail/private preview, transactional decisions and road status, audit UI, public photo eligibility and takedown. **Out:** Public beta activation, automatic repair verification, reporter accounts, notifications, reputation, background broker or government integration.

Keep moderation and road condition separate. Approval does not claim official verification or repair. Proposed minimal moderation transitions: pending → approved/rejected, approved → rejected for takedown; rejected stays terminal unless an explicit reopening policy is approved. M10-01 finalizes road transitions, reason requirements and retry/no-op behavior. Any policy change must update API/UI/tests/master together.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M10-01 | M9 exit, M2-02 | not_started | Transition/audit policy pending |
| M10-02 | M10-01, M9-04, M6-03 | not_started | Admin queue/private preview pending |
| M10-03 | M10-01, M9-04 | not_started | Atomic moderation API pending |
| M10-04 | M10-02, M10-03 | not_started | Moderation/road-status UI pending |
| M10-05 | M10-03, M10-04, M6-05 | not_started | Public safe media/takedown pending |
| M10-06 | M10-02–M10-05, M7-06 | not_started | Full workflow/concurrency regression pending |
| M10-07 | M10-01–M10-06 | not_started | Exit handoff pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M10-01 — State transitions, audit and takedown policy

- **Objective/scope:** Finalize moderation/road transition matrices, expected-version requirement, rejection/takedown reasons, no-op/retry behavior and required event fields. Define who reviews and how incorrect reports/media are hidden.
- **Excluded:** New enums/roles without approved requirement, treating `fixed` as automatically verified repair, deleting audit history to undo a decision.
- **Files:** Transition schemas/policy tests, narrow compatible migrations if justified and moderation runbook.
- **Acceptance:** Each allowed/forbidden transition has an outcome; actor/time derive from verified session/DB; reasons have bounded privacy-conscious text; rejected/taken-down visibility and image retention are explicit. M12/M13 must finalize durations/policy before real data.
- **Verification/security:** Table-driven state/no-op/stale/reason validation tests and master/API consistency review.
- **PR/evidence:** Exact matrices, event contract and retention/takedown handoff.

### M10-02 — Authorized queue, detail and private preview

- **Objective/scope:** Admin-only paginated queue/detail, safe image preview via authorized short-lived access, filters and stable ordering.
- **Excluded:** Public pending-detail endpoint, public bucket, signing arbitrary client-provided paths or leaking signed URLs in logs/cache.
- **Files:** Admin read routes/services, media authorization helper, queue/detail UI and tests.
- **Acceptance:** Guards require verified current admin/MFA; pending preview only for intended report object; non-admin/direct storage calls denied. Missing/corrupt safe object yields an actionable unavailable state, not raw-file fallback. Photo-link lifetime/caching policy explicit.
- **Verification/security:** Guard/direct-data/storage matrix, manipulated report/path input, expiry, paging/tied-order and unavailable object cases.
- **PR/evidence:** Queue/read schemas, private-preview results and accessibility checks.

### M10-03 — Transactional moderation and status endpoints

- **Objective/scope:** Implement both PATCH routes using restricted DB transactions that recheck admin context, expected version, transition and event insertion. Validate safe-media eligibility before approval without allowing concurrent path/version substitution.
- **Excluded:** Separate update/event SDK calls, trusting actor/path from request, partial commit or privileged public RPC execution.
- **Files:** Moderation services/routes, restricted SQL/RPC migration and transaction/concurrency tests.
- **Acceptance:** Exactly one update/event for a winning transition; stale update → 409; invalid/unauthorized/missing report follow documented errors without writes. Road status and moderation stay separate; audit failure rolls back update. Lost-response retry cannot duplicate a transition/event. Approval of missing/unverified media is denied safely.
- **Verification/security:** `pnpm test:db`, competing reviewers, forced audit failure, forged actor/MFA, direct RPC access and immutable media-reference tests.
- **PR/evidence:** Atomicity/concurrency/grants evidence and final API examples.

### M10-04 — Admin decisions, road updates and audit UX

- **Objective/scope:** Approve/reject/reason actions, pending confirmation, road-status control and report audit view; conflict refresh and expired-session behavior.
- **Excluded:** Bulk moderation, public editing, client-only state changes presented as saved.
- **Files:** Admin review/action/history components and client adapters/tests.
- **Acceptance:** UI shows persistence result before success; stale version invites reload without overwriting another reviewer; reason/state copy matches policy; actions are keyboard usable. Audit view is admin-only and escapes text.
- **Verification/security:** Success/failure/409/session-expiry, double click, malicious reason text and keyboard/focus walkthrough.
- **PR/evidence:** Decision/history captures, conflict behavior and targeted test results.

### M10-05 — Public photo publication and takedown integration

- **Objective/scope:** Replace M4/M5 media placeholders only for eligible approved sanitized images; issue controlled short-lived links or approved equivalent, avoid private metadata exposure and honor takedown in reads/new media grants.
- **Excluded:** Permanent public originals, caching beyond revocation policy or assuming signed links instantly revoke on status change.
- **Files:** Public query/detail DTO/media integration, explorer image/error UI, storage/access tests.
- **Acceptance:** Pending/rejected/unverified media never gets public access; approved safe image displays; takedown removes public row/detail and prevents new links. Previously issued link lifetime is bounded/documented; missing approved media fails/degrades according to the explicit publication policy. Synthetic no-image fixtures remain isolated and cannot bypass production eligibility.
- **Verification/security:** Submit/approve/reject/takedown API+storage paths, guessed IDs, old/new link expiry, cache headers and no path/actor leakage.
- **PR/evidence:** Publication/takedown policy, response examples and privacy regression results.

### M10-06 — Full workflow and concurrent-review regression

- **Objective/scope:** Integrate isolated anonymous creation, private queue/MFA review, approve/reject, road changes, public explorer and cleanup protection.
- **Excluded:** Public launch, real people/photos and production role/DB mutations.
- **Files:** API/DB/UI integration suites and scoped fixes.
- **Acceptance:** Two browsers observe the intended moderated journey; rejected stays hidden; two reviewers cannot silently overwrite; audit counts/actors accurate; referenced private objects survive cleanup and takedown follows retention policy.
- **Verification/security:** Existing suites, `pnpm test:db`, shared quality gates and recorded isolated manual/browser journey; full browser automation may be added in M11 rather than mocked as real evidence.
- **PR/evidence:** Scenario matrix, report/object/event evidence and follow-ups for M11/M12.

### M10-07 — Workflow exit and quality handoff

- **Objective/scope:** Finalize routes/state/media/audit documentation, collect prior evidence and list defects/risks for quality review.
- **Excluded:** Enabling production intake or claiming official repair outcomes.
- **Files:** This ledger, moderation/access/cleanup runbooks and downstream contract corrections.
- **Acceptance:** Every admin write is guarded/atomic/audited, public eligibility tested and isolated end-to-end behavior proven; no unresolved security blocker hidden as polish.
- **Verification/security:** Applicable DB/API/media/auth tests, quality gates and publication/config review.
- **PR/evidence:** Exit signoff, handoff matrix and proposed M11 issue; stop here.

## 4. Exit gate

- [ ] Explicit transitions/reason/no-op/takedown policy.
- [ ] Authorized private queue/media preview; no public pending paths.
- [ ] Guarded transactional changes/audit and stale-review protection proven.
- [ ] Approved safe media displayed; rejection/takedown/link expiry verified.
- [ ] Isolated complete journey and signoff recorded; production intake still off.

**Evidence index:** Not recorded. **Result:** Not verified complete.
