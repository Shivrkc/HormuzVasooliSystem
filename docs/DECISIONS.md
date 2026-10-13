# Hormuz Vasooli System — Decision Register

## 1. Purpose

This document records important product, architecture, design,
and implementation decisions.

Its purpose is to prevent contradictory decisions, unnecessary
rewrites, and undocumented changes during development.

Every decision must have:
- A unique ID.
- A clear description.
- A status.
- A reason.
- Any relevant consequences.

Possible statuses:

- CONFIRMED: Explicitly established by the user.
- PROPOSED: Recommended but not yet explicitly approved.
- NEEDS-VERIFICATION: Requires testing or technical evidence.
- REJECTED: Explicitly ruled out.
- SUPERSEDED: Replaced by a newer decision.

Never silently change a confirmed decision.

Never treat a proposed decision as confirmed.

## 2. Confirmed Product Decisions

### DEC-001: Project Identity

Status: CONFIRMED

Decision:
The project is named Hormuz Vasooli System.

Reason:
The user explicitly selected this project name.

### DEC-002: Project Purpose

Status: CONFIRMED

Decision:
Build an educational web application that simulates ship
movement through the Strait of Hormuz and hypothetical toll
collection associated with Iran and Oman.

Reason:
This reflects the professor's described practical scenario.

Constraint:
Simulated toll rates and collections must not be presented
as actual governmental maritime toll policies.

### DEC-003: Existing Development Environment

Status: CONFIRMED

Decision:
The existing environment is based on NetBeans IDE 8.2,
Java 8, Maven, Java EE 7 Web, and GlassFish 4.1.1.

Reason:
These components have been identified in the user's existing
development environment.

Constraint:
Do not migrate to another framework or application server
without explicit user approval.

### DEC-004: Mapping Technology

Status: CONFIRMED

Decision:
Use Google Maps JavaScript API to display the Strait of Hormuz.

Constraint:
A valid API key and appropriate configuration are required.
Do not claim the map is operational until tested.

### DEC-005: Custom Ship Representation

Status: CONFIRMED

Decision:
Ships must use recognizable ship-shaped models rather than
ordinary arrow markers alone.

The visual concept includes a small mast or pole carrying
the corresponding country's flag.

Reason:
The user explicitly requested realistic miniature ship models
with flags attached above them.

### DEC-006: Simulation Scope

Status: CONFIRMED

Decision:
The initial application uses simulated vessel movement and
hypothetical toll policies.

Constraint:
Do not claim live AIS tracking or actual toll collection.

### DEC-007: Separate Revenue Totals

Status: CONFIRMED

Decision:
Display simulated Iran revenue, simulated Oman revenue,
and combined simulated revenue separately.

Constraint:
The combined amount must remain consistent with the
two individual totals.

### DEC-008: Documentation-First Workflow

Status: CONFIRMED

Decision:
Prepare and review project documentation before beginning
the main application implementation.

Reason:
The user wants the AI coding agent to follow an agreed plan
and remain within the project's scope.

Constraint:
Do not start implementation merely because a planning
document has been created.

### DEC-009: Visual Reference

Status: CONFIRMED

Decision:
Use the user-selected Ledgerix dashboard image as visual
inspiration.

Reference path:
docs/references/visual-reference.png

Constraint:
The image is a visual reference, not permission to copy
unrelated product features or replace the functional map
with a static image.

### DEC-026: Expanded Initial-Build Scope for Practical Examination Readiness

Status: CONFIRMED

Decision:
Include common, predictable practical examination modifications
directly in the initial build as working user-facing capabilities:
- Interactive country management (add/edit country, ISO code, SVG flag mapping, default tariff, exemption status).
- Toll policy management (country-specific tariffs, cargo multipliers, exemption toggles).
- Dynamic vessel management (add/edit vessel, cargo, nationality, route).
- Route and checkpoint management (add/modify coordinates, detection radius).
- Real-time vessel search and multi-attribute filtering (nationality, cargo, status).
- Interactive transaction ledger with itemized event inspection.
- Revenue analytics and RFC 4180-compliant CSV report export.
- Configurable simulation speed controls with demonstration presets (1x, 2x, 4x).
- Visual corridor and checkpoint congestion warning alerts based on configurable parameters (waiting count threshold and proximity radius); defaults are illustrative academic simulation data, not official regulations.
- Map layer visibility toggles (routes, checkpoints, labels).
- Robust client-side and form validations.

