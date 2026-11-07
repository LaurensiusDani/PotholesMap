# Contributing to PotholeMap

PotholeMap is under development. Start with the [master plan](docs/PROJECT_MASTER_PLAN.md), [execution index](docs/milestones/README.md), selected milestone contract and [agent instructions](AGENTS.md). Do not implement the roadmap as one change.

## Choose one scoped issue

- Confirm the issue objective, prerequisite evidence, acceptance criteria and exclusions before implementation.
- Use the milestone-task issue template for planned work and the bug template for reproducible defects. A proposal is not permission to implement it.
- Public issues must not contain secrets, private images, reporter identities, database exports or security exploit details. Use [private security reporting](SECURITY.md) for vulnerabilities.
- Unplanned dependencies require an update to the owning contract, not an unrelated refactor or extra infrastructure.

## Branch and PR workflow

1. Start from the latest `main` when the Git repository is established; inspect and preserve existing local changes.
2. Create one short-lived branch, e.g. `chore/m0-01-repository-governance`, `spike/m0-03-cloudflare-runtime` or `feat/m4-03-report-list`.
3. Keep commits focused and use Conventional Commits, e.g. `docs(governance): define contribution workflow` or `fix(reports): reject invalid bounds`.
4. Open a focused PR, link its issue and use `Closes #N` only when merge should close that issue. Explain the problem/result, checks actually run and remaining limitations.
5. Review the complete diff for scope, private data, authorization, failure handling, migration safety and documentation changes. UI changes include recorded browser/viewport verification when applicable.
6. Required available checks must pass. The maintainer performs self-review for solo work; request peer review when available. Merge and deployment need the user's applicable authorization.
7. Squash merge, then remove the merged topic branch when authorized. Never force push/delete `main` or overwrite another contributor's work.

The empty repository needs one documented planning-only seed commit to establish a PR base. This bootstrap exception is recorded in the [governance runbook](docs/runbooks/REPOSITORY_GOVERNANCE.md); it does not permit later direct feature pushes to `main`.

## Author identity

Use your own legitimate Git author name and an email associated with your GitHub account; a GitHub-provided noreply address can keep personal email private. For this maintainer's agent work, preserve **LaurensiusDani** as author/committer. Do not add Codex/AI co-author trailers. Never impersonate another contributor or rewrite existing authorship.

## Verification

At M0-01 there is no application/package manifest, CI or test runner. Verify documentation links, Markdown/template structure, ignore behavior, whitespace and the scoped/staged file list. A tool being planned does not mean it exists or passed.

After M0 introduces the scripts, use the pinned Node/pnpm versions and frozen install when setup or dependencies require it:

```text
pnpm install --frozen-lockfile
pnpm format:check
pnpm lint
pnpm typecheck
pnpm test
pnpm build
```

Run issue-specific DB/API/media/browser/security checks as their contracts require and their tooling becomes available. Documentation-only edits do not require building an app or adding mirror-image tests. Record commands, actual results and checks blocked by unavailable infrastructure; never weaken gates for a green result.

## Data, dependencies and infrastructure

- Keep local, preview and production resources separate; use synthetic isolated fixtures.
- `.env.example` contains names/placeholders only. No service-role tokens, real secrets, user uploads or backups in Git or PR/CI artifacts.
- No new paid/card-required/auto-billed dependencies without explicit approval. Dependencies needed by the selected contract are within scope; unrelated frameworks/services are not.
- SQL migrations are reviewed separately from generic app deployment. No production migration, secret change or deployment is implied by contributing code.
- Public intake stays disabled until the authorized M13 release gates pass.

## Handoff and completion

Update the selected milestone ledger with files/behavior, actual verification, real PR/run links, blockers and the proposed next issue. A prepared PR may be `in_review`; `done` requires acceptance and evidence. Stop at the issue handoff unless continued issue-by-issue work is explicitly authorized. Milestone exit and next-milestone activation are separate decisions.

Source contributions are submitted under the [MIT License](LICENSE). Third-party assets must have appropriate rights; submitted photos and project brand assets need their own applicable policy.
