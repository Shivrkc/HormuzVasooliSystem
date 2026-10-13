# Hormuz Vasooli System — Project Brain

## 1. Purpose of This Document

This document is the central project-context and project-control
document for Hormuz Vasooli System.

It explains:
- What the project is.
- Why the project is being built.
- What the professor has explicitly requested.
- Which features are agreed upon for the initial implementation.
- Which features are only possible future extensions.
- Which technologies and technical restrictions must be preserved.
- How new requirements and changes must be handled.
- How the AI coding agent must work on this project.

This document provides the overall project context.

Detailed functional requirements will be maintained in PRD.md.
Technical architecture and implementation details will be maintained
in TRD.md.
Visual design decisions will be maintained in UI_UX_GUIDELINES.md.

Those documents may not exist yet. Do not assume they have already
been created or approved.

Do not invent requirements to fill missing documentation.

---

## 2. Project Identity

Project Name: Hormuz Vasooli System

Short Name: HVS

Project Type: Interactive educational web application and
maritime-toll simulation for a college practical examination.

Primary Purpose:

Build an interactive website that displays the Strait of Hormuz
on Google Maps, simulates the movement of ships, identifies their
nationalities, displays country flags, applies configurable
hypothetical toll rules, and calculates separate simulated revenue
for Iran and Oman.

The application must be understandable, demonstrable, and sufficiently
modular to support additional requirements introduced by the professor
during the practical examination.

This is a simulation project. It is not an actual maritime authority,
governmental toll collection platform, or real-time vessel surveillance
system.

All simulated toll policies, rates, checkpoints, transactions, and
revenue figures must be clearly identified as fictional demonstration
data unless independently verified data sources are added later.

---

## 3. Current Project Environment

The existing development environment has been inspected.

IDE:
- NetBeans IDE 8.2.

Programming Language:
- Java 8.
- Installed JDK reported as 1.8.0_181.

Build System:
- Maven.

Application Type:
- Java Maven Web Application.

Enterprise Platform:
- Java EE 7 Web.

Application Server:
- GlassFish Server 4.1.1.

Frontend Technologies:
- HTML.
- CSS.
- Vanilla JavaScript.

Mapping:
- Google Maps JavaScript API.

Backend:
- Java Servlets and compatible Java EE 7 APIs where required.

Project Package:
- com.hormuzvasooli.

Current Project Structure:

HormuzVasooliSystem/
├── pom.xml
├── nb-configuration.xml
└── src/
    └── main/
        ├── java/
        │   └── com/
        │       └── hormuzvasooli/
        └── webapp/
            └── index.html

The project is a newly initialized Maven Web Application.

The inspected Maven configuration currently uses Java source and
target version 1.7, while the installed JDK is Java 8.

Any necessary compatibility adjustment must be evaluated separately
before implementation. Do not modify pom.xml as part of creating
this document.

The existing project location is:

C:\Users\godbl\OneDrive\Documents\NetBeansProjects\
HormuzVasooliSystem

The application must remain compatible with the existing
NetBeans, Maven, Java 8, Java EE 7, and GlassFish environment.

Do not migrate the project to a different application framework
without explicit approval.

---

## 4. Original Professor Requirements

The following requirements have been communicated by the professor.

These form the core scope of the project.

### 4.1 Google Maps Integration

Integrate Google Maps into the website using the Google Maps API.

The initial map view must focus on the Strait of Hormuz and
show the surrounding geography of Iran and Oman.

### 4.2 Ship Visualization

Show multiple ships at different positions in and around the
Strait of Hormuz.

Ships may appear in a queue or at different positions along their
simulated routes.

The map must display the ships clearly and allow the user to
observe their movement.

### 4.3 Ship Information

Ships represent vessels from different countries.

The application should associate each ship with a nationality
and corresponding country flag.

The demonstration data may represent cargo such as:
- Crude oil.
- LPG.
- Medicines.
- Food grains.
- Other commodities.

These cargo types are examples for simulation purposes.

