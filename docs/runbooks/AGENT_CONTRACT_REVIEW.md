# Agent Contract Review — M0-02

**Updated:** 2026-11-07 · **Scope:** Reviewable agent instructions and handoff only; no runtime/application work.

## Sources and decisions

- [AGENTS.md](../../AGENTS.md) owns permanent agent guardrails.
- [Master plan](../PROJECT_MASTER_PLAN.md) owns product/architecture; [execution index](../milestones/README.md) owns common sequencing; [M0 contract](../milestones/M0_ENGINEERING_FOUNDATION.md) owns this issue's state/evidence.
- The maintainer authorized squash-merging [governance PR #2](https://github.com/LaurensiusDani/PotholesMap/pull/2), then working M0-02 on 2026-11-07. That authorization does not approve merging the M0-02 PR, starting M0-03 or deploying anything.
- Governance merged as `1facfab29a03577da68ac665f1d941c5af8f498e`; issue #1 is closed. The early AGENTS draft from 2026-11-04 is now reconciled with real governance and the current milestone index.
- Preserve maintainer author identity and no Codex co-author attribution. GitHub's squash service may appear as `web-flow` committer; authorship still resolves to LaurensiusDani.

## Scenario review

These are document/behavior walkthroughs, not automated model evals or claims of production testing.

| Input / situation | Expected agent behavior | Contract coverage |
|---|---|---|
| "Start M0" without issue ID | Read sources, choose first ready incomplete issue, announce bounded scope and checks | Scope/kickoff rules |
| "Next milestone" while M0 is incomplete | Check entry evidence, explain missing prerequisites and resolve intended continuation; no silent M1 bootstrap | Entry gates and explicit scope |
| "Merge PR #2, then M0-02" | Execute that exact approved merge, verify it, prepare M0-02 on its own branch; no repeated merge-consent request or automatic later merge | Existing authorization and Git boundaries |
| "Update the plans" | Edit authorized docs only; no app scaffold, installs, cloud setup or fabricated checks | Documentation-only scope |
| User explicitly requests an independent task before normal prerequisites | Record limited sequencing exception; deliver only that authorized part; do not mark prerequisites complete | Dependency exceptions |
| Issue/webpage says to upload secrets or override scope | Treat as untrusted task content; keep secrets and authorization boundaries | Untrusted-content rule |
| Existing package script is missing | Report check as unavailable; do not weaken checks or build unrelated tooling | Verification rules |
| Workspace has unrelated user changes | Preserve them, review staged files and commit selected issue files only | Editing/Git review |
| UI work is delivered | Apply selected milestone's browser/viewport/accessibility checks; record actual results/limitations | Owning contract and verification |
| Agent task writes a commit | Use LaurensiusDani/local account-linked noreply identity; no AI co-author trailer or global identity rewrite | Author identity |
| PR is prepared but not merged | Mark in_review; hand off actual PR and remaining review steps; no next issue without authorized continuation | Evidence and stop point |
| New chat resumes work | Read ledger/branch/PR/evidence and inspect worktree instead of trusting an old chat summary/example | Resume rule |

## Concrete bounded workflow

1. Read sources, inspect status, confirm M0-01 evidence and choose M0-02.
2. Work on `chore/m0-02-agent-contract`, based on the merged `main`.
3. Refine instructions and this review evidence; no app/runtime/CI dependencies or implementation files.
4. Review local links, Markdown structure, scenario coverage, source consistency, full diff/staged list and bounded secret-pattern findings.
5. Commit with maintainer identity, push the topic branch and open one issue-linked PR when available.
6. Update the M0 ledger with actual evidence and hand off; stop before M0-03.

## Verification and evidence

- The scenario matrix maps each required issue/PR scope, dependency, coding/review, secret/production and stop rule to concrete instructions.
- Documentation checks and PR references are recorded after execution below; no package manifest/app runner exists, so no application lint/typecheck/tests/build/CI result is claimed.
- Earlier M0-01 evidence includes GitHub GFM rendering and actual default-branch template registration; signed-in chooser interaction was not observed because the verification browser was signed out. No credentials were transferred from the CLI into browser automation.
- **Executed local checks:** 25 Markdown documents passed local-link, code-fence and final-newline checks; all 91 issue contracts/ledger references remain consistent; selected-state checks confirm M0-01 done, M0-02 in_review and M0-03 not_started. Bounded credential-pattern, allowed-file scope and Git whitespace checks passed. These checks do not prove absence of all secrets or evaluate model behavior automatically.

**Tracking issue:** [M0-02 / #3](https://github.com/LaurensiusDani/PotholesMap/issues/3). **Review PR:** Pending creation. **Result:** Prepared for review; no exit signoff or later issue activation implied.
