# Hormuz Vasooli System
# Product Requirements Document (PRD)

Version: 1.0
Status: Initial Product Specification
Project Type: College Practical Examination Project

---

## 1. Document Purpose

This document defines the functional requirements, user workflows,
scope, expected behavior, and acceptance criteria for Hormuz
Vasooli System.

The objective is to ensure that all features have a clear purpose
and measurable completion criteria before implementation begins.

The project brain document defines the overall context and
constraints. This PRD defines what the product must do.

Technical implementation details will be finalized separately
in TRD.md.

Visual implementation details will be finalized separately
in UI_UX_GUIDELINES.md.

Requirements introduced by the professor during practical
preparation or examination must be recorded and reviewed before
implementation.

---

## 2. Product Overview

Hormuz Vasooli System is an interactive educational web application
that simulates ship movement and hypothetical toll collection
around the Strait of Hormuz.

The application will use Google Maps JavaScript API to display the
geographical area and visualize simulated ships travelling along
predefined routes.

Each vessel will have a nationality, country flag, cargo details,
route, and simulation status.

Configured hypothetical toll policies will determine the amount
associated with a ship when it passes through a simulated toll
checkpoint.

The application will maintain separate simulated collection
totals for Iran and Oman and display their combined revenue.

All initial vessel records, routes, toll amounts, and transactions
will be identified as simulation data.

The application must not imply that its toll policies represent
actual Iranian or Omani maritime regulations.

---

## 3. Product Goals

The product must achieve the following goals:

1. Display an interactive map of the Strait of Hormuz.
2. Visualize multiple simulated ships moving along predefined routes.
3. Display recognizable ship models rather than relying exclusively
   on generic arrow markers.
4. Display each ship's country flag on a small mast or pole above
   the ship representation.
5. Allow users to inspect vessel information.
6. Support configurable country-based hypothetical toll rules.
7. Support configured toll exemptions for selected countries.
8. Calculate Iran's and Oman's simulated revenue separately.
9. Calculate combined simulated revenue accurately.
10. Prevent duplicate charges for the same checkpoint encounter
    during a vessel's simulated passage.
11. Allow the user to start, pause, resume, and reset the simulation.
12. Provide a clear operational dashboard suitable for a
    college practical examination.
13. Keep the project sufficiently modular to accommodate
    additional requirements from the professor.
14. Provide interactive management interfaces for common examination
    changes (add/edit country, toll policy, vessel, route, and checkpoint).
15. Support responsive vessel search and multi-attribute filtering
    (by nationality, cargo type, and simulation status).
16. Maintain an interactive transaction ledger detailing each toll
    assessment event, status, and outcome.
17. Generate aggregated revenue reports with breakdown by authority,
    country, and cargo type, with CSV export capabilities.
18. Support configurable simulation speed controls with demonstration presets (1x, 2x, 4x).
19. Provide visual congestion warning alerts based on configurable parameters
    (waiting-vessel count threshold and geographic proximity radius). Defaults are
    illustrative demonstration parameters, not real-world maritime regulations.
20. Support layer visibility toggles to show or hide route corridors
    and checkpoint detection zones on the map.

---

## 4. Target User and Primary Use Case

### Primary User

The primary user is the student demonstrating the application
during a college practical examination.

The professor will observe the application and may request
additional functionality or modifications during the practical.

### Primary Use Case

The user opens the application, starts the simulation, observes
ships moving through the Strait of Hormuz, selects a ship to
inspect its information, observes checkpoint events, and verifies
that the hypothetical toll collection and revenue calculations
are correct.

The user can also open management panels to dynamically add or
edit a country, modify a toll tariff or exemption, update a vessel,
alter a route or checkpoint, filter vessels, review transactions,
and export revenue reports to CSV to satisfy professor prompts
in real time without code modifications.

The application must make this workflow easy to demonstrate
without requiring access to real maritime vessel data.

---

## 5. Requirement Classification

Requirements must be classified accurately.

### 5.1 Core Requirements

The following requirements are central to the agreed product scope:

