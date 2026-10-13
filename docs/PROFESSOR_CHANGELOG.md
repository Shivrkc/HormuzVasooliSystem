# Hormuz Vasooli System — Professor Change Log

## 1. Document Purpose

This document is the official change-control ledger for tracking new requirements, scope modifications, and feature variations requested by the professor during practical examination preparation or during the live college practical examination.

The initial baseline requirements of Hormuz Vasooli System are formally documented in [docs/brain.md](brain.md), [docs/PRD.md](PRD.md), [docs/TRD.md](TRD.md), [docs/UI_UX_GUIDELINES.md](UI_UX_GUIDELINES.md), and [docs/DECISIONS.md](DECISIONS.md).

This changelog serves to:
- Formally separate original project requirements from requests introduced later by the professor.
- Maintain a disciplined, auditable record of each new instruction.
- Prevent unapproved scope creep, contradictory implementations, or silent breaking of existing features.
- Provide a standardized protocol for evaluating technical impact, compatibility, and regression safety before modifying code.

---

## 2. Change Identification System

Every new professor instruction must be assigned a unique, sequential, and immutable identifier:

$$\text{CHG-001}, \quad \text{CHG-002}, \quad \text{CHG-003}, \quad \dots$$

Once assigned, an identifier must never be deleted or reassigned to another instruction.

### 2.1 Standard Record Fields
Each recorded change entry must capture:
- **Change ID**: Immutable identifier (`CHG-XXX`).
- **Date Recorded**: Date and time the request was communicated.
- **Original Professor Instruction**: Verbatim statement of the professor's instruction as reported by the user (without embellishment or assumed additions).
- **Interpreted Requirement**: Precise technical translation of the expected system behavior.
- **Reason / Educational Objective**: Educational context or practical examination scenario motivating the request.
- **Status**: Lifecycle status (see Section 3).
- **Priority**: `CRITICAL`, `HIGH`, `MEDIUM`, or `LOW`.
- **Affected Product Requirements**: Referenced sections in `PRD.md`.
- **Affected Technical Components**: Java packages, Servlets, frontend JavaScript modules, JSON configurations, or documentation files.
- **Potential Conflicts**: Identified contradictions with existing approved decisions in `DECISIONS.md`.
- **Proposed Solution**: Architectural approach for fulfilling the request.
- **Approval Status**: Explicit confirmation before implementation begins.
- **Files Expected to Change**: Complete list of target files.
- **Acceptance Criteria**: Measurable verification checkboxes.
- **Verification & Testing Results**: Evidence of unit, API, visual, or manual test execution.
- **Regression Testing Results**: Confirmation that existing features continue operating correctly.
- **Known Limitations**: Any operational boundaries or unhandled edge cases.
- **Completion Date**: Timestamp when the verified change is signed off.

*Note: If the professor's instruction is ambiguous or underspecified, the ambiguity must be explicitly documented under `NEEDS_CLARIFICATION` rather than filled in by inventing unapproved assumptions.*

---

## 3. Change Status Lifecycle

Every request progresses through a strict, transparent lifecycle:

```
[Professor Instruction Received]
               │
               ▼
           RECORDED
               │
        ┌──────┴─────────────────────────┐
        ▼                                ▼
NEEDS_CLARIFICATION                  PROPOSED
        │                                │
        └──────────────┬─────────────────┘
                       ▼
                    APPROVED ──────────► REJECTED
                       │
                       ▼
                  IN_PROGRESS ─────────► BLOCKED
                       │
                       ▼
                  IMPLEMENTED
                       │
                       ▼
                    TESTED
                       │
                       ▼
                   COMPLETED
```

### 3.1 Status Definitions
- **`RECORDED`**: The instruction has been captured in the changelog but has not yet undergone technical impact analysis.
- **`NEEDS_CLARIFICATION`**: The request is ambiguous, contradictory, or underspecified; implementation is suspended pending user clarification.
- **`PROPOSED`**: A technically sound solution has been designed and documented, awaiting user sign-off.
- **`APPROVED`**: The solution, scope, and affected files have been formally agreed upon.
- **`IN_PROGRESS`**: Active coding, configuration editing, or testing has begun.
- **`IMPLEMENTED`**: Code and configuration changes are in place, but verification is incomplete.
- **`TESTED`**: The new behavior has successfully passed its defined acceptance criteria.
- **`COMPLETED`**: Feature testing, regression tests, and documentation updates have all passed without errors.
- **`REJECTED`**: The instruction will not be implemented (reason recorded in the log).
- **`BLOCKED`**: Progress cannot proceed due to an environmental, platform, or dependency obstacle.

