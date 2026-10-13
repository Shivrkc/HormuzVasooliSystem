# Hormuz Vasooli System — Testing & Quality Assurance Plan

Version: 1.1 (Audited & Categorized)  
Status: Draft for Review  
Target Environment: NetBeans IDE 8.2 | JDK 8 (1.8.0_181) | Maven | Java EE 7 Web | GlassFish Server 4.1.1  
Frontend Stack: HTML5 | CSS3 | Vanilla JavaScript (ES6) | Google Maps JavaScript API  

---

## 1. Testing Principles & Quality Policy

This testing plan defines the verification strategy for the **Hormuz Vasooli System (HVS)**. The system combines client-side spatial simulation and navigation visualization with an authoritative server-side Java EE transactional ledger.

### 1.1 Core Verification Principles
1. **Evidence-Based Reporting**: No test shall be marked `PASS` merely because code or markup has been generated. A test is passed only when executed in the target environment and verified against its explicit acceptance criteria.
2. **Strict Stack Verification**: Tests must verify compatibility with **Oracle JDK 8 (`1.8.0_181`)**, **Java EE 7 Web (`javax.*` namespace)**, and **GlassFish Server 4.1.1**. Modern external testing frameworks that require newer JDKs or Node-based pipelines must not be introduced.
3. **Financial Invariance & Authority**:
   $$\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$$
   Financial calculations must be proven authoritative on the server. The client browser cannot independently settle or report revenue without server confirmation.
4. **Idempotency & Duplicate Rejection**: Repeated animation callbacks, proximity loops, and duplicate HTTP requests must be rigorously tested to prove that one vessel transit through a checkpoint yields exactly one financial transaction.
5. **Initial Baseline Status**: Because application implementation has not yet started, all test cases in this document are initially marked **`NOT RUN`**.
6. **No Unapproved Assumptions**: Fictional demonstration data (e.g., sample ships, specific dollar fees, Chinese partner exemptions, domestic flag exemptions, cargo multipliers, specific GPS coordinates) are illustrative examples for testing. They must not be misconstrued as mandatory geopolitical policies or immutable requirements.

---

## 2. Test Case Classification Schema & Status Definitions

To prevent hypothetical examples, proposed architectural patterns, or optional extensions from being treated as mandatory implementation requirements, all test cases are classified into three distinct categories:

### 2.1 Test Categories
- **`[A] Mandatory Baseline Tests`**: Core functional requirements approved in `brain.md`, `PRD.md`, and `DECISIONS.md` (DEC-026, DEC-027). This includes Google Maps integration, custom ship models with masts and flags, vessel animation, country-based toll rules, exemptions, duplicate-charge prevention, separate Iran/Oman/Total revenues, start/pause/resume/reset controls, simulation disclaimers, Java 8/GlassFish deployment, **plus practical examination readiness features**: Country/Policy/Vessel/Route/Checkpoint management CRUD, multi-attribute search and filtering, interactive transaction ledger, client-side RFC 4180 CSV export, speed selection (configurable demonstration presets: 1x, 2x, 4x), congestion warning alerts, layer visibility toggles, and reset configuration preservation. **These tests are required for MVP completion.**
- **`[B] Conditional on Approved Technical Decisions`**: Tests that evaluate architectural proposals documented in `TRD.md` and `DECISIONS.md` that await explicit approval (e.g., compiler 1.8 update under DEC-015, REST Servlet contracts under DEC-013, server-authoritative evaluation under DEC-010, in-memory state under DEC-011, `simulation-config.json` under DEC-012, proposed UI color/layout tokens under DEC-016 to DEC-019). **These tests become active only when the corresponding decision is confirmed.**
- **`[C] Optional Extension & Illustrative Tests`**: Tests for auxiliary enhancements, optional test fixtures, or future enhancements (e.g., cargo-based toll multipliers, domestic flag exemptions, specific Chinese exemption examples, camera centering on ship). **Failure or omission of Category C tests does not block MVP baseline completion.**

### 2.2 Test Case Fields
Every test case follows a standardized, auditable schema:
- **Test ID**: Unique identifier (`TC-[CATEGORY]-[NUMBER]`).
- **Category**: `[A] Mandatory`, `[B] Conditional`, or `[C] Optional/Illustrative`.
- **Feature / Module**: Target component or subsystem.
- **Test Objective**: Clear statement of the verified behavior.
- **Preconditions**: Required system state before step execution.
- **Test Steps**: Concrete sequential actions.
- **Expected Result**: Verifiable, observable criteria for success.
- **Actual Result**: Recorded output during test execution (initially unexecuted).
- **Status**: Lifecycle status (`NOT RUN`, `PASS`, `FAIL`, `BLOCKED`, `NEEDS REVIEW`).
- **Notes / Defect Reference**: Cross-references to decisions or assumptions.

---

## 3. Environment & Build Baseline Tests (`TC-ENV`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-ENV-01** | **[A]** | JDK Environment | Verify installed JDK is Java 8 (`1.8.0_181`). | NetBeans 8.2 installed. | Run `java -version` in terminal. | Output confirms `java version "1.8.0_181"`. | Not executed | **NOT RUN** | Core stack baseline |
| **TC-ENV-02** | **[B]** | Maven Compiler | Verify compiler source and target level configuration. | `pom.xml` inspected. | Verify `<source>1.8</source>` and `<target>1.8</target>` in compiler plugin. | Project builds without Java 8 compiler warnings or bytecode version mismatches. | Not executed | **NOT RUN** | Conditional on DEC-015 approval |
| **TC-ENV-03** | **[A]** | Packaging | Verify Maven packaging produces a valid WAR file. | JDK 8 and Maven configured. | Execute `mvn clean package`. | Build succeeds (`BUILD SUCCESS`); `target/HormuzVasooliSystem-1.0-SNAPSHOT.war` exists. | Not executed | **NOT RUN** | Endorsed API check |
| **TC-ENV-04** | **[A]** | GlassFish Deploy | Verify deployment to GlassFish 4.1.1 without classloader conflicts. | GlassFish 4.1.1 running locally. | Deploy WAR via NetBeans or `asadmin deploy`. | Deployment succeeds; application starts cleanly; `server.log` has zero fatal exceptions. | Not executed | **NOT RUN** | Target: GlassFish 4.1.1 |
| **TC-ENV-05** | **[A]** | Context URL | Verify application accessibility at local context root. | WAR deployed on GlassFish. | Open `http://localhost:8080/HormuzVasooliSystem/` in browser. | HTTP 200 returned; starter HTML page renders without 404 or 500 error. | Not executed | **NOT RUN** | Local endpoint |
| **TC-ENV-06** | **[A]** | Hot Redeploy | Verify application redeployment preserves clean state. | Running GlassFish instance. | Redeploy application using NetBeans Run/Deploy. | Context redeploys without orphaned threads, port conflicts, or memory leak warnings. | Not executed | **NOT RUN** | Stability check |

