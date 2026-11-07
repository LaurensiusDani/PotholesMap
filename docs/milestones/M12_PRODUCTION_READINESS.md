# PotholeMap — M12 Production Readiness

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M11](M11_QUALITY_ACCESSIBILITY.md) · **Next:** [M13](M13_PUBLIC_BETA_LAUNCH.md).

## 1. Objective and boundaries

Prepare an evidence-backed release candidate with secure operations, tested recovery and explicit privacy/retention. Entry requires M11 exit, M8 controls, M10 moderation and user activation.

**In:** Production configuration/migration plan, quota/health observability, DB+image backup/restore, cleanup/retention/takedown operations, security review and incident/rollback drill. **Out:** Public intake activation, actual public announcement, paid monitoring, unreviewed production schema changes or new product features.

Actual account/schema/secret/deployment changes follow applicable user authorization. Prepare reviewable configuration/migrations/runbooks first; do not interpret this planning file as approval. Production may host a closed/maintenance release candidate; use synthetic fixtures in a disposable recovery target for destructive drills. No real reporter data before M13.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M12-01 | M11 exit, M0-11 | not_started | Release/configuration inventory pending |
| M12-02 | M12-01, M8-04 | not_started | Quota/observability pending |
| M12-03 | M12-01, M2-06, M6-04 | not_started | DB/photo recovery pending |
| M12-04 | M12-01, M6-04, M10-05 | not_started | Retention/takedown operations pending |
| M12-05 | M12-02, M12-03, M12-04 | not_started | Security/configuration audit pending |
| M12-06 | M12-05, M0-11 | not_started | Incident/rollback drill pending |
| M12-07 | M12-01–M12-06 | not_started | Release-candidate signoff pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M12-01 — Release inventory, environments and migration rehearsal

- **Objective/scope:** Inventory resources, data flows, secret owners/scopes, actual free-plan/billing/branch gates, deployment SHA and DB migrations since M0. Rehearse schema application/compatible rollback strategy on a fresh isolated target; prepare any production change as a separate reviewed operation.
- **Excluded:** Automatic generic-deploy migrations, paid resource activation or claiming local migrations prove production settings.
- **Files:** Release inventory, database/deployment/setup runbooks, redacted env/config and compatible migration fixes if needed.
- **Acceptance:** Local/preview/production separation verified; credentials scoped/rotatable; public intake default-off; migration order/backups/recovery understood. Account terms/quotas/billing state rechecked with dates; manual approval gates only claimed if actually enforceable.
- **Verification/security:** Fresh schema rehearsal, shared suites, built-bundle/secret scan, preview resource inspection and exact-SHA deploy-gate evidence.
- **PR/evidence:** Redacted inventory, migration rehearsal results and explicit authorized/unexecuted production steps.

### M12-02 — Health, safe observability and quota headroom

- **Objective/scope:** Health/version diagnostics, sanitized request/error identifiers, upload/error/latency and DB/storage/egress/worker/tile quota review; free-compatible alerts where available and a manual review schedule otherwise.
- **Excluded:** Privileged internals in public health, raw IP/photo/token logs, paid telemetry or pretend alerts that no provider supports.
- **Files:** Operations logging/health helpers, monitoring runbook and redacted dashboard setup notes.
- **Acceptance:** Maintainer can distinguish DB/storage/bot/runtime outage without sensitive data; rate-limit/failed-upload/cleanup evidence observable; thresholds and named response/owner defined. Dashboard/manual checks work with actual account features.
- **Verification/security:** Isolated induced errors, log redaction/retention, health-secret inspection, quota threshold/alert walkthrough and maintenance message check.
- **PR/evidence:** Monitoring matrix, safe sample events, capacity/headroom and review schedule.

### M12-03 — Independent DB and photo backup/restore

- **Objective/scope:** Choose private no-cost backup destination, retention/frequency, encryption/access policy, recovery-point/time targets and consistency strategy; export DB and photo bytes plus manifest; restore into fresh isolated environment.
- **Excluded:** Public Git/CI backups, assuming a DB dump includes files, destructive production restore as a demonstration or claiming snapshot consistency without coordination.
- **Files:** `docs/runbooks/BACKUP_RESTORE.md`, bounded export/verify helpers if needed and synthetic recovery fixtures.
- **Acceptance:** Restored schema/rows, auth/admin recovery scope, safe photo bytes/paths and sample statuses/audit are verified. If Auth identities/secrets/provider settings cannot be exported identically, document tested re-provisioning and FK/actor implications. Use a submission pause/consistent cut or explicit reconciliation to align DB/object exports; destinations stay private and capacity fits free budget.
- **Verification/security:** Export→fresh restore→path/hash/sample journey, missing-file/failed-export detection and target guards; record actual recovery elapsed time and data-loss window vs targets.
- **PR/evidence:** Redacted restore report, consistency manifest, secure destination/access review and runbook. Never attach backup contents to PR/chat.

