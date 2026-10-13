# Hormuz Vasooli System — Practical Examination Demonstration Guide

A practical demonstration manual designed to assist the student in presenting the **Hormuz Vasooli System** to the examining professor in a structured, repeatable sequence during a college laboratory examination.

---

> ### CURRENT DOCUMENTATION STATUS
> 
> **Status**: **PLANNED DEMONSTRATION WORKFLOW — PENDING IMPLEMENTATION & VERIFICATION**  
> **Lifecycle Stage**: Phase 0 (Planning & Documentation Stage).  
> 
> This guide outlines the *intended* demonstration sequence for the completed application. None of the steps described below should be assumed to be operational until application implementation (Phases 1 through 10) and full system verification (Phase 11) have been executed and confirmed with test logs.

---

## 1. Purpose

The objective of this guide is to provide a calm, structured, step-by-step walkthrough for demonstrating the application to the professor during practical evaluation. It ensures the student can systematically showcase core simulation features, explain architectural principles, execute live data modifications, and address unanticipated examination questions with confidence.

---

## 2. Pre-Demonstration Checklist

Before inviting the professor to observe the demonstration, the student must complete and verify the following operational checks:

- [ ] **Check 1: Clean Build Verification** (`PENDING VERIFICATION`)
  - Execute `mvn clean package` in terminal.
  - Verify build status is `BUILD SUCCESS` and `target/HormuzVasooliSystem-1.0-SNAPSHOT.war` exists.
- [ ] **Check 2: Application Server Deployment** (`PENDING VERIFICATION`)
  - Confirm local GlassFish Server 4.1.1 is running on port 8080.
  - Deploy WAR file and verify deployment finishes with zero unhandled exceptions in `server.log`.
- [ ] **Check 3: Google Maps API Connectivity** (`PENDING VERIFICATION`)
  - Verify active Internet connection and valid Google Maps API Key availability.
  - Confirm map tiles load without authentication errors (`window.gm_authFailure`).
- [ ] **Check 4: Viewport Initialization** (`PENDING VERIFICATION`)
  - Open `http://localhost:8080/HormuzVasooliSystem/` in browser.
  - Verify map centers cleanly on the Strait of Hormuz (latitude $\approx 26.56^\circ\text{N}$, longitude $\approx 56.25^\circ\text{E}$).
- [ ] **Check 5: Seed Manifest Loaded** (`PENDING VERIFICATION`)
  - Confirm initial fleet of demonstration vessels, routes, and checkpoints appear on the map.
- [ ] **Check 6: Ship Models & Flags Visible** (`PENDING VERIFICATION`)
  - Verify ship hulls render cargo-specific colors, mast poles are centered, and local vector SVG flags display upright.
- [ ] **Check 7: Financial Counters Baseline** (`PENDING VERIFICATION`)
  - Confirm Iran Revenue, Oman Revenue, and Total Revenue initialize at `$0.00 USD`.
- [ ] **Check 8: Pre-Demo Reset to Clean State** (`PENDING VERIFICATION`)
  - Click the **[Reset]** button to ensure all vessels are at starting waypoints and active run ledgers are empty.

---

## 3. Recommended Demonstration Sequence

Follow this ordered sequence during the practical examination. Each step specifies the action to take, what to point out to the professor, and the underlying principle demonstrated.

```
[A. Introduce Scope] ──────► [B. Show Map & TSS] ──────► [C. Show Ship Models]
          │
          ▼
[D. Start Simulation] ─────► [E. Speed & Pause] ───────► [F. Vessel Inspection]
          │
          ▼
[G. Toll Crossing] ────────► [H. Itemized Receipt] ────► [I. Revenue Invariant]
          │
          ▼
[J. Exempt Passage] ───────► [K. Rate Modification] ───► [L. Dynamic CRUD Drawers]
          │
          ▼
[M. Search & Filters] ─────► [N. Ledger & Reports] ────► [O. RFC 4180 CSV Export]
          │
          ▼
[P. Congestion Alert] ─────► [Q. Layer Toggles] ───────► [R. Multi-Run Session Reset]
```