---

## 4. Google Maps Integration Tests (`TC-MAP`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-MAP-01** | **[A]** | Maps API | Verify Google Maps initializes with valid API key. | Valid Google Maps API key configured. | Load application in browser. | Interactive map tiles load; Google Maps controls are functional. | Not executed | **NOT RUN** | Core map requirement |
| **TC-MAP-02** | **[A]** | Center & Bounds | Verify initial viewport focuses on Strait of Hormuz. | Map initialized. | Inspect initial map center and zoom level. | Map centers on Strait of Hormuz region; Iran and Oman coasts clearly visible. | Not executed | **NOT RUN** | Coordinates are configurable |
| **TC-MAP-03** | **[A]** | Pan / Zoom | Verify smooth pan, drag, and zoom navigation. | Map initialized. | Drag map across Strait area; zoom between levels 8 and 12. | Map responds fluidly; tiles refresh without blank grey patches. | Not executed | **NOT RUN** | Interaction check |
| **TC-MAP-04** | **[A]** | Missing Key | Verify graceful fallback when API key is missing. | `API_KEY` set to placeholder. | Load application in browser. | Non-blocking banner alerts user; API key entry prompt/modal accessible. | Not executed | **NOT RUN** | Graceful degradation |
| **TC-MAP-05** | **[A]** | Invalid Key | Verify error handling when Google rejects API key. | Invalid/expired key supplied. | Trigger `window.gm_authFailure`. | System displays clear guidance; dashboard metrics remain accessible; no uncaught crashes. | Not executed | **NOT RUN** | Security fallback |
| **TC-MAP-06** | **[B]** | Dark Styling | Verify dark maritime map styling applied. | Map loaded with custom styles. | Inspect ocean and landmass colors. | Dark palette applied per proposed visual styling; clutter minimized. | Not executed | **NOT RUN** | Conditional on DEC-018 approval |

---

## 5. Configuration & Vessel Data Tests (`TC-DATA`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-DATA-01** | **[B]** | JSON Schema | Verify `simulation-config.json` schema validity. | Config file in `src/main/resources`.| Parse file using JSON validator. | JSON conforms to configuration schema; zero syntax or type errors. | Not executed | **NOT RUN** | Conditional on DEC-012 approval |
| **TC-DATA-02** | **[A]** | Fleet Manifest | Verify multiple simulated vessels contain required fields. | Config loaded. | Inspect loaded vessels manifest. | Multiple vessels exist, each with unique ID, name, nationality, cargo description, and route. | Not executed | **NOT RUN** | 6 ships is sample size, not rigid limit |
| **TC-DATA-03** | **[A]** | Route Waypoints | Verify predefined navigation route waypoints. | Config loaded. | Inspect navigation route waypoints. | Sequential GPS coordinates follow realistic navigation channels through Strait. | Not executed | **NOT RUN** | Waypoints are configurable |
| **TC-DATA-04** | **[A]** | Checkpoint Config| Verify Iran and Oman hypothetical checkpoints defined. | Config loaded. | Inspect checkpoint configuration. | Checkpoints associated with Iran and Oman exist with coordinates and detection zones. | Not executed | **NOT RUN** | Educational simulation setup |
| **TC-DATA-05** | **[C]** | Tariff Matrix | Verify hypothetical toll rates and cargo multiplier schema. | Config loaded. | Inspect tariff structure. | Fictional country rates and cargo multipliers defined; rates not hard-coded in logic. | Not executed | **NOT RUN** | Cargo multipliers are optional |
| **TC-DATA-06** | **[A]** | Disclaimer Text | Verify mandatory educational disclaimer presence. | Config loaded. | Inspect simulation metadata. | Explicit disclaimer stating fictional demonstration nature is present. | Not executed | **NOT RUN** | Simulation boundary requirement |

---

## 6. Custom Ship Model & Flag Visual Tests (`TC-VIS`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-VIS-01** | **[A]** | Top-Down Hull | Verify ships render as recognizable top-down models. | Ship markers added to map. | Visually inspect vessel markers. | Markers display distinct ship hull silhouettes; not ordinary arrows alone. | Not executed | **NOT RUN** | Requirement 4.6 |
| **TC-VIS-02** | **[A]** | Mast Pole | Verify small vertical mast rendered on each ship. | Ship markers visible. | Inspect superstructure center. | Visible small vertical mast/pole extends upward from deck center. | Not executed | **NOT RUN** | Requirement 4.6 |
| **TC-VIS-03** | **[A]** | Flag Mapping | Verify national flag corresponds to configured nationality. | Ship markers visible. | Inspect flags on vessels. | Local SVG/image flag corresponds to vessel's configured nationality; no Unicode flag emojis. | Not executed | **NOT RUN** | SVG flag specification |
| **TC-VIS-04** | **[A]** | Heading Rotation| Verify ship hull rotates to match navigation heading. | Simulation running. | Observe ship as it turns waypoints. | Hull smoothly rotates to forward azimuth ($\theta$); bow points along transit direction. | Not executed | **NOT RUN** | Heading calculation |
| **TC-VIS-05** | **[A]** | Upright Flag | Verify flag remains horizontal and readable during turns. | Ship turning along curve. | Inspect flag badge orientation. | Counter-rotation ($-\theta$) or independent layer keeps flag horizontal and legible. | Not executed | **NOT RUN** | Readability contract |
| **TC-VIS-06** | **[A]** | Coordinated Anchor| Verify hull, mast, and flag move as a single rigid unit. | Ship moving. | Observe marker movement at 60fps. | Hull, mast, and flag stay locked to same geographic coordinate; no visual separation. | Not executed | **NOT RUN** | Coordinated component |
| **TC-VIS-07** | **[B]** | Zoom Scaling | Verify bounded marker scaling across zoom levels 8 to 12. | Map zoomed in/out. | Zoom from 8 to 12. | Flag scales within bounds without obscuring the ship model. | Not executed | **NOT RUN** | Conditional on DEC-021 approval |
| **TC-VIS-08** | **[A]** | Fallback Ensign | Verify graceful fallback for unknown/missing flag. | Ship with missing flag asset. | Render vessel on map. | Neutral nautical ensign/fallback renders; zero console errors or broken icons. | Not executed | **NOT RUN** | Resilience check |
| **TC-VIS-09** | **[A]** | Selection State | Verify visual feedback upon selecting a ship marker. | Ship marker clicked. | Click vessel on map. | Vessel receives clear visual selection feedback; detail view opens. | Not executed | **NOT RUN** | User interaction |

