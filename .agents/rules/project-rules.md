---
trigger: always_on
description: "Persistent project-specific instructions and governance for Hormuz Vasooli System."
---

# Hormuz Vasooli System — Permanent Project Rules

## 1. Project Purpose & Scope
The project is **Hormuz Vasooli System**, an educational ship-movement and hypothetical toll-simulation web application designed for a college practical examination. It demonstrates maritime traffic flow through the Strait of Hormuz, visualizes vessels with masts and country flags, and models hypothetical toll assessments for Iran and Oman.

## 2. Documentation-First & Operating Workflow
1. **Mandatory Documentation Review**: Before designing or implementing any feature, read the relevant project documentation:
   - `docs/brain.md` (Project context, constraints, and non-negotiable rules)
   - `docs/PRD.md` (Product requirements and acceptance criteria)
   - `docs/TRD.md` (Technical architecture, component contracts, and schemas)
   - `docs/UI_UX_GUIDELINES.md` (Visual design specifications and tokens)
   - `docs/DECISIONS.md` (Decision register and open question status)
   - `docs/PROFESSOR_CHANGELOG.md` (Examination change-control protocol)
   - `docs/IMPLEMENTATION_PLAN.md` (Phased roadmap and acceptance criteria)
   - `docs/TESTING_PLAN.md` (Categorized test cases and quality gates)

2. **Strict Category Discipline**: Always distinguish between:
   - **Confirmed Requirements**: Core scope locked in `brain.md` and `PRD.md`.
   - **Approved Decisions**: Formally confirmed choices in `DECISIONS.md`.
   - **Proposed Decisions**: Architecture or UI proposals awaiting explicit approval.
   - **Optional Extensions**: Supporting features that must not block baseline MVP.
   - **Professor-Requested Changes**: New instructions tracked in `PROFESSOR_CHANGELOG.md`.

3. **No Premature Approval**: Never treat a proposed decision or optional feature as approved merely because it appears in a project document.

4. **Surgical, Incremental Execution**: Work strictly one approved implementation phase at a time. Do not begin downstream phases until the active phase has been verified.

5. **Pre-Modification Disclosure**: Before executing major modifications, clearly explain:
   - What will change.
   - Which specific files are affected.
   - Why the change is necessary.
   - Potential compatibility and regression risks.

6. **Preservation of Working Code**: Do not modify unrelated files or rewrite working components without an explicit, approved technical reason.

7. **Prohibition of Unapproved Migrations**: Never migrate to another framework (e.g., React, Vite, Spring Boot), language, or application server without explicit user approval.

## 3. Data Integrity & Educational Simulation Policy
1. **Fictional Demonstration Boundary**: All simulated toll policies, ship positions, tariffs, transactions, and revenue figures are strictly fictional demonstration data for academic simulation.
2. **Explicit Disclaimers**: The application must never claim to represent actual Iranian or Omani governmental maritime toll policy, real-time AIS surveillance feeds, or real commercial payment processing.
3. **No Fabricated Real-World Claims**: Do not add fake live traffic feeds, fake testimonials, fake financial statistics, or claim unverified real-world data integration.

## 4. Verification & Testing Standards
1. **Evidence-Based Completion**: A feature is complete only after it has been executed and verified against its explicit acceptance criteria. Generating code is not completion.
2. **Honest Reporting**: Report actual build, deployment, and test outcomes honestly. Never claim a test has passed unless it has actually been executed and verified.
3. **No Unexecuted Passes**: Never mark a planned or unexecuted test as `PASS`. All tests begin as `NOT RUN`.

## 5. Professor Change Management Protocol
When the professor requests a new requirement or modification:
1. Record the exact instruction verbatim in `docs/PROFESSOR_CHANGELOG.md` under a new `CHG-XXX` ID.
2. Evaluate technical, architectural, and compatibility impact.
3. Check for conflicts against existing approved decisions in `docs/DECISIONS.md`.
4. Update affected project documentation.
5. Implement only the agreed scope with surgical precision.
6. Execute relevant feature and regression tests, then record the result.

## 6. Architectural Stability & Documentation Sync
1. **Conflict Surfacing**: If requirements conflict, identify the contradiction explicitly instead of silently selecting a convenient interpretation.
2. **No Modernization for Its Own Sake**: Never alter project architecture, libraries, or build configurations merely because an alternative appears newer or more modern.
3. **Documentation Stage Lock**: During the documentation stage, do not implement application features until the user explicitly authorizes the transition to implementation.
4. **Continuous Synchronization**: Keep documentation synchronized with actual implementation and approved decisions at all times.
