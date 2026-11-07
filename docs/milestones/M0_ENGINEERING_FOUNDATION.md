# PotholeMap — M0 Engineering Foundation

**Version:** 1.5 · **Updated:** 2026-11-07 · **Status:** M0-01 governance prepared for review; milestone not verified complete.

**Parent:** [Project Master Plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Next:** [M1](M1_APPLICATION_SHELL.md). Product scope, backend architecture and the M0–M13 roadmap belong in the master; this document owns **only M0 execution**. New M1–M13 drafts do not change this ledger's evidence or activate implementation.

## 1. Milestone boundary and execution control

**Objective:** Establish a reproducible, reviewable, safely deployable application skeleton before building product features.

**In scope:** Governance, agent contract, hosting/runtime spike, bootstrap, quality tools, tests, CI, protected main, environment isolation, isolated preview, gated harmless production deployment, rollback and setup/deployment evidence.

**Out of scope:** Actual map, report APIs/schema, real uploads, image pipeline implementation, Turnstile/limiter implementation, admin login, moderation, message broker, job system, real user data, public beta and paid services. Architectural feasibility assessment does not authorize implementing those features.

### Agent handoff rules

1. Read applicable `AGENTS.md`, the master plan and this contract.
2. Select only the user-authorized issue; check its dependencies and existing evidence. If asked to start M0 without an issue, select the first ready issue and announce it.
3. State objective, exclusions and verification before editing implementation files. Do not batch M0 into one PR.
4. Deliver one issue with a focused diff, verification evidence and a handoff. Continue to further issues only if the user authorized continued issue-by-issue execution.
5. Mark blocked checks honestly; do useful authorized local preparation before any required external approval. Do not invent GitHub runs, URLs or settings.
6. Track evidence below. M0 exits only after all acceptance criteria pass and the maintainer signs off. It does not automatically activate M1.
7. This planning revision selects no implementation issue and authorizes no infrastructure changes. Existing explicit user authorization takes precedence over generic approval wording.

### Status and evidence ledger

Allowed issue states: `not_started`, `in_progress`, `blocked`, `in_review`, `done`. Initial states below describe planning only; `done` requires acceptance/verification evidence. A ready dependency does not imply an active task.

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M0-01 | None | in_review | Public repo and [issue #1](https://github.com/LaurensiusDani/PotholesMap/issues/1); MIT selected, governance files/local checks prepared; see repository runbook; PR/default-branch template review pending |
| M0-02 | M0-01 (default; limited exception below) | in_review | Root `AGENTS.md` prepared at user's explicit request; local documentation checks recorded below; governance and remote PR evidence remain outstanding |
| M0-03 | M0-01, M0-02 | not_started | Await runtime spike |
| M0-04 | M0-03 core-hosting go decision | not_started | Await approved spike outcome |
| M0-05 | M0-04 | not_started | Await bootstrap |
| M0-06 | M0-04 | not_started | Await bootstrap |
| M0-07 | M0-05, M0-06 | not_started | Await local quality/test checks |
| M0-08 | M0-07 | not_started | Await actual CI check name |
| M0-09 | M0-04 | not_started | Await runtime/environment shape |
| M0-10 | M0-07, M0-09 | not_started | Await isolated preview setup |
| M0-11 | M0-08, M0-09, M0-10 | not_started | Await gates and preview evidence |
| M0-12 | M0-01–M0-11 | not_started | Await final audit |

**Selected issue:** M0-01 — repository governance, authorized on 2026-11-07. M0-02's earlier artifact remains in review; no later issue is started. **M0 exit signoff:** Not recorded. **Next milestone:** M1 remains blocked.

**User-directed sequencing exception — 2026-11-04:** The maintainer explicitly requested `AGENTS.md` setup before M0-01 governance. Prepare this independent documentation artifact now; retain M0-01 as incomplete and the default dependency for normal execution. This does not authorize repository initialization, remote setup, runtime work or any later issue.

## 2. Shared engineering contracts

### Baseline and decision gates

- Public GitHub; protected main; short-lived issue branches; Conventional Commits; squash merges.
- React + Vite + strict TypeScript; Workers Free assets/API; pnpm with pinned Node/pnpm; GitHub Actions + Wrangler. M0-03 proves integration before M0-04 adopts it.
- Supabase Free/PostGIS/Storage/Auth are later application dependencies. M0 may document environments and use mocked external calls; no production schema or real data required.
- Verify current quota/account terms in M0-03 and record dated source links and measurements. Do not hardcode a vendor quota as a permanent guarantee.
- Assess safe server-side image processing under the actual CPU/memory/package limits. Record one of: feasible with evidence, infeasible with alternatives, or unresolved with an explicit M6 blocker. Core hosting may proceed only if the unresolved image limitation is documented and public intake/publication stays disabled. Do not solve it by adding an unapproved paid runtime or queue.
- No microservices, Redis, broker, job table or separate backend server. The master plan owns backend modules and reconsideration criteria.

### Branches and checks

Examples: `chore/m0-01-repository-governance`, `spike/m0-03-cloudflare-runtime`, `ci/m0-07-github-actions`.

Each PR meets its issue criteria, stays within scope, includes relevant verification, updates docs/env examples as needed, exposes no secrets, and receives manual self-review. A solo public-repo workflow may use zero required approving reviews while enforcing PR/check requirements; confirm actual account support. No direct push/force push/delete on protected main except separately authorized documented emergency work.

### CI contract

PR/main workflow uses minimal permissions (`contents: read`), SHA-pinned third-party actions, safe lockfile cache and cancellation/concurrency controls. Required logical check: `CI / verify`; M0-08 must record and protect the actual emitted check context.

```text
pnpm install --frozen-lockfile
pnpm format:check
pnpm lint
pnpm typecheck
pnpm test
pnpm build
```

Dependency audit is initially advisory with an explicit policy for triaging/blocking severe exploitable findings before release. Never give untrusted PR code deployment credentials. Demonstrate failing checks safely; final committed tests/checks must pass.

### Environments and CD

| Environment | Data | Credentials / exposure |
|---|---|---|
| Local | Fixtures in M0; local Supabase later | Ignored `.env.local`; no production credentials |
| Preview | Fixtures or isolated dev resources | Preview-only secrets; no public feature intake |
| Production skeleton | No user data or product schema | Scoped production secrets; health/skeleton only |

Public `VITE_*` values are nonsecret. Backend secrets stay in runtime bindings, out of source/client bundles/logs. Missing required variables fail safely; do not invent unused Supabase credentials just to fill an example file.

Preview must be maintainer-controlled when untrusted PR deployment cannot be secured. Use unique names with cleanup or a serialized shared preview. Production is main-only after successful CI for the **exact deployed SHA**. Check real environment protection availability; document enforceable alternatives if unavailable rather than pretending manual GitHub approval exists. Use least-privilege Cloudflare credentials, record deployed version/SHA, smoke `/health`, and demonstrate rollback. Generic app deploy does not apply DB migrations.

Merging, actual deployments, provider/account settings and production secrets follow the user's explicit authorization. Complete local code/config/review preparation first when external action requires approval; do not halt reversible authorized work unnecessarily.

### Target layout

Create files/directories when needed, not empty placeholders. M0-03 may refine runtime configuration filenames with ADR rationale.

```text
.github/
  ISSUE_TEMPLATE/
  workflows/ci.yml
  workflows/deploy-preview.yml
  workflows/deploy-production.yml
  pull_request_template.md
  dependabot.yml
docs/
  PROJECT_MASTER_PLAN.md
  milestones/M0_ENGINEERING_FOUNDATION.md
  diagrams/
  adr/ADR-001-runtime-and-hosting.md
  runbooks/SETUP.md
  runbooks/DEPLOYMENT.md
  runbooks/ROLLBACK.md
src/
  app/
  server/index.ts                # health/runtime skeleton only
  components/                    # only if needed by skeleton
public/                          # only when assets exist
tests/
AGENTS.md
README.md
CONTRIBUTING.md
SECURITY.md
LICENSE                          # after maintainer chooses
.env.example
.gitignore
.editorconfig
package.json
pnpm-lock.yaml
tsconfig.json
vite.config.ts
wrangler.jsonc
```

## 3. Issue contracts

Each implementation issue/PR expands the following bounded contract with actual files, commands and evidence. Shared requirements in section 2 apply to every issue.

### M0-01 — Repository governance

- **Dependencies:** None.
- **Objective/scope:** Prepare public repository governance: README with MVP/budget/state, CONTRIBUTING, SECURITY disclosure channel, license selection, issue/PR templates, ignore/editor rules. Document target repo settings.
- **Excluded:** Framework code, feature implementation, publishing credentials or choosing a license without maintainer input.
- **Files:** Root governance documents and `.github/` templates.
- **Acceptance:** Templates render; README distinguishes plans from implementation; source license is explicitly selected or recorded as an unresolved gate; no sensitive files tracked. Issue remains incomplete until license selection is resolved.
- **Verification/security:** Review rendered Markdown/templates and intended ignore behavior. Inspect staged files for secrets. Check real repo visibility/settings when authorized.
- **PR/evidence:** Focused governance diff, maintainer license decision, settings evidence and outstanding setup steps.

### M0-02 — AI agent contract

- **Dependencies:** M0-01.
- **Objective/scope:** Create/refine root `AGENTS.md` with architecture boundaries, one-issue handoff, dependency checks, commands, secrets/review conventions and production-operation limits. Preserve applicable existing instructions.
- **Excluded:** App code and expanded roadmap scope.
- **Files:** `AGENTS.md`; links in CONTRIBUTING if useful.
- **Acceptance:** Agent can identify selected milestone/issue, exclusions, checks and stop point; examples cannot be read as permission to build all milestones. Instructions link to the two canonical plans instead of duplicating them.
- **Verification/security:** Document review with a sample M0 issue handoff; check contradictions with user-level instructions.
- **PR/evidence:** Agent contract and sample scoped handoff.

### M0-03 — Cloudflare runtime and hosting spike

- **Dependencies:** M0-01, M0-02.
- **Objective/scope:** Disposable minimal React/Vite page + Worker `/health`; verify local dev/build, static/deep-link/API routing, mocked outbound fetch, secret bindings, free deployment and preview/rollback feasibility. Measure runtime/package constraints and image-processing feasibility.
- **Excluded:** Product APIs/schema, production secrets/data, building media pipeline, adding broker/services as a shortcut.
- **Files:** Minimal spike files and `docs/adr/ADR-001-runtime-and-hosting.md`; record what is retained or superseded by M0-04.
- **Acceptance:** Local and authorized disposable-deploy evidence; actual commands; dated quota/terms references; core-hosting go/no-go; separate image assessment and M6 blocker if unresolved. If no-go, document concrete alternatives without silently switching stack.
- **Verification/security:** Build/dev, `/health`, deep-link and asset routing, mocked API failure, secret exclusion and runtime measurements. No benchmark claims from desktop-only tests.
- **PR/evidence:** ADR, temporary URL/deployment ID when authorized, measurements and maintainer decision. M0-04 waits for a core-hosting go decision.

### M0-04 — Application bootstrap

- **Dependencies:** M0-03 core-hosting go decision.
- **Objective/scope:** Reproducible React/Vite/strict TypeScript skeleton, server `/health`, Node/pnpm pins, lockfile and dev/build scripts. Integrate or replace spike files deliberately.
- **Excluded:** Map/report/admin modules, empty feature folders, unused backend integrations.
- **Files:** Minimal `src/`, package/lock/runtime/TS/Vite config, README setup.
- **Acceptance:** Fresh clone installs and dev/build work; lockfile is deterministic; health response contains no sensitive runtime details.
- **Verification/security:** `pnpm install --frozen-lockfile`, `pnpm dev`, `pnpm build`, local health and SPA/API routes; inspect generated bundle.
- **PR/evidence:** Bootstrap diff, pinned versions and fresh-setup command results.

### M0-05 — Code quality

- **Dependencies:** M0-04.
- **Objective/scope:** ESLint, Prettier, strict TypeScript, scripts and formatting policy.
- **Excluded:** Product code/refactors or extra quality services.
- **Files:** Quality/TS configs, package scripts and relevant docs.
- **Acceptance:** Lint/format/typecheck pass and a temporary deliberate violation is detected; no weakened checks to obtain green status.
- **Verification/security:** `pnpm lint`, `pnpm typecheck`, `pnpm format:check`; remove temporary violation before delivery.
- **PR/evidence:** Config diff and successful/negative-check results.

### M0-06 — Test foundation

- **Dependencies:** M0-04.
- **Objective/scope:** Vitest unit harness and meaningful skeleton/health smoke tests; finite non-watch CI command.
- **Excluded:** Heavy E2E infrastructure or tests for unbuilt product features.
- **Files:** Vitest config, package scripts and skeleton tests.
- **Acceptance:** A meaningful positive case passes; deliberate regression fails; final suite is green and exits reliably.
- **Verification/security:** `pnpm test`; demonstrate failure without leaving a broken test committed. Use fixtures, no production network/data.
- **PR/evidence:** Harness/tests and positive/negative results.

### M0-07 — GitHub Actions CI

- **Dependencies:** M0-05, M0-06.
- **Objective/scope:** PR/main quality pipeline, frozen install, safe caching, least privilege, pinned actions and concurrency behavior.
- **Excluded:** Deployment credentials, production jobs or product tests.
- **Files:** `.github/workflows/ci.yml`, Dependabot config, CI docs.
- **Acceptance:** Format/lint/types/tests/build gate; controlled failing PR produces failure; no secrets available to untrusted code; actual check name recorded.
- **Verification/security:** Local shared CI commands plus real authorized workflow/PR evidence; review token permissions and event triggers.
- **PR/evidence:** Workflow diff, green/failure run links and actual status context.

### M0-08 — Main protection

- **Dependencies:** M0-07.
- **Objective/scope:** Public-repo ruleset requiring PR, actual CI context, linear history and no force push/deletion. Solo workflow stays operable with self-review.
- **Excluded:** Assuming unavailable plan features or silently bypassing main protection.
- **Files:** Repo settings evidence/runbook, normally no app code.
- **Acceptance:** Controlled verification of direct-push blocking and failing-check merge blocking; document admin bypass policy/account limitations honestly.
- **Verification/security:** Review ruleset and safe test branch/PR. Do not damage main to demonstrate protection.
- **PR/evidence:** Redacted settings, check context and controlled test results.

### M0-09 — Environment and secrets isolation

- **Dependencies:** M0-04.
- **Objective/scope:** Actual env matrix, `.env.example`, runtime validation, ignored local secrets, preview/production separation and scoped bindings.
- **Excluded:** Live product schema, public write policies, unnecessary provider secrets.
- **Files:** Env example/validation, ignore/runtime config, environment documentation.
- **Acceptance:** Missing required server values fail safely; no credential in source/client bundle/logs; preview cannot use production credentials; fixtures sufficient for M0.
- **Verification/security:** Build inspection, secret scan, negative missing-variable case and environment matrix review.
- **PR/evidence:** Redacted config and check results; never secret values.

### M0-10 — Isolated preview deployment

- **Dependencies:** M0-07, M0-09.
- **Objective/scope:** Safe Wrangler preview workflow, naming/concurrency, cleanup and `/health` verification using fixtures.
- **Excluded:** Fork-secret exposure, production DB connections and feature intake.
- **Files:** Preview workflow/runtime config and deployment runbook.
- **Acceptance:** Authorized preview URL works, expected SHA is identifiable, nonproduction resources proven and cleanup documented. Untrusted PR code cannot access deployment credentials.
- **Verification/security:** Maintainer-controlled preview smoke; review checkout/event trust and secret scopes; exercise cleanup safely.
- **PR/evidence:** Workflow, preview/run/version evidence and trust model.

### M0-11 — Gated production skeleton and rollback

- **Dependencies:** M0-08, M0-09, M0-10.
- **Objective/scope:** Main-only production skeleton deployment gated by successful exact-SHA CI; least-privilege token, deployment record, health smoke and rollback drill.
- **Excluded:** Public beta, user data, automatic schema migrations and broad account tokens.
- **Files:** Production workflow/runtime config, deployment/rollback runbooks.
- **Acceptance:** Failed/non-main/unverified commits cannot deploy; known-good SHA/version recorded; harmless production skeleton works; rollback returns a known-good version with health verified. Use actual enforceable protection features.
- **Verification/security:** Authorized harmless release/rollback drill; inspect branch/SHA gating, token scope, concurrency and secret isolation.
- **PR/evidence:** Successful and blocked-path evidence, deployed SHA/version and rollback result. Local config alone does not satisfy this issue.

### M0-12 — Documentation and exit audit

- **Dependencies:** All prior issues complete with evidence.
- **Objective/scope:** Consolidate setup/deploy/rollback runbooks, ADR and diagram links; verify fresh clone, CI, preview, harmless production, recovery and quotas. Update ledger and request maintainer signoff on concrete evidence.
- **Excluded:** Starting M1, inventing implementation evidence or implementing backend features during audit.
- **Files:** Runbooks, ADR refinements, README and this contract's evidence/signoff sections.
- **Acceptance:** Fresh-clone verification passed; all M0 evidence linked; security review complete; unresolved image feasibility explicitly remains a later blocking gate, never a false success claim.
- **Verification/security:** Shared CI commands, authorized health checks and previous rollback evidence; no real user data, secret/backup leaks or silent paid services.
- **PR/evidence:** Exit report, outstanding risks, maintainer signoff and proposed next issue. Revalidate the drafted M1 contract against actual M0 decisions; M1 execution still requires the user's instruction.

## 4. Reusable issue and handoff templates

Use this structure for GitHub issues and future milestone contracts; listing future milestones here does not activate them.

```text
Title: [MX-YY] ...
Parent milestone / selected issue:
Objective:
Dependencies and evidence:
In scope:
Out of scope:
Files likely affected:
Implementation constraints:
Acceptance criteria:
Verification commands / manual checks:
Security and failure cases:
PR deliverables:
Stop point:
```

```text
Issue handoff:
- Completed behavior and changed files:
- Acceptance evidence / PR / CI links:
- Checks run and actual results:
- Blocked or unverified checks:
- Remaining risks / decisions:
- Ledger status and proposed next issue (not automatically started):
```

## 5. M0 exit checklist and signoff

- [ ] Public repo, license decision and governance/templates ready.
- [ ] Agent contract clearly enforces selected-issue scope and stop point.
- [ ] ADR-001 records hosting go decision, measurements and image feasibility/blockers.
- [ ] Fresh-clone setup reproducible with pinned Node/pnpm and lockfile.
- [ ] Formatting, lint, strict types, unit/smoke tests and build pass.
- [ ] Actual required CI context protects main; negative gate verified.
- [ ] Local/preview/production isolation and secret checks pass.
- [ ] Preview health/cleanup and untrusted-code isolation verified.
- [ ] Harmless production deploy gated on exact successful SHA.
- [ ] Known-good rollback and health evidence recorded.
- [ ] No real user data, product feature exposure or automatic DB migrations.
- [ ] Runbooks and PR/run/deployment evidence complete; free-tier/account constraints recorded.
- [ ] Maintainer reviews evidence and signs off; M1 remains inactive until instructed.

**Evidence index:**

- **M0-01 governance (2026-11-07):** Public [LaurensiusDani/PotholesMap](https://github.com/LaurensiusDani/PotholesMap), [issue #1](https://github.com/LaurensiusDani/PotholesMap/issues/1), explicit maintainer MIT selection, README/CONTRIBUTING/SECURITY/LICENSE, issue/PR templates and ignore/editor/line-ending rules. Planning-only seed commit `38459dd` establishes the empty repository's `main` base; topic branch `chore/m0-01-repository-governance` owns governance changes. Actual API checks confirm public visibility, main, issues enabled and private vulnerability reporting enabled. Local checks cover links/fences/newlines/frontmatter, ignore samples, bounded credential-pattern scan and author identity. Author/committer is LaurensiusDani with GitHub noreply email and no Codex co-author. See [repository runbook](../runbooks/REPOSITORY_GOVERNANCE.md) for actual PR/settings evidence and deferred checks. No app tooling/CI/cloud deployment is claimed. Keep `in_review` until maintainer/default-branch template verification; M0 is not complete.

- **M0-02 local artifact (2026-11-04):** [Root agent contract](../../AGENTS.md) defines instruction sources, selected-issue scope, dependency exceptions, stop points, architecture/security rules, quality commands, Git/production boundaries and a sample bounded handoff. Documentation verification checks local plan links, balanced code fences, all 12 issue-ledger rows and agreement with the two canonical plans. Application tests are not applicable to this documentation change; app tooling has not been created. No Git repository, PR, remote CI or infrastructure evidence is claimed. Status remains `in_review` pending maintainer review and M0-01 governance reconciliation; M0 is not complete.

**Maintainer / signoff date:** Not recorded. **M0 result:** Not verified complete.

## 6. References to verify during implementation

Architecture references and reviewed backend constraints are in the [master plan](../PROJECT_MASTER_PLAN.md#13-official-technical-references). The links below are implementation checkpoints, not claims that current account settings were inspected:

- [Workers limits](https://developers.cloudflare.com/workers/platform/limits/)
- [Supabase pricing](https://supabase.com/pricing)
- [GitHub repository rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets)
- [GitHub deployment environments](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments)
