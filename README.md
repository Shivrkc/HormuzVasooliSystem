# Hormuz Vasooli System

An educational maritime traffic flow and hypothetical toll-assessment simulation web application demonstrating commercial shipping operations through the Strait of Hormuz.

---

## 1. Project Overview

The **Hormuz Vasooli System** (HVS) is an academic web application designed for a college practical examination. It models maritime vessel navigation along Traffic Separation Schemes (TSS) in the Strait of Hormuz, visualizes vessels using composite top-down hulls with mast poles and national flags, applies configurable hypothetical toll policies for Iranian and Omani waters, and tracks separate and combined revenue metrics in real time.

> **Status Notice**: The project is currently in the **Documentation and Planning Stage** (Phase 0). Application source code, mapping components, simulation loops, and backend services are planned and scheduled for sequential execution in accordance with [docs/IMPLEMENTATION_PLAN.md](docs/IMPLEMENTATION_PLAN.md).

---

## 2. Project Objectives

The planned baseline capabilities for the initial build include:

1. **Interactive Geographical Map**: High-detail mapping of the Strait of Hormuz focused on Iranian and Omani coastal waters using the Google Maps JavaScript API.
2. **Ship Movement Simulation**: Linear waypoint interpolation along predefined inbound and outbound corridors with Start, Pause, Resume, Reset, and speed scaling presets (1x, 2x, 4x).
3. **Custom Ship Models & Flag Assets**: Coordinated composite markers featuring cargo-colored top-down silhouettes, centered mast poles, and upright, counter-rotated local SVG national flags (no Unicode emojis).
4. **Country & Toll Policy Management**: Interactive management drawers allowing live configuration of national tariffs, friendly exemptions, and cargo multipliers without code modifications.
5. **Vessel, Route & Checkpoint Management**: Real-time CRUD interfaces to register vessels, modify route waypoints, adjust checkpoint detection radii, and assign authorities.
6. **Authoritative Transaction Ledger**: Detailed tabular audit log capturing every crossing event outcome (`CHARGED`, `EXEMPT`, `DUPLICATE`), with itemized receipt modals.
7. **Revenue Analytics & CSV Export**: Real-time revenue tallies adhering to mathematical conservation, coupled with client-side RFC 4180 CSV export for transaction logs and summary reports.
8. **Search, Multi-Attribute Filtering & Congestion Alerts**: Substring searching by vessel name/ID, dropdown filters (nationality, cargo, status), layer visibility toggles, and visual congestion alert badges based on configurable density thresholds.

*All described capabilities represent planned initial-build requirements; none should be considered operational until verified by concrete execution evidence.*

---

## 3. Technology Stack

The project operates strictly within the target development and execution runtime:

- **Integrated Development Environment (IDE)**: NetBeans IDE 8.2
- **Java Runtime & Bytecode Target**: Java 8 (Installed JDK: `1.8.0_181`)
- **Build System**: Apache Maven 3.x (Bundled with NetBeans 8.2)
- **Application Type**: Java Web Application (`packaging: war`)
- **Enterprise Platform**: Java EE 7 Web Profile (`javax.*` namespace)
- **Application Server**: GlassFish Server 4.1.1 Open Source Edition (Target ID: `gfv3ee6`)
- **Frontend Technologies**: Standards-compliant HTML5, CSS3 (translucent oceanic design tokens), and Vanilla JavaScript (ES6)
- **Mapping Engine**: Google Maps JavaScript API v3
- **Backend APIs**: Java Servlets 3.1 and JSON-P (`javax.json.*`, JSR 353)

*Hard Platform Constraints: No migrations to Spring Boot, Quarkus, React, Vue, Angular, or Node-based build tooling are permitted. Native GlassFish Java EE 7 APIs are utilized exclusively.*

---

## 4. Project Structure

### Current Starter Structure
The repository currently maintains an uncompiled, clean starter Maven WebApp structure:

```
HormuzVasooliSystem/
├── .agents/
│   └── rules/
│       ├── coding-standards.md       # Architectural boundaries & coding rules
│       ├── compatibility-rules.md    # Java 8 & GlassFish 4.1.1 constraints
│       └── project-rules.md          # Project governance & change protocols
├── docs/                             # Complete project documentation suite
├── src/
│   └── main/
│       └── webapp/
│           ├── WEB-INF/
│           └── index.html            # Starter HTML placeholder
├── nb-configuration.xml              # NetBeans 8.2 GlassFish deployment config
└── pom.xml                           # Maven project configuration (WAR)
```

### Planned Modular Architecture
During implementation, the codebase will be populated incrementally into modular components:

