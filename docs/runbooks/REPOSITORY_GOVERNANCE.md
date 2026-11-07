# Repository Governance — M0-01

**Updated:** 2026-11-07 · **Owner:** LaurensiusDani · **Scope:** Governance only; no app/CI/deployment implementation.

## Decisions and intended repository

- Owner/name: `LaurensiusDani/PotholesMap`, matching the project directory; public visibility is the master-plan baseline.
- Product display name: **PotholeMap**. Repository directory/name spelling does not rename the product.
- License: **MIT**, explicitly selected by maintainer on 2026-11-07; copyright holder LaurensiusDani. User photos and brand rights require separate applicable policies before beta.
- Agent-authored changes preserve LaurensiusDani as Git author/committer; use the authenticated account's GitHub noreply identity to avoid publishing personal email. No Codex/AI co-author trailer.
- Security disclosure: GitHub private vulnerability reporting, with the actual enabled state verified before claiming setup complete.

## Empty-repository bootstrap exception

There is no existing Git history. Establish a planning-only seed commit on `main` from the pre-existing `AGENTS.md` and `docs/` baseline, then create the M0-01 topic branch for governance files and ledger changes. Push the seed only to establish the empty remote's base branch; open the governance PR against it. This is the documented initialization exception to the normal PR-only workflow, not permission for subsequent direct feature pushes.

Do not rewrite existing history if remote inspection finds an existing repository. Review the actual account/name and current remote before creation. Creating the public repository is within the maintainer's instruction to start M0-01; merging the governance PR and later protected-branch/deployment operations remain separate authorized actions.

## Settings inventory

| Setting | M0-01 target | Verification state |
|---|---|---|
| Owner/name | LaurensiusDani/PotholesMap | Actual account ID/login and repository verified through GitHub API |
| Visibility | Public | Actual `private: false` verified |
| Default branch | main | Seed `38459dd` pushed; actual `default_branch: main` verified |
| Issues and templates | Issues on; milestone task + bug; blank issues off | Issues enabled; issue #1 exists; local files prepared; default-branch chooser verification pending merge |
| Pull requests | Focused issue-linked PRs; squash workflow | Squash is available; GitHub also currently allows merge/rebase; policy requires squash, settings enforcement/protection deferred to M0-08 |
| Security disclosure | Private vulnerability reporting enabled | Actual `enabled: true` verified via repository API |
| License | MIT | Maintainer selection recorded; local LICENSE prepared |
| Main protection / required CI | Configure and negative-test in M0-08 after M0-07 emits actual check | Intentionally deferred; no CI/protection success claimed |
| Preview / production / secrets | M0-09–M0-11 | Intentionally deferred; no cloud deployment or secret change |

## Verification and handoff

Check local Markdown/links/template frontmatter, final newlines, ignore examples, staged file list, secret patterns and Git whitespace. GitHub Markdown rendering/template availability and actual settings need remote evidence; a local check is not a substitute. There is no `package.json`, so app tests/lint/build are not applicable to M0-01.

**Evidence — 2026-11-07:**

- Public repository: [LaurensiusDani/PotholesMap](https://github.com/LaurensiusDani/PotholesMap).
- Scoped tracking issue: [M0-01 / #1](https://github.com/LaurensiusDani/PotholesMap/issues/1).
- Git author/committer: LaurensiusDani using account ID `98627653` noreply identity; seed commit has no co-author trailers.
- Local verification: 24 Markdown files checked for local links, code fences and final newlines; issue-template frontmatter checked; bounded credential-pattern scan had no finding. This is not an exhaustive security audit.
- GitHub's GFM rendering API successfully rendered README, CONTRIBUTING, SECURITY, both issue-template bodies and the PR template. This verifies Markdown rendering, not the default-branch template chooser or a visual browser inspection.
- Ignore samples: env/runtime secrets, dependencies/builds, backup/upload objects and local Supabase state ignored; `.env.example`, `.dev.vars.example`, SQL migrations and README remain trackable.
- Remote API: public visibility, default `main`, issues enabled, wiki disabled and private vulnerability reporting enabled verified. No required checks/main protection/CI/deployment is claimed.

**Governance PR:** Pending creation. **Result:** Prepared for review, not verified complete. Templates will only become the default issue/PR experience after an authorized merge to `main`; the chooser/manual rendering check remains outstanding. No merge or later issue is automatically started.