### 4.4 Country-Based Hypothetical Toll Rules

Different countries may have different toll amounts.

Toll amounts must be expressed in US dollars.

Certain configured friendly countries may be exempt from tolls.

Toll amounts, country classifications, and exemptions must be
configurable rather than scattered throughout the source code.

### 4.5 Separate Revenue Calculation

The application must calculate and display:
- Simulated revenue collected by Iran.
- Simulated revenue collected by Oman.
- Combined simulated revenue.

Revenue must update consistently when valid simulated toll
transactions occur.

Do not invent actual governmental toll rates or represent the
simulation as a description of real-world toll arrangements.

### 4.6 Custom Ship Models and Flags

An additional requirement established during project planning is
that ships should not be represented only by ordinary arrows.

Ships should use recognizable miniature ship models on the map.

The intended appearance includes:
- A ship-shaped model viewed from above.
- A small mast or pole.
- The corresponding country's flag on that pole.
- Ship orientation that follows the vessel's movement direction.
- Sufficient visual clarity at different map zoom levels.

SVG-based custom markers are the preferred starting approach.
A genuine 3D rendering system is not mandatory unless later
approved as a requirement.

The ship model, mast, and flag should behave as one coordinated
visual unit.

A ship icon must not be considered complete merely because a
generic arrow has been recolored.

---

## 5. Agreed Product Direction

The application will be an interactive operations dashboard rather
than a marketing or promotional website.

The main screen should prioritize the map and ship simulation.

The intended high-level experience is:

1. The user opens the application.
2. The map displays the Strait of Hormuz.
3. Multiple simulated ships appear on the map.
4. Each ship displays a recognizable ship model and country flag.
5. The user starts or controls the simulation.
6. Ships move along predefined routes.
7. Ships encounter configured hypothetical toll checkpoints.
8. The toll engine evaluates the applicable rules.
9. A valid transaction is recorded.
10. Iran's or Oman's simulated revenue is updated.
11. The dashboard reflects the current simulation state.
12. The user can inspect vessels and review their toll records.

The frontend should be designed for an educational demonstration:
clear, interactive, visually polished, and easy to explain during
a practical examination.

Do not prioritize decorative presentation over working functionality.

---

## 6. Confirmed Initial Application Capabilities

The initial product baseline has been expanded upon explicit user decision to include practical configuration, management, and examination-readiness features as working capabilities BEFORE the practical examination, rather than treating them as hypothetical future extensions.

### 6.1 Map and Navigation Workspace
- Display Google Maps centered on the Strait of Hormuz with surrounding Iran and Oman geography.
- Display configured vessel routes and hypothetical toll checkpoints with interactive layer controls (show/hide routes, show/hide checkpoints).
- Support full map zoom and navigation controls.
- Provide clear visual indicators for Iranian and Omani waters and checkpoint zones.

### 6.2 Vessel Simulation & Custom Models
- Maintain structured, configurable vessel records.
- Render recognizable top-down SVG ship hulls, visible centered mast poles, and local SVG national flags (no Unicode emojis).
- Heading-aware hull rotation paired with counter-rotated ($-\theta$) upright flags for permanent readability.
- Simulate movement along predefined waypoints with linear interpolation and status transitions (`WAITING`, `PASSING`, `PASSED`).
- Display visible congestion alerts when vessels in a sector exceed configurable demonstration thresholds (such as waiting vessel count and geographic proximity radius). These illustrative defaults are academic demonstration parameters, not real-world maritime safety regulations.