```
HormuzVasooliSystem/
├── src/main/java/com/hormuzvasooli/
│   ├── model/                        # Country, Vessel, Route, Checkpoint, Transaction
│   ├── service/                      # SimulationConfigService, SimulationStateService, TollPolicyService
│   ├── servlet/                      # ConfigServlet, StateServlet, TollServlet, ResetServlet
│   └── listener/                     # ApplicationStartupListener (ServletContext lifecycle)
├── src/main/resources/
│   └── simulation-config.json        # Configurable seed manifest (Proposed DEC-012)
├── src/main/webapp/
│   ├── assets/
│   │   ├── flags/                    # Local SVG flag assets (ind.svg, sg.svg, cn.svg, etc.)
│   │   └── icons/                    # UI icons and vector graphics
│   ├── css/
│   │   └── styles.css                # Dark oceanic glassmorphic stylesheet
│   ├── js/
│   │   ├── config.js                 # Global constants & coordinate anchors
│   │   ├── api-client.js             # HTTP REST wrapper for backend endpoints
│   │   ├── map.js                    # Google Maps initialization & styling
│   │   ├── ship-renderer.js          # Coordinated hull, mast, and upright flag rendering
│   │   ├── simulation.js             # Movement loop & spatial crossing detection
│   │   ├── toll-ui.js                # Receipt modals & real-time crossing toasts
│   │   ├── dashboard.js              # Vessel registry, search & multi-attribute filter stack
│   │   ├── management.js             # Dynamic CRUD drawers (Country, Vessel, Route, CP)
│   │   ├── reports.js                # Revenue analytics & client-side RFC 4180 CSV export
│   │   ├── alert-system.js           # Traffic density monitoring & congestion badges
│   │   ├── layer-control.js          # Map layer toggles (routes, checkpoints, labels)
│   │   └── app.js                    # Core bootstrap & component orchestration
│   ├── WEB-INF/
│   │   └── web.xml                   # Java EE 7 Web deployment descriptor
│   └── index.html                    # Single-page simulation interface
└── pom.xml                           # Maven packaging definition
```

---

## 5. Prerequisites

Before building or running the project, verify that the following dependencies are installed and configured:

1. **Java Development Kit**: JDK 8 (specifically `1.8.0_181` or compatible update).
2. **NetBeans IDE**: Version 8.2 with the Java EE plugin bundle active.
3. **GlassFish Server**: Version 4.1.1 configured locally in NetBeans (under the "Services" -> "Servers" tab).
4. **Apache Maven**: Version 3.x (NetBeans bundled Maven is sufficient).
5. **Google Maps API Access**: A valid Google Maps JavaScript API key enabled for Map Tiles and geometry services.

*Verification Note: Environment baseline checks must be executed and logged (as planned in Implementation Phase 1) before declaring the stack operational.*

---

## 6. Setup and Run Instructions

Follow these practical steps to load, build, and deploy the application:

### Step 1: Open the Project in NetBeans
1. Launch **NetBeans IDE 8.2**.
2. Select **File -> Open Project**.
3. Browse to `c:\Users\godbl\OneDrive\Documents\NetBeansProjects\HormuzVasooliSystem`.
4. Verify that NetBeans recognizes the project as a Maven WebApp with the GlassFish server icon.

### Step 2: Verify the JDK Configuration
1. Right-click the project root in NetBeans -> **Properties**.
2. Navigate to **Build -> Compile**.
3. Confirm that the Java Platform points to **JDK 1.8**.

### Step 3: Build the Project
Open a terminal in the project directory or use NetBeans **Clean and Build**:
```bash
mvn clean package
```
*Expected Outcome*: Maven produces `target/HormuzVasooliSystem-1.0-SNAPSHOT.war` with `BUILD SUCCESS`.

### Step 4: Deploy to GlassFish Server 4.1.1
- **Via NetBeans**: Right-click the project -> **Run** or **Deploy**.
- **Via GlassFish CLI**:
  ```bash
  asadmin deploy target/HormuzVasooliSystem-1.0-SNAPSHOT.war
  ```

### Step 5: Access the Deployed Application
Open a standards-compliant web browser (Google Chrome, Mozilla Firefox, or Microsoft Edge) and navigate to:
```
http://localhost:8080/HormuzVasooliSystem/
```

### Step 6: Configure Google Maps API Key
*(Planned Feature)*: Upon loading the application for the first time:
1. Click the **[API Key]** button in the floating top header.
2. Enter your valid Google Maps API Key in the prompt modal.
3. The application will initialize map tiles and store the key in browser `sessionStorage` for the duration of the demonstration session.

---

## 7. Google Maps API Configuration

To enable interactive map tiles and coordinate projections:

