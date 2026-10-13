# Hormuz Vasooli System — Phased Implementation Plan

Version: 1.0  
Status: Draft for User Review  
Target Stack: NetBeans IDE 8.2 | JDK 8 (1.8.0_181) | Maven | Java EE 7 Web | GlassFish Server 4.1.1  
Frontend Stack: HTML5 | CSS3 | Vanilla JavaScript (ES6) | Google Maps JavaScript API  

---

## 1. Implementation Principles & Governance

The implementation of **Hormuz Vasooli System (HVS)** follows an incremental, phased development roadmap. Each phase delivers a verified, functional layer of the system before dependent phases commence.

### 1.1 Core Development Principles
1. **Strict Stack Preservation**: The application must deploy to **GlassFish Server 4.1.1** as a **Java EE 7 Maven Web Application** compiled for **Java 8**. No migrations to React, Vite, Spring Boot, Quarkus, or alternate servers are permitted without explicit written user approval.
2. **Surgical, Reviewable Increments**: Implementation proceeds one phase at a time. Code must be tested and verified against defined acceptance criteria before moving to the next phase.
3. **Decoupled Financial Calculations & Authority Status**: Toll policy evaluation and revenue calculations are strictly decoupled from visual rendering and animation loops. (Note: Server-authoritative tolling remains a proposed architecture [DEC-010] awaiting formal sign-off).
4. **Interactive Configuration & Dynamic Management**: Seed data is defined in structured JSON (`src/main/resources/simulation-config.json`), while interactive UI management drawers allow live additions and edits of countries, toll tariffs, vessels, routes, and checkpoints before or during the practical examination (DEC-026).
5. **Historical Immutability & Multi-Run Reset Behavior**: Modifications to toll rates or exemptions apply strictly to future encounters; past transactions remain permanently immutable. Simulation reset closes the current run and begins a new active run with zero revenue and an empty active-run ledger, preserving past-run transactions in-session as immutable history. All user-configured countries, vessels, routes, checkpoints, and toll policies strictly survive simulation reset. In-memory run history is session-bound (DEC-027).
6. **Client-Side RFC 4180 CSV Reporting**: Downloadable transaction ledgers and revenue summary reports are generated directly in-browser using standard Web APIs.
7. **Anti-Template Restraint**: UI implementation must strictly follow [docs/UI_UX_GUIDELINES.md](UI_UX_GUIDELINES.md) (inspired by `docs/references/visual-reference.png`), avoiding generic AI templates, excessive blur, and emoji icons in interface controls.
8. **Educational & Simulation Transparency**: All toll rules, checkpoints, and transactions are strictly hypothetical simulation models for a college practical examination and must be accompanied by explicit disclaimers.

---

## 2. Phase 0: Documentation and Scope Review

### Objective
Ensure that the product requirements, technical architecture, visual design guidelines, decision register, and change management procedures are completely aligned and verified before any application source code is written.

### Prerequisites
- Inspection of the existing project structure in NetBeans IDE 8.2.
- Availability of the user-selected visual reference image (`docs/references/visual-reference.png`).

### Planned Work
- Cross-review [docs/brain.md](brain.md), [docs/PRD.md](PRD.md), [docs/TRD.md](TRD.md), [docs/UI_UX_GUIDELINES.md](UI_UX_GUIDELINES.md), [docs/DECISIONS.md](DECISIONS.md), and [docs/PROFESSOR_CHANGELOG.md](PROFESSOR_CHANGELOG.md).
- Verify that no architectural contradictions exist between documents.
- Identify the exact minimal configuration changes needed in `pom.xml` for Java 8 compilation.
- Confirm baseline scope boundaries (in-memory demonstration ledger; no unapproved database).

### Affected Files / Components
- `docs/brain.md`, `docs/PRD.md`, `docs/TRD.md`, `docs/UI_UX_GUIDELINES.md`, `docs/DECISIONS.md`, `docs/PROFESSOR_CHANGELOG.md`.

### Acceptance Criteria
- [ ] All 6 core planning documents are created and mutually consistent.
- [ ] All open design questions and confirmed decisions are explicitly catalogued in `DECISIONS.md`.
- [ ] No application source files or configuration files (`pom.xml`, `nb-configuration.xml`) have been modified yet.
- [ ] Formal sign-off on Phase 0 is obtained from the user before Phase 1 begins.

### Verification Steps
1. Verify document presence, timestamps, and line counts.
2. Confirm zero git/working tree modifications to existing source files.

### Risks & Compatibility Concerns
- *Risk*: Premature coding before documentation consensus leads to rework.
- *Mitigation*: Hard stop after documentation phase; wait for user confirmation.

### Phase Completion Condition
Phase 0 is complete when the user reviews and approves the documentation baseline and authorizes execution of Phase 1.

---

## 3. Phase 1: Environment & Build Baseline

### Objective
Verify that the existing Maven Web Application builds cleanly into a `.war` artifact and can be deployed to GlassFish Server 4.1.1 without blocking environment or classloader errors before writing application features.

### Prerequisites
- Phase 0 approved.
- NetBeans IDE 8.2 with JDK 8 (`1.8.0_181`) and GlassFish Server 4.1.1 running locally.

