---
trigger: always_on
description: "Permanent technical environment constraints and legacy compatibility rules for Hormuz Vasooli System."
---

# Hormuz Vasooli System — Permanent Compatibility Rules

## 1. Approved Technical Runtime Stack
All development, packaging, and execution must adhere strictly to the identified development environment:
1. **IDE**: NetBeans IDE 8.2.
2. **Java Runtime & Bytecode Target**: Java 8 (Installed JDK: `1.8.0_181`).
3. **Build System**: Apache Maven 3.x (Bundled with NetBeans 8.2).
4. **Application Type**: Java Maven Web Application (`packaging: war`).
5. **Enterprise Platform**: Java EE 7 Web Profile.
6. **Application Server**: GlassFish Server 4.1.1 Open Source Edition (Local target: `gfv3ee6`).
7. **Frontend Runtime**: Standards-compliant HTML5, CSS3, and Vanilla JavaScript (ES6).
8. **Mapping Engine**: Google Maps JavaScript API v3.
9. **Backend APIs**: Java EE 7-compatible Servlets 3.1 and JSR 353 JSON-P (`javax.json.*`).

## 2. Hard Platform Constraints & Prohibitions
1. **No Framework Migrations**: Do not migrate the application to Spring, Spring Boot, Quarkus, Micronaut, React, Vue, Angular, Vite, or any Node-based tooling without explicit written user approval.
2. **Preserve Maven Archetype**: Do not replace or dismantle the standard Maven Web Application project structure (`src/main/java`, `src/main/webapp`, `src/main/resources`).
3. **No Unreviewed POM Changes**: Do not modify `pom.xml`, `nb-configuration.xml`, compiler settings, or deployment descriptors without first inspecting the existing configuration, explaining the reason for the proposed change, and obtaining approval.
4. **Java 8 Bytecode Verification**: Every Java class, enum, and method must be verified for Java 8 compatibility. Language features or library calls introduced in Java 9+ (e.g. `List.of()`, `var`, `java.lang.ProcessHandle`) are strictly prohibited.
5. **Preserve Java EE 7 Namespace (`javax.*`)**: Do not replace `javax.*` packages (`javax.servlet.*`, `javax.json.*`) with Jakarta EE 9+ `jakarta.*` packages, as GlassFish 4.1.1 only supports Java EE 7 (`javax.*`).
6. **No External Jar Conflicts**: Rely on native GlassFish 4.1.1 libraries provided by `javaee-web-api:7.0` (scope `provided`). Avoid adding Jackson, Gson, or third-party JSON jars that trigger classloader linkage errors.

## 3. Build, Deployment & Secret Safety
1. **Mandatory Build Verification**: Verify Maven packaging (`mvn clean package`) and GlassFish deployment after any change affecting `pom.xml` or backend Java code.
2. **Credential & API Key Safety**: Keep Google Maps API configuration separate from application logic. Never commit active Google Cloud API keys or secrets to public source control.
3. **Conflict Surfacing Over Silent Mutation**: If a requested feature appears to conflict with the Java 8 / GlassFish 4.1.1 stack, explain the limitation clearly and propose compatible Java EE 7 alternatives before altering architecture.
4. **Evidence-Based Compatibility Claims**: Never claim that code compiles, packages, or deploys cleanly on GlassFish 4.1.1 unless supported by an actual build log or deployment verification.