---

### Step A: Introduce the System Purpose
- **Action**: Open the application landing page.
- **Explain to Professor**:
  - *"This is Hormuz Vasooli System, an educational web application modeling commercial maritime transit and hypothetical toll assessment in the Strait of Hormuz."*
  - *"It visualizes vessel navigation along international shipping lanes, renders coordinated composite ship markers with masts and national flags, applies separate Iranian and Omani toll policies, and guarantees financial revenue conservation."*
  - Point to the persistent **Educational Simulation Disclaimer** badge in the header.

### Step B: Show the Interactive Map & Geography
- **Action**: Pan and zoom the map across the Strait of Hormuz.
- **Explain to Professor**:
  - Point out the northern coastline (Islamic Republic of Iran) and southern coastline (Sultanate of Oman / Musandam Peninsula).
  - Point out the two predefined Traffic Separation Scheme corridors: Inbound Lane (entering Persian Gulf) and Outbound Lane (exiting into Gulf of Oman).
  - Point out the designated hypothetical checkpoint circles (Iran Northern Passage Checkpoint and Oman Inbound Checkpoint).

### Step C: Inspect Composite Ship Markers & Flags
- **Action**: Zoom in on an idle vessel marker before starting simulation.
- **Explain to Professor**:
  - Highlight the three coordinated visual layers of each marker:
    1. A top-down ship hull silhouette colored by cargo classification (e.g., Black for Crude Oil, Blue for LNG, Amber for Container Goods).
    2. A small vertical mast pole centered on the superstructure.
    3. A national flag badge rendered from a local vector SVG asset (never system emojis).
  - Mention that as ships turn, the hull rotates along the heading angle ($\theta$), while the flag badge is counter-rotated ($-\theta$) to remain permanently upright, horizontal, and legible.

### Step D: Start Simulation & Observe Movement
- **Action**: Click the **[► Start]** button.
- **Explain to Professor**:
  - Vessels begin moving smoothly along their assigned navigation corridors.
  - Movement uses client-side great-circle distance interpolation between route waypoints.
  - Point out hull rotation as ships adjust course at waypoints while flag badges stay level.

### Step E: Demonstrate Speed Controls & Pause
- **Action**: Toggle simulation speed through demonstration presets: **[1x]**, **[2x]**, and **[4x]**.
- **Explain to Professor**:
  - Movement accelerates smoothly without skipping waypoint segments or bypassing spatial checkpoint boundary detection.
  - Click **[❚❚ Pause]**: vessels freeze instantly at their exact coordinates.
  - Click **[► Resume]**: vessels seamlessly continue their journey.

### Step F: Vessel Inspection & Manifest Drawer
- **Action**: Click any moving ship on the map canvas.
- **Explain to Professor**:
  - The vessel receives a highlighted cyan accent glow and camera focus.
  - A slide-out Vessel Detail drawer displays full manifest details: Vessel Name, IMO Number, Registered Nationality, Cargo Type, Origin, Destination, Route ID, Speed, and Current Checkpoint Status (`WAITING`, `PASSING`, or `PASSED`).

### Step G: Observe a Toll Checkpoint Encounter
- **Action**: Track a non-exempt vessel as it enters a checkpoint catchment circle.
- **Explain to Professor**:
  - As the vessel crosses the checkpoint boundary radius, the proximity latch detects the encounter.
  - A subtle pulse wave animates on the checkpoint circle.
  - The toll engine evaluates applicable tariff rules for the vessel's configured nationality.
  - A transient toast notification announces the event (e.g., *"Assessed $15,000 USD for SHIP-001 at Iran Checkpoint"*).

### Step H: Inspect the Itemized Toll Receipt
- **Action**: Click the toll notification toast or open the transaction record.
- **Explain to Professor**:
  - Show the itemized transaction receipt dialog:
    - Unique Transaction ID.
    - Timestamp / Simulation Tick.
    - Vessel ID & Registered Flag.
    - Checkpoint Authority (IRAN or OMAN).
    - Base Tariff + applicable Cargo Multiplier.
    - Final Assessed Amount in USD.
    - Outcome Status: `CHARGED`.