### Planned Work
- Inspect `pom.xml` and document the exact modification required to update `maven-compiler-plugin` `<source>` and `<target>` from `1.7` to `1.8`.
- Verify the `maven-dependency-plugin` endorsed-API setup required by GlassFish 4.1.1.
- Execute a baseline build (`mvn clean package`) using NetBeans bundled Maven.
- Verify that the resulting `target/HormuzVasooliSystem-1.0-SNAPSHOT.war` packages correctly.
- Test deployment to GlassFish 4.1.1 at `http://localhost:8080/HormuzVasooliSystem/`.
- Record baseline build logs and environment health.

### Affected Files / Components
- `pom.xml` (Update compiler source/target to 1.8 after explicit review).
- `nb-configuration.xml` (Preserve IDE deployment settings).

### Dependencies
- Depends on Phase 0.

### Acceptance Criteria
- [ ] `pom.xml` compiler plugin is cleanly configured for Java 8 (`1.8`) without breaking endorsed libraries.
- [ ] Maven build (`mvn clean package`) finishes with `BUILD SUCCESS`.
- [ ] GlassFish 4.1.1 deploys the application without deployment descriptor or classloading errors.
- [ ] Accessing `http://localhost:8080/HormuzVasooliSystem/` returns HTTP 200 with the starter page.

### Verification Steps
1. Run `mvn clean package` via NetBeans/command line; verify exit code 0.
2. Deploy WAR to local GlassFish server; inspect `server.log` for zero fatal exceptions.
3. Verify browser HTTP response at local context root.

### Risks & Compatibility Concerns
- *Risk*: Compiler plugin versions or endorsed directory paths causing build failure under JDK 8.
- *Mitigation*: Keep `javaee-web-api` 7.0 in `provided` scope; avoid adding third-party jars.

### Phase Completion Condition
Phase 1 is complete when a clean Java 8 WAR build and successful GlassFish deployment are demonstrated and recorded.

---

## 4. Phase 2: Application Foundation & Design System

### Objective
Implement the responsive single-page dashboard shell, layout structure, CSS design tokens, and glassmorphic styling according to the approved visual guidelines.

### Prerequisites
- Phase 1 completed and verified.

### Planned Work
- Create frontend web directories: `css/`, `js/`, `assets/flags/`, `assets/ships/`.
- Implement `src/main/webapp/css/styles.css` containing CSS custom properties (tokens), dark oceanic palette (`#0B0E14`), glassmorphism (`backdrop-filter: blur(14px)`), and typography scales.
- Structure `src/main/webapp/index.html` with:
  - Top floating navigation bar (branding, disclaimer badge, API key trigger, status indicator).
  - Left floating stack (controls container, traffic KPI widget, search/filter panel, fleet drawer).
  - Top-right floating financial overview bar (Iran, Oman, Combined totals).
  - Central map container (`#map-canvas`).
  - Bottom collapsible transaction ledger.
  - Right slide-out vessel detail drawer.
- Implement responsive breakpoints (1920px, 1366px, 1024px) ensuring map visibility is prioritized.

### Affected Files / Components
- `src/main/webapp/index.html`
- `src/main/webapp/css/styles.css`
- `src/main/webapp/js/app.js` (Initial boot coordinator)

### Dependencies
- Depends on Phase 1.

### Acceptance Criteria
- [ ] Application loads in browser with the authentic maritime operations theme.
- [ ] Layout matches the asymmetric card hierarchy defined in `UI_UX_GUIDELINES.md`.
- [ ] Central map area occupies 70%+ of the viewport.
- [ ] Zero decorative fake charts or unauthorized template widgets exist.
- [ ] Font fallbacks function cleanly offline.

### Verification Steps
1. Build and redeploy application.
2. Open in browser; visually verify layout across 1920x1080 and 1366x768 resolutions.
3. Inspect DOM and CSS styles in browser DevTools.

### Risks & Compatibility Concerns
- *Risk*: High backdrop-blur values causing GPU stuttering in older browsers.
- *Mitigation*: Limit blur to `14px` and set fallback solid rgba backgrounds.

### Phase Completion Condition
Phase 2 is complete when the full dashboard structure renders cleanly in GlassFish with valid CSS and zero layout overlap.

---

## 5. Phase 3: Google Maps Integration

### Objective
Integrate the Google Maps JavaScript API, centering the map dynamically on the Strait of Hormuz with custom dark maritime styling and robust API key handling.

### Prerequisites
- Phase 2 completed.

### Planned Work
- Implement `src/main/webapp/js/config.js` with `GOOGLE_MAPS_CONFIG` (placeholder key, coordinates `26.5667 N, 56.2500 E`, default zoom `9`).
- Implement `src/main/webapp/js/map.js` to dynamically load the Google Maps API script.
- Apply custom dark maritime styling JSON array (obsidian water, slate coastlines, muted boundary borders).
- Add runtime API Key Configuration Modal in the UI allowing the user to supply or update their key.
- Implement `window.gm_authFailure` and network load error handlers displaying a non-blocking notification banner.
- Add boundary and zoom constraints preventing users from panning away from the Persian Gulf / Gulf of Oman region.

### Affected Files / Components
- `src/main/webapp/js/config.js`
- `src/main/webapp/js/map.js`
- `src/main/webapp/index.html`

### Dependencies
- Depends on Phase 2.