- Google Maps integration focused on the Strait of Hormuz.
- Visible Iran and Oman geographical context and checkpoints.
- Multiple simulated vessels moving along predefined routes.
- Recognizable ship models with mast and upright country flag badge.
- Ship information inspection and status tracking.
- Country-based hypothetical toll calculation and exemptions.
- Separate Iran and Oman revenue tracking and combined revenue invariant.
- Duplicate-charge prevention for checkpoint encounters.
- Simulation controls: Start, Pause, Resume, Reset.
- Simulation speed selection with configurable demonstration presets (1x, 2x, 4x).
- Country management (add/edit country name, ISO code, SVG flag, default toll, exemption status).
- Toll policy management (configure country tariffs, cargo rates, toggle exemptions).
- Vessel management (add/edit vessel name, nationality, cargo, route, status).
- Route and checkpoint management (add/modify waypoint coordinates and detection radius).
- Vessel search and multi-attribute filtering (nationality, cargo, status).
- Transaction history ledger with detailed event inspection.
- Revenue breakdown reporting and downloadable RFC 4180 CSV export.
- Congestion warning alerts based on configurable vessel density thresholds (count and proximity radius).
- Map layer visibility toggles (routes, checkpoints).
- Robust client-side and form validations (non-empty fields, positive numeric rates, duplicate code prevention).
- Historical transaction immutability (policy changes apply strictly to future transits).
- Multi-run simulation reset with configuration preservation (reset begins a new run, preserving past-run session history and user-configured entities).
- Clear simulation-data and educational disclaimers.

### 5.2 Supporting Requirements

The following capabilities support the main workflow:

- Responsive drawer and modal management layouts.
- Toast notifications for user actions and validation errors.
- Visual status indicators and cargo color coding.
- Map panning and smooth zoom focus on selected vessels.
- Clear empty-state messages for filtered lists and ledgers.

### 5.3 Potential Extensions

The following are not part of the initial product build:

- Persistent database storage (SQL or NoSQL beyond memory/session).
- User authentication and role-based access control.
- Live AIS vessel tracking feeds.
- Real maritime vessel APIs.
- Real-time foreign exchange currency conversion.
- Automated PDF document generation.
- Machine learning / AI congestion prediction.
- Automated dynamic rerouting / pathfinding algorithms.
- Fully three-dimensional WebGL ship rendering.
- Real financial payment gateway integration.
- External government or port authority integrations.

These features require separate approval before being added
to the project scope.

---

## 6. Main Application Sections

### 6.1 Operations Dashboard

The Operations Dashboard is the main application view.

It should provide direct access to the interactive map and
the most important simulation information.

The dashboard should include:

- Total vessel count.
- Waiting vessel count.
- Passing vessel count.
- Passed vessel count.
- Simulated Iran revenue.
- Simulated Oman revenue.
- Combined simulated revenue.
- Simulation status.
- Access to vessel information.
- Access to relevant simulation controls.

Dashboard figures must be derived from the current simulation
state and valid transaction records.

Decorative statistics that do not correspond to actual application
state must not be used.

### 6.2 Operations Map

The map is the primary functional element of the application.

Requirements:

- Load Google Maps JavaScript API.
- Center the initial view on the Strait of Hormuz.
- Display the surrounding geography of Iran and Oman.
- Display configured vessel routes.
- Display configured hypothetical toll checkpoints.
- Display multiple simulated vessels.
- Support map zoom and normal map navigation.
- Allow a vessel to be selected.
- Display relevant vessel information when selected.

The map must remain usable when the simulation is paused.

If the Google Maps API cannot load, display an understandable
error message rather than silently pretending the map is working.

The application must not claim to provide live vessel positions.

### 6.3 Vessel Representation

Each vessel must have a recognizable visual marker.

The preferred representation is a top-down ship-shaped SVG model.

The marker should include:

- Ship hull and deck silhouette.
- A small mast or pole.
- The corresponding country flag attached to the mast.
- Orientation that reflects the vessel's heading.
- Visual clarity at supported map zoom levels.

