# PotholeMap — M3 Map Foundation

**Version:** 1.0 · **Updated:** 2026-11-04 · **Status:** Draft execution baseline; blocked for implementation.

**Parent:** [Master plan](../PROJECT_MASTER_PLAN.md) · **Shared contract/index:** [Execution index](README.md) · **Previous:** [M2](M2_DATABASE_FOUNDATION.md) · **Next:** [M4](M4_PUBLIC_REPORT_API.md).

## 1. Objective and boundaries

Implement the map and location-selection primitives without live report data. Entry requires M2/default-order signoff, M1 shell and user activation.

**In:** Leaflet integration, compliant tile-provider decision, viewport contract, optional GPS, manual adjustable pin and accessible location controls. **Out:** Live markers/API, report persistence, photo uploads, submission, geocoding/search services or paid tiles.

M3-01 must settle initial map center/zoom and expected pilot-scale tile demand. A default map view does not itself commit the public launch geography; M13 still resolves coverage/capacity. Do not assume public OSM tiles are an unrestricted CDN.

## 2. Issue ledger

| Issue | Dependencies | State | Evidence / blocker |
|---|---|---|---|
| M3-01 | M2 exit, M1 shell | not_started | Tile policy/default view pending |
| M3-02 | M3-01 | not_started | Map lifecycle pending |
| M3-03 | M3-02 | not_started | Viewport/coordinate contract pending |
| M3-04 | M3-02, M3-03 | not_started | Location selection pending |
| M3-05 | M3-01–M3-04 | not_started | Mobile/attribution exit evidence pending |

**Selected issue:** None. **Exit signoff:** Not recorded.

## 3. Issue contracts

### M3-01 — Tile-provider and map policy decision

- **Objective/scope:** Verify current production-use policy, attribution, cache/prefetch rules, quota/billing behavior and key restrictions; document chosen free-compatible provider and initial view.
- **Excluded:** Paid upgrades, hidden third-party geocoding, promises of unlimited tile traffic.
- **Files:** Dated map-provider ADR, nonsecret map config, relevant env example/runbook notes.
- **Acceptance:** Provider explicitly supports intended use at expected volume without unauthorized billing; attribution text/placement and outage behavior specified. If no suitable provider is available, record blocker instead of using a knowingly noncompliant source.
- **Verification/security:** Official policy/account review, sample allowed tile fetch, key exposure/restriction review; no bulk download to test quota.
- **PR/evidence:** Source links/date, provider limits, configuration choice and fallback-to-error UX.

### M3-02 — Map component lifecycle

- **Objective/scope:** Integrate Leaflet in the shell, correct sizing, mount/unmount cleanup and resize handling; display selected tiles/attribution and loading/unavailable messaging.
- **Excluded:** Report data, unnecessary wrapper dependencies, tile scraping/cache backend.
- **Files:** Map component, styles/config and focused lifecycle tests.
- **Acceptance:** No duplicate maps/listeners after navigation/remount; mobile sheet/layout resize updates map; failed tiles do not freeze other UI; attribution remains visible.
- **Verification/security:** Built app route/remount/resize smoke, network observation and lifecycle regression checks; use mocks in automated tests, not repeated external tile calls.
- **PR/evidence:** Desktop/mobile captures, cleanup evidence and dependency rationale.

### M3-03 — Viewport and coordinate adapter

- **Objective/scope:** Typed bounds/point interface shared with future API consumers, explicit west/south/east/north and longitude/latitude conventions, debounced viewport events and limits-aware zoom guidance.
- **Excluded:** Fetching reports, downloading all coordinates or silently wrapping unsupported antimeridian bboxes.
- **Files:** Map/geo client helpers, typed contracts and unit tests.
- **Acceptance:** Coordinate conversion tested against known points; finite valid bounds emitted; movement settles to one debounced event; event listeners clean up. Cross-antimeridian handling is explicitly rejected/normalized according to the later API contract, not ambiguous.
- **Verification/security:** Boundary/swap/invalid-number tests and rapid-pan observation; no reporter location logging.
- **PR/evidence:** Documented coordinate contract and event lifecycle results for M4/M5.

### M3-04 — Optional GPS and manual road pin

- **Objective/scope:** User-triggered geolocation, denial/timeout/unavailable messaging and accessible manually adjustable road-location pin. Allow correcting approximate GPS.
- **Excluded:** Automatic permission request on load, EXIF geolocation, background tracking or report persistence.
- **Files:** Location selector/hooks and interaction tests.
- **Acceptance:** Denial still permits manual placement; selected coordinates remain visible/adjustable; no claim GPS equals exact pothole location; keyboard-accessible controls provide a practical non-drag path.
- **Verification/security:** Granted/denied/timeout mocks, keyboard and touch walkthrough on HTTPS-capable test environment; no persistent location collection.
- **PR/evidence:** Permission-state matrix and manual-pin checks, ready for M7 reuse.

### M3-05 — Map quality and exit handoff

- **Objective/scope:** Verify tile policy/attribution, map lifecycle, viewport and manual/GPS flows on representative screens; resolve map-foundation defects only.
- **Excluded:** Live report explorer or creation form.
- **Files:** Scoped fixes, test evidence and this ledger.
- **Acceptance:** Controls/attribution remain usable with mobile sheet, zoom and keyboard; future list alternative integration documented; no live report/submission dependency introduced.
- **Verification/security:** Existing quality gates, route smoke and recorded browser/device conditions; distinguish emulation from real-device evidence.
- **PR/evidence:** M3 exit report, known map limitations, M4/M5/M7 interface notes and signoff; stop here.

## 4. Exit gate

- [ ] Dated compliant tile decision and visible attribution.
- [ ] Stable mount/resize/unmount and graceful tile failure.
- [ ] Typed debounced viewport/coordinate contract tested.
- [ ] Optional GPS with practical manual/keyboard pin fallback.
- [ ] Quality evidence/signoff complete; no live data/intake added.

**Evidence index:** Not recorded. **Result:** Not verified complete.