### Step I: Verify Revenue Conservation Invariant
- **Action**: Direct the professor's attention to the floating Financial Bar in the top header.
- **Explain to Professor**:
  - Iran Revenue reflects collections by the Iranian authority.
  - Oman Revenue reflects collections by the Omani authority.
  - Verify the fundamental mathematical invariant holds true at all times:
    $$\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$$
  - Note that figures display in standardized US Dollar currency format (`$XX,XXX.00 USD`).

### Step J: Demonstrate a Configured Exemption
- **Action**: Track a vessel whose nationality is configured with an active toll exemption (or configure one live in Step K).
- **Explain to Professor**:
  - When an exempt vessel crosses a checkpoint, the engine recognizes the exemption rule.
  - The transaction is recorded with Status `EXEMPT`, assessed fee is `$0.00 USD`, and revenue tallies remain completely unchanged.
  - *Note: Emphasize that exemptions are configuration-driven rules and no country is hard-coded as permanently exempt.*

### Step K: Modify Toll Rates & Verify Historical Immutability
- **Action**: Open the **[Toll Policies]** management drawer. Edit a base tariff (e.g., change India's Iran tariff from `$15,000` to `$18,000 USD`).
- **Explain to Professor**:
  - Point out the **Historical Immutability Rule**: The updated rate applies strictly to *future* transits.
  - Inspect the transaction ledger: previously recorded transactions at `$15,000 USD` remain unaltered, and prior revenue is NOT retroactively recalculated.
  - When the next vessel of that nationality crosses, it is billed at the new `$18,000 USD` rate.

### Step L: Demonstrate Dynamic Entity Management Drawers
- **Action**: Open the management drawers via the navigation bar:
  1. **Country Management**: Add a new country (e.g., ISO code `"SGP"`, Name `"Singapore"`, SVG flag mapping, base tariff). Show form validation preventing empty names or duplicate codes.
  2. **Vessel Management**: Register a new vessel and assign it to an existing route. The new vessel immediately appears at the route origin.
  3. **Route & Checkpoint Management**: Adjust checkpoint coordinates or increase detection radius; show map circle updating immediately.

### Step M: Demonstrate Vessel Search & Multi-Attribute Filters
- **Action**: In the left floating panel, demonstrate search and filtering:
  1. Type a vessel name or IMO substring into the search box: only matching ships remain in the list.
  2. Select a Nationality filter (e.g., `"India"`): the map dims non-matching ships and isolates matching vessels.
  3. Filter by Cargo Category or Status (`WAITING`, `PASSING`, `PASSED`).
  4. Click **[Reset Filters]**: all ships return to standard visibility.
  - Point out that filtering affects visual display only and never alters background movement or revenue accounting.

### Step N: Review Transaction Ledger & Analytics Reports
- **Action**: Open the **[Collection Ledger]** and **[Reports]** views.
- **Explain to Professor**:
  - The Ledger displays a chronological audit log with column sorting, outcome badges (`CHARGED`, `EXEMPT`, `DUPLICATE`), and full receipt inspection on row click.
  - The Reports view shows graphical and tabular breakdowns by Authority, by Nationality, and by Cargo category.

### Step O: Export Reports as RFC 4180 CSV
- **Action**: Click the **[⬇ Export Transaction Log (CSV)]** button.
- **Explain to Professor**:
  - The browser generates and downloads `transactions-export.csv` directly via client-side Web APIs (`Blob` and `URL.createObjectURL`) without server overhead.
  - Open the file in Excel or text editor to show valid RFC 4180 formatting: standard comma delimiters, quoted text fields, and accurate revenue values matching on-screen counters.

### Step P: Demonstrate Configurable Traffic Congestion Alerts
- **Action**: In the Settings drawer, show the configurable alert parameters (waiting count threshold and proximity radius). Move or simulate $\ge 3$ vessels into a checkpoint sector.
- **Explain to Professor**:
  - The traffic density monitor updates the status badge from `FLUID` (Green) to `MODERATE` (Amber) or `CONGESTED` (Pulsing Coral).
  - Clarify that thresholds are illustrative academic demonstration defaults, not actual maritime safety regulations.

### Step Q: Demonstrate Map Layer Visibility Controls
- **Action**: Click the floating pill toggles top-left of the map:
  - Toggle **[Routes 👁]**: Shipping lane polylines hide/show.
  - Toggle **[Checkpoints 👁]**: Checkpoint circles hide/show.
  - Toggle **[Labels 👁]**: Floating ship name tags hide/show.
  - Note that toggling layers maintains background simulation without interrupting timers.

### Step R: Reset Simulation & Verify Multi-Run Session History
- **Action**: Click the **[⟲ Reset]** button.
- **Explain to Professor**:
  - **Run Lifecycle**: The active run closes, and a fresh run begins with an incremented Run ID.
  - Vessels return to their initial route origin waypoints.
  - Active run revenue totals return to `$0.00 USD`, and the active ledger clears.
  - **Past-Run Session History Archive**: Previously recorded transactions from Run 1 remain accessible in session history for historical audit during the current server session.
  - **Configuration Preservation Invariant**: All custom-created countries, modified toll rates, added vessels, and adjusted checkpoint coordinates **remain completely intact**.

---

## 4. Illustrative Professor Practical Examination Requests

During practical examinations, the professor frequently tests system flexibility by asking for live modifications. The table below details how to handle anticipated prompts using built-in management interfaces without code edits:

| Anticipated Professor Request | Live UI Action | Expected Observable Result | Core Concept Demonstrated |
| :--- | :--- | :--- | :--- |
| **"Change the toll rate for a country."** | Open **Toll Policies** drawer; select country; enter new base tariff (USD); click **Save**. | Future transits billed at new rate; existing ledger entries and past revenue remain unchanged. | Historical Immutability ([DEC-027](DECISIONS.md)). |
| **"Make this country exempt from tolls."** | Open **Toll Policies** drawer; toggle exemption switch to `ON` for the target nation. | Subsequent checkpoint passages generate `$0.00 USD` receipts with status `EXEMPT`. | Configuration-driven policy decoupling. |
| **"Add a new country to the system."** | Open **Country Management** drawer; enter code (e.g., `"MYS"`), name, select SVG flag, set tariff; submit. | Country appears in country selectors; vessels can now be assigned to this nation. | Extensible schema without recompilation. |
| **"Add a new ship into the simulation."** | Open **Vessel Management** drawer; enter ship name, IMO, choose nationality, cargo, and route; submit. | New ship appears at route origin with composite hull/mast/flag marker and animates. | Dynamic fleet instantiation. |
| **"Reroute ships or change checkpoint location."** | Open **Route / Checkpoint** drawer; adjust coordinates or radius slider; click **Update**. | Map overlay moves to new coordinates; boundary detection triggers at new geographic location. | Spatial decoupling from rendering. |
| **"Show me all crude oil tankers from India."** | In Filter panel: set Nationality to `"India"` and Cargo Category to `"Crude Oil"`. | Map dims unrelated vessels; registry list filters to matching vessels only. | Non-destructive multi-attribute filtering. |
| **"Export the transactions to Excel."** | Click **[⬇ Export CSV]** in the ledger header. | Browser downloads RFC 4180 compliant CSV file matching current screen data. | Client-side reporting & data auditability. |
| **"Change the congestion warning threshold."** | Open **Settings** panel; change threshold count slider (e.g., from 3 to 2 vessels). | Congestion badge re-evaluates density and triggers alert earlier. | Configurable demonstration parameters. |

*Disclaimer: These are illustrative anticipated prompts for examination preparation. No specific country (such as China or Iran) should be assumed to be exempt unless configured.*

---

## 5. Core Correctness Invariants

Always verify these mathematical and operational rules during the demonstration:

1. **Combined Revenue Conservation**:
   $$\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$$
   *Toll payments never vanish or duplicate across authorities.*
2. **Strict Transit Idempotency (Single-Charge Rule)**:
   A vessel dwelling inside or slowly crossing a checkpoint radius must generate **exactly one** financial transaction per passage.
3. **Exemption Integrity**:
   An exempt vessel crossing must generate an explicit transaction record with status `EXEMPT` and assessed amount **strictly equal to `$0.00 USD`**.
4. **Historical Ledger Immutability**:
   Modifying a tariff or toggling an exemption applies strictly to subsequent encounters; historical records and finalized revenues are never retroactively modified.
5. **Configuration Preservation Across Reset**:
   Clicking reset restores run positions and zeroes active revenue, but must never erase user-configured countries, modified tariffs, added vessels, or custom routes.

---

## 6. Troubleshooting Guide

If an issue occurs during preparation or the examination, follow these diagnostic steps:

| Issue | Likely Cause | Recommended Immediate Action |
| :--- | :--- | :--- |
| **Google Maps Fails to Load** | Missing or rejected API key; network offline. | Check Internet connection; click **[API Key]** button in header to verify or re-enter key; check browser console for `gm_authFailure`. |
| **Maven Build Fails** | JDK source/target mismatch or corrupted repo cache. | Verify JDK 8 (`1.8.0_181`) is active in NetBeans; run `mvn clean package -U` from terminal to refresh plugins. |
| **GlassFish Deployment Fails** | Port 8080 conflict or stale context. | Check GlassFish server log; undeploy old context via `asadmin undeploy HormuzVasooliSystem`; restart GlassFish. |
| **Ship Flag Appears Missing** | SVG asset not found in `assets/flags/`. | System falls back to nautical ensign gracefully; verify country ISO code matches lowercase SVG filename (e.g., `ind.svg`). |
| **Ships Frozen / Not Moving** | Simulation paused or timer halted. | Check simulation control panel; click **[► Resume]** or **[► Start]**; check for unhandled JS errors in console. |
| **Checkpoint Crossing Not Recorded** | Proximity radius too small or ship route bypassed zone. | Open Checkpoint Management drawer and inspect detection radius; verify ship coordinates intersect checkpoint circle. |
| **Duplicate Charge Observed** | Proximity debounce latch reset prematurely. | Verify ship status transition lifecycle (`WAITING` -> `PASSING` -> `PASSED`); ensure vessel has exited radius before latch unlocks. |
| **Revenue Math Inconsistency** | Discrepancy between client UI and backend ledger. | Trigger manual sync via `GET /api/simulation/state` or check transaction ledger sum to verify $\text{Total} \equiv \text{Iran} + \text{Oman}$. |
| **CSV Export Button Unresponsive** | Browser blocked Blob download popup. | Check browser download permissions; verify browser supports native HTML5 `Blob` and `URL.createObjectURL`. |

---

## 7. Demonstration Rehearsal Strategy

To ensure a smooth exam performance:

1. **Rehearse the Core Flow First**: Practice Steps A through I (Start -> Waypoint Navigation -> Checkpoint Crossing -> Receipt Inspection -> Revenue Invariant -> Reset) until effortless.
2. **Practice the Management Actions**: Rehearse adding a country, changing a rate, and toggling an exemption (Steps J through L).
3. **Practice Filtering and CSV Export**: Rehearse searching vessels and downloading the CSV audit log (Steps M through O).
4. **Speak in Clear Architectural Terms**: Emphasize *separation of concerns*, *idempotency*, *configuration-driven design*, and *Java EE 7 standards*.

---

## 8. Current Document Status

- **Status**: **Draft Planned Workflow for Review**.
- **Execution State**: **PENDING IMPLEMENTATION**. All demonstration steps remain unverified until Phases 1–11 are completed.
