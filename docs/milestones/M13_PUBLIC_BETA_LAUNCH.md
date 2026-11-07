# PotholeMap — M13 Public Beta Launch

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M12](M12_PRODUCTION_READINESS.md).

## 1. Objective and boundaries

Launch an explicitly authorized limited public beta, observe real operation within free quotas and hand off a maintained service with a feedback backlog. Entry requires M12 readiness signoff, identified release candidate and user activation of launch preparation.

**In:** Pilot/capacity decision, finalized privacy/terms/attribution/contact, release checklist, reviewable deployment/enablement package, authorized rollout, bounded observation and maintenance handoff. **Out:** Silent billing, national scale by default, new feature implementation, unsolicited announcements/messages, guaranteed repair/uptime claims or declaring a launched service maintenance-free.

Planning/preparing M13 does not itself authorize public release. Collect a concrete candidate, change list, policy URLs, quota state, checks and rollback/pause procedure first. Existing explicit release authorization remains valid; if missing, request it for that concrete release before deploy/enablement. Do independent preparation while awaiting required authorization; never treat elapsed time as permission.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M13-01 | M12 exit | not_started | Pilot/moderation/observation decisions pending |
| M13-02 | M13-01, M12-04 | not_started | Final policies/contact/attribution pending |
| M13-03 | M13-01, M13-02, M12-07 | not_started | Reviewable release package pending |
| M13-04 | M13-03, explicit release authorization | not_started | Authorized deploy/intake enablement pending |
| M13-05 | M13-04 | not_started | Real beta observation pending |
| M13-06 | M13-05 | not_started | Maintenance/feedback handoff pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M13-01 — Pilot geography, capacity and observation plan

- **Objective/scope:** Decide limited launch area vs nationwide, moderation owner/schedule, expected volume, quota envelope, support/takedown owner and measurable beta observation window/stop criteria.
- **Excluded:** Assuming Bandung/Jatinangor is already selected, pretending map default center enforces geographic coverage or promising official repairs.
- **Files:** Pilot/release plan and documented scope/config changes if already authorized.
- **Acceptance:** Maintainer explicitly selects geography/capacity and observation duration; geographic restriction if promised is enforceable server-side, otherwise wording describes focus rather than restriction. Define quota/error/moderation backlog thresholds for pausing and responsibility for resuming. No fake traffic/feedback targets.
- **Verification/security:** Review actual M11/M12 capacity evidence and policy/implementation agreement; any new geographic validation requires focused tests and compatible plan update.
- **PR/evidence:** Recorded product decisions, named operational owner and release stop/go criteria.

### M13-02 — Final public disclosures and contact flow

- **Objective/scope:** Publish reviewable Indonesian privacy/terms/community guidelines, image-rights/consent/retention, attribution and abuse/takedown contact reflecting actual providers/data flows; clear beta/no-repair-guarantee messaging.
- **Excluded:** Invented legal assurance, fictitious inbox/contact, analytics data collection or sending test emails/messages without authorization.
- **Files:** Utility pages/copy, form notice/consent and policy documentation in established app/docs locations.
- **Acceptance:** No placeholder policy/contact remains; code license and user-image rights distinguished; location/photo/log disclosure and retention match M12; contact owner/method real and reachable through authorized verification. No guaranteed uptime/repair promise.
- **Verification/security:** Link/render/accessibility/copy review, policy-to-data-flow comparison and contact verification without unsolicited communication.
- **PR/evidence:** Final URLs/text, maintainer content decisions and attribution/contact checks.

### M13-03 — Concrete release package and go/no-go review

- **Objective/scope:** Assemble candidate SHA/artifact/config, migration/secret/provider state, successful exact-SHA checks, mobile/security/recovery evidence, intake enablement steps, release notes and rollback/pause plan.
- **Excluded:** Deploy/enablement before applicable authorization, relying on old green CI for a different candidate or public announcements.
- **Files:** Release checklist/notes and this ledger; scoped corrections split into focused issues if needed.
- **Acceptance:** Master launch checklist is evidence-backed; all mandatory gates pass, pilot/contact/moderation capacity ready, free-tier terms/billing rechecked. Candidate changes after review invalidate affected checks/authorization scope and must be reconciled. Record go/no-go; obtain any missing authorization for the exact proposed external actions.
- **Verification/security:** Link/check/SHA/resource review, required suites for candidate changes and secret/intake-state inspection.
- **PR/evidence:** Reviewable release package, unresolved blocker list (empty for go) and actual authorization record; no release claimed yet.