---

## 7. Ship Movement Simulation Tests (`TC-SIM`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-SIM-01** | **[A]** | Start Control | Verify Start button initiates vessel progression. | Simulation idle at startup. | Click "Start" button. | Animation loop begins; vessels progress along waypoints; status changes to Running. | Not executed | **NOT RUN** | Core control requirement |
| **TC-SIM-02** | **[A]** | Pause Control | Verify Pause button freezes vessel progression. | Simulation running. | Click "Pause" button. | Vessels freeze at current coordinates; animation stops; status displays Paused. | Not executed | **NOT RUN** | Core control requirement |
| **TC-SIM-03** | **[A]** | Resume Control| Verify Resume continues progression from paused position. | Simulation paused. | Click "Resume" button. | Vessels resume movement from exact paused coordinates without teleporting. | Not executed | **NOT RUN** | Core control requirement |
| **TC-SIM-04** | **[A]** | Reset Control | Verify Reset closes active run, restores initial vessel positions, and initializes fresh run. | Simulation mid-transit. | Click "Reset" button. | All ships return to starting positions; active run initializes with clean state. | Not executed | **NOT RUN** | Multi-run reset requirement |
| **TC-SIM-05** | **[A]** | Speed Scaling | Verify simulation speed adjustment (demonstration presets: 1x, 2x, 4x). | Simulation running. | Toggle speed controls. | Vessel movement scales proportionally without position warping or dropped ticks. | Not executed | **NOT RUN** | Speed scaling requirement |
| **TC-SIM-06** | **[A]** | Waypoint Interp | Verify linear interpolation between sequential waypoints. | Simulation running. | Trace vessel path. | Movement follows predefined route segments smoothly. | Not executed | **NOT RUN** | Navigation model |
| **TC-SIM-07** | **[A]** | Status Lifecycle| Verify vessel status transitions (Waiting/Passing/Passed). | Ship transiting channel. | Track status during transit. | Status updates appropriately from waiting, to passing, to passed. | Not executed | **NOT RUN** | Core status tracking |
| **TC-SIM-08** | **[A]** | Proximity Latch | Verify checkpoint entry triggers crossing event once. | Ship enters checkpoint zone.| Monitor crossing triggers. | Entering zone triggers event once per passage; does not spam requests every frame. | Not executed | **NOT RUN** | Debounce/latch |

---

## 8. Toll Engine & Policy Evaluation Tests (`TC-TOLL`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-TOLL-01** | **[A]** | Iran Checkpoint | Verify non-exempt vessel assessed at Iran checkpoint. | Vessel with non-exempt nationality.| Simulate crossing at Iran checkpoint. | Configured hypothetical toll calculated; Status: `CHARGED`; Iran revenue updates. | Not executed | **NOT RUN** | Dollar amounts are sample data |
| **TC-TOLL-02** | **[A]** | Oman Checkpoint | Verify non-exempt vessel assessed at Oman checkpoint. | Vessel with non-exempt nationality.| Simulate crossing at Oman checkpoint. | Configured hypothetical toll calculated; Status: `CHARGED`; Oman revenue updates. | Not executed | **NOT RUN** | Dollar amounts are sample data |
| **TC-TOLL-03** | **[A]** | Exemption Rule | Verify configured exempt nationality receives zero toll. | Vessel with configured exempt flag.| Simulate crossing at designated checkpoint. | Fee: `$0.00 USD`; Status: `EXEMPT`; Authority revenue does not increase. | Not executed | **NOT RUN** | China is an illustrative sample flag |
| **TC-TOLL-04** | **[C]** | Domestic Flag | Verify domestic national flag exemption. | Vessel matching authority nation. | Simulate crossing at authority checkpoint. | Fee: `$0.00 USD`; Status: `EXEMPT`; Authority revenue does not increase. | Not executed | **NOT RUN** | Optional exemption rule |
| **TC-TOLL-05** | **[C]** | Cargo Multiplier| Verify cargo hazard multiplier formula applied. | Vessel with configured cargo adjustment.| Evaluate toll assessment. | Charge adjusted by configured cargo multiplier; itemized in receipt. | Not executed | **NOT RUN** | Cargo multiplier is optional |
| **TC-TOLL-06** | **[B]** | Invalid Ship ID | Verify rejection of unknown vessel ID. | Unknown vessel identifier. | Submit toll assessment request. | System rejects request (HTTP 404); revenue unchanged. | Not executed | **NOT RUN** | Conditional on DEC-010/013 |
| **TC-TOLL-07** | **[B]** | Invalid CP ID | Verify rejection of unknown checkpoint ID. | Unknown checkpoint identifier. | Submit toll assessment request. | System rejects request (HTTP 404); revenue unchanged. | Not executed | **NOT RUN** | Conditional on DEC-010/013 |
| **TC-TOLL-08** | **[A]** | Duplicate Reject | Verify duplicate encounter during same passage is blocked. | Same checkpoint encounter repeated. | Trigger second crossing in same passage. | Duplicate event prevented; fee is not charged again; revenue unchanged. | Not executed | **NOT RUN** | Core duplicate prevention |
| **TC-TOLL-09** | **[A]** | Dual CP Passage | Verify ship can be assessed at both checkpoints sequentially. | Vessel route intersects both zones.| Cross Iran checkpoint then Oman checkpoint.| Both checkpoints process their own valid transactions independently. | Not executed | **NOT RUN** | Separate authorities |

---