### Acceptance Criteria
- [ ] Google Maps loads centered at the Strait of Hormuz (`~26.56° N, 56.25° E`).
- [ ] Iran (north) and Oman (south/Musandam) are clearly visible and distinguishable.
- [ ] Zoom controls, pan controls, and dark map tiles operate smoothly.
- [ ] Missing or invalid API key triggers an informative user prompt without crashing the page.
- [ ] No static image is used as a fake substitute for the interactive map.

### Verification Steps
1. Load page with valid API key: verify interactive map tiles and pan/zoom gestures.
2. Load page with placeholder key: verify graceful error banner and runtime key entry modal.

### Risks & Compatibility Concerns
- *Risk*: Google Maps API quota exhaustion or network restrictions in local lab environments.
- *Mitigation*: Runtime modal allows switching keys immediately without recompiling.

### Phase Completion Condition
Phase 3 is complete when interactive map rendering, navigation bounds, and error handling are verified.

---

## 6. Phase 4: Configuration & Vessel Data

### Objective
Define and establish the authoritative, decoupled simulation configuration file containing checkpoints, routes, vessel manifests, and hypothetical toll tariff rules.

### Prerequisites
- Phase 1 and Phase 3 completed.

### Planned Work
- Create `src/main/resources/simulation-config.json` containing:
  - System metadata and mandatory educational simulation disclaimer.
  - Hypothetical checkpoints: `CP-IRAN-NORTH` (Larak/Bandar Abbas sector) and `CP-OMAN-SOUTH` (Musandam sector) with coordinates and detection radii.
  - Inbound and Outbound Traffic Separation Scheme (TSS) route waypoints through the Strait.
  - Vessel records (6 initial ships) with IDs, names, IMO numbers, nationalities, cargo types, deadweight tonnage, and route assignments.
  - Tariff matrix specifying base USD fees, cargo hazard multipliers, and national exemptions.
- Provide local SVG flag assets in `src/main/webapp/assets/flags/` for all configured nations (`in.svg`, `cn.svg`, `pa.svg`, `lr.svg`, `om.svg`, `ir.svg`) and `fallback-maritime.svg`.

### Affected Files / Components
- `src/main/resources/simulation-config.json`
- `src/main/webapp/assets/flags/*.svg`

### Dependencies
- Depends on Phase 1.

### Acceptance Criteria
- [ ] `simulation-config.json` is syntactically valid JSON conforming to TRD Section 5 schema.
- [ ] All 6 demo vessels have complete manifests and valid route assignments.
- [ ] Toll tariffs and exemptions are defined entirely within the JSON configuration.
- [ ] All corresponding SVG flags and the fallback nautical ensign exist locally in `assets/flags/`.
- [ ] Disclaimer text is prominently included in the configuration payload.

### Verification Steps
1. Validate JSON syntax via parser.
2. Verify all referenced SVG flag assets exist on disk and render crisply.

### Risks & Compatibility Concerns
- *Risk*: Maven packaging omitting resources from `WEB-INF/classes`.
- *Mitigation*: Standard Maven convention automatically places `src/main/resources` onto the classpath.

### Phase Completion Condition
Phase 4 is complete when the configuration resource is packaged and validated.

---

## 7. Phase 5: Custom Ship Rendering & Flags

### Objective
Render recognizable top-down SVG ship models on the Google Map, complete with masts, national flags, heading-aware rotation, and zoom-compensated proportions.

### Prerequisites
- Phase 3 (Google Maps) and Phase 4 (SVG Flag Assets) completed.

### Planned Work
- Implement `src/main/webapp/js/ship-renderer.js`.
- Construct composite ship markers using Google Maps `OverlayView` or custom SVG HTML markers:
  - Top-down tapered ship hull silhouette colored by cargo classification.
  - Centered vertical mast pole.
  - Rectangular vector flag badge anchored at the masthead (`assets/flags/${flagIcon}.svg`).
- Implement dynamic heading rotation: hull rotates to vessel azimuth ($\theta$), while the flag counter-rotates ($-\theta$) to remain permanently horizontal and legible.
- Implement zoom-level scaling matrix (Zoom 8: `36x10px`, Zoom 9–10: `48x14px`, Zoom 11+: `60x18px`).
- Enforce the rule that flag surface area never exceeds 35% of hull surface area.
- Implement graceful fallback to `fallback-maritime.svg` if a flag icon fails to load.
- Add click listener on ship markers to trigger vessel selection.

### Affected Files / Components
- `src/main/webapp/js/ship-renderer.js`
- `src/main/webapp/css/styles.css`

### Dependencies
- Depends on Phase 3 and Phase 4.

### Acceptance Criteria
- [ ] Ships appear as authentic top-down models rather than generic arrows.
- [ ] Each ship displays a visible mast and the correct national SVG flag.
- [ ] The ship hull rotates along its heading; the flag badge remains upright and readable.
- [ ] Markers remain proportional and legible across map zoom levels 8 through 12.
- [ ] Missing flag assets fall back smoothly to the neutral maritime ensign without console errors.
- [ ] Clicking a ship marker fires the selection event.

### Verification Steps
1. Render markers at test coordinates on the map.
2. Zoom in and out (zoom levels 8 to 12); verify proportional scaling.
3. Test rotation through 0°, 90°, 180°, and 270°; verify flag stays horizontal.
4. Pass an invalid flag code; verify fallback icon renders cleanly.

