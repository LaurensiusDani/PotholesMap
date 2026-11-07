# PotholeMap — M8 Abuse Prevention

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M7](M7_REPORT_SUBMISSION.md) · **Next:** [M9](M9_ADMIN_AUTHENTICATION.md).

## 1. Objective and boundaries

Harden and measure existing read/submission controls before admin/publication integration. Entry requires M7 exit, M4 read API and user activation.

**In:** Abuse threat matrix, shared limiter integration/read coverage, hard caps/budgets, secure headers/origin policy, operational pause and adversarial regression tests. **Out:** Public intake enablement, WAF paid upgrade, Redis/broker, reporting reputation and full admin login implementation.

Reuse M7's atomic shared baseline limiter. If it cannot safely handle target traffic within approved quotas, record measured alternatives/ADR and change the plan deliberately. A CAPTCHA alone is not rate limiting; an authoritative per-instance memory counter is insufficient. Thresholds are operational configuration chosen from measured pilot assumptions, with local/preview/production separation.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M8-01 | M7 exit, M4-05 | not_started | Threat/budget matrix pending |
| M8-02 | M8-01, M7-02 | not_started | Shared read/write limits pending |
| M8-03 | M8-02 | not_started | Headers/origin policy pending |
| M8-04 | M8-01, M8-02 | not_started | Emergency pause/quota runbook pending |
| M8-05 | M8-02, M8-03, M8-04 | not_started | Abuse regression pending |
| M8-06 | M8-01–M8-05 | not_started | Exit audit pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M8-01 — Threat model and request budgets

- **Objective/scope:** Map abusive submission/read patterns, bot verifier exhaustion, image CPU, DB/storage/egress caps, idempotency-key growth and trusted proxy/address context. Define per-route resource/limit policy and small pilot traffic assumptions.
- **Excluded:** Security certification, guessed unlimited free quotas, identifying every reporter by long-lived raw IP logs.
- **Files:** Security/abuse policy and final limit decisions in this contract/runbook.
- **Acceptance:** Each expensive operation has body/query/time/rate/storage bounds; failure mode and accepted limitations explicit; current provider features/quotas verified with dated official references before implementation.
- **Verification/security:** Trace each threat to an existing control/test or owning follow-up; review privacy/retention and outage decisions.
- **PR/evidence:** Threat/control/route matrix, selected budget assumptions and blockers.

### M8-02 — Shared read/write limiter hardening

- **Objective/scope:** Extend/review M7 shared limiter for reads, submissions and expensive media work; enforce atomic updates, trustworthy request identity, global budget where feasible, expiry/cleanup and 429/Retry-After semantics.
- **Excluded:** Memory-only guarantee, broad public counter access or a second parallel limiter system without reason.
- **Files:** Security/data helpers and narrow SQL/config changes, limiter tests, retention runbook.
- **Acceptance:** Independent Worker instances observe the same limits; concurrency does not overspend the intended bucket; forged address headers cannot reset identity. Required write protection outages fail closed; bounded reads use the documented unavailable/degraded policy without silently disabling caps. DB limiter load fits measured budget or produces a blocking decision.
- **Verification/security:** Multi-instance/concurrent/boundary-window tests, spoofing, expiry, unavailable limiter and modest synthetic load; review grants and identifier retention.
- **PR/evidence:** Algorithm/consistency model, configured thresholds, resource measurements and direct-access checks.

### M8-03 — Web security headers and origin boundaries

- **Objective/scope:** Enforce tested CSP, `nosniff`, framing policy, HTTPS-related configuration, minimal CORS and origin checks appropriate to actual endpoints; document M9 cookie/CSRF integration point.
- **Excluded:** Claiming CORS stops nonbrowser abuse, blind CSP wildcard exemptions or credentialed cross-origin sharing by default.
- **Files:** Worker/static-asset headers/routing config, middleware and browser integration tests.
- **Acceptance:** Map tiles, Turnstile, safe media and app bundles work under deliberate allowlists; untrusted origins cannot use credentialed mutations. Anonymous origin policy is defense in depth; CAPTCHA/limits remain enforced. No auth cookie settings are invented before M9 chooses the model.
- **Verification/security:** Response/header inspection, built-app tile/bot flows, blocked origin/frame/inline-script checks and regression against SPA/API routing.
- **PR/evidence:** Effective header policy, allowlist rationale and browser test results.

### M8-04 — Emergency pause and quota response

- **Objective/scope:** Prove intake pause through deployed configuration/shared control, audit operational activation, expiry of stale enabled settings and clear maintenance messaging. Document quota-full/spam response and recovery criteria.
- **Excluded:** Unauthenticated public pause endpoint, secret logging or claiming already in-flight requests can be instantly undone.
- **Files:** Operations/config helpers, `docs/runbooks/ABUSE_RESPONSE.md`, client maintenance messaging and tests.
- **Acceptance:** Disabled intake blocks new work across instances within documented propagation time; existing in-flight completion/cleanup behavior is explicit. Low/critical quota actions and resume criteria are actionable; test enablement cannot accidentally enable production.
- **Verification/security:** Isolated distributed pause/resume drill, config-missing/error cases, in-flight operation test and protected control access review.
- **PR/evidence:** Measured propagation/limitations, quota checklist and recovery drill evidence.

### M8-05 — Adversarial integration regressions

- **Objective/scope:** Integrate malformed multipart/coordinate/cursor, spoofed images, token misuse, limiter bypass, XSS text, idempotency growth and paused/dependency-outage scenarios across existing endpoints.
- **Excluded:** Attacking third-party services/real production, unbounded stress or a broad penetration-test claim.
- **Files:** Bounded security/DB/API tests and scoped fixes.
- **Acceptance:** Rejected attempts cause no public/private leakage or uncontrolled resource work; pending rows remain hidden; sensitive logs absent; app handles 429/503 without infinite retry loops.
- **Verification/security:** Existing suites plus `pnpm test:db`, shared quality gates, bounded local/isolated abuse harness and log/bundle inspection.
- **PR/evidence:** Attack/control/result matrix, measured counts/resource use and residual risks for M12.

### M8-06 — Exit audit and admin handoff

- **Objective/scope:** Review controls/evidence, reconcile quotas/limits with M6/M7 and provide actual origin/header/limiter contracts to M9/M10.
- **Excluded:** Enabling public intake or implementing admin identity.
- **Files:** This ledger, abuse/security runbooks and downstream corrections if needed.
- **Acceptance:** Shared controls and pause have evidence; remaining known limits are recorded without replacing mandatory controls; production intake still off.
- **Verification/security:** Required quality/security suites and config/secret review.
- **PR/evidence:** Exit signoff, admin guard integration notes and M12 security-review backlog; stop here.

## 4. Exit gate

- [ ] Threat/route/resource budgets documented and dated provider assumptions verified.
- [ ] Shared read/write limits and outage/expiry/concurrency behavior proven.
- [ ] Headers, origin policy and bot/map interactions tested.
- [ ] Safe emergency pause/quota drill completed in isolation.
- [ ] Adversarial regressions and signoff complete; production intake off.

**Evidence index:** Not recorded. **Result:** Not verified complete.
