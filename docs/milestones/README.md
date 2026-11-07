# PotholeMap — Milestone Execution Index

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Full M0–M13 planning set; not an implementation authorization.

Read [AGENTS.md](../../AGENTS.md), the [master plan](../PROJECT_MASTER_PLAN.md), this shared execution contract and **the selected milestone file**. Do not load/implement every milestone as one task. The master owns product/architecture; each milestone owns its issues, ledger and evidence. This index owns navigation and common execution rules, not a duplicate status ledger.

## 1. Milestone files and default order

| Milestone | Contract | Deliverable |
|---|---|---|
| M0 | [Engineering Foundation](M0_ENGINEERING_FOUNDATION.md) | Governance, runtime proof, bootstrap, CI/CD and rollback |
| M1 | [Application Shell](M1_APPLICATION_SHELL.md) | Responsive accessible shell and navigation |
| M2 | [Database Foundation](M2_DATABASE_FOUNDATION.md) | Local schema, PostGIS, grants/RLS and reproducible fixtures |
| M3 | [Map Foundation](M3_MAP_FOUNDATION.md) | Compliant map tiles, GPS/manual pin and map lifecycle |
| M4 | [Public Report API](M4_PUBLIC_REPORT_API.md) | Bounded approved-only read API |
| M5 | [Report Explorer](M5_REPORT_EXPLORER.md) | API-backed map/list/filter/detail experience |
| M6 | [Safe Photo Pipeline](M6_SAFE_PHOTO_PIPELINE.md) | Proven sanitization, private storage and reconciliation |
| M7 | [Report Submission](M7_REPORT_SUBMISSION.md) | Isolated anonymous creation, receipts and safe retries |
| M8 | [Abuse Prevention](M8_ABUSE_PREVENTION.md) | Shared rate limits, pause control and abuse regression tests |
| M9 | [Admin Authentication](M9_ADMIN_AUTHENTICATION.md) | Verified admin identity, MFA and authorization |
| M10 | [Moderation Workflow](M10_MODERATION_WORKFLOW.md) | Atomic audit/decisions, controlled photos and status changes |
| M11 | [Quality & Accessibility](M11_QUALITY_ACCESSIBILITY.md) | Full-journey, real-device, accessibility and performance QA |
| M12 | [Production Readiness](M12_PRODUCTION_READINESS.md) | Recovery, observability, security and release evidence |
| M13 | [Public Beta Launch](M13_PUBLIC_BETA_LAUNCH.md) | Authorized limited launch, observation and feedback handoff |

**Default sequence:** M0 → M1 → … → M13. Each milestone requires the preceding milestone's exit signoff unless the user explicitly authorizes a recorded sequencing exception. Minimum technical dependencies inside a contract do not waive this order. All M1–M13 files are **draft execution baselines, blocked for implementation** until activated. M0 status remains in its existing ledger; drafting this set completes no implementation issue.

The project's planned endpoint is an operated public beta and a reviewed follow-up backlog, not a claim that a live service needs no further maintenance. Post-v1 features remain deferred rather than being silently added to this roadmap.

## 2. Activation and issue execution

1. Confirm the user's authorized milestone/issue and inspect actual files, ledger and prerequisite evidence. A request to prepare plans does not authorize Git setup, package installs, cloud setup, deployment or feature work.
2. Before activating a drafted milestone, reconcile it with prior ADRs, current package versions/provider terms and actual implementation. Record changed assumptions in the owning contract/master as appropriate. These files are detailed plans, not predictions that every early technology assumption will hold.
3. Resolve the milestone's blocking decisions. Distinguish user/product choices from routine implementation choices the agent can make within the approved contract. Continue independent authorized work while a necessary answer is pending; never treat silence as approval.
4. Select one ready issue. Announce objective, exclusions and verification. Use one focused branch/PR when Git is available; record local-only evidence honestly otherwise.
5. Implement only that issue, run its targeted checks plus required existing quality gates, review the diff, update its ledger/evidence and hand off. Stop unless continued issue-by-issue work was explicitly authorized.
6. New blockers cause a plan/dependency update, not an unbounded expansion into later features. Split an oversized issue before implementation and update dependency IDs/ledger together.
7. Completing all issue acceptance criteria permits exit review. Only actual evidence and maintainer signoff close the milestone. Do not automatically start its successor or publish the app.