### M12-04 — Retention, cleanup and takedown operations

- **Objective/scope:** Finalize report/photo/rejected-object/log/idempotency/counter retention, public removal contact workflow, manual cleanup schedule and privacy disclosures needed by M13. Exercise M6 reconciliation with bounded batches/checkpoints.
- **Excluded:** Unbounded deletes, storing permanent raw reporter IPs, deleting audit facts to hide abuse or adding cron/queue without demonstrated need.
- **Files:** Media/abuse/moderation/privacy runbooks, bounded cleanup helpers and policy configuration.
- **Acceptance:** User-visible policy matches actual deletion/hiding behavior; referenced/recent/in-flight objects protected; takedown hides first, then deletes per policy; retryable cleanup failure has owner/observation. Audit retention/de-identification decisions are explicit.
- **Verification/security:** Isolated dry-run/apply/rerun and interrupted-cleanup tests, takedown link-expiry test and contact/ownership review without sending unauthorized messages.
- **PR/evidence:** Retention matrix, actual cleanup/takedown drill and privacy-policy input for M13.

### M12-05 — Security and supply-chain release review

- **Objective/scope:** Review unauth/non-admin/MFA boundaries, direct Data API/RPC/storage grants, media sanitation, input caps, anti-bot/limits, headers/CSRF, secret exposure, dependency findings and deploy trust.
- **Excluded:** Security certification, dismissing mandatory failures as beta limitations or noisy unrelated dependency rewrites.
- **Files:** Release security checklist/report, focused fixes and regressions; split independent fixes into reviewable issues.
- **Acceptance:** Mandatory privacy/auth/image/abuse/deploy gates pass; severe exploitable dependency findings block release until resolved/mitigated with evidence. Any accepted nonblocking risk is specific, owned and documented; no secret/PII artifacts retained.
- **Verification/security:** Existing security/API/media/auth tests, `pnpm test:db`, `pnpm test:e2e`, dependency audit and generated-client/repo/workflow inspection.
- **PR/evidence:** Threat/control/result matrix, remaining risks and accountable disposition.

### M12-06 — Incident, pause, rollback and recovery drill

- **Objective/scope:** Rehearse spam/quota outage and failed release response: pause submissions, observe propagation, restore known-good app version, verify health and data compatibility, resume only after criteria pass. Exercise leaked-key response with disposable secrets or dry-run steps.
- **Excluded:** Rotating real production credentials or destructive DB restore without applicable explicit authorization; app rollback treated as automatic schema rollback.
- **Files:** Incident/deploy/rollback/access runbooks and scoped operational fixes.
- **Acceptance:** Named maintainer can follow steps; prior app works with schema or explicit compatibility plan; no orphan/public leakage during interrupted writes; recovery evidence and known timing/limits recorded. Production remains closed after rehearsal.
- **Verification/security:** Authorized harmless deployment/rollback where applicable plus isolated failure/recovery drill; verify exact SHA, secrets, pause state and restored test journey.
- **PR/evidence:** Timeline, observed recovery values, action-owner checklist and unresolved release blockers.

### M12-07 — Release-candidate readiness review

- **Objective/scope:** Assemble immutable candidate SHA, migrations/config inventory, M11 quality, security/recovery/operations evidence and unresolved product decisions for M13.
- **Excluded:** Publishing/opening intake, claiming an external approval not received or starting beta automatically.
- **Files:** This ledger, release-readiness report/runbooks and M13 contract reconciliation.
- **Acceptance:** All M12 exit evidence verified, no mandatory gate unresolved; backup/restore and pause/rollback executable; maintainer signs readiness with clear remaining launch actions. Privacy/terms/pilot/contact completion is explicitly owned by M13 before enablement.
- **Verification/security:** Final required checks for candidate changes, evidence-link/config review and public-intake-off confirmation.
- **PR/evidence:** Readiness signoff, candidate SHA, outstanding launch decisions and concrete M13 handoff; stop here.

## 4. Exit gate

- [ ] Actual environment/secrets/free-tier/deployment/migration inventory verified.
- [ ] Safe observability, thresholds and operational owner in place.
- [ ] Independent DB+photo export/restore and consistency proof passed.
- [ ] Retention, takedown and bounded cleanup procedures tested.
- [ ] Security/dependency review and pause/rollback/incident drill passed.
- [ ] Candidate SHA/evidence/readiness signoff recorded; public intake off.

**Evidence index:** Not recorded. **Result:** Not verified complete. Readiness signoff is not release authorization.