*A change must never be marked `COMPLETED` merely because code was written or generated. Full verification is mandatory.*

---

## 4. Request Handling & Implementation Workflow

When the user receives a new instruction from the professor during practical preparation or the live exam:

1. **Capture Verbatim**: Record the exact words of the instruction under a new `CHG-XXX` ID.
2. **Interpret Scope**: Determine precisely what the system must do differently.
3. **Check Environmental Compatibility**: Verify that the requested behavior is compatible with:
   - Java 8 (`1.8.0_181`)
   - Java EE 7 Web Profile (`javax.*` namespace, Servlet 3.1, JSON-P)
   - GlassFish Server 4.1.1
   - Maven Web Application structure
   - Vanilla HTML5, CSS3, and ES6 JavaScript (no modern npm/React/Vite frameworks)
4. **Conflict Analysis**: Cross-check against `DECISIONS.md` to ensure the request does not violate confirmed architectural principles (e.g., server-authoritative tolling, separate Iran/Oman revenues, simulation disclaimers).
5. **Classify Impact Level**:
   - *Level 1 (Configuration Only)*: Adding a new country, changing a toll rate, granting an exemption, or adding a ship (modified only in `simulation-config.json` and `assets/flags/`).
   - *Level 2 (Feature Extension)*: Adding a search filter, ledger view, or CSV export.
   - *Level 3 (Major Architectural Change)*: Persistent database, live data feeds, or framework migrations (requires explicit warning, impact assessment, and formal approval).
6. **Define Acceptance Criteria**: Write verifiable conditions for completion before touching any file.
7. **Obtain Approval**: Confirm the plan with the user.
8. **Implement Incrementally**: Make targeted edits without rewriting unrelated files.
9. **Verify Feature**: Execute unit, API, and visual tests.
10. **Run Regression Checks**: Verify that the map, ship movement, checkpoint encounters, duplicate prevention, and revenue calculations remain 100% operational.
11. **Update Documentation**: Update `PROFESSOR_CHANGELOG.md` and any affected project documents (`TRD.md`, `PRD.md`, `DECISIONS.md`).
12. **Sign Off**: Advance status to `COMPLETED`.

---

## 5. Reusable Change Entry Template

The following template must be copied and populated whenever a new request is introduced:

```markdown
## CHG-XXX: [Short Descriptive Title]

**Status**: RECORDED  
**Date Recorded**: [YYYY-MM-DD or Unknown]  
**Priority**: [CRITICAL | HIGH | MEDIUM | LOW]  

### 1. Original Professor Instruction
> "[Paste verbatim instruction from the professor here.]"

### 2. Interpreted Requirement
[Detail the exact functional behavior and outputs expected from the system.]

### 3. Scope & Boundaries
- **In Scope**: [What must be added or changed]
- **Out of Scope / Preserved**: [What must remain unchanged]

### 4. Affected Requirements & Components
- **PRD Requirements**: [e.g., Section 4.4, AC-06]
- **TRD Components**: [e.g., TollPolicyService, simulation-config.json]
- **Frontend Modules**: [e.g., js/dashboard.js, js/ship-renderer.js]
- **Target Files**: [e.g., src/main/resources/simulation-config.json]

### 5. Conflict & Compatibility Review
- **DECISIONS.md Check**: [No conflicts | Identifies conflict with DEC-XXX]
- **Java 8 / GlassFish 4.1.1 Check**: [Verified compatible | Identifies risk]

### 6. Proposed Solution
[Concise description of the implementation approach.]

### 7. Acceptance Criteria
- [ ] Condition 1: [Specific measurable outcome]
- [ ] Condition 2: [Specific measurable outcome]
- [ ] Condition 3: No duplicate toll charges occur.
- [ ] Condition 4: Existing simulation controls and revenue tallies remain intact.

### 8. Implementation Record
- **Implementation Status**: Not Started
- **Changed Files**: None
- **Commit / Edit Summary**: None

### 9. Verification & Regression Results
- **Feature Test Result**: Not Run
- **Regression Test Result**: Not Run
- **Known Limitations**: None recorded

### 10. Sign-Off & Completion
- **Completed Date**: Not Applicable
- **Final Status**: Not Completed
```