### Risks & Compatibility Concerns
- *Risk*: DOM overlay jitter during rapid map panning.
- *Mitigation*: Use CSS `will-change: transform` and hardware-accelerated transforms.

### Phase Completion Condition
Phase 5 is complete when multi-vessel rendering with masts, flags, and heading rotation is fully verified.

---

## 8. Phase 6: Ship Movement Simulation

### Objective
Animate simulated vessels along predefined TSS waypoints with linear interpolation, forward azimuth heading calculation, simulation lifecycle controls, and spatial checkpoint proximity detection.

### Prerequisites
- Phase 3 (Map) and Phase 5 (Ship Rendering) completed.

### Planned Work
- Implement `src/main/webapp/js/simulation.js`.
- Compute route segment distances using the Haversine formula and interpolate positions on each animation frame (`requestAnimationFrame`).
- Calculate instantaneous heading angle ($\theta$) using forward azimuth mathematics.
- Implement simulation controls:
  - **Start**: Launches movement progression loop.
  - **Pause**: Halts progress while preserving positions.
  - **Resume**: Continues progression from current coordinates.
  - **Reset**: Returns all vessels to initial progress coordinates (`progress = 0`).
  - **Speed Multiplier**: Supports 1x, 2x, and 4x speed scaling.
- Implement vessel status transitions: `WAITING` -> `PASSING` -> `PASSED`.
- Implement spatial checkpoint proximity detection: Calculate distance from vessel to checkpoint coordinates; trigger a crossing event upon boundary entry (`distance <= radiusKm`).
- Implement client-side debounce/latch so that a ship inside a checkpoint zone triggers the crossing event exactly once per passage.

### Affected Files / Components
- `src/main/webapp/js/simulation.js`
- `src/main/webapp/js/app.js`

### Dependencies
- Depends on Phase 3 and Phase 5.

### Acceptance Criteria
- [ ] Vessels move smoothly along their assigned inbound and outbound shipping lanes.
- [ ] Vessel hulls accurately orient toward their next route waypoints.
- [ ] Start, Pause, Resume, and Reset buttons reliably control the animation loop.
- [ ] Speed toggles scale animation speed proportionally without position warping.
- [ ] Checkpoint entry triggers exactly one crossing event per vessel transit.
- [ ] Reset restores vessels to their starting waypoints and clears active transit flags.

### Verification Steps
1. Start simulation; observe vessel traversal through the Strait.
2. Click Pause; verify movement freezes. Click Resume; verify seamless continuation.
3. Observe checkpoint zone crossing; verify event fires once and does not spam requests each frame.
4. Click Reset; verify vessels return to initial coordinates.

### Risks & Compatibility Concerns
- *Risk*: Delta-time accumulation causing ships to skip waypoints during background tab throttling.
- *Mitigation*: Clamp maximum $\Delta t$ per frame to 100ms.

### Phase Completion Condition
Phase 6 is complete when movement, heading calculation, lifecycle controls, and crossing detections operate reliably.

---

## 9. Phase 7: Java Backend & API Contracts

### Objective
Implement the Java EE 7 service layer, domain models, and Servlets to serve static configuration and provide authoritative backend simulation endpoints.

### Prerequisites
- Phase 1 (Build Baseline) and Phase 4 (Configuration Data) completed.

### Planned Work
- Implement domain models (`com.hormuzvasooli.model`): `Ship.java`, `Checkpoint.java`, `TollRule.java`, `TollTransaction.java`, `SimulationState.java`, `TollReceipt.java`.
- Implement `com.hormuzvasooli.service.SimulationConfigService`: Loads `simulation-config.json` via classpath and parses into domain objects using `javax.json`.
- Implement `com.hormuzvasooli.service.SimulationStateService`: Thread-safe in-memory state manager tracking `runId`, revenues, and transaction history.
- Implement `com.hormuzvasooli.listener.ApplicationStartupListener` to initialize singletons on GlassFish context startup.
- Implement Servlets (`com.hormuzvasooli.servlet`):
  - `BaseServlet.java`: Standard JSON headers and error serialization.
  - `SimulationConfigServlet.java` (`@WebServlet("/api/simulation/config")`): Emits public configuration JSON.
  - `SimulationStateServlet.java` (`@WebServlet("/api/simulation/state")`): Emits current session financials and state.
  - `SimulationResetServlet.java` (`@WebServlet("/api/simulation/reset")`): Closes active run, generates new runId, archives previous run transactions into in-memory session history, and zeroes active run revenue.
- Implement frontend `src/main/webapp/js/api-client.js` to communicate with backend endpoints.

### Affected Files / Components
- `src/main/java/com/hormuzvasooli/model/*.java`
- `src/main/java/com/hormuzvasooli/service/*.java`
- `src/main/java/com/hormuzvasooli/servlet/*.java`
- `src/main/java/com/hormuzvasooli/listener/ApplicationStartupListener.java`
- `src/main/webapp/js/api-client.js`

### Dependencies
- Depends on Phase 1 and Phase 4.