## 9. Transaction & Revenue Accounting Tests (`TC-TX`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-TX-01** | **[A]** | Total Invariance| Verify Total Revenue strictly equals Iran + Oman. | Multiple charges recorded. | Inspect revenue totals. | $\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$ holds true at all times. | Not executed | **NOT RUN** | Core mathematical invariant |
| **TC-TX-02** | **[A]** | Transaction Record| Verify completed toll transactions recorded. | Valid transaction recorded. | Inspect transaction record. | Contains transaction ID, vessel identity, nationality, authority, amount, timestamp. | Not executed | **NOT RUN** | Audit compliance |
| **TC-TX-03** | **[A]** | Currency Format | Verify all displayed financial values use standard USD. | Financial displays rendered. | Inspect revenue numbers. | Formatted in consistent US Dollar currency presentation (`$XX,XXX USD`). | Not executed | **NOT RUN** | Display check |
| **TC-TX-04** | **[B]** | State Reconcile| Verify browser reload reconciles with server ledger. | Server has recorded revenue. | Reload browser window (`F5`). | Frontend reconciles with server state; does not falsely show zero revenue. | Not executed | **NOT RUN** | Conditional on DEC-011 |
| **TC-TX-05** | **[A]** | Reset Financials| Verify reset zeroes active run revenue while archiving past run in session. | Active run with revenue. | Trigger simulation reset. | Active run revenues initialize to `$0.00`; prior run transactions remain in session history. | Not executed | **NOT RUN** | Multi-run reset requirement |
| **TC-TX-06** | **[B]** | Concurrency Safety| Verify concurrent events do not corrupt revenue tally. | Multiple simultaneous hits. | Submit multiple events concurrently. | All processed atomically; revenue sum matches all valid fees accurately. | Not executed | **NOT RUN** | Conditional on DEC-011/014 |

---

## 10. Backend API Endpoint Tests (`TC-API`)

*Note: All tests in this section belong to **Category [B]** and are conditional on explicit user approval of proposed technical decisions DEC-010 (Server-Authoritative Tolling) and DEC-013 (REST-Like Servlets).*

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-API-01** | **[B]** | `GET /config` | Verify static configuration retrieval. | Server running. | Send `GET /api/simulation/config`.| HTTP 200; `application/json`; returns checkpoints, routes, ships, policies, disclaimer. | Not executed | **NOT RUN** | Conditional on DEC-013 |
| **TC-API-02** | **[B]** | `GET /state` | Verify authoritative state retrieval. | Server running. | Send `GET /api/simulation/state`. | HTTP 200; `application/json`; returns active `runId`, revenues, transaction count. | Not executed | **NOT RUN** | Conditional on DEC-013 |
| **TC-API-03** | **[B]** | `POST /toll` (Valid)| Verify successful toll assessment. | Valid event payload. | Send `POST /api/simulation/toll`. | HTTP 200; returns outcome `"SUCCESS"`, transaction receipt, and updated financials. | Not executed | **NOT RUN** | Conditional on DEC-010/013 |
| **TC-API-04** | **[B]** | `POST /toll` (Bad JSON)| Verify error response on malformed JSON payload. | Malformed body. | Send invalid JSON string to `/toll`. | HTTP 400 Bad Request; returns structured error JSON; server does not crash. | Not executed | **NOT RUN** | Conditional on DEC-013 |
| **TC-API-05** | **[B]** | `POST /toll` (Missing)| Verify rejection of missing required parameters. | Missing required field. | Send incomplete body to `/toll`. | HTTP 400 Bad Request; error message specifies missing required field. | Not executed | **NOT RUN** | Conditional on DEC-013 |
| **TC-API-06** | **[B]** | `POST /reset` | Verify simulation session reset endpoint. | Active simulation. | Send `POST /api/simulation/reset`.| HTTP 200; outcome `"RESET_SUCCESSFUL"`; returns fresh `newRunId` and zero revenues. | Not executed | **NOT RUN** | Conditional on DEC-013 |
| **TC-API-07** | **[B]** | Content-Type | Verify strict UTF-8 JSON response headers. | Any API call. | Inspect response headers. | `Content-Type: application/json; charset=UTF-8` present on all servlet responses. | Not executed | **NOT RUN** | Conditional on DEC-013 |

---

## 11. Search, Filters, and Supporting Views Tests (`TC-UI`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-UI-01** | **[A]** | Vessel Search | Verify real-time search by vessel name or ID. | Fleet manifest loaded. | Type vessel name into search box. | Manifest and map filter to matching vessel; non-matching hidden. | Not executed | **NOT RUN** | Core search requirement (DEC-026) |
| **TC-UI-02** | **[A]** | Nation Filter | Verify dropdown filtering by nationality. | Fleet manifest loaded. | Select nationality in filter. | Only vessels of selected nation display; background simulation continues. | Not executed | **NOT RUN** | Core filter requirement (DEC-026) |
| **TC-UI-03** | **[A]** | Cargo Filter | Verify filtering by cargo category. | Fleet manifest loaded. | Select cargo category in filter. | Only vessels carrying selected cargo remain visible. | Not executed | **NOT RUN** | Core filter requirement (DEC-026) |
| **TC-UI-04** | **[A]** | Status Filter | Verify filtering by vessel status. | Simulation in progress. | Select status (Waiting/Passing/Passed). | Only vessels in selected status are shown. | Not executed | **NOT RUN** | Core filter requirement (DEC-026) |
| **TC-UI-05** | **[A]** | Empty Search | Verify informative message when search matches zero ships. | Fleet manifest loaded. | Search for nonexistent ship. | UI displays "No vessels matching search criteria"; map remains functional. | Not executed | **NOT RUN** | UI resilience (DEC-026) |
| **TC-UI-06** | **[A]** | Inspection Drawer| Verify vessel detail inspection content accuracy. | Ship clicked. | Inspect opened detail view. | Displays vessel identity, nationality, cargo description, route, and status. | Not executed | **NOT RUN** | Core inspection requirement |
| **TC-UI-07** | **[C]** | Center on Ship | Verify "Center on Ship" camera focus control. | Detail view open. | Click "Center Map on Vessel". | Map smoothly pans to vessel's current GPS coordinate. | Not executed | **NOT RUN** | Optional UX enhancement |
| **TC-UI-08** | **[A]** | Ledger View | Verify Collection Ledger displays transaction history. | Transactions recorded. | Open Collection Ledger view. | Displays chronological receipts with timestamp, vessel, authority, fee, and status. | Not executed | **NOT RUN** | Core ledger requirement (DEC-026) |
| **TC-UI-09** | **[A]** | Policies View | Verify Toll Policies modal displays transparent rules. | Application running. | Open Toll Policies view. | Displays hypothetical country rates and exemption rules cleanly. | Not executed | **NOT RUN** | Core policy view (DEC-026) |

---