*(The above template is a structural model for future entries. It is not an active change entry.)*

---

## 6. Anticipated Potential Request Categories

During a practical examination, professors frequently ask students to modify the system on the spot to test their understanding and code modularity. The following are anticipated possibilities categorized to aid rapid response:

| Category | Example Examination Request | Target Architecture Layer |
| :--- | :--- | :--- |
| **Tariff Adjustment** | "Increase Iran's toll rate for crude oil tankers by 20%." | `simulation-config.json` (`cargoAdjustments`) |
| **Geopolitical Rule** | "Exempt Japanese vessels from Oman tolls as a friendly nation." | `simulation-config.json` (`tollPolicies.rules`) |
| **Fleet Expansion** | "Add a container ship from Germany heading to Dubai." | `simulation-config.json` (`ships`), `assets/flags/de.svg` |
| **Route Modification** | "Add an intermediate waypoint to route outbound ships closer to Abu Musa." | `simulation-config.json` (`routes`) |
| **Toll Checkpoint** | "Add an auxiliary patrol checkpoint near Bandar Abbas." | `simulation-config.json` (`checkpoints`), `map.js` |
| **UI Filter** | "Filter the map to display only ships carrying hazardous cargo (Crude Oil, LNG)."| `js/dashboard.js`, `index.html` |
| **Transaction Audit** | "Add a button to download the current run's toll receipts as a CSV file." | `js/toll-ui.js` or backend export servlet |
| **Currency Display** | "Display toll amounts and revenue in Euros as well as USD." | `js/toll-ui.js`, `js/dashboard.js` |
| **Congestion Alert** | "Display an alert when 3 or more ships are within a checkpoint zone simultaneously."| `js/simulation.js`, `js/toll-ui.js` |

*Important: These are hypothetical examples to illustrate readiness. None of these items are approved or in progress until explicitly requested by the professor.*

---

## 7. Change Impact & Safety Principles

1. **Non-Regression Guarantee**: An approved modification must never silently break or degrade core MVP features (Google Maps rendering, vessel movement, flag visibility, checkpoint tolling, duplicate prevention, and revenue calculations).
2. **Surgical Precision**: Make the smallest necessary change. Avoid refactoring unrelated classes or rewriting entire scripts.
3. **Configuration Over Code**: Whenever a request can be satisfied via `simulation-config.json`, resolve it there rather than altering compiled Java classes or JavaScript animation algorithms.
4. **Transparent Conflict Reporting**: If a requested change directly contradicts an approved decision in `DECISIONS.md` (e.g., migrating to React or removing the simulation disclaimer), the conflict must be surfaced immediately before taking action.

---

## 8. Change Summary Table

| Change ID | Title | Status | Priority | Testing Status | Completion Date |
| :--- | :--- | :--- | :--- | :--- | :--- |
| *(None)* | *(No additional professor requests recorded yet)* | — | — | — | — |

*Current Status: The changelog is initialized and empty of active changes. All current requirements reside within the baseline documentation set.*

---

## 9. Relationship to Other Documentation

- **[docs/brain.md](brain.md)**: Governs overall project identity, environmental constraints, and agent working rules.
- **[docs/PRD.md](PRD.md)**: Governs functional requirements and acceptance criteria.
- **[docs/TRD.md](TRD.md)**: Governs technical architecture, API contracts, and implementation design.
- **[docs/UI_UX_GUIDELINES.md](UI_UX_GUIDELINES.md)**: Governs visual design tokens, layout hierarchy, and marker specifications.
- **[docs/DECISIONS.md](DECISIONS.md)**: Governs confirmed and proposed decisions.
- **`docs/PROFESSOR_CHANGELOG.md`** (This Document): Governs new post-baseline requests and scope modifications.

---

## 10. Document Status

- **Title**: Hormuz Vasooli System — Professor Change Log
- **Status**: **Ready for Future Requests**
- **Active Entries**: 0 (No additional professor requests have been introduced at this time)