### Acceptance Criteria
- [ ] Backend compiles under Java 8 with zero errors.
- [ ] `GET /api/simulation/config` returns HTTP 200 with complete JSON configuration.
- [ ] `GET /api/simulation/state` returns HTTP 200 with initial run ID and zero revenues.
- [ ] `POST /api/simulation/reset` returns HTTP 200, generates a new run ID, archives prior transactions in-session, and zeroes active run revenue.
- [ ] Only native Java EE 7 APIs (`javax.json.*`, `javax.servlet.*`) are used; zero third-party jar dependencies added.

### Verification Steps
1. Deploy to GlassFish 4.1.1.
2. Issue HTTP requests to `/api/simulation/config`, `/api/simulation/state`, and `/api/simulation/reset` via browser/curl.
3. Verify JSON response structure and correct headers (`application/json; charset=UTF-8`).

### Risks & Compatibility Concerns
- *Risk*: ClassLoader conflicts with GlassFish native JSON-P.
- *Mitigation*: Rely exclusively on `javax.json` provided by `javaee-web-api:7.0`.

### Phase Completion Condition
Phase 7 is complete when configuration loading, state queries, and reset endpoints operate correctly on GlassFish.

---

## 10. Phase 8: Toll Engine & Transaction Safety

### Objective
Implement server-authoritative hypothetical toll assessment, exemption evaluation, compound idempotency duplicate prevention, and separate Iran/Oman/Combined revenue aggregation.

### Prerequisites
- Phase 7 (Backend Services) completed.

### Planned Work
- Implement `com.hormuzvasooli.service.TollPolicyService`:
  - Executes the authoritative 13-step assessment pipeline (TRD Section 7).
  - Validates `runId`, `vesselId`, `checkpointId`, and `passageId`.
  - Enforces compound idempotency key: `runId:vesselId:checkpointId:passageId`.
  - Rejects duplicate checkpoint encounters with HTTP 409 Conflict / duplicate acknowledgment.
  - Applies country exemptions: Friendly nation vessels receive `$0.00 USD` fee with status `"EXEMPT"`.
  - Applies positive charges: $\text{Charge} = \text{BaseFee} \times \text{CargoMultiplier}$ with status `"CHARGED"`.
  - Updates in-memory revenue accumulators atomically (`iranRevenueUsd`, `omanRevenueUsd`).
  - Derives `totalRevenueUsd = iranRevenueUsd + omanRevenueUsd`.
- Implement `com.hormuzvasooli.servlet.TollAssessmentServlet` (`@WebServlet("/api/simulation/toll")`):
  - Handles HTTP POST with JSON body.
  - Returns itemized `TollReceipt` and updated financial snapshot.
- Connect client-side `simulation.js` crossing events to `api-client.js -> POST /api/simulation/toll`.

### Affected Files / Components
- `src/main/java/com/hormuzvasooli/service/TollPolicyService.java`
- `src/main/java/com/hormuzvasooli/servlet/TollAssessmentServlet.java`
- `src/main/webapp/js/api-client.js`
- `src/main/webapp/js/simulation.js`

### Dependencies
- Depends on Phase 7.

### Acceptance Criteria
- [ ] Toll fees are calculated authoritatively on the server; client cannot inject trusted fee amounts.
- [ ] A non-exempt ship crossing `CP-IRAN-NORTH` generates a valid receipt and increments Iran revenue.
- [ ] A non-exempt ship crossing `CP-OMAN-SOUTH` generates a valid receipt and increments Oman revenue.
- [ ] An exempt ship (e.g. China at Iran checkpoint, Oman flag at Oman checkpoint) yields `$0 USD` with status `"EXEMPT"`.
- [ ] Submitting the same encounter twice returns a duplicate rejection without incrementing revenue.
- [ ] A vessel charged at one checkpoint can still be legitimately assessed at the other checkpoint.
- [ ] Total revenue strictly equals $\text{Iran Revenue} + \text{Oman Revenue}$.

### Verification Steps
1. Execute unit/integration tests for `TollPolicyService`.
2. Post valid toll event; verify HTTP 200, receipt generation, and revenue increment.
3. Post identical toll event immediately; verify duplicate rejection and unchanged revenue.
4. Post exempt country event; verify `$0 USD` fee and zero revenue increment.

### Risks & Compatibility Concerns
- *Risk*: Race conditions during simultaneous vessel crossing events.
- *Mitigation*: Use thread-safe `ConcurrentHashMap` and synchronized accumulator locks.

### Phase Completion Condition
Phase 8 is complete when authoritative toll assessment, duplicate rejection, and revenue calculations are fully verified.

---

## 11. Phase 9: Dashboard & Vessel Inspection

### Objective
Connect the real-time simulation and backend financial state to the UI dashboard, binding live counters, the interactive vessel inspection drawer, and toll receipts.

### Prerequisites
- Phase 6 (Simulation), Phase 7 (Backend API), and Phase 8 (Toll Engine) completed.

### Planned Work
- Implement `src/main/webapp/js/dashboard.js`:
  - Binds live counters for Active Fleet, Waiting, Passing, and Passed vessels.
  - Updates Iran Revenue, Oman Revenue, and Total Revenue displays in real time upon server confirmation.
  - Implements formatted USD currency strings (`$XX,XXX USD`).
- Implement `src/main/webapp/js/toll-ui.js`:
  - Renders live transaction stream/ticker at bottom of screen.
  - Displays color-coded badges: Green (`CHARGED`), Gold (`EXEMPT`), Grey (`DUPLICATE`).
  - Animates checkpoint pulse waves when crossings occur.