## 12. Management Interfaces, CSV Reporting & Traffic Alert Tests (`TC-MGMT` / `TC-REP`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-MGMT-01**| **[A]** | Country CRUD | Verify adding a new country dynamically via UI. | Country management open. | Enter ISO code "SGP", Name "Singapore", select flag, set base tariff; submit. | Country added; appears in vessel country dropdown; form rejects duplicate code. | Not executed | **NOT RUN** | Management requirement (DEC-026) |
| **TC-MGMT-02**| **[A]** | Policy Editing | Verify editing toll rate applies strictly to future transits. | Toll policy open. | Edit India Iran tariff; trigger crossing. | Future crossings billed at new rate; prior ledger records in active and past runs remain immutable. | Not executed | **NOT RUN** | Historical immutability (DEC-027) |
| **TC-MGMT-03**| **[A]** | Vessel CRUD | Verify adding and editing simulated vessels. | Vessel drawer open. | Add new vessel with name, IMO, nationality, route; submit. | Vessel appears on map at route origin and animates; form rejects empty name. | Not executed | **NOT RUN** | Management requirement (DEC-026) |
| **TC-MGMT-04**| **[A]** | Coords Editing | Verify modifying checkpoint and route coordinates. | Coordinate editor open. | Modify checkpoint radius and route waypoint coordinates; save. | Map overlay updates; detection triggers at new boundary; validates bounds. | Not executed | **NOT RUN** | Management requirement (DEC-026) |
| **TC-MGMT-05**| **[A]** | Reset Invariance| Verify simulation reset preserves custom configuration. | Custom country/vessel added.| Click "Reset Simulation". | Active run revenue zeroes, prior run records archive to session history, and custom entities survive. | Not executed | **NOT RUN** | Reset preservation (DEC-027) |
| **TC-REP-01** | **[A]** | Ledger CSV Export| Verify transaction audit log exports as RFC 4180 CSV. | Transactions in ledger. | Click "Export Transaction CSV". | Browser downloads `transactions-export.csv`; valid headers and RFC 4180 escaping. | Not executed | **NOT RUN** | CSV export requirement (DEC-026) |
| **TC-REP-02** | **[A]** | Summary CSV Export| Verify aggregated revenue summary exports as CSV. | Revenue accrued. | Click "Export Summary CSV". | Browser downloads `revenue-summary.csv` with authority/country/cargo breakdowns. | Not executed | **NOT RUN** | CSV export requirement (DEC-026) |
| **TC-ALERT-01**| **[A]** | Congestion Alerts| Verify corridor congestion warnings against configurable parameters.| Multiple ships in sector. | Move $\ge 3$ ships into checkpoint proximity zone. | Visual congestion warning badge updates from FLUID to MODERATE/CONGESTED based on configurable thresholds (academic defaults). | Not executed | **NOT RUN** | Alert requirement (DEC-026) |
| **TC-LAYER-01**| **[A]** | Layer Toggles | Verify map layer visibility controls. | Map rendered. | Toggle Routes, Checkpoints, and Labels pills. | Corresponding map overlays show/hide without resetting simulation. | Not executed | **NOT RUN** | Layer control requirement (DEC-026) |

---

## 13. UI/UX, Design Tokens & Responsive Tests (`TC-RESP`)

| Test ID | Category | Module | Objective | Preconditions | Steps | Expected Result | Actual Result | Status | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-RESP-01** | **[B]** | Layout Priority | Verify map workspace has visual priority over panels. | Desktop viewport. | Inspect layout proportions. | Map dominates workspace; information panels do not obstruct navigation channels. | Not executed | **NOT RUN** | Conditional on DEC-016 |
| **TC-RESP-02** | **[B]** | Glassmorphism | Verify translucent panels maintain readable contrast. | Panels over ocean tiles. | Inspect contrast of text on panels. | Text is sharp and readable; meets WCAG AA standards against map background. | Not executed | **NOT RUN** | Conditional on DEC-017 |
| **TC-RESP-03** | **[B]** | Color Hierarchy | Verify accent colors applied to checkpoints and actions. | UI loaded. | Inspect primary buttons and zones. | Restrained accents applied per proposed palette; distinct Iran/Oman authority colors. | Not executed | **NOT RUN** | Conditional on DEC-018 |
| **TC-RESP-04** | **[B]** | Monospace KPIs | Verify revenue and coordinates use monospace font. | Financial bar visible. | Inspect font-family of numbers. | Monospace font applied with tabular numerals; figures do not jitter during updates. | Not executed | **NOT RUN** | Conditional on DEC-019 |
| **TC-RESP-05** | **[C]** | Laptop View | Verify responsive layout adaptation at 1366x768. | Viewport set to 1366x768. | Inspect dashboard arrangement. | Panels adapt gracefully; no horizontal scrollbar; map controls remain accessible. | Not executed | **NOT RUN** | Optional enhancement |
| **TC-RESP-06** | **[C]** | Compact View | Verify layout adaptation on smaller screens. | Viewport set to 1024x768. | Inspect dashboard arrangement. | Secondary panels collapse or become drawers; map remains usable. | Not executed | **NOT RUN** | Optional enhancement |
| **TC-RESP-07** | **[A]** | Anti-Template | Verify absence of generic AI templates and purple neon. | Full UI rendered. | Audit visual aesthetics. | Zero purple neon glows; zero fake marketing stats; zero emoji in controls. | Not executed | **NOT RUN** | Core design constraint |

---

## 14. End-to-End Practical Examination Scenario (`TC-E2E-01`)

**Category**: **`[A] Mandatory Baseline Test`**

This comprehensive manual test scenario verifies the complete student demonstration workflow for the practical examination:

```
[Start App] ──► [Inspect Map] ──► [Select Ship] ──► [Start Simulation] ──► [Observe Movement]
      │
      ▼
[Iran Checkpoint Hit] ──► [Verify Toll Assessment] ──► [Verify Iran Revenue] ──► [Exempt Ship Pass]
      │
      ▼
[Oman Checkpoint Hit] ──► [Verify Oman Revenue] ──► [Verify Total = Iran + Oman]
      │
      ▼
[Duplicate Zone Test] ──► [Pause & Resume] ──► [Inspection Check] ──► [Clean Reset]
```

### Step-by-Step E2E Protocol

