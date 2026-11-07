# PotholeMap — M1 Application Shell

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M0](M0_ENGINEERING_FOUNDATION.md) · **Next:** [M2](M2_DATABASE_FOUNDATION.md).

## 1. Objective and boundaries

Deliver a responsive, accessible Indonesian application shell that later features can inhabit. Entry requires M0 exit signoff, user activation and a preflight against the actual runtime/ADRs.

**In:** Routes, app layout, navigation, small shared primitives, deterministic loading/error/empty states and route smoke checks. **Out:** Live map, report API, real form submission, Supabase tables, login logic, feature flags that enable public intake, large design systems or UI frameworks without need.

Decide routing/deep-link behavior within the established SPA runtime. Prefer existing dependencies; justify any router. Mock content must be clearly development-only and must not resemble real public reports. No admin-route placeholder is an authorization mechanism.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M1-01 | M0 exit | not_started | Activation and route contract pending |
| M1-02 | M1-01 | not_started | Layout pending |
| M1-03 | M1-02 | not_started | Shared primitives pending |
| M1-04 | M1-02, M1-03 | not_started | Route states pending |
| M1-05 | M1-03, M1-04 | not_started | Accessibility/responsiveness evidence pending |
| M1-06 | M1-01–M1-05 | not_started | Exit audit pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M1-01 — Routes and shell contract

- **Objective/scope:** Specify and implement route wiring for map home, report entry, report detail, admin entry and utility pages, with SPA deep links and a real 404 path. Product pages are shells only.
- **Excluded:** Fetching reports, map SDK, auth and submissions.
- **Files:** `src/app/` routing/entry, runtime routing config if necessary, route-contract notes in this file.
- **Acceptance:** Refresh/deep-link works; unknown paths show 404; navigation is deterministic; placeholders explain unavailable features without fake success.
- **Verification/security:** Route/unit smoke, local built-app navigation; ensure `/api/*` is not swallowed by SPA fallback and no secrets enter client config.
- **PR/evidence:** Route map, dependency rationale if any, smoke results and scope review.

### M1-02 — Responsive application frame

- **Objective/scope:** Header/branding, desktop content/sidebar region, mobile content/bottom-sheet region, stable page containers and navigation. Establish minimal spacing/type/color variables in existing styling conventions.
- **Excluded:** Real map, drag-heavy interactions, elaborate theme/design-system library.
- **Files:** `src/app/`, relevant `src/components/`, app styles.
- **Acceptance:** Representative small/large widths have no accidental horizontal overflow; controls remain usable; no layout assumption requires a map implementation.
- **Verification/security:** Browser screenshots at recorded dimensions, keyboard navigation and existing quality commands; no third-party tracking assets.
- **PR/evidence:** Before/after or rendered shell screenshots and recorded viewport checks.

### M1-03 — Accessible shared primitives

- **Objective/scope:** Only needed buttons/links, labeled field/message patterns, focus styling and an accessible panel/dialog primitive if the shell uses one.
- **Excluded:** Generic component catalogue and unused variants.
- **Files:** Shared UI components/styles and focused behavior tests.
- **Acceptance:** Semantic controls, visible focus, correct disabled/busy behavior; any modal traps/restores focus, closes by the defined keyboard action and has an accessible name.
- **Verification/security:** Keyboard walkthrough and targeted interaction tests; no raw HTML for untrusted messages.
- **PR/evidence:** Implemented primitive list, interaction checks and limitations.

### M1-04 — Route states and utility shells

- **Objective/scope:** Reusable loading, empty, retryable error and not-found states; Indonesian copy and placeholders for privacy/terms/contact/report/admin pages.
- **Excluded:** Inventing finalized legal text, fake report receipts or login success.
- **Files:** Page/state components and fixture-only test wiring.
- **Acceptance:** Each state is deterministic in tests; retry/empty actions have meaningful destinations; unfinished legal/contact content is labeled as pending and not presented as launch-ready.
- **Verification/security:** State rendering and screen-reader announcements where appropriate; no real personal data in fixtures.
- **PR/evidence:** State matrix, copy review and future owner notes for M7/M9/M13.

### M1-05 — Shell accessibility and mobile verification

- **Objective/scope:** Verify the shell with keyboard, zoom and representative mobile/desktop layouts; fix shell defects only.
- **Excluded:** Full app E2E/performance program, report/map behavior.
- **Files:** Shell fixes, focused regression tests and evidence entry.
- **Acceptance:** Logical heading/focus order, usable controls at 200% zoom, text contrast meeting WCAG AA targets and no keyboard trap. Record actual tested environments; emulation is not a real-device claim.
- **Verification/security:** Existing quality gates, keyboard/zoom checks and recorded accessibility tooling/manual observations.
- **PR/evidence:** Findings/resolutions, screenshots and residual limitations for M11.

### M1-06 — Exit review and handoff

- **Objective/scope:** Reconcile routes/components with master, collect prior evidence and hand off the shell contracts to map/report/admin owners.
- **Excluded:** Starting DB setup or feature implementation.
- **Files:** This ledger, relevant README/setup notes if changed.
- **Acceptance:** M1-01–M1-05 criteria satisfied; built shell and health route coexist; no placeholder is publicly represented as a working reporting system.
- **Verification/security:** Existing quality gates, built-route smoke and scoped diff review.
- **PR/evidence:** Exit evidence, maintainer signoff and proposed M2 issue; stop here.

## 4. Exit gate

- [ ] Route/deep-link/API separation proven.
- [ ] Responsive frame and minimal accessible controls verified.
- [ ] Loading/error/empty/404 and unfinished-feature copy reviewed.
- [ ] Existing CI remains green; no live intake/auth/data added.
- [ ] Ledger/evidence complete and maintainer signoff recorded.

**Evidence index:** Not recorded. **Result:** Not verified complete. M2 activation is separate.