1. Obtain a valid API key from the [Google Cloud Console](https://console.cloud.google.com/).
2. Enable the **Maps JavaScript API** for your project.
3. Configure appropriate HTTP Referrer restrictions (e.g., `http://localhost:8080/*`).
4. **Security Notice**: Never commit raw API keys or Google Cloud secrets to public source repositories.
5. **Graceful Fallback**: The application is architected to display a non-blocking configuration prompt modal if the API key is missing, allowing entry directly in the browser.

*Verification Note: The map canvas cannot be claimed as functional until Phase 3 test cases (`TC-MAP-01` through `TC-MAP-06`) are formally executed.*

---

## 8. Simulation Data & Educational Disclaimer

> ### IMPORTANT ACADEMIC DISCLAIMER
> 
> The **Hormuz Vasooli System** is an educational engineering demonstration developed exclusively for academic coursework and a college practical examination.
> 
> - **Fictional Tolling Rules**: All toll tariffs, national charges, cargo category multipliers, and country exemption statuses are entirely hypothetical simulation models. They do not represent real maritime toll policies, official tariffs, or sovereign revenue collections of the **Islamic Republic of Iran** or the **Sultanate of Oman**.
> - **Synthetic Vessel Traffic**: Ship positions, names, call signs, and routes are generated demonstration data. The application does not connect to live AIS (Automatic Identification System) surveillance feeds, military radar, or official port authority vessel dispatch networks.
> - **No Real Commercial Transactions**: All recorded transactions, ledger entries, and revenue totals are simulated numbers in USD for algorithmic validation. No actual payment gateways or financial accounts are connected.

---

## 9. Documentation Index

The complete architectural, design, testing, and operational specifications are maintained across repository documentation:

| Document | Description |
| :--- | :--- |
| **[`docs/brain.md`](docs/brain.md)** | Core project identity, technical constraints, confirmed scope, and operational invariants. |
| **[`docs/PRD.md`](docs/PRD.md)** | Product Requirements Document defining functional scope, UX specifications, and acceptance criteria. |
| **[`docs/TRD.md`](docs/TRD.md)** | Technical Requirements Document covering software architecture, data schemas, REST contracts, and algorithms. |
| **[`docs/UI_UX_GUIDELINES.md`](docs/UI_UX_GUIDELINES.md)** | Visual design system, dark oceanic color tokens, wireframes, and composite ship marker specifications. |
| **[`docs/DECISIONS.md`](docs/DECISIONS.md)** | Architectural decision register recording confirmed decisions, proposed options, and rejected alternatives. |
| **[`docs/PROFESSOR_CHANGELOG.md`](docs/PROFESSOR_CHANGELOG.md)** | Change management protocol for recording and evaluating live examination requests. |
| **[`docs/IMPLEMENTATION_PLAN.md`](docs/IMPLEMENTATION_PLAN.md)** | Phased implementation roadmap with explicit acceptance criteria and progress tracking. |
| **[`docs/TESTING_PLAN.md`](docs/TESTING_PLAN.md)** | Categorized verification suite comprising 87 test cases across mandatory baseline, conditional, and optional tests. |
| **[`docs/DEMO_GUIDE.md`](docs/DEMO_GUIDE.md)** | Step-by-step practical examination walkthrough and professor request troubleshooting guide. |

---

## 10. Current Project Status

- **Lifecycle Stage**: **Documentation & Scope Review (Phase 0 - Under Review)**.
- **Application Code**: Starter Maven Web Application (`HormuzVasooliSystem-1.0-SNAPSHOT.war`) without custom application features implemented.
- **Implementation Status**: **Phases 1 through 12 are strictly NOT STARTED**.
- **Testing Status**: All **87 test cases** in `TESTING_PLAN.md` are documented, classified, and strictly recorded as **`NOT RUN`**.
- **Next Milestone**: Formal approval of Phase 0 documentation, followed by Phase 1 (Environment & Build Baseline Verification).

---

## 11. Known Scope Boundaries & Limitations

In accordance with explicit project constraints, the following capabilities are outside the approved initial scope:

- **Live AIS Feeds**: Real-time satellite or terrestrial transponder tracking is excluded in favor of configurable waypoint interpolation.
- **Real Payment Gateways**: Credit card, banking, or merchant clearinghouse integrations are prohibited.
- **External Government APIs**: No integration with Iranian or Omani port authority or defense network systems.
- **Mandatory User Authentication**: No login screens, password management, or role-based access control for the practical exam demonstration.
- **Persistent Database Storage**: All simulation state, custom configured entities, and transaction history are maintained in-memory for the local server session; data resets on GlassFish restart.
- **Live Forex Feeds**: All tariffs and revenue figures operate exclusively in simulated US Dollars (USD).
- **3D WebGL Rendering**: Ships are visualized using lightweight 2D SVG canvas overlays rather than resource-intensive 3D meshes.