| Step | Action | Expected Observable Result | Verification Check |
| :---: | :--- | :--- | :--- |
| **1** | Open `http://localhost:8080/HormuzVasooliSystem/` | Application loads; header shows "Hormuz Vasooli System" and simulation disclaimer badge. | Header rendered; zero console errors. |
| **2** | Inspect Google Map | Map loads centered on Strait of Hormuz; Iran and Oman coasts clearly visible. | Interactive pan/zoom works. |
| **3** | Inspect Fleet Markers | Multiple simulated ships visible with top-down hulls, masts, and local SVG national flags. | Vector ship models visible; no generic arrows. |
| **4** | Click a simulated vessel | Detail view opens displaying configured name, IMO, nationality, cargo description, and route. | Details accurate; coordinates match. |
| **5** | Click "Start Simulation" | Ships begin smooth forward movement along predefined routes; hulls rotate along heading. | Simulation active; flags remain upright. |
| **6** | Observe non-exempt vessel reach Iran Checkpoint | Checkpoint zone indicates encounter; toll policy evaluated for vessel nationality. | Checkpoint crossing triggered. |
| **7** | Verify Iran Toll Assessment | Transaction recorded; valid hypothetical toll assessed for non-exempt nationality. | Receipt issued; status `CHARGED`. |
| **8** | Verify Financial Bar Update | Iran Revenue updates by assessed fee; Total Revenue increments by same amount. | Live counters update; USD format. |
| **9** | Observe exempt vessel pass Checkpoint | Vessel with configured exempt flag crosses checkpoint; fee assessed as `$0 USD`. | Status `EXEMPT`; fee `$0`; revenue unchanged. |
| **10**| Observe vessel reach Oman Checkpoint | Vessel crosses Oman checkpoint zone; toll policy evaluated under Oman authority. | Status `CHARGED`; fee added to Oman revenue. |
| **11**| Verify Combined Revenue Invariance | Check revenue counters: $\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$. | Mathematical consistency confirmed. |
| **12**| Test Duplicate Prevention | Vessel remains inside checkpoint radius; verify no second charge is assessed. | Zero duplicate toll charges; no double-billing. |
| **13**| Test Simulation Controls | Click "Pause" (ships freeze); Click "Resume" (ships continue movement). | Simulation lifecycle controls responsive. |
| **14**| Inspect another vessel | Click a different vessel on map; verify detail drawer reflects that specific ship. | Correct vessel details displayed. |
| **15**| Click "Reset Simulation" | Ships return to starting positions; revenues reset to `$0.00 USD`; ready for rerun. | State cleanly reconciled to initial baseline. |

- **Actual Result**: Not executed — awaiting Phase 11.
- **Overall E2E Status**: **`NOT RUN`**

---

## 15. Professor-Requested Change Regression Suites (`TC-REG`)

**Category**: **`[A] Mandatory Baseline Test`**

Whenever the professor requests a live modification during the examination (tracked in `PROFESSOR_CHANGELOG.md`), the corresponding regression test suite must be executed:

### Suite REG-01: Adding a New Country
- **Objective**: Verify adding a country configuration with a local SVG flag does not break existing ships.
- **Test Steps**:
  1. Add country and flag token to configuration.
  2. Add local SVG flag to `assets/flags/`.
  3. Assign a simulated vessel to the new country.
  4. Reload application and start simulation.
- **Verification**: New vessel renders new country flag; toll engine applies configured rate; existing nations continue operating normally.

### Suite REG-02: Modifying Toll Rates
- **Objective**: Verify editing a hypothetical base toll rate updates revenue without modifying Java code.
- **Test Steps**:
  1. Modify configured rate for a nation in configuration.
  2. Trigger checkpoint encounter.
- **Verification**: New fee calculated per modified rate; other country rates remain unchanged.

### Suite REG-03: Adding / Toggling an Exemption
- **Objective**: Verify toggling an exemption immediately eliminates toll collection for that nation.
- **Test Steps**:
  1. Set nation exemption flag to true.
  2. Simulate passage through designated checkpoint.
- **Verification**: Fee returns `$0.00 USD`; status recorded as `EXEMPT`; non-exempt countries continue paying positive fees.

### Suite REG-04: Relocating a Checkpoint
- **Objective**: Verify modifying checkpoint coordinates updates both map rendering and detection zone.
- **Test Steps**:
  1. Adjust checkpoint coordinates or radius in configuration.
  2. Reload application.
- **Verification**: Checkpoint moves to new coordinates on map; crossing detection triggers at new geographic boundary.

---

## 16. Defect Severity & Classification Matrix

| Severity Level | Definition | Examples | Impact on Release |
| :--- | :--- | :--- | :--- |
| **`CRITICAL`** | System fails to start, deploy, load the map, or crashes GlassFish. | Build fails; WAR deployment throws classloader error; Google Maps fails to initialize. | **Blocks demonstration completely.** Must be resolved before any other task. |
| **`HIGH`** | Core financial, simulation, or visualization rules fail. | Ships move in wrong direction; flags do not counter-rotate; duplicate toll charges occur; Total != Iran + Oman. | **Blocks sign-off.** Core practical requirements compromised. |
| **`MEDIUM`** | Supporting feature malfunction that does not corrupt core tolling. | Search filter fails on case sensitivity; vessel drawer does not center camera; ledger modal layout misalignment. | Fix in active phase before proceeding. |
| **`LOW`** | Cosmetic or minor non-blocking usability issue. | Tooltip typo; button hover animation delay; slightly misaligned badge padding on mobile. | Defer to polish phase. |

---

## 17. Test Execution & Reporting Summary Table

### 17.1 Categorized Test Master Table