- Implement Selected Vessel Inspection Drawer:
  - Displays full vessel manifest (Name, Flag, IMO, Cargo, Tonnage, Origin, Destination, Route).
  - Shows checkpoint status (Approaching, Cleared, Charged, Exempt).
  - Displays itemized toll invoice breakdown when available.
  - Provides "Center Map on Ship" button.
- Implement page load reconciliation: On startup/refresh, query `/api/simulation/state` to reconcile UI with active server state.

### Affected Files / Components
- `src/main/webapp/js/dashboard.js`
- `src/main/webapp/js/toll-ui.js`
- `src/main/webapp/index.html`

### Dependencies
- Depends on Phases 6, 7, and 8.

### Acceptance Criteria
- [ ] Displayed revenue values match the authoritative server ledger exactly.
- [ ] Selecting any ship on the map or registry opens the detailed inspection drawer.
- [ ] Checkpoint crossings trigger real-time ledger entries and animated feedback.
- [ ] No hard-coded or fabricated metrics appear anywhere in the interface.
- [ ] Page reload reconciles displayed financial state with the server.

### Verification Steps
1. Run simulation through multiple checkpoint crossings.
2. Verify live counters update immediately upon server confirmation.
3. Click different ships; verify manifest accuracy and invoice details.
4. Refresh browser; verify state is preserved from server.

### Risks & Compatibility Concerns
- *Risk*: UI showing optimistic toll success before backend validation succeeds.
- *Mitigation*: UI updates financial figures only after receiving HTTP 200 response from server.

### Phase Completion Condition
Phase 9 is complete when live dashboard counters, receipt rendering, and vessel inspection operate seamlessly.

---

## 12. Phase 10: Management Interfaces, Search/Filter, Ledger & CSV Reporting

### Objective
Implement the interactive configuration and management interfaces (Country, Toll Policy, Vessel, Route, Checkpoint CRUD), multi-attribute search and filtering, the complete transaction audit ledger, and client-side RFC 4180 CSV export.

### Prerequisites
- Phase 9 completed.

### Planned Work
- Implement interactive management drawers and modals in `src/main/webapp/js/management.js`:
  - **Country Management**: Form to add/edit country name, 3-letter ISO code, SVG flag association, default tariff, and exemption status.
  - **Toll Policy Management**: Interface to adjust Iran/Oman tariffs and cargo multipliers, and toggle national exemptions.
  - **Vessel Management**: Interface to add/edit vessels (name, IMO, nationality dropdown, cargo category, route assignment).
  - **Route & Checkpoint Management**: Interface to adjust waypoint coordinates and checkpoint detection radii.
  - **Form Validation**: Client-side validation ensuring unique codes/IDs, non-empty names, valid coordinate bounds ($[-90,90]$, $[-180,180]$), and non-negative tariffs.
- Implement business integrity rules:
  - **Historical Immutability**: Ensure policy updates apply strictly to future encounters; past transactions in both active and historical runs remain permanently immutable.
  - **Reset Invariance & Multi-Run History**: Ensure simulation reset closes the active run, archives previous transactions into in-memory session history, zeroes active revenue, and strictly preserves all user-configured countries, vessels, routes, checkpoints, and toll policies.
- Implement search and multi-attribute filtering in `src/main/webapp/js/dashboard.js`:
  - Search by vessel name or IMO number.
  - Filters for Nationality, Cargo Category, and Simulation Status.
  - Filters operate without altering background simulation progression.
- Implement Transaction History Ledger:
  - Chronological tabular ledger of all events (`CHARGED`, `EXEMPT`, `DUPLICATE`).
  - Column sorting, search, and detail receipt modal inspection on row click.
- Implement Revenue Analytics and Reporting in `src/main/webapp/js/reports.js`:
  - Breakdown by Authority (Iran, Oman, Total).
  - Breakdown by Country and Cargo category.
  - Client-side RFC 4180 CSV export for both the transaction audit log and aggregated revenue summaries via native `Blob` and download triggers.
- Implement Traffic Congestion Alerts (`alert-system.js` with configurable waiting count threshold and proximity radius; illustrative defaults) and Map Layer Toggles (`layer-control.js`).

### Affected Files / Components
- `src/main/webapp/js/management.js`
- `src/main/webapp/js/reports.js`
- `src/main/webapp/js/alert-system.js`
- `src/main/webapp/js/layer-control.js`
- `src/main/webapp/js/dashboard.js`
- `src/main/webapp/js/app.js`
- `src/main/webapp/index.html`
- `src/main/webapp/css/styles.css`

### Dependencies
- Depends on Phase 9.

### Acceptance Criteria
- [ ] User can add/edit a country with custom ISO code, SVG flag, and default tariff.
- [ ] User can edit toll rates and toggle exemptions, with changes applying strictly to subsequent transits.
- [ ] User can dynamically add/edit vessels and assign routes with validated inputs.
- [ ] User can adjust checkpoint coordinates and detection radii.
- [ ] Search and multi-attribute filters dynamically filter manifests and map markers without disturbing simulation state.
- [ ] Transaction ledger logs every toll outcome and provides complete audit history.
- [ ] Clicking "Export CSV" generates compliant RFC 4180 CSV downloads directly from the browser.
- [ ] Simulation reset preserves all custom configuration and archives past transactions in-session while resetting active run positions and revenue.
- [ ] Congestion alerts indicate bottleneck density based on configurable parameters (illustrative defaults) and layer toggles show/hide map overlays.