### 6.3 Management & Configuration Interfaces
To empower the student during practical examination questioning, the application includes functional management interfaces:
- **Country Management**: Add countries, edit country metadata, assign unique IDs, associate local SVG flag assets, validate duplicates, and handle missing flag fallbacks gracefully.
- **Toll Policy Management**: Configure separate hypothetical Iran and Oman rates, modify fees in USD, toggle exemptions on/off, apply updated rates to future transits, and preserve historical transaction amounts.
- **Vessel Management**: Add vessels, edit vessel details, remove vessels safely, assign nationality, flag, cargo, origin, destination, and route, and validate missing fields or duplicate IDs.
- **Route Management**: Define routes with configurable waypoints, edit coordinates, assign routes to vessels, and update future simulation movement without breaking active transit.
- **Checkpoint Management**: Add hypothetical checkpoints, edit coordinates and radii, assign simulated authority (Iran/Oman), enable/disable checkpoints, and update map visualization without erasing transaction history.

### 6.4 Search, Filtering, and Inspection
- Real-time search by vessel name and vessel ID.
- Multi-criteria filtering by nationality, cargo type, simulation status, and toll status without mutating simulation state or financial totals.
- Selected vessel inspection drawer displaying full manifest, destination, and itemized toll receipts.

### 6.5 Transaction Ledger, Revenue Reports & CSV Export
- Authoritative transaction ledger recording event outcomes: positive charges (`CHARGED`), friendly nation exemptions (`EXEMPT`), duplicate events (`DUPLICATE`), and rejected submissions.
- Real-time revenue tallies obeying $\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$.
- Revenue reports with country-wise and checkpoint-wise breakdowns.
- CSV export for transaction ledger and revenue summary data.

### 6.6 Simulation Controls & Reset Separation
- Interactive controls: Start, Pause, Resume, Reset, and simulation-speed selection with configurable demonstration presets (1x, 2x, 4x).
- Layer toggles to show or hide route lines and checkpoint catchment zones.
- **Strict Reset Separation & Session History**: Resetting closes the current simulation run and starts a new active run with zero revenue, empty active-run ledger, and vessels returned to initial coordinates. Previous run transactions remain accessible during the active server session as immutable past-run history. In-memory run records may be lost upon server restart as persistent database storage is not approved. Custom configuration (countries, toll policies, vessels, routes, and checkpoints) strictly survives reset. Toll-rate or exemption edits never retroactively alter past assessed transactions.

---

## 7. Scope Boundaries and Excluded Features

The expansion of initial-build management features does NOT authorize unrelated production or external capabilities.

### 7.1 Features Strictly Excluded from Initial Build
Unless separately and explicitly approved, the initial application does NOT include:
- Live AIS vessel surveillance or external radar feeds.
- Real payment processing, banking gateways, or financial clearinghouses.
- Government, military, or official port authority API integrations.
- Mandatory user login, authentication, or multi-role permission systems.
- Mandatory database persistence (in-memory state is maintained for the local examination demo).
- Live currency-conversion network APIs (all simulation tariffs remain in USD).
- Genuine 3D WebGL/Three.js ship rendering (lightweight SVG markers fulfill the visual requirement).

### 7.2 Handling Genuine Future Examination Changes
While common modifications (adding countries, altering rates, rerouting vessels) are now supported via built-in management interfaces, the professor may still introduce unforeseen requirements during the practical:
1. Capture the exact instruction verbatim in `docs/PROFESSOR_CHANGELOG.md` under a new `CHG-XXX` ID.
2. Evaluate technical, architectural, and compatibility impact against the Java 8 / GlassFish 4.1.1 baseline.
3. Check for conflicts against existing approved decisions in `docs/DECISIONS.md`.
4. Update affected project documentation before touching code.
5. Implement only the approved scope with surgical precision.
6. Execute relevant feature and regression tests, then record the result.

---

## 8. Core Architectural Principles

These principles must guide implementation.

### 8.1 Modularity

Keep responsibilities separated.

Conceptual modules include:
- Map rendering.
- Ship visualization.
- Ship simulation.
- Toll policy evaluation.
- Checkpoint processing.
- Transaction records.
- Revenue aggregation.
- Dashboard presentation.
- Backend communication, where required.

Avoid placing the entire application in one oversized JavaScript
file or one oversized Java class.

Exact file organization must be decided in TRD.md.

### 8.2 Configuration-Driven Rules