| Test ID | Category | Feature / Objective | Status | Defect Reference | Notes |
| :--- | :---: | :--- | :--- | :--- | :--- |
| `TC-ENV-01` | **[A]** | JDK 8 availability (`1.8.0_181`) | **NOT RUN** | — | Mandatory baseline |
| `TC-ENV-02` | **[B]** | Maven compiler source/target 1.8 | **NOT RUN** | — | Conditional on DEC-015 |
| `TC-ENV-03` | **[A]** | WAR packaging (`mvn clean package`) | **NOT RUN** | — | Mandatory baseline |
| `TC-ENV-04` | **[A]** | GlassFish 4.1.1 clean deployment | **NOT RUN** | — | Mandatory baseline |
| `TC-ENV-05` | **[A]** | HTTP 200 at context root | **NOT RUN** | — | Mandatory baseline |
| `TC-ENV-06` | **[A]** | Clean hot redeployment | **NOT RUN** | — | Mandatory baseline |
| `TC-MAP-01` | **[A]** | Interactive map initialization | **NOT RUN** | — | Mandatory baseline |
| `TC-MAP-02` | **[A]** | Initial focus on Strait of Hormuz | **NOT RUN** | — | Mandatory baseline |
| `TC-MAP-03` | **[A]** | Smooth pan and zoom navigation | **NOT RUN** | — | Mandatory baseline |
| `TC-MAP-04` | **[A]** | Missing API key fallback banner | **NOT RUN** | — | Mandatory baseline |
| `TC-MAP-05` | **[A]** | Invalid API key error handling | **NOT RUN** | — | Mandatory baseline |
| `TC-MAP-06` | **[B]** | Custom dark maritime map styling | **NOT RUN** | — | Conditional on DEC-018 |
| `TC-DATA-01`| **[B]** | Structured config file schema | **NOT RUN** | — | Conditional on DEC-012 |
| `TC-DATA-02`| **[A]** | Multiple demo vessels manifest | **NOT RUN** | — | Mandatory baseline (sample size) |
| `TC-DATA-03`| **[A]** | Predefined navigation route waypoints | **NOT RUN** | — | Mandatory baseline (configurable) |
| `TC-DATA-04`| **[A]** | Iran and Oman checkpoint configs | **NOT RUN** | — | Mandatory baseline (configurable) |
| `TC-DATA-05`| **[C]** | Tariff matrix cargo adjustment schema | **NOT RUN** | — | Optional extension |
| `TC-DATA-06`| **[A]** | Educational simulation disclaimer | **NOT RUN** | — | Mandatory baseline |
| `TC-VIS-01` | **[A]** | Top-down SVG ship hulls | **NOT RUN** | — | Mandatory baseline |
| `TC-VIS-02` | **[A]** | Centered vertical mast pole | **NOT RUN** | — | Mandatory baseline |
| `TC-VIS-03` | **[A]** | National flag matching nationality | **NOT RUN** | — | Mandatory baseline (SVG, no emoji) |
| `TC-VIS-04` | **[A]** | Heading-aware hull rotation | **NOT RUN** | — | Mandatory baseline |
| `TC-VIS-05` | **[A]** | Upright flag counter-rotation | **NOT RUN** | — | Mandatory baseline |
| `TC-VIS-06` | **[A]** | Coordinated hull/mast/flag anchor | **NOT RUN** | — | Mandatory baseline |
| `TC-VIS-07` | **[B]** | Bounded zoom scaling (Zoom 8–12) | **NOT RUN** | — | Conditional on DEC-021 |
| `TC-VIS-08` | **[A]** | Fallback nautical ensign | **NOT RUN** | — | Mandatory baseline |
| `TC-VIS-09` | **[A]** | Selected vessel highlight state | **NOT RUN** | — | Mandatory baseline |
| `TC-SIM-01` | **[A]** | Start button launches movement | **NOT RUN** | — | Mandatory baseline |
| `TC-SIM-02` | **[A]** | Pause button freezes movement | **NOT RUN** | — | Mandatory baseline |
| `TC-SIM-03` | **[A]** | Resume continues from position | **NOT RUN** | — | Mandatory baseline |
| `TC-SIM-04` | **[A]** | Reset closes active run & restores coords | **NOT RUN** | — | Mandatory baseline |
| `TC-SIM-05` | **[A]** | Speed scaling (1x, 2x, 4x presets) | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-SIM-06` | **[A]** | Waypoint linear interpolation | **NOT RUN** | — | Mandatory baseline |
| `TC-SIM-07` | **[A]** | Status lifecycle (Waiting/Passing/Passed) | **NOT RUN** | — | Mandatory baseline |
| `TC-SIM-08` | **[A]** | Checkpoint proximity latch | **NOT RUN** | — | Mandatory baseline |
| `TC-TOLL-01`| **[A]** | Iran checkpoint toll calculation | **NOT RUN** | — | Mandatory baseline (sample rate) |
| `TC-TOLL-02`| **[A]** | Oman checkpoint toll calculation | **NOT RUN** | — | Mandatory baseline (sample rate) |
| `TC-TOLL-03`| **[A]** | Configured exemption ($0 fee) | **NOT RUN** | — | Mandatory baseline (sample flag) |
| `TC-TOLL-04`| **[C]** | Domestic flag exemption ($0 fee) | **NOT RUN** | — | Optional extension |
| `TC-TOLL-05`| **[C]** | Cargo hazard multiplier formula | **NOT RUN** | — | Optional extension |
| `TC-TOLL-06`| **[B]** | Unknown vessel ID rejection (404) | **NOT RUN** | — | Conditional on DEC-010/013 |
| `TC-TOLL-07`| **[B]** | Unknown checkpoint ID rejection (404)| **NOT RUN** | — | Conditional on DEC-010/013 |
| `TC-TOLL-08`| **[A]** | Duplicate charge prevention | **NOT RUN** | — | Mandatory baseline |
| `TC-TOLL-09`| **[A]** | Dual checkpoint passage eligibility | **NOT RUN** | — | Mandatory baseline |
| `TC-TX-01`  | **[A]** | Invariant: Total == Iran + Oman | **NOT RUN** | — | Mandatory baseline |
| `TC-TX-02`  | **[A]** | Transaction record completeness | **NOT RUN** | — | Mandatory baseline |
| `TC-TX-03`  | **[A]** | Standard USD currency formatting | **NOT RUN** | — | Mandatory baseline |
| `TC-TX-04`  | **[B]** | State reconciliation on page reload | **NOT RUN** | — | Conditional on DEC-011 |
| `TC-TX-05`  | **[A]** | Reset active financials & session archive | **NOT RUN** | — | Mandatory baseline (DEC-027) |
| `TC-TX-06`  | **[B]** | Concurrency safety under load | **NOT RUN** | — | Conditional on DEC-011/014 |
| `TC-API-01` | **[B]** | `GET /api/simulation/config` (200) | **NOT RUN** | — | Conditional on DEC-013 |
| `TC-API-02` | **[B]** | `GET /api/simulation/state` (200) | **NOT RUN** | — | Conditional on DEC-013 |
| `TC-API-03` | **[B]** | `POST /api/simulation/toll` (200) | **NOT RUN** | — | Conditional on DEC-010/013 |
| `TC-API-04` | **[B]** | Malformed JSON handling (400) | **NOT RUN** | — | Conditional on DEC-013 |
| `TC-API-05` | **[B]** | Missing parameters handling (400) | **NOT RUN** | — | Conditional on DEC-013 |
| `TC-API-06` | **[B]** | `POST /api/simulation/reset` (200) | **NOT RUN** | — | Conditional on DEC-013 |
| `TC-API-07` | **[B]** | Standard UTF-8 JSON headers | **NOT RUN** | — | Conditional on DEC-013 |
| `TC-UI-01`  | **[A]** | Search by vessel name or ID | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-UI-02`  | **[A]** | Nationality dropdown filter | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-UI-03`  | **[A]** | Cargo category dropdown filter | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-UI-04`  | **[A]** | Status dropdown filter | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-UI-05`  | **[A]** | Empty search result feedback | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-UI-06`  | **[A]** | Vessel detail inspection content | **NOT RUN** | — | Mandatory baseline |
| `TC-UI-07`  | **[C]** | "Center on Ship" camera focus | **NOT RUN** | — | Optional extension |
| `TC-UI-08`  | **[A]** | Collection Ledger modal audit trail | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-UI-09`  | **[A]** | Toll Policies transparency modal | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-MGMT-01`| **[A]** | Country Management CRUD | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-MGMT-02`| **[A]** | Toll policy editing & immutability | **NOT RUN** | — | Mandatory baseline (DEC-027) |
| `TC-MGMT-03`| **[A]** | Vessel Management CRUD | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-MGMT-04`| **[A]** | Route & checkpoint coordinate editing | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-MGMT-05`| **[A]** | Reset preservation & session history | **NOT RUN** | — | Mandatory baseline (DEC-027) |
| `TC-REP-01` | **[A]** | Transaction log RFC 4180 CSV export | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-REP-02` | **[A]** | Revenue summary CSV export | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-ALERT-01`| **[A]** | Corridor congestion warning alerts | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-LAYER-01`| **[A]** | Map layer visibility toggles | **NOT RUN** | — | Mandatory baseline (DEC-026) |
| `TC-RESP-01`| **[B]** | Map dominates screen area | **NOT RUN** | — | Conditional on DEC-016 |
| `TC-RESP-02`| **[B]** | Glassmorphic contrast (WCAG AA) | **NOT RUN** | — | Conditional on DEC-017 |
| `TC-RESP-03`| **[B]** | Accent color token compliance | **NOT RUN** | — | Conditional on DEC-018 |
| `TC-RESP-04`| **[B]** | Monospace tabular numeral display | **NOT RUN** | — | Conditional on DEC-019 |
| `TC-RESP-05`| **[C]** | Laptop screen adaptation (1366x768)| **NOT RUN** | — | Optional extension |
| `TC-RESP-06`| **[C]** | Compact screen adaptation (1024x768)| **NOT RUN** | — | Optional extension |
| `TC-RESP-07`| **[A]** | Strict anti-template compliance | **NOT RUN** | — | Mandatory baseline |
| `TC-E2E-01` | **[A]** | 15-Step Practical Demo Workflow | **NOT RUN** | — | Mandatory baseline |
| `TC-REG-01` | **[A]** | Suite REG-01 (Add New Country) | **NOT RUN** | — | Mandatory baseline |
| `TC-REG-02` | **[A]** | Suite REG-02 (Modify Toll Rate) | **NOT RUN** | — | Mandatory baseline |
| `TC-REG-03` | **[A]** | Suite REG-03 (Toggle Exemption) | **NOT RUN** | — | Mandatory baseline |
| `TC-REG-04` | **[A]** | Suite REG-04 (Relocate Checkpoint) | **NOT RUN** | — | Mandatory baseline |