Reason:
The user explicitly directed that likely examination requests should
be built and verified before the examination rather than merely
specifying an architecture that could support them in the future.

Constraint:
Strict scope boundaries remain fully enforced: no live AIS data,
no real-world payment processing, no external government integrations,
no mandatory user authentication, no mandatory database persistence,
no foreign exchange APIs, and no genuine 3D WebGL rendering.

### DEC-027: Historical Ledger Immutability and Multi-Run Reset Behavior

Status: CONFIRMED

Decision:
1. Historical Immutability: Modifications to a toll policy or exemption
apply strictly to subsequent checkpoint encounters; existing transaction
ledger records and finalized revenue must never be retroactively altered
in either active or prior runs.
2. Multi-Run Simulation Reset: Resetting closes the current simulation run
and begins a new active run with zero revenue ($0.00 USD), an empty active-run
ledger, and vessels restored to their starting route coordinates.
3. Past-Run Session History: Transactions assessed in prior runs remain accessible
during the active server session as immutable historical archives for examination audit.
4. In-Memory Persistence Boundary: In-memory session history may reset if the server
restarts because external database persistence is not approved for MVP.
5. Configuration Preservation: Resetting the simulation run strictly clears dynamic run state;
all user-configured countries, vessels, toll tariffs, exemptions, routes, and checkpoints
must strictly survive simulation reset.

Reason:
Guarantees financial auditability, resolves contradictions between ledger immutability
and simulation reset, and ensures custom demonstration scenarios configured during an
examination survive simulation restarts.

## 3. Proposed Technical Decisions

The following decisions have been recommended in TRD.md
but must remain proposals until explicitly approved.

### DEC-010: Server-Authoritative Toll Processing

Status: PROPOSED

Proposal:
Use a Java backend service as the authoritative source for
toll assessment, exemptions, transaction records, and revenue.

Reason:
This separates financial rules from browser animation code.

Approval needed:
Confirm the intended frontend/backend division before
implementation.

### DEC-011: In-Memory Initial Simulation State

Status: PROPOSED

Proposal:
Use in-memory server state for the initial single-user demo.

Reason:
This avoids introducing unnecessary database complexity.

Consequence:
Transactions may be lost when the application restarts.

Approval needed:
Confirm whether this persistence limitation is acceptable.

### DEC-012: JSON Simulation Configuration

Status: PROPOSED

Proposal:
Use a structured configuration file for ships, routes,
checkpoints, country information, toll rules, and exemptions.

Proposed path:
src/main/resources/simulation-config.json

Reason:
Configuration-driven rules should make examination changes
easier to implement.

Approval needed:
Confirm the exact configuration structure during TRD review.

### DEC-013: REST-Like Servlet Endpoints

Status: PROPOSED

Proposal:
Use Java Servlets and JSON responses for configuration,
simulation state, toll assessment, and reset operations.

Reason:
This provides a defined interface between the browser
and the Java backend.

Approval needed:
Finalize endpoint responsibilities and request contracts
before implementation.

### DEC-014: Idempotent Toll Transactions

Status: PROPOSED

Proposal:
Use a transaction identity based on the simulation run,
vessel, checkpoint, and passage.

Reason:
Repeated animation callbacks must not produce duplicate charges.

Approval needed:
Finalize the passage model and transaction lifecycle
during technical design review.

### DEC-015: Java 8 Compiler Configuration

Status: NEEDS-VERIFICATION

Proposal:
Verify the Maven compiler configuration and use Java 8
source and target compatibility if required.

Reason:
The existing Maven configuration reports source/target 1.7,
while the installed JDK is Java 8.

Constraint:
Do not modify pom.xml without reviewing the current
configuration and obtaining approval for the change.

## 4. Proposed UI/UX Decisions

### DEC-016: Map-Centered Operations Dashboard

Status: PROPOSED

Proposal:
Use a map-dominant dashboard with compact navigation,
vessel details, financial summaries, and simulation controls.

Reason:
The map and simulated ships are the main functional elements.

Approval needed:
Finalize panel placement and screen proportions before
implementation.

### DEC-017: Selective Glassmorphism

Status: PROPOSED

Proposal:
Use translucent panels selectively, inspired by the
selected visual reference.

Constraint:
Preserve readability and map visibility.

Approval needed:
Verify the final density of transparency and blur effects
during visual review.

### DEC-018: Dark Maritime Palette