Ship data, country information, toll rates, exemptions, routes,
and checkpoints should be represented as structured data.

Avoid embedding country-specific toll rules repeatedly in
unrelated functions.

Adding a country or modifying a configured rate should require
minimal changes to the business logic.

### 8.3 Separation of Responsibilities

Ship movement and financial calculations are different concerns.

The visual movement of a ship must not directly manipulate
revenue counters without passing through the toll-processing logic.

Revenue totals must be derived from valid transaction records
or another explicitly defined authoritative data structure.

Keep map rendering, simulation state, toll decisions, and dashboard
presentation logically separate.

### 8.4 Reliable Transactions

A ship must not be charged repeatedly simply because the animation
continues, the ship remains near a checkpoint, or a callback fires
multiple times.

Define transaction identity and duplicate-prevention rules.

A vessel may encounter both configured checkpoints during a
simulated transit. A transaction at one checkpoint must not
incorrectly suppress the separate valid transaction at the other.

The exact transaction identity and transit model will be defined
in TRD.md.

### 8.5 Testability

Important business rules should be testable independently from
the visual map.

Country exemptions, toll calculations, revenue aggregation,
and duplicate-prevention behavior should not require manually
watching ship animations to verify their correctness.

---

## 9. Technical Restrictions

The following constraints apply unless an explicit approved
decision changes them.

- Preserve NetBeans IDE 8.2 compatibility.
- Target Java 8.
- Preserve the Maven Web Application structure.
- Maintain Java EE 7 compatibility.
- Maintain GlassFish Server 4.1.1 deployment compatibility.
- Use HTML, CSS, and vanilla JavaScript for the initial frontend.
- Use Java Servlets and compatible Java EE APIs where a backend
  is required.
- Use the Google Maps JavaScript API for the map.
- Use configurable simulation data.
- Avoid unnecessary external dependencies.

Do not migrate to:
- React.
- Vite.
- Spring Boot.
- A different application server.
- A different Java enterprise platform.

Do not perform such a migration without explicit user approval.

Do not assume that the latest JavaScript package or modern Java API
will work in the existing environment.

Verify compatibility before introducing new dependencies.

Do not modify pom.xml or deployment configuration without first
explaining why the change is needed and obtaining approval when
the change materially affects project configuration.

Do not introduce a database simply because a database is commonly
used in production applications. The need for persistent storage
has not yet been finalized.

---

## 10. Google Maps API Configuration

Google Maps JavaScript API requires appropriate API configuration.

The application must not assume that a usable API key already exists.

Requirements:
- Use a clearly identified configuration placeholder.
- Keep the API key out of public source-control commits.
- Document the configuration process.
- Provide a meaningful error or guidance when the key is missing.
- Do not claim that live map functionality has been tested unless
  it has actually been tested.

Live ship tracking is outside the initial scope.

The initial application must use clearly identified simulated
ship positions and predefined routes.

---

## 11. Data Integrity and Simulation Disclaimer

The application must clearly communicate that:

- Ship records are simulated unless an actual source is later added.
- Ship movement is simulated.
- Toll rates are hypothetical.
- Toll exemptions are fictional configuration rules.
- Checkpoints are elements of the educational simulation.
- Revenue figures are simulated calculations.

The interface must not present hypothetical Iran or Oman toll
collection as established real-world maritime policy.

The simulation should remain usable even when live vessel data
is unavailable because live vessel tracking is not an initial
requirement.

---

## 12. Visual Design Direction

A user-selected dashboard reference image is intended to be stored
inside the project folder and used as a visual reference.

The current visual direction is:

- Premium operations-dashboard appearance.
- Strong map-centered layout.
- Carefully arranged asymmetric information panels.
- Selective glassmorphism and translucent surfaces.
- Restrained accent colors.
- Clear typography and spacing.
- High readability.
- Professional icons.
- Functional animation rather than decorative motion.

The reference is an inspiration for visual style and hierarchy.
It is not an instruction to copy its exact layout or content.

