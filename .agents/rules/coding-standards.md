---
trigger: always_on
description: "Permanent coding standards, modularity principles, and visual quality rules for Hormuz Vasooli System."
---

# Hormuz Vasooli System — Permanent Coding Standards

## 1. Code Quality, Modularity & Separation of Concerns
1. **Readable and Maintainable Code**: Write clean, self-documenting code with clear variable, function, class, and endpoint names. Avoid obfuscated shortcuts or overly clever abstractions.
2. **Strict Separation of Responsibilities**: Maintain clean architectural boundaries between:
   - Map rendering and controls
   - Custom ship visualization and marker styling
   - Vessel movement simulation and waypoint interpolation
   - Toll policy calculation and exemption logic
   - Transaction records, ledger, and state management
   - Dashboard presentation, search, and filtering
   - Client-server API communication (if backend tolling is approved)
3. **No Monolithic Files**: Do not put the entire frontend application into a single oversized JavaScript file, nor the entire backend into a single monolithic Java class.
4. **No Scrambled Business Logic**: Avoid embedding country-specific toll rates, exemptions, or tariff logic repeatedly across unrelated JavaScript functions or Servlets. Keep toll calculation and financial state logically decoupled from ship animation loops.

## 2. Configuration-Driven Rules & Financial Integrity
1. **Configurable Simulation Data**: Store vessel manifests, routes, checkpoint coordinates, country identifiers, toll tariffs, and national exemptions in a structured, configurable format rather than hard-coding them in logic. The exact configuration source and schema (such as `src/main/resources/simulation-config.json`) remain proposed (DEC-012) and must follow approved technical decisions upon sign-off.
2. **Toll Calculation Separation & Authority**: Keep toll evaluation strictly separate from visual movement and UI rendering. If server-authoritative tolling (DEC-010) is approved, the backend will serve as the authoritative financial ledger, and the client will submit event identifiers rather than trusted revenue amounts. Until DEC-010 is formally approved, do not treat server-side tolling as an unalterable constraint.
3. **Strict Duplicate Prevention**: Prevent continuous animation frames or lingering inside a checkpoint radius from generating repeated toll assessments for the same transit. Client proximity latches and idempotency mechanisms must enforce one transaction per encounter.
4. **Internal Revenue Consistency**: Ensure all financial calculations obey the fundamental invariant at all times:
   $$\text{Total Revenue} \equiv \text{Iran Revenue} + \text{Oman Revenue}$$

## 3. Decision Discipline & Platform Preservation
1. **No Premature Elevation of Proposed Decisions**: Never treat a `PROPOSED` or `NEEDS-VERIFICATION` decision (e.g., in `DECISIONS.md`) as a mandatory implementation constraint. Code strictly against `CONFIRMED` requirements and explicitly approved decisions.
2. **Surface Unresolved Issues**: If implementing a feature depends on an unresolved or proposed architectural decision, highlight the open item for approval before executing major modifications.
3. **Preserve Legacy Platform Constraints**: All code must strictly honor the Java 8 (`1.8.0_181`), Java EE 7 Web (`javax.*`), Maven WebApp, and GlassFish Server 4.1.1 baseline.

## 4. Resilience, Error Handling & Dependencies
1. **Graceful Degradation**: Implement defensive handling for missing configuration keys, invalid identifiers, network request failures, missing flag assets, or Google Maps load errors. Never let an unhandled error crash the animation loop.
2. **No Unapproved Third-Party Libraries**: Do not introduce third-party JavaScript libraries, npm packages, or external Java JARs without explicit necessity and compatibility review. Use native browser APIs and Java EE 7 built-in APIs (`javax.json.*`).
3. **No Phantom Controls or Fabricated Data**: Do not build UI buttons, toggles, or sliders that appear interactive but possess no implemented behavior. Do not display fabricated numbers or dummy charts that do not map to real simulation state.

## 5. Visual Component & Rendering Standards
1. **Coordinated Composite Ship Markers**: Every vessel marker must function as a single rigid visual unit comprising:
   - A top-down ship hull silhouette colored by cargo classification.
   - A small visible mast pole centered on the superstructure.
   - A national flag badge anchored at the masthead.
2. **Local SVG Flag Assets (No Unicode Emojis)**: Use dedicated, locally stored vector SVG assets (`assets/flags/{flagIcon}.svg`) for national flags. System Unicode flag emojis are strictly prohibited due to cross-platform rendering inconsistencies and pixelation.
3. **Heading Rotation & Upright Flags**: The ship hull rotates dynamically with its forward azimuth heading ($\theta$), while the flag badge is counter-rotated ($-\theta$) to remain permanently horizontal, upright, and legible.
4. **Strict UI/UX Guideline Compliance**: Follow the approved `docs/UI_UX_GUIDELINES.md` (inspired by `docs/references/visual-reference.png`) featuring dark oceanic foundations, selective glassmorphism, and warm coral accents. Avoid generic purple gradients or excessive rounded cards.
5. **Independent Testing**: Test individual modules independently where practical, and verify interactions with related components before declaring completion.