### Verification Steps
1. Add a new country "Singapore" (SGP) and verify it appears in nationality selectors.
2. Update Iran toll rate for India; verify only subsequent crossings reflect the new rate and previous transactions are unaltered.
3. Add a new vessel; verify it animates along its assigned route.
4. Execute search and multi-attribute filters; verify correct subset is shown and clear button restores view.
5. Trigger CSV export; verify downloaded file opens correctly and conforms to RFC 4180.
6. Trigger simulation reset; verify custom entities persist, prior run transactions remain accessible in session history, and active run counters zero out.

### Risks & Compatibility Concerns
- *Risk*: Client-side state mutations causing memory leaks or orphaned map markers.
- *Mitigation*: Centralize state management within coordinated registries and ensure marker cleanup routines are executed.

### Phase Completion Condition
Phase 10 is complete when dynamic management CRUD, search/filter, transaction ledger, CSV export, congestion alerts, and layer toggles are fully verified.

---

## 13. Phase 11: Testing & Regression

### Objective
Perform end-to-end verification, automated/manual test execution, and regression testing across the entire application stack.

### Prerequisites
- Phases 1 through 10 completed.

### Planned Work
- Execute full test matrix covering:
  - Clean Maven build and GlassFish deployment.
  - Google Maps loading, pan/zoom responsiveness, and offline/missing key handling.
  - Custom ship model rendering, mast positioning, and SVG flag legibility.
  - Heading rotation accuracy and upright flag counter-rotation.
  - Simulation lifecycle (Start, Pause, Resume, Reset, Speed multipliers).
  - Authoritative toll calculations, cargo adjustments, and exemptions.
  - Idempotent duplicate prevention (repeated events rejected).
  - Revenue accumulation accuracy: $\text{Total} = \text{Iran} + \text{Oman}$.
  - Vessel search and multi-criteria filtering.
  - State reconciliation after page refresh and clean reset.
- Verify that no console errors, memory leaks, or unhandled exceptions occur during extended simulation runs.
- Document test outcomes in `TESTING_PLAN.md` (when created).

### Affected Files / Components
- Entire application stack.

### Dependencies
- Depends on Phases 1 through 10.

### Acceptance Criteria
- [ ] 100% of defined acceptance criteria across Phases 1–10 pass without blocking bugs.
- [ ] Continuous simulation runs for 15+ minutes without memory degradation or marker drift.
- [ ] Zero unhandled JavaScript exceptions in browser console.
- [ ] Zero unhandled Java exceptions in GlassFish `server.log`.
- [ ] Regression checks confirm that existing features remain fully operational.

### Verification Steps
1. Execute full end-to-end simulation cycle.
2. Inspect browser DevTools Console and Network tabs.
3. Inspect GlassFish `server.log`.

### Risks & Compatibility Concerns
- *Risk*: Subtle duplicate toll counting on fast animation speeds.
- *Mitigation*: Rigorously test duplicate prevention under 4x simulation speed.

### Phase Completion Condition
Phase 11 is complete when all test suites pass and results are verified and recorded.

---

## 14. Phase 12: Practical Demonstration Readiness

### Objective
Prepare a seamless, repeatable college practical examination demonstration workflow, complete with documentation, quick-start instructions, and live question readiness.

### Prerequisites
- Phase 11 completed.

### Planned Work
- Create [docs/DEMO_GUIDE.md](DEMO_GUIDE.md) providing step-by-step presentation scripts for the student.
- Create [README.md](README.md) with complete setup, build, GlassFish deployment, and API key configuration instructions.
- Rehearse the standard demonstration narrative:
  1. Open dashboard; highlight Strait of Hormuz geography and simulation disclaimer.
  2. Explain custom ship models with masts and national flags.
  3. Start simulation; observe inbound and outbound shipping lanes.
  4. Click a ship to inspect its cargo manifest and destination.
  5. Demonstrate toll assessment at Iran Northern Checkpoint (revenue increment).
  6. Demonstrate friendly nation exemption (China/Oman flags yielding $0 fee).
  7. Demonstrate duplicate prevention (ship remaining in zone without double-billing).
  8. Demonstrate Oman Checkpoint toll collection and verify combined revenue.
  9. Demonstrate search, nationality filters, and simulation pause/reset.
- Rehearse rapid responses to potential professor modifications (adding a country, editing a tariff rate in `simulation-config.json`).
- Verify that all disclaimers regarding simulated data are prominently visible.

### Affected Files / Components
- `docs/DEMO_GUIDE.md`
- `README.md`

### Dependencies
- Depends on Phase 11.

### Acceptance Criteria
- [ ] Demonstration workflow runs end-to-end in under 5 minutes without technical hitches.
- [ ] Student can clearly explain backend vs frontend component boundaries.
- [ ] The system can absorb a live configuration change (e.g., adding a country) within 60 seconds.
- [ ] All documentation files are synchronized, clean, and complete.

### Verification Steps
1. Conduct mock practical examination dry-run from cold server start to final reset.
2. Verify live configuration change test.

