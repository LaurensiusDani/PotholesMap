# PotholeMap — M5 Report Explorer

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M4](M4_PUBLIC_REPORT_API.md) · **Next:** [M6](M6_SAFE_PHOTO_PIPELINE.md).

## 1. Objective and boundaries

Connect the map/list/detail UI to the approved-only API using synthetic isolated data. Entry requires M4 exit, M3 map primitives, M1 shell and user activation.

**In:** Typed fetch adapter, debounced viewport reads, bounded marker/list rendering, detail selection, road-status filters and accessible failure states. **Out:** Real submission, auth/moderation, publishing photos, real-time subscriptions, geocoding/search service and analytics.

M4 owns API defaults. A wide/truncated view must honestly ask for zoom/filter or offer bounded paging, not imply every report was loaded. Proposed client cap: five pages / 1,000 summaries per view maximum, lowered if measurements require it. No unbounded automatic page drain.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M5-01 | M4 exit | not_started | Client contract pending |
| M5-02 | M5-01, M3-03 | not_started | Viewport fetching pending |
| M5-03 | M5-02 | not_started | Markers/list selection pending |
| M5-04 | M5-03 | not_started | Detail/filter states pending |
| M5-05 | M5-01–M5-04 | not_started | Explorer exit evidence pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M5-01 — Typed read client and observable states

- **Objective/scope:** API client consuming M4 DTO/errors with abort support, bounded retry policy and explicit loading/empty/error/truncated states.
- **Excluded:** Client-side privileged Supabase access, framework-wide state library without demonstrated need, infinite retries.
- **Files:** Client API adapter, report feature state/types and focused tests.
- **Acceptance:** 400/404/429/503 map to useful Indonesian messages/actions; retry respects server hints where supplied; stale/aborted responses do not overwrite current data; no retry loop on invalid input.
- **Verification/security:** Mock success/error/timeout/abort/race cases and response-field review; avoid logging private URLs or full payloads.
- **PR/evidence:** Client contract, state table and exact targeted test results.

### M5-02 — Bounded viewport data lifecycle

- **Objective/scope:** Subscribe to settled map bounds, debounce reads, cancel superseded requests and reset paging on bbox/filter changes; handle too-wide bounds with zoom guidance.
- **Excluded:** Whole-database loads, unlimited prefetching or unbounded persistent browser cache.
- **Files:** Map/report hooks and viewport integration tests.
- **Acceptance:** Rapid movement yields bounded requests; late old response cannot change new view; client summary/page caps enforced; response truncation is visible. Mount/unmount has no dangling requests/listeners.
- **Verification/security:** Controlled request-count/race/abort tests, representative pan/zoom network trace and cap tests.
- **PR/evidence:** Request lifecycle observations, selected cap rationale and API alignment.

### M5-03 — Markers, clusters and accessible list

- **Objective/scope:** Render approved summaries, selection state and synchronized accessible list/bottom sheet. Use clustering only if justified by measured density; document its dependency if added.
- **Excluded:** Fabricated report locations, markers for pending reports, 3D map or advanced spatial analytics.
- **Files:** Map markers/list components, selection helpers/styles and relevant interaction tests.
- **Acceptance:** Same report identity across list/marker; keyboard list can open detail; empty/truncated dataset clearly described; clustering if present does not hide the list alternative.
- **Verification/security:** Dense/empty fixture rendering, keyboard/touch selection and measured small-device responsiveness; no unsafe HTML in marker popups.
- **PR/evidence:** Render captures, density observations, component/selection tests and dependency decision.

### M5-04 — Detail panel and road-status filters

- **Objective/scope:** Lazy-fetch approved detail, display date/description/location/road status, placeholder media and road-status controls; selection close/back restores useful focus.
- **Excluded:** Private photos, editing road status, treating filters as authorization.
- **Files:** Detail/filter components, routing/selection integration and tests.
- **Acceptance:** Plain-text content; no report shown after detail becomes 404; filter changes reset cursor/selection as documented; placeholder never tries a private path. Detail deep-link behavior matches M1 routes.
- **Verification/security:** Slow/missing/changed detail, malicious text, filter races, keyboard focus and direct-link smoke.
- **PR/evidence:** UX states, filter/API examples and M10 media integration point.

### M5-05 — Explorer regression and exit handoff

- **Objective/scope:** Verify map/list/detail/filter journey, mobile/keyboard access, request caps and graceful API/tile outage; fix explorer defects only.
- **Excluded:** Upload/submission, moderation or full launch QA.
- **Files:** Focused regression tests, explorer fixes and this evidence ledger.
- **Acceptance:** Another isolated visitor can explore approved fixtures; no private fields, uncontrolled requests or misleading completion states; open issues explicitly passed to M11.
- **Verification/security:** Shared quality gates, applicable DB/API checks and recorded browser smoke/network inspection.
- **PR/evidence:** Exit report with representative layouts, request counts, limitations and signoff; stop before M6.

## 4. Exit gate

- [ ] Abort/debounce/race behavior and page/summary caps tested.
- [ ] API-backed markers, accessible list, detail and road-status filtering work.
- [ ] Placeholder media safe; no raw HTML or private-record inference.
- [ ] Empty/truncated/invalid/wait/error flows are useful on mobile and keyboard.
- [ ] Evidence/signoff complete; no submission or photo publication enabled.

**Evidence index:** Not recorded. **Result:** Not verified complete.