### M13-04 — Authorized deployment and controlled intake enablement

- **Objective/scope:** Execute only authorized migration/config/deployment steps in the reviewed order, verify health/build SHA, enable limited public intake and perform minimal consented smoke within actual quotas.
- **Excluded:** Unapproved destructive migrations, exporting reporter data, silent paid upgrades, invitations/announcements or fabricating public activity.
- **Files:** Release evidence/runbook and authorized configuration changes; product fixes remain separate scoped issues.
- **Acceptance:** Actual URL/version and enablement time recorded; anonymous valid test report remains pending until authorized admin approval; approved appears to separate visitor, rejected stays private; real-world smoke uses controlled non-identifying data with documented cleanup/consent. If any mandatory failure occurs, pause and follow rollback criteria.
- **Verification/security:** Actual health/SHA/mobile/core/private-access smoke, quota/config review and production preview separation. No destructive adversarial load on live service.
- **PR/evidence:** Real deploy/run IDs, smoke results, intake/billing state and go/no-go outcome. External actions only within existing authorization.

### M13-05 — Observe the agreed beta window

- **Objective/scope:** Observe real errors, upload failures, request/storage/tile quota usage, moderation backlog, takedown/contact readiness and user feedback for the window agreed in M13-01.
- **Excluded:** Invented feedback, scraping identities, creating an unsolicited automation or calling a minutes-old deploy a completed multi-day observation.
- **Files:** Dated minimal beta observation log, incident records and scoped defect tickets.
- **Acceptance:** Actual observation dates/metrics and limitations recorded; thresholds trigger pause/response when met; no critical unresolved incident. If the observation window is still running, keep this issue in progress and hand off honestly; completion requires the elapsed real window, not generated reports. Schedule automation only on explicit user request.
- **Verification/security:** Provider dashboards/safe logs and maintainer review at agreed cadence; retain only necessary nonpersonal evidence and avoid secret/signed-link captures.
- **PR/evidence:** Real observation summary, incidents/resolutions, moderation/quota values and resume/continue decision.

### M13-06 — Beta exit, maintenance and next backlog

- **Objective/scope:** Review actual beta outcomes, close launch blockers, assign recurring quota/moderation/backup/access-review ownership and convert observed feedback into a prioritized future backlog.
- **Excluded:** Starting deferred AI/social/notification features, declaring permanent zero cost/security or automatically archiving the service/thread.
- **Files:** Beta outcome/maintenance handoff, this ledger and separately scoped future backlog if warranted.
- **Acceptance:** M13-01–M13-05 evidence complete, service state/limitations and active incidents clear; maintainer accepts ongoing operating responsibilities. Success criteria from master are demonstrated; unmet criteria remain explicit blockers. Future work has no implied implementation authorization.
- **Verification/security:** Outcome-to-criteria review, operational schedule/owner confirmation and final no-secret/public-link check; do not rerun invasive live tests just to close paperwork.
- **PR/evidence:** Maintainer exit signoff, actual public-beta URL/version, observation summary and proposed next backlog; stop at this handoff.

## 4. Exit gate

- [ ] Pilot, moderation capacity, observation window and stop criteria decided.
- [ ] Final policies/attribution/contact/consent match actual service.
- [ ] Reviewed candidate/checks/backup/rollback and explicit release authorization recorded.
- [ ] Actual authorized live journey/private-access/health smoke passed.
- [ ] Agreed real observation window complete; quotas/incidents reviewed.
- [ ] Maintainer accepts maintenance handoff and records beta exit signoff.

**Evidence index:** Not recorded. **Result:** Not verified complete. No public URL/deployment/feedback is claimed by this plan.