### Risks & Compatibility Concerns
- *Risk*: Student nervousness during live professor interrogation.
- *Mitigation*: Clear, step-by-step demonstration script in `DEMO_GUIDE.md`.

### Phase Completion Condition
Phase 12 is complete when the practical demonstration script is validated and project sign-off is achieved.

---

## 15. Change Management Protocol

If the professor introduces an unexpected requirement or modification during practical preparation or the live exam:

1. **Log Instruction**: Record verbatim in [docs/PROFESSOR_CHANGELOG.md](PROFESSOR_CHANGELOG.md) under a new `CHG-XXX` identifier.
2. **Impact Assessment**: Classify change level (Configuration Only vs Feature Extension vs Architecture Change).
3. **Compatibility Check**: Verify compatibility with Java 8, Java EE 7, and GlassFish 4.1.1.
4. **Approval**: Obtain user sign-off.
5. **Implement Surgically**: Make targeted edits (preferring `simulation-config.json` over code changes).
6. **Regression Verification**: Verify that the core simulation, map, and financial ledger remain 100% operational.
7. **Sign Off**: Update `PROFESSOR_CHANGELOG.md` status to `COMPLETED`.

---

## 16. Phased Progress Tracking Table

| Phase | Description | Status | Dependencies | Verification Method | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Phase 0** | Documentation & Scope Review | **UNDER REVIEW** | None | Document review | Planning baseline established |
| **Phase 1** | Environment & Build Baseline | **NOT STARTED** | Phase 0 | Maven package & GlassFish deploy | Compiler 1.8 verification required |
| **Phase 2** | Application Foundation & Design | **NOT STARTED** | Phase 1 | Browser visual layout check | CSS tokens & asymmetric layout |
| **Phase 3** | Google Maps Integration | **NOT STARTED** | Phase 2 | Map render & pan/zoom test | Strait of Hormuz coordinates |
| **Phase 4** | Configuration & Vessel Data | **NOT STARTED** | Phase 1 | JSON parsing & SVG assets check | `simulation-config.json` |
| **Phase 5** | Custom Ship Rendering & Flags | **NOT STARTED** | Phase 3, 4 | Marker rotation & zoom check | SVG hulls, masts, national flags |
| **Phase 6** | Ship Movement Simulation | **NOT STARTED** | Phase 3, 5 | Animation & control loop test | TSS routes, Start/Pause/Reset |
| **Phase 7** | Java Backend & API Contracts | **NOT STARTED** | Phase 1, 4 | HTTP JSON endpoint queries | Java EE 7 Servlets & JSON-P |
| **Phase 8** | Toll Engine & Transaction Safety| **NOT STARTED** | Phase 7 | Idempotency & toll math tests | Server-authoritative tolling |
| **Phase 9** | Dashboard & Vessel Inspection | **NOT STARTED** | Phase 6, 7, 8 | Live UI counter & receipt check | Real-time state reconciliation |
| **Phase 10**| Management, Search & CSV Reports| **NOT STARTED** | Phase 9 | CRUD, filter & CSV export check | Dynamic config, ledger & export |
| **Phase 11**| Testing & Regression | **NOT STARTED** | Phase 1–10 | Full test suite execution | End-to-end stability check |
| **Phase 12**| Practical Demo Readiness | **NOT STARTED** | Phase 11 | Mock examination dry-run | DEMO_GUIDE & README complete |

*Rule: All implementation phases (Phase 1 through Phase 12) are strictly NOT STARTED. No phase shall be advanced without verification evidence and user approval.*

---

## 17. Implementation Rules for AI Coding Agent

The AI coding agent must strictly adhere to these operating rules:

1. **Read Before Writing**: Inspect the relevant documentation (`brain.md`, `PRD.md`, `TRD.md`, `UI_UX_GUIDELINES.md`, `DECISIONS.md`) before editing any file in any phase.
2. **Single Phase Execution**: Work exclusively on the currently approved phase. Never execute future phases ahead of time.
3. **No Unapproved Frameworks**: Never introduce React, Vite, Spring Boot, or npm dependencies.
4. **Preserve Working Code**: Avoid refactoring working components unless directly required by the active phase.
5. **No Silent Changes**: If a technical obstacle or contradiction arises, halt and present the decision for approval.
6. **Mandatory Build Verification**: Run Maven builds or browser checks to confirm changes compile and run without errors.
7. **Transparent Failure Reporting**: Report actual build and runtime errors immediately rather than pretending an unverified task succeeded.

---

## 18. Current Project Status

- **Planning Documentation Completed**:
  - `docs/brain.md` (Project context, constraints, and working rules)
  - `docs/PRD.md` (Product requirements and acceptance criteria)
  - `docs/TRD.md` (Technical architecture, API contracts, and models)
  - `docs/UI_UX_GUIDELINES.md` (Visual design, maritime tokens, and ship/flag specifications)
  - `docs/DECISIONS.md` (Confirmed, proposed, and rejected decision register)
  - `docs/PROFESSOR_CHANGELOG.md` (Change tracking protocol for examination requests)
  - `docs/IMPLEMENTATION_PLAN.md` (This phased roadmap)
- **Application Code Status**:
  - Unmodified starter Maven WebApp structure.
  - Zero Java, HTML, CSS, or JS application features implemented yet.
  - All implementation phases marked **NOT STARTED**.
- **Document Status**: **Draft for User Review**.