Status: PROPOSED

Proposal:
Use deep navy or charcoal surfaces with orange accents
and contrasting route colours.

Reason:
This supports the selected maritime operations visual direction.

Approval needed:
Finalize exact colour tokens and contrast requirements.

### DEC-019: Typography System

Status: PROPOSED

Proposal:
Evaluate Manrope for headings, Inter for interface text,
and IBM Plex Mono for numerical data.

Constraint:
Maintain readable fallback fonts and avoid introducing
an unnecessary build system merely to load fonts.

Approval needed:
Finalize font choices in UI/UX review.

### DEC-020: SVG Ship Models and Local Flag Assets

Status: PROPOSED

Proposal:
Use top-down SVG ship models and locally stored flag assets.

The mast, flag, and ship must remain coordinated as one
visual marker.

The displayed flag must correspond to the vessel's configured
nationality.

Constraint:
Do not use Unicode flag emojis as the primary ship-marker
flag representation.

Approval needed:
Verify the marker design and zoom behavior through testing.

### DEC-021: Zoom-Dependent Marker Dimensions

Status: NEEDS-VERIFICATION

Proposal:
Use controlled scaling or bounded dimensions to maintain
ship visibility across supported map zoom levels.

Constraint:
Avoid abrupt size jumps, flag overlap, and markers that
obscure the vessel.

Approval needed:
Finalize exact pixel dimensions after testing the
actual map rendering.

## 5. Rejected Approaches

### DEC-022: Unapproved Framework Migration

Status: REJECTED

Decision:
Do not migrate to React, Vite, Spring Boot, or another
framework without explicit user approval.

Reason:
The project must preserve the existing Java web application
environment and remain suitable for the practical examination.

### DEC-023: Static Screenshot as the Main Map

Status: REJECTED

Decision:
Do not replace the interactive Google Map with a static
dashboard image.

Reason:
Map interaction and simulated ship movement are core requirements.

### DEC-024: Unverified Live-Data Claims

Status: REJECTED

Decision:
Do not describe simulated ships, rates, routes, or revenue
as real-time or verified data.

Reason:
The initial project does not include a verified live
maritime data feed or actual governmental toll policies.

### DEC-025: Decorative-Only Functionality

Status: REJECTED

Decision:
A visible control or dashboard metric must not be presented
as functional unless its behavior is implemented and tested.

Reason:
The application is intended for a live practical demonstration.

## 6. Open Decisions

The following architectural and design questions remain open for verification:

1. Approval status of DEC-010: Server-authoritative toll assessment servlet pipeline.
2. Approval status of DEC-012: `simulation-config.json` exact seed schema on classpath.
3. Approval status of DEC-013: Exact REST servlet endpoint structure.
4. Approval status of DEC-014: Specific compound server idempotency key format (`runId:vesselId:checkpointId:passageId`).
5. Final default seed values for vessels, routes, and checkpoint coordinates.
6. Verification of NetBeans 8.2 Maven compiler plugin upgrade to Java 8 (`1.8`) in Phase 1.
7. Verification of Google Maps `OverlayView` vs inline SVG markers for rotation and flag counter-rotation performance.
8. What build and deployment tests are required for final sign-off?

Do not treat proposed decisions as confirmed until formally approved.

## 7. Decision Change Procedure

When a decision must change:

1. Identify its decision ID.
2. Explain why the existing choice is insufficient.
3. Identify affected documents and components.
4. Describe compatibility and testing consequences.
5. Obtain approval when the change materially affects
   architecture, product scope, or visual direction.
6. Update this register.
7. Update dependent documents.
8. Implement only after the change is approved.
9. Verify the resulting implementation.

Keep superseded decisions in the record with their status
updated instead of erasing important historical context.

## 8. Relationship to Other Documents

brain.md:
Overall project context and constraints.

PRD.md:
Functional requirements and acceptance criteria.

TRD.md:
Technical architecture and implementation plan.

UI_UX_GUIDELINES.md:
Visual design specifications.

PROFESSOR_CHANGELOG.md:
Additional examination requirements.

IMPLEMENTATION_PLAN.md:
Development phases and progress.

TESTING_PLAN.md:
Testing strategy and cases.

DEMO_GUIDE.md:
Practical examination demonstration.

README.md:
Setup and usage instructions.

This document records the status and rationale of decisions
rather than duplicating the complete contents of other files.

Document Status: Draft for Review