The final layout, palette, typography, density, and glassmorphism
usage have not all been finalized.

Keep unresolved visual choices provisional until they are explicitly
approved and recorded in UI_UX_GUIDELINES.md.

The application must remain functional and readable when map
details appear behind translucent panels.

Avoid:
- Generic AI-generated dashboard styling.
- Excessive rounded cards.
- Unnecessary gradients.
- Purple neon effects without a design reason.
- Excessive blur.
- Fake statistics.
- Decorative charts without meaningful data.
- Fake testimonials.
- Emoji used instead of suitable interface icons.
- Oversized marketing hero sections.
- Unnecessary animations.

The map and vessel simulation are the core visual elements.

---

## 13. AI Coding Agent Working Rules

The AI coding agent must follow these rules throughout the project.

### Before Editing
- Inspect the relevant existing files.
- Read the relevant project documentation.
- Identify affected components and dependencies.
- Preserve working functionality.
- Explain material architectural changes before implementing them.

### While Editing
- Make focused, incremental changes.
- Avoid rewriting unrelated files.
- Preserve the current technology stack.
- Keep code modular and readable.
- Avoid speculative features.
- Never silently change confirmed requirements.
- Do not add fake functionality or pretend that an unimplemented
  feature is operational.

### After Editing
- Report the files changed.
- Explain the behavior implemented.
- Run relevant build or verification steps where possible.
- Report actual errors instead of claiming success.
- Record unfinished work and known limitations.
- Update the implementation plan and change log when appropriate.

### When Requirements Are Ambiguous
Do not silently invent major requirements.

For a low-risk implementation detail, choose a reasonable default
and document it.

For a major product, architecture, data, or design decision,
ask for clarification or present the decision for approval.

### When a Professor Requests a Change
Treat the new instruction as a proposed requirement until its
scope and consequences are understood.

Record it, check for conflicts, and implement the approved change
with appropriate testing.

---

## 14. Documentation Authority

The project will use separate documents for distinct responsibilities.

brain.md:
Overall project context, constraints, requirements status, and
agent working principles.

PRD.md:
Detailed functional requirements and expected user behavior.

TRD.md:
Technical architecture, component responsibilities, data flow,
and implementation decisions.

UI_UX_GUIDELINES.md:
Visual design, layout, typography, colors, and interaction rules.

PROFESSOR_CHANGELOG.md:
Requests introduced during practical preparation or examination.

IMPLEMENTATION_PLAN.md:
Development phases, progress, dependencies, and completion status.

TESTING_PLAN.md:
Test cases, acceptance criteria, and regression checks.

DECISIONS.md:
Approved technical and design decisions with reasons.

DEMO_GUIDE.md:
Practical-examination demonstration workflow.

README.md:
Project overview, setup, configuration, and run instructions.

These files will be created and finalized incrementally.

Do not assume that a document exists until it has been created
and verified.

Do not copy the complete contents of every document into brain.md.

If two documents conflict, do not silently pick whichever is more
convenient. Identify the conflict and resolve it through an explicit
decision.

---

## 15. Current Project Status

Completed:
- NetBeans Maven Web Application created.
- Java 8 environment identified.
- GlassFish Server 4.1.1 identified.
- Initial project structure inspected.
- Initial Antigravity architecture proposal reviewed.
- Premium dashboard visual reference selected by the user,
  with the image intended to be added to the project folder.

Not yet completed:
- Documentation set.
- Final PRD.
- Final TRD.
- Final UI/UX specification.
- Google Maps API configuration.
- Ship icons and flags.
- Ship movement simulation.
- Toll engine.
- Revenue dashboard.
- Backend endpoints, if required by the approved architecture.
- Testing and deployment verification.

Do not report these unimplemented features as complete.

Current task:
Create docs/brain.md only.

After creating the file:
1. Confirm its exact path.
2. Confirm that it was saved successfully.
3. Report whether any existing files were modified.
4. Do not begin implementation.
5. Do not create the other documentation files yet.