### 17.2 Category Summary Statistics
- **Category [A] Mandatory Baseline Tests**: **62 Tests** (Must pass for MVP completion)
- **Category [B] Conditional on Technical Decisions**: **19 Tests** (Active only upon approval of corresponding DEC item)
- **Category [C] Optional Extensions & Illustrative Tests**: **6 Tests** (Non-blocking for MVP completion)
- **Total Test Cases**: **87 Tests**
- **Execution Progress**: **0 Executed / 87 (100% NOT RUN)**

---

## 18. Acceptance & Release Readiness Checklist

Before Hormuz Vasooli System is declared ready for the practical examination, the following mandatory release gates must be satisfied:

- [ ] **Gate 1: Build & Deploy (Mandatory)**: Maven packages `HormuzVasooliSystem-1.0-SNAPSHOT.war` with Java 8 bytecode; deploys to local GlassFish 4.1.1 without errors.
- [ ] **Gate 2: Interactive Map (Mandatory)**: Google Maps renders centered on Strait of Hormuz with custom dark maritime styles; handles missing API keys gracefully.
- [ ] **Gate 3: Custom Ship Markers (Mandatory)**: Simulated vessels render top-down hulls, masts, and local SVG national flags; hulls rotate with heading; flags remain upright and legible.
- [ ] **Gate 4: Simulation Controls (Mandatory)**: Start, Pause, Resume, Reset, and Speed (configurable presets: 1x, 2x, 4x) operate without coordinate warping or orphaned timers.
- [ ] **Gate 5: Authoritative Tolling (Mandatory)**: Configured country toll rules evaluated; at least one configured exemption yields $0; duplicate hits within the same transit are blocked.
- [ ] **Gate 6: Revenue Invariance (Mandatory)**: Live counters reflect $\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$ with formatted USD figures.
- [ ] **Gate 7: Operational UI & Inspection (Mandatory)**: Selecting any vessel displays its configured manifest, destination, and checkpoint status without breaking background simulation.
- [ ] **Gate 8: Dynamic Management & CSV Export (Mandatory)**: Management drawers allow live additions/edits of countries, toll tariffs, vessels, routes, and checkpoints; search/filters operate smoothly; RFC 4180 CSV export functions offline.
- [ ] **Gate 9: Historical Immutability & Multi-Run Reset Preservation (Mandatory)**: Policy edits apply strictly to future transits; simulation reset closes active run and begins new run while archiving past transactions in session history and preserving user configuration.
- [ ] **Gate 10: Transparency & Disclaimer (Mandatory)**: Mandatory educational simulation disclaimer is visible across all views.
- [ ] **Gate 11: Examination Rehearsal (Mandatory)**: The 15-step End-to-End scenario (`TC-E2E-01`) completes in under 5 minutes without technical glitches.
- [ ] **Gate 12: Approved Extensions (Optional)**: Any explicitly approved supporting features (cargo adjustments, domestic flag exemptions) pass their respective Category C tests.

---

## 19. Current Testing Status

- **Status**: **Draft for Review**
- **Test Suite Health**: All 87 test cases are audited, categorized, and verified against project requirements.
- **Execution Progress**: **0 / 87 Executed (100% NOT RUN)**.
- **Next Action**: Execute Category [A] environment tests (`TC-ENV-01` through `TC-ENV-06`) upon formal user authorization of Implementation Phase 1.