The ship, mast, and flag must behave as a coordinated visual unit.

The flag must correspond to the vessel's configured nationality.

A generic arrow alone does not satisfy this requirement.

The implementation may begin with lightweight SVG models.
Full 3D rendering is an optional enhancement, not an MVP
requirement unless separately approved.

#### 6.4 Vessel Registry & Dynamic Management

The registry allows the user to review, search, and dynamically manage
simulated vessels.

Each vessel record must support:
- Vessel ID (unique alphanumeric identifier).
- Vessel name.
- Nationality (linked to a configured country).
- Country flag (dynamically rendered from the country's flag asset).
- Cargo type (e.g., Crude Oil, LNG, Container, Bulk, General Cargo).
- Assigned Route ID.
- Simulation status (Waiting, Passing, Passed, or Stationary).
- Current waypoint progress and speed.
- Applicable hypothetical toll status.
- Transaction history links.

Vessel Management Capabilities:
- Add a new vessel: The user can provide a unique ID, vessel name, nationality,
  cargo classification, and assigned route.
- Edit an existing vessel: The user can modify vessel name, cargo, nationality,
  or route assignment.
- Validation: Vessel IDs must be unique, names must be non-empty strings, and
  assigned routes must exist.

Selecting a vessel in the registry or map must focus and highlight that vessel.

### 6.5 Toll Policy Management

The application must support configurable hypothetical toll policies.

A toll policy defines:
- Target country or nationality identifier.
- Base toll tariff in USD.
- Cargo-type multipliers or fixed adjustments (if configured).
- Exemption status flag (Active / Inactive).
- Authority-specific policy distinctions (Iran vs Oman).

Management Capabilities:
- View all active country toll policies.
- Edit base toll rate for any country.
- Toggle toll exemption on or off for any country.
- Add specific cargo tariff multipliers.
- Validation: Toll rates must be non-negative numeric values ($ \ge 0 $).

Historical Transaction Immutability Rule:
Modifying a toll rate or toggling an exemption applies strictly to
subsequent checkpoint crossings. It must never retroactively alter or
recalculate previously recorded transaction entries or finalized revenue.

### 6.6 Checkpoint & Route Management

The simulation models hypothetical checkpoints and vessel navigation routes
in the Strait of Hormuz.

Checkpoint Definition:
- Checkpoint ID (unique identifier).
- Displayed name (e.g., "Iran Northern Passage Checkpoint", "Oman Inbound Checkpoint").
- Authority association ("IRAN" or "OMAN").
- Geographic position (latitude, longitude).
- Detection area radius (in nautical miles or meters).
- Active status toggle.

Route Definition:
- Route ID and corridor name.
- Ordered sequence of geographic waypoints (latitude, longitude).
- Default transit direction (Inbound, Outbound, or Bi-directional).

Management Capabilities:
- Add or modify route waypoints to adjust ship trajectories.
- Modify checkpoint geographic coordinates and detection radius.
- Validation: Latitude must be in range $[-90, 90]$, longitude in $[-180, 180]$,
  and detection radius must be positive ($> 0$).

All checkpoints and routes remain educational simulation constructs and must
not be represented as verified real-world maritime installations.

### 6.7 Toll Processing

When a vessel completes a qualifying checkpoint encounter, the system
evaluates the relevant toll policy:

1. Identify the vessel and its configured nationality.
2. Identify the checkpoint and its governing authority (Iran or Oman).
3. Check proximity latch to ensure this encounter has not already been billed.
4. Look up the active toll tariff and exemption status for that country.
5. If exempt, record a zero-dollar (\$0.00) exempt transaction.
6. If non-exempt, compute the hypothetical charge (Base Tariff $\times$ Cargo Multiplier).
7. Generate a unique transaction record with timestamp/tick and outcome.
8. Increment the governing authority's revenue total and combined revenue.
9. Update the vessel's status and dashboard counters.

A failed or out-of-bounds encounter must not generate a toll charge.
Exempt transactions must be logged for auditability but must add \$0.00 to revenue.

### 6.8 Duplicate-Charge Prevention

The system must prevent duplicate billing for the same checkpoint
encounter during a vessel's simulated passage.

- Latch Mechanism: Entering a checkpoint detection zone latches the encounter.
  Continuous animation frames or hovering within the zone must not re-trigger tolling.
- Distinct Authorities: Iran and Oman represent independent checkpoints.
  Paying a toll at an Iranian checkpoint must not block a subsequent legitimate
  encounter at an Omani checkpoint.
- Re-entry Safety: A vessel cannot be charged again at the same checkpoint
  until it has completed its passage and reset or restarted the route.

### 6.9 Revenue Dashboard & Analytics

The revenue dashboard presents the financial state of the simulation:
- Iran Simulated Revenue.
- Oman Simulated Revenue.
- Combined Total Revenue.
- Total transaction count, exempt transit count, and billed transit count.
- Revenue breakdown by country and cargo category.

Fundamental Financial Invariant:
$$\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$$

Revenue values must display in standardized USD currency format (\$#,##0.00).
Figures must derive strictly from recorded valid transactions.

### 6.10 Simulation Controls & Speed Selection

The user must have complete control over simulation execution:
- Start: Initiates or resumes vessel progression from the initial state.
- Pause: Freezes vessel movement and halts checkpoint event detection.
- Resume: Continues movement from the exact paused coordinates.
- Reset: Restores the simulation to initial conditions (see 6.13).
- Speed Selection: Toggles simulation execution speed between configurable demonstration presets (1x, 2x, and 4x)
  without causing position jumping or missed checkpoint detection.

### 6.11 Vessel Search and Multi-Attribute Filtering

The application must provide real-time search and filtering:
- Search: Full-text or prefix search by vessel name or vessel ID.
- Nationality Filter: Multi-select or dropdown filter by vessel country.
- Cargo Category Filter: Filter by cargo type (Crude Oil, LNG, Container, etc.).
- Status Filter: Filter by simulation state (Waiting, Passing, Passed).
- Reset Filters: A single action restores the unfiltered view.

Filtering modifies only the presentation in the registry and map focus;
it must never alter the underlying simulation state, vessel movements, or revenue.

### 6.12 Transaction History Ledger

The application must maintain an interactive ledger of all toll events:
- Transaction ID (unique identifier).
- Timestamp / Simulation Tick.
- Vessel Name and ID.
- Flag / Nationality.
- Checkpoint Name and Authority (Iran / Oman).
- Assessed Fee (USD) or \$0.00 (Exempt).
- Status Outcome (BILLED, EXEMPT, BLOCKED_DUPLICATE).

The ledger must support column sorting, filtering by authority/status, and
detail inspection on click.

### 6.13 Simulation Reset & Multi-Run Session History

When the user triggers a simulation reset:
- Run Lifecycle Transition: Reset closes the active simulation run and starts a new run.
- Active Run Initialization: The new active run begins with zero revenue ($0.00 USD),
  an empty active-run transaction ledger, and vessels returned to initial route coordinates.
- Past-Run Session History: Transactions assessed during prior runs remain accessible
  during the active server session as immutable past-run history, guaranteeing full auditability
  without data loss.
- In-Memory Persistence Boundary: In-memory session history may be lost when the server
  restarts because external database persistence is not approved.
- Historical Immutability Invariant: Changes to toll rates or exemptions apply strictly to
  future transits and never retroactively alter previously recorded transactions in either the active
  run or historical runs.
- Configuration Preservation Invariant: All user-created or edited countries, custom
  toll tariffs, modified vessel attributes, added routes, and edited checkpoints MUST
  REMAIN INTACT across simulation reset. Resetting the simulation run must never wipe out
  custom configuration.

### 6.14 Country Registry & Flag Management

The system must maintain an extensible country registry:
- Country Name (e.g., "India", "China", "Panama").
- ISO Country Code (e.g., "IND", "CHN", "PAN").
- Flag Asset Reference (relative path to local SVG asset, e.g., `assets/flags/ind.svg`).
- Default Toll Tariff (USD).
- Exemption Status (boolean).

Management Capabilities:
- Add a new country with its ISO code, SVG flag association, and default tariff.
- Edit existing country name, default tariff, or exemption flag.
- Validation: ISO code must be unique and non-empty; flag asset must resolve
  to a valid local SVG file.

### 6.15 Revenue Reports and CSV Export

The application must provide downloadable tabular reports:
- Aggregated Revenue Summary: Export breakdown by authority, country, and cargo type.
- Detailed Transaction Audit Log: Export all recorded transactions with fields:
  `Transaction_ID`, `Timestamp`, `Vessel_ID`, `Vessel_Name`, `Nationality`,
  `Cargo_Type`, `Checkpoint_ID`, `Authority`, `Toll_Amount_USD`, `Status`.
- Format: Compliant RFC 4180 CSV generated in-browser via Blob/Data URI.

### 6.16 Congestion Warning Alerts

To support traffic management demonstrations:
- Active corridor monitoring tracks the number of vessels currently within
  each route segment and checkpoint detection area.
- Configurable parameters:
  - Waiting-vessel count threshold (illustrative default: $\ge 3$ vessels).
  - Geographic proximity radius (illustrative default: 5 km or checkpoint catchment area).
- Disclaimer: Threshold parameters and default values are purely illustrative demonstration
  data for academic simulation. They must never be represented as real-world maritime safety
  regulations, official waterway quotas, or governmental traffic policies.
- Visual warning badges and status indicators highlight congested sectors dynamically.

### 6.17 Layer Visibility Controls

To maintain map clarity during complex demonstrations:
- Route Layer Toggle: Show or hide vessel route polylines.
- Checkpoint Layer Toggle: Show or hide checkpoint location markers and detection radii.
- Vessel Label Toggle: Show or hide vessel name tags.

---

## 7. Simulation Data Requirements

The initial application will use clearly identified sample data.

Each simulated vessel should have enough information to demonstrate:

- Nationality.
- Flag.
- Cargo.
- Route.
- Current position.
- Heading.
- Simulation status.
- Applicable toll policy.

Initial data should include multiple nationalities and vessel
categories so that country-based toll calculations and exemptions
can be demonstrated.

At least one configured country should be toll-exempt for the
demo, using an explicitly identified hypothetical rule.

The exact list of ships, flags, cargo categories, routes, and
example toll amounts will be finalized in the implementation plan
or technical configuration.

The initial dataset must not be presented as a real-time or
verified list of actual ships.

---

## 8. User Experience Requirements

The application must prioritize operational clarity.

Requirements:

- The map is the primary workspace.
- Important revenue figures are visible and readable.
- Vessel details are accessible without navigating through
  unnecessary screens.
- Checkpoints and routes are visually distinguishable.
- Simulation status is clearly communicated.
- Selected vessels are identifiable.
- Loading failures and API configuration problems are visible.
- Toll collection and exemption outcomes are understandable.
- Important controls have clear labels.
- Responsive layout should not make the main workflow unusable
  on smaller screens.

Detailed colors, typography, spacing, glassmorphism, and panel
placement will be finalized in UI_UX_GUIDELINES.md.

This PRD does not yet lock the final visual design.

---

## 9. Non-Functional Requirements

### 9.1 Compatibility

The application must remain compatible with:

- NetBeans IDE 8.2.
- Java 8.
- Maven Web Application.
- Java EE 7 Web.
- GlassFish Server 4.1.1.

Do not introduce incompatible frameworks or runtime requirements
without approval.

### 9.2 Maintainability

The application must use clear module boundaries.

Configuration data, ship movement, toll processing, transaction
records, and presentation logic should not be unnecessarily
coupled.

A small feature change should not require rewriting unrelated
application modules.

### 9.3 Accuracy

Revenue calculations must be internally consistent.

For example:

Combined Revenue = Iran Revenue + Oman Revenue

Each transaction must have one clearly defined outcome.

Duplicate charges must be prevented according to the defined
passage and checkpoint rules.

### 9.4 Performance

The initial demonstration should support multiple simulated ships
without obvious interface freezing or excessive redundant updates.

The implementation should avoid unnecessary map-object creation
and uncontrolled animation loops.

Specific performance benchmarks may be defined later if needed.

### 9.5 Security and Configuration

Google Maps API configuration must be documented.

Secrets must not be committed to public source control.

If backend endpoints are introduced, input validation must be
considered for values that affect toll rules or transaction records.

### 9.6 Accessibility and Usability

The interface should use readable text, clear status indicators,
sufficient contrast, and understandable control labels.

Important vessel and transaction information must not be conveyed
through color alone.

---

## 10. Scope Exclusions for the Initial MVP

Unless separately approved, the initial MVP does not include:

- Live AIS data feeds.
- Real-time vessel surveillance.
- Actual toll collection or payment processing.
- Real governmental toll policies.
- Government or port authority integrations.
- Mandatory user login.
- Multi-user permissions.
- Mandatory database persistence.
- Artificial intelligence prediction.
- Automated geopolitical decision-making.
- Production deployment infrastructure.

These exclusions prevent the initial college project from becoming
unnecessarily complex.

---

## 11. Acceptance Criteria

The initial product may be considered functionally ready for
demonstration when the following criteria are satisfied.

### AC-01: Application Startup

The project builds and deploys in the approved NetBeans and
GlassFish environment without unresolved blocking errors.

### AC-02: Map Display

The application displays Google Maps focused on the Strait of
Hormuz with the surrounding Iran and Oman geography visible.

The map is genuinely interactive rather than a static screenshot.

### AC-03: Vessel Rendering

Multiple simulated vessels are visible as recognizable ship models.

Each displayed vessel has the corresponding country flag on
a small mast or pole.

### AC-04: Vessel Movement

Starting the simulation causes vessels to progress along
predefined routes.

Pausing stops progression; resuming continues it.

### AC-05: Vessel Inspection

Selecting a ship displays its configured identity, nationality,
cargo, route, status, and applicable toll information.

### AC-06: Toll Rules

A non-exempt vessel receives the correct configured hypothetical
toll when it completes a qualifying checkpoint encounter.

### AC-07: Exemptions

An exempt vessel produces no positive toll charge.

### AC-08: Revenue

A valid positive charge updates the appropriate authority revenue.

The combined revenue equals the sum of Iran and Oman revenue.

### AC-09: Duplicate Prevention

Repeated detection of the same checkpoint encounter does not
create duplicate charges.

### AC-10: Transactions

The system's transaction history corresponds to the actual
toll-processing outcomes.

### AC-11: Reset & Multi-Run Lifecycle

Reset closes the active simulation run and initializes a new run with $0.00 revenue,
an empty active-run transaction ledger, and vessels restored to their starting coordinates.
Previous run transactions remain accessible during the active server session as immutable
past-run history. All configured entities (countries, vessels, routes, checkpoints, toll tariffs)
strictly survive reset.

### AC-12: Data Transparency

The interface clearly identifies simulation data and hypothetical
toll policies.

It does not falsely represent the application as a live vessel
tracker or real governmental toll system.

### AC-13: Compatibility

The final application can be packaged and deployed in the
approved Java 8, Java EE 7, and GlassFish environment.

### AC-14: Regression Safety

Adding a supported configuration change, such as a new country
or modified toll rate, does not break unrelated existing features.

### AC-15: Country Management

The user can add a new country or edit an existing country (ISO code,
country name, SVG flag asset mapping, default toll tariff, exemption flag).
Form validation rejects duplicate ISO codes, empty names, and negative tariffs.

### AC-16: Toll Policy Management

The user can modify toll rates for any configured country and toggle
exemption status on or off. Edits strictly affect future checkpoint
encounters without mutating past transactions.

### AC-17: Vessel Management

The user can dynamically add a new simulated vessel or edit an existing
vessel's name, nationality, cargo category, or assigned route. Form validation
rejects duplicate vessel IDs, empty names, and invalid route assignments.

### AC-18: Checkpoint & Route Management

The user can modify checkpoint positions, detection radius, and route
waypoint coordinates. Validations enforce latitude $[-90, 90]$,
longitude $[-180, 180]$, and positive detection radius.

### AC-19: Vessel Search and Filtering

The user can search vessels by name/ID and filter by nationality, cargo,
and simulation status. Active filters update the visible registry and focus
without disturbing the underlying simulation state.

### AC-20: Transaction Ledger and Revenue Reporting

The system maintains an interactive transaction ledger logging every toll event
(BILLED, EXEMPT, BLOCKED_DUPLICATE). Aggregated reports display revenue
breakdown by authority, country, and cargo type.

### AC-21: CSV Report Export

The user can export transaction audit logs and aggregated revenue reports
as downloadable RFC 4180-compliant CSV files directly from the browser.

### AC-22: Simulation Speed Controls & Congestion Alerts

Simulation speed can be adjusted between demonstration presets (1x, 2x, and 4x) without skipping
checkpoint detection. Visual congestion warning badges appear when vessel density within a
corridor or checkpoint exceeds configurable thresholds (with illustrative defaults).

### AC-23: Historical Immutability & Reset Configuration Preservation

Altering a toll policy or exemption never recalculates past ledger entries.
Resetting the simulation closes the current run and begins a new active run with zero revenue
and an empty active ledger while preserving prior-run history in-session. All user-configured
countries, vessels, routes, checkpoints, and custom tariffs strictly survive reset.

---

## 12. Professor-Requested Change Workflow

The professor may request changes to functionality during the
practical examination.

Each new request must be recorded separately.

The process is:

1. Capture the requirement in clear language.
2. Classify it as a mandatory change or a proposed extension.
3. Identify its affected requirements.
4. Check compatibility and implementation impact.
5. Obtain approval for major scope or architecture changes.
6. Update the relevant documentation.
7. Implement the agreed change.
8. Test the changed behavior.
9. Run relevant regression tests.
10. Record completion in the change log.

Do not silently replace previously approved behavior.

If a new requirement conflicts with an existing requirement,
identify the conflict before making the change.

---

## 13. Assumptions and Open Decisions

The following details require later confirmation or refinement:

- Final visual design and layout details (specified in UI_UX_GUIDELINES.md).
- Final default list of demo vessels and sample toll amounts.
- Final default country exemptions and route waypoint coordinates.
- Exact checkpoint-crossing geometric algorithms.
- Whether backend servlets are required for every operation (DEC-010 server-authoritative tolling remains PROPOSED).
- Whether persistent database storage is necessary (in-memory/session state is confirmed for MVP).
- Google Maps API key availability and configuration.
- Exact visual scaling behavior at different map zoom levels.
- Whether genuine 3D ship models are worth implementing in future phases.

Do not treat proposed architecture decisions as finalized unless they are
explicitly approved and documented in DECISIONS.md.

---

## 14. Relationship to Other Documentation

brain.md:
Overall project context and non-negotiable constraints.

PRD.md:
Product requirements and expected behavior.

TRD.md:
Technical implementation, architecture, data flow, and
technology-specific decisions.

UI_UX_GUIDELINES.md:
Visual design and interaction specifications.

PROFESSOR_CHANGELOG.md:
Additional requests and their implementation status.

IMPLEMENTATION_PLAN.md:
Build phases and development progress.

TESTING_PLAN.md:
Test cases and verification workflow.

DECISIONS.md:
Approved technical and design decisions with reasons.

DEMO_GUIDE.md:
Steps for demonstrating the working application.

README.md:
Setup, configuration, and run instructions.

These documents must remain consistent with each other.

---

## 15. Current PRD Status

This PRD defines the initial product scope and intended behavior.

It does not authorize implementation by itself.

Before development begins, the technical architecture, design
guidelines, and implementation plan must be reviewed and approved.

Status: Draft for Review