### States and evidence

Issue states: `not_started`, `in_progress`, `blocked`, `in_review`, `done`. New issues start `not_started` even though milestone activation is blocked. Every new file starts with **Selected issue: None; exit signoff: Not recorded**. Historical M0-02 progress is preserved in M0.

For each issue record changed files/behavior, actual commands/results, PR/run/deployment links when real, negative/security evidence, limitations and proposed next issue. `in_review` means a concrete artifact is ready for review; `done` requires its acceptance criteria and required evidence. Never fabricate run URLs, benchmarks, security results or provider settings.

## 3. Shared verification and operational boundaries

After M0 creates the scripts, use the frozen lockfile for reproducible installs and run required quality checks:

```text
pnpm install --frozen-lockfile
pnpm format:check
pnpm lint
pnpm typecheck
pnpm test
pnpm build
```

Run the install when setup/dependencies require it, not for every docs edit. `pnpm test` is finite/non-watch. Targeted tests use the actual configured runner; record exact invocations at implementation. Additional commands below are **planned interfaces introduced by their owning issues**, not existing scripts:

| Command | Introduced by | Contract |
|---|---|---|
| `pnpm db:start` | M2-01 | Start the documented isolated local database stack |
| `pnpm db:reset` | M2-01 | Reset only an explicitly verified local development target |
| `pnpm test:db` | M2-05 | Integration/security checks against isolated DB fixtures |
| `pnpm test:e2e` | M11-01 | Finite browser journey suite against isolated resources |

If local database/browser infrastructure is unavailable, record the blocked checks. Do not test against production or weaken assertions to get green. Documentation-only changes require local link/structure/consistency checks; do not bootstrap an app to test Markdown.

### Permanent boundaries

- Rp0 start; no automatic billing/paid service without applicable explicit authorization. Provider limits are reverified at implementation gates, not copied as permanent guarantees.
- Single Workers API modular monolith, Supabase/PostGIS/private media and admin-only Auth. No broker/Redis/microservices by default. A runtime failure needs an ADR and revised plan, not a silent stack substitution.
- Local/preview/production are separate. No real reporter data in fixtures, PR artifacts or public CI. Production schema operations and deploys follow existing user authorization and the exact-SHA CI gate.
- **Public intake is off throughout M0–M12.** Isolated test submissions are allowed within the owning issues. M13 alone may enable public intake after release authorization. No raw images or pending/rejected rows become public for demonstration.
- **Public media gate:** Approved report + safe processed media + authorized short-lived access. M4/M5 use safe placeholders until M10 integrates publication.
- **Security starts with the feature:** M4 bounds reads, M6 enforces file safety, M7 verifies bot tokens and conservative shared write limits. M8 integrates/hardens and measures these controls; it is not permission to expose an unprotected earlier endpoint.
- Do not use ordinary background promises as durable work. No queue is introduced by the existence of a moderation queue or cleanup runbook.

## 4. Handoff and plan maintenance

Each contract lists issue dependencies, objective/scope, exclusions, likely files, acceptance, verification/security and PR evidence. File paths are intended ownership, not a requirement to create empty folders. Use the master for backend details and decisions; use a milestone for concrete implementation acceptance.

Before closing an issue, identify any downstream contract affected by the final implementation and update references without activating it. Keep API fields/state names, test interfaces and ADR references consistent. M13 carries unresolved non-MVP ideas into a separate future backlog only after beta feedback; it does not authorize building them.

**Planning verification:** Link integrity, one M0–M13 contract each, unique issue IDs, ledger/contract agreement, dependency existence/order and absence of stale “contract not drafted” claims. No application behavior is claimed by passing those documentation checks.
