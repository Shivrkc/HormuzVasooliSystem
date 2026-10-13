# Hormuz Vasooli System
# Technical Requirements Document (TRD)

Version: 1.0  
Status: Draft for Review  
Project Type: College Practical Examination Project  

---

## 1. Project Environment and Platform Constraints

The Hormuz Vasooli System (HVS) is developed within a strictly defined academic and technical runtime environment. All architectural components and code artifacts must adhere to these environmental constraints.

### 1.1 Development and Runtime Stack

| Layer | Technology | Version / Specification | Notes / Constraints |
| :--- | :--- | :--- | :--- |
| **IDE** | NetBeans IDE | 8.2 | Native project structure and deployment integration |
| **Java Platform** | Oracle JDK | 1.8.0_181 (Java 8) | Language features limited strictly to Java 8 |
| **Build System** | Apache Maven | 3.x (Bundled in NetBeans) | Standard Maven WebApp packaging (`war`) |
| **Enterprise Platform** | Java EE | Java EE 7 Web Profile | Servlets 3.1, JSP 2.3, JSR 353 JSON-P 1.0 |
| **Application Server** | GlassFish Server | 4.1.1 Open Source Edition | Local deployment target (`gfv3ee6`) |
| **Package Root** | Java Package | `com.hormuzvasooli` | Standard package hierarchy |
| **Frontend Runtime** | HTML5 / CSS3 / ES6 Vanilla JS | Evergreen Browsers (Chrome, Edge, Firefox) | No transpilation; no external JS framework |
| **Mapping Engine** | Google Maps JavaScript API | v3 | Script-loaded with API key placeholder |
| **JSON Handling** | Java EE JSON-P | `javax.json.*` (JSR 353) | Native GlassFish 4.1.1 library; no Jackson/Gson |

### 1.2 Platform Compatibility & POM Verification Notes

- **Current POM Configuration**: Inspection of `pom.xml` indicates that `maven-compiler-plugin` version 3.1 currently declares `<source>1.7</source>` and `<target>1.7</target>`.
- **Identified Requirement**: Because the development environment uses JDK 8 (`1.8.0_181`), writing Java 8 constructs (such as lambdas, Stream API, and `java.time`) requires the compiler plugin source and target levels to be updated to `1.8`.
- **Strict Constraint**: `pom.xml` must not be edited during the documentation phase. The compiler upgrade to 1.8 is formally logged as a prerequisite step for Phase 1 of the implementation plan and must preserve the `maven-dependency-plugin` endorsed-directory configuration required by GlassFish 4.1.1.
- **Framework Restrictions**: Do not introduce Spring, Spring Boot, Quarkus, Micronaut, Jakarta EE 9+ (`jakarta.*` namespace), React, Angular, Vue, Vite, Webpack, or Node-based tooling.

---

## 2. Architectural Approach

Hormuz Vasooli System implements a decoupled web architecture designed to demonstrate maritime traffic simulation and hypothetical toll calculations. 

> [!NOTE] Architectural Decision Status
> The proposed architecture models the Java EE backend as an authoritative transactional ledger (DEC-010, currently PROPOSED). Until DEC-010 receives formal approval, the system design accommodates both server-authoritative tolling and robust client-side simulation state coordination.

```
+---------------------------------------------------------------------------------------+
|                                    CLIENT (BROWSER)                                   |
|                                                                                       |
|   +-------------------------------------------------------------------------------+   |
|   |                              index.html / CSS UI                              |   |
|   |  - Operations Dashboard   - Vessel Registry Panel   - Financial Counters Bar  |   |
|   |  - Simulation Controls    - Search & Filter Controls - Itemized Receipt Drawer |   |
|   |  - Management Drawers     - Revenue Reports & CSV    - Congestion Alert Badge  |   |
|   |  - Layer Toggles          - Speed Multipliers (1x-4x)- Custom Modal Dialogs   |   |
|   +-------------------------------------------------------------------------------+   |
|              |                                        ^                               |
|              v                                        | State Updates                 |
|   +-----------------------+              +-----------------------+                    |
|   |   simulation.js       |              |      toll-ui.js       |                    |
|   | - Tick-based Stepper  |              | - Real-time Display   |                    |
|   | - Waypoint Navigation |              | - Receipt Modals      |                    |
|   | - Spatial Proximity   |              | - Event Log Render    |                    |
|   +-----------------------+              +-----------------------+                    |
|              |                                        ^                               |
|              v Crossing Event                         | Toll Outcome & Reports        |
|   +-------------------------------------------------------------------------------+   |
|   |                 api-client.js / management.js / reports.js                    |   |
|   |  - GET /config    - GET /state    - POST /toll    - POST /reset               |   |
|   |  - Country CRUD   - Policy CRUD   - Vessel CRUD   - Checkpoint/Route CRUD     |   |
|   |  - Client-Side RFC 4180 CSV Generation & Blob Download                        |   |
|   +-------------------------------------------------------------------------------+   |
|              |                                        ^                               |
|              | HTTP JSON                              | JSON Payload                  |
|   +----------|----------------------------------------|---------------------------+   |
|              v                                        |                               |
|   +-------------------------------------------------------------------------------+   |
|   |                         BACKEND (GLASSFISH 4.1.1 / JAVA EE 7)                 |   |
|   |                                                                               |   |
|   |   +-----------------------------------------------------------------------+   |   |
|   |   |                            Servlet Layer                              |   |   |
|   |   |   SimulationConfigServlet     SimulationStateServlet                  |   |   |
|   |   |   TollAssessmentServlet       SimulationResetServlet                  |   |   |
|   |   |   (Proposed: Country/Policy/Vessel/Route/Checkpoint Servlets)         |   |   |
|   |   +-----------------------------------------------------------------------+   |   |
|   |              |                                                                |   |
|   |              v                                                                |   |
|   |   +-----------------------------------------------------------------------+   |   |
|   |   |                            Service Layer                              |   |   |
|   |   |   SimulationConfigService     TollPolicyService   SimulationStateService  |   |   |
|   |   +-----------------------------------------------------------------------+   |   |
|   |              |                                        |                       |   |
|   |              v Seed Setup                             v In-Memory State       |   |
|   |   +----------------------+               +--------------------------------+   |   |
|   |   |simulation-config.json|               | In-Memory Authoritative State: |   |   |
|   |   | - Checkpoint Coords  |               | - Active Run ID                |   |   |
|   |   | - Vessel Manifests   |               | - Dynamic Countries & Tariffs  |   |   |
|   |   | - Tariff Matrix      |               | - Active Run Revenue Ledger    |   |   |
|   |   | - Exempt Nations     |               | - Past-Run Session History     |   |   |
|   |   |                      |               | - Duplicate Prevention Cache   |   |   |
|   |   +----------------------+               +--------------------------------+   |   |
|   +-------------------------------------------------------------------------------+   |
+---------------------------------------------------------------------------------------+
```

### 2.1 Core Architectural Principles

1. **Authoritative Toll Calculations (Proposed DEC-010)**:
   - When a ship enters a checkpoint's geographic boundary, an assessment event is triggered.
   - Toll evaluation (tariff lookup, cargo multiplier, exemption check) is decoupled from movement animation.
   - The authoritative ledger updates revenue counters and returns the itemized transaction receipt.
2. **Strict Duplicate-Charge Prevention**:
   - Continuous animation frames could repeatedly trigger detection at a checkpoint.
   - A multi-tier idempotency system on both client and server guarantees that one vessel transit through a checkpoint generates at most one financial transaction.
3. **Interactive Configuration & Dynamic Management**:
   - Seed data is loaded from structured JSON (`simulation-config.json`).
   - Users can dynamically add or edit countries, modify toll rates, toggle exemptions, add or edit vessels, and adjust routes or checkpoints via UI management drawers without recompilation.
4. **Historical Transaction Immutability**:
   - Modifying a toll rate or exemption applies strictly to future checkpoint encounters.
   - Past ledger transactions and historical revenue calculations remain permanently immutable in both active and prior runs.
5. **Reset Invariance & Multi-Run Session History**:
   - Resetting closes the active simulation run and initializes a new run with $0.00 revenue and an empty active-run ledger, restoring vessels to route origins.
   - Previous run transactions remain accessible as immutable past-run history for the current server session. In-memory run history is session-bound and may reset if the server restarts.
   - All user-created or edited countries, toll policies, vessels, routes, and checkpoints strictly survive reset.
6. **Client-Side CSV Reporting**:
   - Aggregated revenue summaries and detailed transaction ledgers can be exported directly from the browser as RFC 4180-compliant CSV files.
7. **Traffic Congestion Alerts & Layer Toggles**:
   - Spatial monitoring detects vessel density against configurable parameters (waiting count threshold and proximity radius, with illustrative demonstration defaults) and triggers visual alert badges. Thresholds do not represent official maritime regulations.
   - Map layers for route polylines and checkpoint radii can be shown or hidden dynamically.
8. **Educational & Simulation Boundary**:
   - All data is explicitly identified as fictional demonstration data for a college examination.
   - No actual maritime policy of Iran or Oman is modeled or implied.

---

## 3. Proposed Package Structure & Backend Classes

All server code resides within `src/main/java/com/hormuzvasooli/`. Classes are organized into clean, single-responsibility packages.

```
src/main/java/com/hormuzvasooli/
├── listener/
│   └── ApplicationStartupListener.java
├── model/
│   ├── Checkpoint.java
│   ├── Country.java
│   ├── Route.java
│   ├── Ship.java
│   ├── SimulationState.java
│   ├── TollReceipt.java
│   ├── TollRule.java
│   └── TollTransaction.java
├── service/
│   ├── SimulationConfigService.java
│   ├── SimulationStateService.java
│   └── TollPolicyService.java
└── servlet/
    ├── BaseServlet.java
    ├── SimulationConfigServlet.java
    ├── SimulationResetServlet.java
    ├── SimulationStateServlet.java
    └── TollAssessmentServlet.java
```

### 3.1 Class Responsibilities

#### Model Package (`com.hormuzvasooli.model`)
- **`Country.java`**: POJO representing a nation. Attributes: `code` (String, ISO-3), `name` (String), `flagAsset` (String), `defaultFeeUsd` (double), `exempt` (boolean).
- **`Ship.java`**: POJO representing a vessel. Attributes: `id` (String), `name` (String), `imo` (String), `flag` (String, ISO-3 code), `cargoType` (String), `deadweightTonnage` (int), `routeId` (String), `initialSpeed` (double).
- **`Checkpoint.java`**: POJO representing a hypothetical maritime checkpoint. Attributes: `id` (String, e.g., `"CP-IRAN-NORTH"`), `name` (String), `authority` (String: `"IRAN"` or `"OMAN"`), `latitude` (double), `longitude` (double), `radiusKm` (double), `description` (String).
- **`Route.java`**: POJO representing a navigation corridor. Attributes: `id` (String), `name` (String), `waypoints` (List<Waypoint>).
- **`TollRule.java`**: POJO representing a hypothetical tariff rule. Attributes: `countryCode` (String), `authority` (String), `baseFeeUsd` (double), `exempt` (boolean), `cargoMultiplier` (double), `description` (String).
- **`TollTransaction.java`**: Immutable record of a processed toll assessment. Attributes: `transactionId` (String, UUID), `runId` (String), `passageId` (String), `vesselId` (String), `checkpointId` (String), `authority` (String), `amountUsd` (double), `status` (String: `"CHARGED"`, `"EXEMPT"`, `"DUPLICATE"`), `timestamp` (long), `notes` (String).
- **`TollReceipt.java`**: Response DTO returned to client upon toll assessment. Contains itemized calculation details (base rate, cargo adjustment, nationality status, resulting fee, timestamp).
- **`SimulationState.java`**: Thread-safe snapshot of the active simulation session. Attributes: `runId` (String), `startedAt` (long), `iranRevenueUsd` (double), `omanRevenueUsd` (double), `totalRevenueUsd` (double), `transactionCount` (int), `exemptCount` (int), `recentTransactions` (List<TollTransaction>).

#### Service Package (`com.hormuzvasooli.service`)
- **`SimulationConfigService.java`**:
  - Loads seed configuration (`simulation-config.json`) from classpath during startup.
  - Maintains in-memory collections of countries, vessels, routes, checkpoints, and toll rules.
  - Supports dynamic additions and modifications (add/update country, toll rate, vessel, route, checkpoint) with validation.
  - Thread-safe access via `ConcurrentHashMap`.
- **`SimulationStateService.java`**:
  - Manages in-memory session run state for the college demonstration.
  - Generates unique `runId` strings (e.g., `RUN-20261014-XXXX`).
  - Maintains concurrent collections:
    - `ConcurrentHashMap<String, TollTransaction> processedTransactions`: Keyed by idempotency key `runId:vesselId:checkpointId:passageId`.
    - Double accumulators for `iranRevenueUsd`, `omanRevenueUsd`.
    - Thread-safe deque/list for transaction history.
  - Implements atomic `resetRun()`: wipes transactions and zeroes revenues for the active run while preserving all configured countries, ships, routes, and checkpoints.
- **`TollPolicyService.java`**:
  - Core business engine for hypothetical toll calculation.
  - Evaluates toll policy given `vesselId`, `checkpointId`, `runId`, and `passageId`.
  - Enforces duplicate detection against `SimulationStateService`.
  - Applies exemptions: If vessel nationality is exempt, returns `0.0 USD` with status `"EXEMPT"`.
  - Calculates positive charges: `Charge = BaseFee * CargoMultiplier`.
  - Atomically commits valid positive transactions to `SimulationStateService` revenue accumulators.

#### Servlet Package (`com.hormuzvasooli.servlet`)
- **`BaseServlet.java`**: Abstract servlet setting JSON headers (`application/json; charset=UTF-8`) and uniform error response formatting.
- **`SimulationConfigServlet.java`** (`@WebServlet("/api/simulation/config")`): Handles HTTP `GET`. Returns active configuration (ships, routes, checkpoints, countries, tariffs, and disclaimer).
- **`SimulationStateServlet.java`** (`@WebServlet("/api/simulation/state")`): Handles HTTP `GET`. Returns the current `runId`, revenue totals, transaction counters, and recent event logs.
- **`TollAssessmentServlet.java`** (`@WebServlet("/api/simulation/toll")`): Handles HTTP `POST`. Accepts checkpoint encounter payloads, invokes `TollPolicyService`, and returns the toll outcome and updated revenue totals.
- **`SimulationResetServlet.java`** (`@WebServlet("/api/simulation/reset")`): Handles HTTP `POST`. Resets the simulation run ID, zeroes revenue tallies, clears event caches, and returns the fresh `SimulationState`.

#### Listener Package (`com.hormuzvasooli.listener`)
- **`ApplicationStartupListener.java`** (`@WebListener`): Initializes services on servlet context startup, binding them as attributes.

---

## 4. Proposed Frontend Architecture

The client application runs entirely within standard modern browser environments without third-party frameworks.

```
src/main/webapp/
├── index.html                  # Main operations dashboard single-page shell
├── css/
│   └── styles.css              # Asymmetric dashboard layout, glassmorphism, responsive design
├── js/
│   ├── config.js               # Client constants, fallback API key placeholder, default map center
│   ├── app.js                  # Main coordinator: orchestrates boot, UI wiring, and lifecycle
│   ├── api-client.js           # Fetch wrapper for /api/simulation/* endpoints with error handling
│   ├── map.js                  # Google Maps initialization, bounds fitting, checkpoint circles
│   ├── ship-renderer.js        # SVG ship model generator with mast, country flag, and rotation
│   ├── simulation.js           # Animation loop, waypoint interpolation, spatial proximity detection
│   ├── toll-ui.js              # Toll receipt modals, event stream ticker, duplicate alert toasts
│   ├── dashboard.js            # Statistics counters (Waiting/Passing/Passed), revenue display, search/filters
│   ├── management.js           # Interactive CRUD forms/drawers for countries, policies, ships, routes, checkpoints
│   ├── reports.js              # Revenue aggregation, breakdowns by authority/country/cargo, RFC 4180 CSV export
│   ├── alert-system.js         # Real-time traffic density monitoring and visual congestion warning alerts
│   └── layer-control.js        # Visibility toggles for route corridors, checkpoint circles, and vessel labels
└── assets/
    ├── flags/                  # Dedicated SVG vector flags (e.g., in.svg, cn.svg, pa.svg, etc.)
    └── ships/                  # Base SVG hull vectors (bulk, tanker, container silhouettes)
```

### 4.1 Frontend Component Responsibilities

1. **`index.html`**:
   - Single-page application container.
   - Header bar with title, simulation disclaimer badge, Google Maps API key modal button, speed selector (configurable demonstration presets: 1x, 2x, 4x), and server status.
   - Floating side panel: Simulation controls (Start, Pause, Reset), Search & Multi-Filter bar, Traffic Summary, and Congestion Alert badge.
   - Top-right revenue banner: Iran Revenue (USD), Oman Revenue (USD), and Combined Total Revenue (USD).
   - Central map container (`#map-canvas`) with Layer Visibility toggles (Routes, Checkpoints, Labels).
   - Bottom slide-out drawer: Live Toll Transaction Ledger with CSV Export button and detail modal inspector.
   - Management Drawers / Modals: Panels for adding/editing countries, toll policies, vessels, routes, and checkpoints.
2. **`config.js`**:
   - Holds default coordinates for the Strait of Hormuz (`{ lat: 26.5667, lng: 56.2500 }`), default zoom level (`9`), and animation intervals.
   - Declares `GOOGLE_MAPS_API_KEY_PLACEHOLDER = "YOUR_GOOGLE_MAPS_API_KEY"`.
3. **`api-client.js`**:
   - Encapsulates `window.fetch` calls to backend servlets.
   - Methods: `fetchConfig()`, `fetchState()`, `submitTollEvent(payload)`, `resetSimulation()`.
   - Handles network failures gracefully without freezing animation.
4. **`map.js`**:
   - Dynamically loads Google Maps JavaScript API.
   - Renders checkpoint zones as styled circles (`google.maps.Circle`) and routes as polylines.
5. **`ship-renderer.js`**:
   - Renders coordinated composite markers: hull silhouette + mast pole + local SVG flag badge.
   - Rotates hull with heading ($\theta$) while counter-rotating flag ($-\theta$) to remain upright.
6. **`simulation.js`**:
   - Manages `requestAnimationFrame` loop with variable speed multiplier (configurable demonstration presets: 1x, 2x, 4x).
   - Interpolates coordinates between waypoints and computes heading.
   - Evaluates Haversine distance to checkpoints; triggers toll assessment on boundary crossing.
7. **`toll-ui.js`**:
   - Displays real-time toast alerts upon toll assessments and itemized receipt modal on click.
8. **`dashboard.js`**:
   - Binds live counters for vessel states and financial totals.
   - Implements multi-attribute filtering (name/ID, nationality, cargo, status).
9. **`management.js`**:
   - Manages dynamic form validation and data binding for live additions and edits of countries, toll rates, exemptions, vessels, routes, and checkpoints.
10. **`reports.js`**:
    - Aggregates revenue breakdown by authority, country, and cargo type.
    - Generates and triggers browser download of RFC 4180-compliant CSV reports.
11. **`alert-system.js`**:
    - Monitors vessel concentration in checkpoints and bottleneck sectors; highlights visual warning badges when density exceeds thresholds.
12. **`layer-control.js`**:
    - Provides toggle handlers to show or hide route polylines, checkpoint circles, and vessel labels.

---

## 5. Configuration & Simulation Data Strategy

To satisfy the requirement that simulated ship data and hypothetical toll rates remain decoupled from business logic, all seed data will reside in a single authoritative server resource:

`src/main/resources/simulation-config.json`

### 5.1 JSON Configuration Schema

```json
{
  "system": {
    "version": "1.0",
    "disclaimer": "All toll checkpoints, tariff rates, and vessel records in this system are strictly hypothetical demonstration data for academic simulation and do not reflect actual maritime policies of the Islamic Republic of Iran or the Sultanate of Oman."
  },
  "checkpoints": [
    {
      "id": "CP-IRAN-NORTH",
      "name": "Iran Northern Sector Checkpoint",
      "authority": "IRAN",
      "latitude": 26.8500,
      "longitude": 56.4000,
      "radiusKm": 12.0,
      "description": "Hypothetical northern maritime corridor checkpoint off Larak Island"
    },
    {
      "id": "CP-OMAN-SOUTH",
      "name": "Oman Musandam Sector Checkpoint",
      "authority": "OMAN",
      "latitude": 26.3500,
      "longitude": 56.5500,
      "radiusKm": 12.0,
      "description": "Hypothetical southern maritime corridor checkpoint off Musandam Peninsula"
    }
  ],
  "tollPolicies": {
    "currency": "USD",
    "rules": [
      {
        "country": "India",
        "authority": "IRAN",
        "baseFeeUsd": 12000.0,
        "exempt": false,
        "cargoMultiplier": 1.0,
        "notes": "Standard commercial rate"
      },
      {
        "country": "India",
        "authority": "OMAN",
        "baseFeeUsd": 10000.0,
        "exempt": false,
        "cargoMultiplier": 1.0,
        "notes": "Standard commercial rate"
      },
      {
        "country": "China",
        "authority": "IRAN",
        "baseFeeUsd": 0.0,
        "exempt": true,
        "cargoMultiplier": 0.0,
        "notes": "Hypothetical strategic partner exemption"
      },
      {
        "country": "China",
        "authority": "OMAN",
        "baseFeeUsd": 9500.0,
        "exempt": false,
        "cargoMultiplier": 1.0,
        "notes": "Standard commercial rate"
      },
      {
        "country": "Panama",
        "authority": "IRAN",
        "baseFeeUsd": 15000.0,
        "exempt": false,
        "cargoMultiplier": 1.1,
        "notes": "Flag of convenience rate"
      },
      {
        "country": "Panama",
        "authority": "OMAN",
        "baseFeeUsd": 14000.0,
        "exempt": false,
        "cargoMultiplier": 1.1,
        "notes": "Standard commercial rate"
      },
      {
        "country": "Liberia",
        "authority": "IRAN",
        "baseFeeUsd": 15000.0,
        "exempt": false,
        "cargoMultiplier": 1.1,
        "notes": "Flag of convenience rate"
      },
      {
        "country": "Liberia",
        "authority": "OMAN",
        "baseFeeUsd": 14000.0,
        "exempt": false,
        "cargoMultiplier": 1.1,
        "notes": "Standard commercial rate"
      },
      {
        "country": "Oman",
        "authority": "OMAN",
        "baseFeeUsd": 0.0,
        "exempt": true,
        "cargoMultiplier": 0.0,
        "notes": "Domestic national flag exemption"
      },
      {
        "country": "Iran",
        "authority": "IRAN",
        "baseFeeUsd": 0.0,
        "exempt": true,
        "cargoMultiplier": 0.0,
        "notes": "Domestic national flag exemption"
      }
    ],
    "cargoAdjustments": {
      "Crude Oil": 1.25,
      "Liquefied Natural Gas (LNG)": 1.30,
      "Chemicals": 1.20,
      "Container Goods": 1.00,
      "Food Grains": 0.85,
      "Medicines": 0.80
    }
  },
  "routes": [
    {
      "id": "ROUTE-INBOUND",
      "name": "Gulf of Oman to Persian Gulf (Inbound TSS)",
      "waypoints": [
        { "latitude": 25.5000, "longitude": 57.2000 },
        { "latitude": 26.1000, "longitude": 56.8000 },
        { "latitude": 26.3500, "longitude": 56.5500 },
        { "latitude": 26.7000, "longitude": 56.3000 },
        { "latitude": 26.9000, "longitude": 55.7000 },
        { "latitude": 26.8000, "longitude": 54.8000 }
      ]
    },
    {
      "id": "ROUTE-OUTBOUND",
      "name": "Persian Gulf to Gulf of Oman (Outbound TSS)",
      "waypoints": [
        { "latitude": 26.7000, "longitude": 54.8000 },
        { "latitude": 26.8500, "longitude": 55.6000 },
        { "latitude": 26.8500, "longitude": 56.4000 },
        { "latitude": 26.5000, "longitude": 56.7000 },
        { "latitude": 25.9000, "longitude": 57.1000 },
        { "latitude": 25.4000, "longitude": 57.4000 }
      ]
    }
  ],
  "ships": [
    {
      "id": "SHIP-001",
      "name": "MT Hormuz Pioneer",
      "imo": "9312011",
      "nationality": "India",
      "flagIcon": "in",
      "cargo": "Crude Oil",
      "tonnage": 156000,
      "origin": "Basra, Iraq",
      "destination": "Jamnagar, India",
      "routeId": "ROUTE-OUTBOUND",
      "initialProgress": 0.05
    },
    {
      "id": "SHIP-002",
      "name": "MV Yangtze Dragon",
      "imo": "9456722",
      "nationality": "China",
      "flagIcon": "cn",
      "cargo": "Liquefied Natural Gas (LNG)",
      "tonnage": 125000,
      "origin": "Ras Laffan, Qatar",
      "destination": "Shanghai, China",
      "routeId": "ROUTE-OUTBOUND",
      "initialProgress": 0.15
    },
    {
      "id": "SHIP-003",
      "name": "MV Pacific Trader",
      "imo": "9522100",
      "nationality": "Panama",
      "flagIcon": "pa",
      "cargo": "Container Goods",
      "tonnage": 94000,
      "origin": "Jebel Ali, UAE",
      "destination": "Rotterdam, Netherlands",
      "routeId": "ROUTE-OUTBOUND",
      "initialProgress": 0.00
    },
    {
      "id": "SHIP-004",
      "name": "MT Arabian Breeze",
      "imo": "9287411",
      "nationality": "Liberia",
      "flagIcon": "lr",
      "cargo": "Chemicals",
      "tonnage": 68000,
      "origin": "Jubail, Saudi Arabia",
      "destination": "Singapore",
      "routeId": "ROUTE-OUTBOUND",
      "initialProgress": 0.25
    },
    {
      "id": "SHIP-005",
      "name": "MV Indus Grain",
      "imo": "9618420",
      "nationality": "India",
      "flagIcon": "in",
      "cargo": "Food Grains",
      "tonnage": 52000,
      "origin": "Kandla, India",
      "destination": "Bandar Abbas, Iran",
      "routeId": "ROUTE-INBOUND",
      "initialProgress": 0.10
    },
    {
      "id": "SHIP-006",
      "name": "MV Sohar Express",
      "imo": "9723145",
      "nationality": "Oman",
      "flagIcon": "om",
      "cargo": "Medicines",
      "tonnage": 35000,
      "origin": "Muscat, Oman",
      "destination": "Kuwait City, Kuwait",
      "routeId": "ROUTE-INBOUND",
      "initialProgress": 0.30
    }
  ]
}
```

### 5.2 Loading Mechanism

1. During server bootstrap, `ApplicationStartupListener` triggers `SimulationConfigService.init()`.
2. The service uses `getClass().getClassLoader().getResourceAsStream("simulation-config.json")`.
3. The JSON stream is parsed via `javax.json.Json.createReader(stream).readObject()`.
4. Domain collections (`ships`, `checkpoints`, `routes`, `policies`) are instantiated as unmodifiable memory maps.
5. `GET /api/simulation/config` returns the configuration needed by the client. Sensitive server-only configuration (if any is added in future) can be filtered out before JSON serialization.

---

## 6. Backend API Contracts & Data Specifications

All API communications use HTTP over JSON with `Content-Type: application/json; charset=UTF-8`.

### 6.1 `GET /api/simulation/config`

- **Description**: Returns static simulation configuration required by the client to render the map, routes, checkpoints, and vessel manifests.
- **Request Parameters**: None.
- **Response Status**: `200 OK`
- **Response Schema**:
```json
{
  "system": {
    "version": "1.0",
    "disclaimer": "All toll checkpoints, tariff rates, and vessel records in this system are strictly hypothetical demonstration data..."
  },
  "checkpoints": [
    {
      "id": "CP-IRAN-NORTH",
      "name": "Iran Northern Sector Checkpoint",
      "authority": "IRAN",
      "latitude": 26.85,
      "longitude": 56.40,
      "radiusKm": 12.0
    }
  ],
  "routes": [ ... ],
  "ships": [ ... ],
  "tollPoliciesSummary": {
    "currency": "USD",
    "supportedCountries": ["India", "China", "Panama", "Liberia", "Oman", "Iran"],
    "exemptCountries": ["China", "Oman", "Iran"]
  }
}
```

### 6.2 `GET /api/simulation/state`

- **Description**: Retrieves the authoritative in-memory state of the active simulation session. Used for client reconciliation upon page load or refresh.
- **Request Parameters**: None.
- **Response Status**: `200 OK`
- **Response Schema**:
```json
{
  "runId": "RUN-20261014-9182",
  "startedAt": 1791924500000,
  "financials": {
    "currency": "USD",
    "iranRevenueUsd": 27000.0,
    "omanRevenueUsd": 10000.0,
    "totalRevenueUsd": 37000.0
  },
  "statistics": {
    "totalTransactions": 3,
    "exemptPassages": 1,
    "chargedPassages": 2
  },
  "recentTransactions": [
    {
      "transactionId": "TX-4c8e-8a1b-3f2d",
      "passageId": "PASS-SHIP-001-1",
      "vesselId": "SHIP-001",
      "vesselName": "MT Hormuz Pioneer",
      "nationality": "India",
      "checkpointId": "CP-IRAN-NORTH",
      "authority": "IRAN",
      "amountUsd": 15000.0,
      "status": "CHARGED",
      "timestamp": 1791924560000
    }
  ]
}
```

### 6.3 `POST /api/simulation/toll`

- **Description**: Evaluates and authoritatively records a toll checkpoint encounter. Client submits vessel and checkpoint identity; server computes the financial charge.
- **Request Headers**: `Content-Type: application/json`
- **Request Body Schema**:
```json
{
  "runId": "RUN-20261014-9182",
  "vesselId": "SHIP-001",
  "checkpointId": "CP-IRAN-NORTH",
  "passageId": "PASS-SHIP-001-1",
  "clientTimestamp": 1791924559800
}
```
- **Field Definitions**:
  - `runId` (Required): Active simulation session ID obtained from `/config` or `/state`.
  - `vesselId` (Required): Valid vessel ID matching a loaded ship record.
  - `checkpointId` (Required): Valid checkpoint ID matching a loaded checkpoint record.
  - `passageId` (Required): Client transit identifier distinguishing multiple voyages of the same ship.
- **Response Statuses**:
  - `200 OK`: Toll successfully evaluated (either positive charge or valid exemption).
  - `400 Bad Request`: Missing or malformed parameters.
  - `404 Not Found`: Unknown `vesselId`, `checkpointId`, or invalid `runId`.
  - `409 Conflict`: Duplicate checkpoint encounter detected for this vessel passage (idempotent rejection).
- **Success Response Schema (`200 OK`)**:
```json
{
  "outcome": "SUCCESS",
  "transaction": {
    "transactionId": "TX-4c8e-8a1b-3f2d",
    "runId": "RUN-20261014-9182",
    "passageId": "PASS-SHIP-001-1",
    "vesselId": "SHIP-001",
    "vesselName": "MT Hormuz Pioneer",
    "nationality": "India",
    "checkpointId": "CP-IRAN-NORTH",
    "authority": "IRAN",
    "status": "CHARGED",
    "currency": "USD",
    "amountUsd": 15000.0,
    "calculation": {
      "baseFeeUsd": 12000.0,
      "cargo": "Crude Oil",
      "cargoMultiplier": 1.25,
      "exempt": false
    },
    "timestamp": 1791924560000
  },
  "updatedFinancials": {
    "iranRevenueUsd": 15000.0,
    "omanRevenueUsd": 0.0,
    "totalRevenueUsd": 15000.0
  }
}
```
- **Duplicate Response Schema (`409 Conflict` or handled `200` with status `"DUPLICATE"`)**:
```json
{
  "outcome": "DUPLICATE_IGNORED",
  "message": "Vessel SHIP-001 has already completed toll assessment at CP-IRAN-NORTH for passage PASS-SHIP-001-1.",
  "originalTransactionId": "TX-4c8e-8a1b-3f2d",
  "currentFinancials": {
    "iranRevenueUsd": 15000.0,
    "omanRevenueUsd": 0.0,
    "totalRevenueUsd": 15000.0
  }
}
```

### 6.4 `POST /api/simulation/reset`

- **Description**: Closes the active simulation run and starts a fresh run. Generates a new `newRunId`, archives previous run transactions into in-memory past-run session history, clears active run idempotency latches, and initializes active revenue counters to zero ($0.00 USD). All user-configured entities (countries, vessels, routes, checkpoints, and toll policies) strictly survive reset.
- **Request Parameters**: None.
- **Response Status**: `200 OK`
- **Response Schema**:
```json
{
  "outcome": "RESET_SUCCESSFUL",
  "newRunId": "RUN-20261014-1044",
  "resetAt": 1791924700000,
  "financials": {
    "iranRevenueUsd": 0.0,
    "omanRevenueUsd": 0.0,
    "totalRevenueUsd": 0.0
  },
  "activeRunTransactions": 0,
  "pastRunsRetained": 1
}
```

### 6.5 Management Contracts & Payload Schemas

To support dynamic examination modifications without server recompilation, the system provides standard CRUD data schemas handled via client-side state and proposed backend management endpoints:

#### 1. Country & Flag Schema
```json
{
  "code": "SGP",
  "name": "Singapore",
  "flagAsset": "assets/flags/sg.svg",
  "defaultFeeUsd": 11000.0,
  "exempt": false
}
```
*Validation*: `code` must be unique 3-letter alphanumeric; `name` non-empty; `defaultFeeUsd` $\ge 0$; `flagAsset` must map to an available SVG asset.

#### 2. Toll Tariff & Exemption Update Schema
```json
{
  "countryCode": "IND",
  "authority": "IRAN",
  "baseFeeUsd": 14000.0,
  "exempt": false,
  "cargoMultiplier": 1.25
}
```
*Immutability Rule*: Modifications apply solely to transits occurring after the update. Existing transactions in the ledger retain their original assessed fee.

#### 3. Vessel Creation & Edit Schema
```json
{
  "id": "SHIP-007",
  "name": "MV Strait Voyager",
  "imo": "9812345",
  "nationality": "SGP",
  "cargoType": "Container Goods",
  "routeId": "ROUTE-INBOUND",
  "initialSpeed": 1.0
}
```
*Validation*: `id` must be unique; `nationality` must reference an active configured country; `routeId` must reference an existing route.

#### 4. Route & Checkpoint Schema
```json
{
  "checkpointId": "CP-IRAN-NORTH",
  "latitude": 26.8500,
  "longitude": 56.4000,
  "radiusKm": 15.0
}
```
*Validation*: Latitude $[-90, 90]$, Longitude $[-180, 180]$, `radiusKm` $> 0$.

### 6.6 Client-Side RFC 4180 CSV Export Specifications

Reporting operates client-side via native Web APIs (`Blob`, `URL.createObjectURL`, `<a>` download), ensuring instant offline export capability during examinations:

1. **Transaction Audit Log Export (`transactions-export.csv`)**:
   - Header: `Transaction_ID,Timestamp,Vessel_ID,Vessel_Name,Nationality,Cargo,Checkpoint_ID,Authority,Amount_USD,Status`
   - Content: Each ledger entry escaped according to RFC 4180 (double quotes around commas).
2. **Aggregated Revenue Summary Export (`revenue-summary.csv`)**:
   - Header: `Category,Metric,Amount_USD,Transaction_Count`
   - Records:
     - `Authority,IRAN,<iranRevenue>,<iranTxCount>`
     - `Authority,OMAN,<omanRevenue>,<omanTxCount>`
     - `Summary,COMBINED_TOTAL,<totalRevenue>,<totalTxCount>`
     - Per-country and per-cargo subtotals.

### 6.7 Traffic Congestion Alert Rules

1. **Configurable Alert Parameters**:
   - `waitingThresholdCaution`: Vessel count triggering caution status (illustrative default: $3$ vessels).
   - `waitingThresholdCongested`: Vessel count triggering congested status (illustrative default: $5$ vessels).
   - `proximityRadiusKm`: Detection radius for sector density evaluation (illustrative default: $5.0\text{ km}$ or configured checkpoint catchment radius).
2. **Academic Simulation Disclaimer**:
   - The alert threshold parameters and default values are configurable demonstration data for educational simulation.
   - They do not represent official maritime safety regulations, governmental waterway management policies, or real-world traffic separation standards.
3. **Sector Density Evaluation**:
   - The simulation continuously scans the positions of all active vessels.
   - For each checkpoint catchment area and corridor segment, the number of vessels within the radius is evaluated:
     $$N_{\text{active}} = \sum_{v \in \text{Vessels}} \mathbf{1}_{\{d(v, \text{CP}) \le \text{proximityRadiusKm}\}}$$
4. **Alert Thresholds**:
   - `NORMAL`: $N_{\text{active}} < \text{waitingThresholdCaution}$ (Status badge: Green, subtle glow).
   - `CAUTION`: $\text{waitingThresholdCaution} \le N_{\text{active}} < \text{waitingThresholdCongested}$ (Status badge: Amber warning).
   - `CONGESTED`: $N_{\text{active}} \ge \text{waitingThresholdCongested}$ (Status badge: Coral pulsing alert).
5. **UI Propagation**:
   - Triggers warning badge in the side panel.
   - Highlights the affected checkpoint zone on the map canvas.

---

## 7. Toll Processing Engine & Transaction Safety

To guarantee financial integrity and prevent duplicate billing during continuous animation loops, toll assessment strictly adheres to an authoritative 13-step pipeline.

```
       [Client Spatial Crossing Event Detected]
                          │
                          ▼
            POST /api/simulation/toll
                          │
                          ▼
        Step 1: Check Content-Type & Parse JSON
                          │
                          ▼
        Step 2: Validate active runId
                          │
                          ▼
        Step 3: Validate vesselId against Registry
                          │
                          ▼
        Step 4: Validate checkpointId against Checkpoint Registry
                          │
                          ▼
        Step 5: Resolve Checkpoint Authority (IRAN / OMAN)
                          │
                          ▼
        Step 6: Resolve Vessel Nationality & Cargo Type
                          │
                          ▼
        Step 7: Check Idempotency Key:
                Key = runId + ":" + vesselId + ":" + checkpointId + ":" + passageId
                Is key in processedTransactions?
                ├── YES ──► Return 409 Conflict / DUPLICATE_IGNORED
                └── NO  ──► Proceed
                          │
                          ▼
        Step 8: Retrieve Toll Policy for (Nationality, Authority)
                          │
                          ▼
        Step 9: Is Nation Exempt under Authority?
                ├── YES ──► Fee = 0.0, Status = "EXEMPT"
                └── NO  ──► Fee = BaseFee * CargoMultiplier, Status = "CHARGED"
                          │
                          ▼
        Step 10: Create Immutable TollTransaction Record
                          │
                          ▼
        Step 11: Atomic Update to In-Memory Ledger:
                 - Store key in processedTransactions
                 - Add Fee to authority revenue (Iran / Oman)
                 - Recalculate Total = Iran + Oman
                          │
                          ▼
        Step 12: Append to Recent Transaction Stream
                          │
                          ▼
        Step 13: Emit 200 OK JSON Response with Itemized Receipt
```

### 7.1 Idempotency Key Structure

Duplicate billing is prevented using a 4-part compound idempotency key:

$$\text{IdempotencyKey} = \text{runId} \mathbin{\Vert} \text{":"} \mathbin{\Vert} \text{vesselId} \mathbin{\Vert} \text{":"} \mathbin{\Vert} \text{checkpointId} \mathbin{\Vert} \text{":"} \mathbin{\Vert} \text{passageId}$$

- **`runId`**: Prevents cross-run conflicts after a simulation reset.
- **`vesselId`**: Uniquely identifies the ship.
- **`checkpointId`**: Ensures Iran and Oman checkpoints are treated as distinct independent entities. A ship charged at `CP-OMAN-SOUTH` is still eligible for toll assessment when crossing `CP-IRAN-NORTH`.
- **`passageId`**: Identifies a specific trip or voyage cycle (e.g., `"PASS-SHIP-001-1"`). If a ship loops around on a new voyage, its `passageId` increments, allowing legitimate assessment for the subsequent voyage.

### 7.2 Concurrency & Single-User Scope

- Thread-safety is achieved using `java.util.concurrent.ConcurrentHashMap` and synchronized revenue accumulators within `SimulationStateService`.
- **Scope Clarification**: The application is tailored for a single-user college practical examination running on a local workstation. Multi-tenant database locking is out of scope for the MVP. Server restarts reset the in-memory state; this design boundary is clearly documented in the application footer.

---

## 8. Server State Management & Reset Lifecycle

### 8.1 Service Lifecycle

1. `ApplicationStartupListener` initializes singleton instances of `SimulationConfigService` and `SimulationStateService`.
2. Services are attached to `ServletContext` via standard attribute keys:
   - `ctx.setAttribute("configService", configService);`
   - `ctx.setAttribute("stateService", stateService);`
   - `ctx.setAttribute("tollPolicyService", tollPolicyService);`
3. Servlets retrieve shared instances in their `init()` or `service()` methods:
   - `configService = (SimulationConfigService) getServletContext().getAttribute("configService");`
4. This ensures a single authoritative revenue ledger across all HTTP threads.

### 8.2 Client-Server State Reconciliation & Reset Invariance

- When the browser reloads `index.html`, `app.js` issues a `GET /api/simulation/state`.
- If a valid `runId` with existing revenue exists on the server, the client updates its UI financial counters to match the server, preventing "zero revenue" UI discrepancies.
- If the user explicitly clicks the **Reset** button:
  1. The client sends `POST /api/simulation/reset` (or triggers client reset if client-side mode is active).
  2. The server/client closes the active simulation run and starts a new run with an incremented `newRunId`.
  3. The active run ledger is cleared and active run revenues are set to $0.00 USD.
  4. **Multi-Run Session History Archive**: Previously recorded transactions are moved into in-memory past-run history (`sessionHistory`), remaining inspectable for audit throughout the active server session.
  5. **In-Memory Retention Boundary**: In-memory historical run records may reset if the application server restarts, as external database persistence is not approved for MVP.
  6. **Historical Immutability Invariant**: Toll rate modifications and exemption updates apply strictly to subsequent crossings; past-run and active-run historical receipts remain immutable.
  7. **Configuration Preservation Invariant**: Resetting the simulation run strictly clears dynamic run state. All user-created or edited countries, custom toll tariffs, added vessels, edited routes, and modified checkpoints MUST REMAIN INTACT across simulation resets. Reset must never wipe out user configuration.

---

## 9. Ship Movement & Map Rendering Engine

### 9.1 Waypoint Navigation & Interpolation

Routes are defined as sequential geographic coordinates: $[W_0, W_1, W_2, \dots, W_n]$.

1. **Path Parametrization**: Total path length $D_{total}$ is calculated as the sum of great-circle distances between adjacent waypoints:
   $$D_{total} = \sum_{i=0}^{n-1} \text{haversine}(W_i, W_{i+1})$$
2. **Animation Loop**:
   - Driven by `window.requestAnimationFrame`.
   - On each frame with elapsed time $\Delta t$, progress along the route increments:
     $$\Delta d = \text{speed} \times \Delta t \times \text{speedMultiplier}$$
     $$\text{distance} = \text{distance} + \Delta d$$
3. **Segment Resolution**:
   - Determine which segment $[W_k, W_{k+1}]$ contains the current distance.
   - Interpolate latitude and longitude linearly along the segment:
     $$\text{lat} = W_k.\text{lat} + t \cdot (W_{k+1}.\text{lat} - W_k.\text{lat})$$
     $$\text{lng} = W_k.\text{lng} + t \cdot (W_{k+1}.\text{lng} - W_k.\text{lng})$$
     where $t \in [0, 1]$ is the fractional distance along segment $k$.
4. **Heading Calculation**:
   - Vessel orientation angle $\theta$ (in degrees clockwise from North) is computed via forward azimuth:
     $$\theta = \text{atan2}\left(\sin(\Delta \lambda) \cos(\phi_2), \cos(\phi_1)\sin(\phi_2) - \sin(\phi_1)\cos(\phi_2)\cos(\Delta \lambda)\right)$$
   - Converted to degrees and normalized to $[0, 360^\circ)$.

### 9.2 Spatial Proximity & Checkpoint Crossing Detection

- For each active vessel, distance to each checkpoint center is calculated using the Haversine formula:
  $$d = 2R \arcsin\left(\sqrt{\sin^2\left(\frac{\Delta \phi}{2}\right) + \cos(\phi_1)\cos(\phi_2)\sin^2\left(\frac{\Delta \lambda}{2}\right)}\right)$$
- If $d \le \text{checkpoint.radiusKm}$ and the vessel has not yet triggered this checkpoint for the current `passageId`:
  - Vessel flags checkpoint as `TRIGGERED_LOCALLY`.
  - Client emits `POST /api/simulation/toll`.
- When $d > \text{checkpoint.radiusKm} \times 1.5$, the vessel exits the catchment zone, clearing the local proximity lock.

---

## 10. Custom Ship Model & Flag Visualization

To fulfill Requirement 4.6 (avoiding generic arrows and displaying a vessel viewed from above with a mast and country flag), the application uses an SVG-based marker rendered via custom Google Maps HTML overlay or SVG data-URI marker.

### 10.1 Coordinated Composite Visual Structure

```
                         [ Country Flag Badge ]  <-- Rectangular national flag (e.g. IN, CN, PA)
                                   |
                             [ Mast Pole ]      <-- Small central mast / vertical line
                                   |
                   +---------------+---------------+
                   |           Superstructure      |
       Bow         |       +---------------+       |       Stern
       |\          |       |  Bridge Deck  |       |          /|
       | \---------+-------+---------------+-------+---------/ |
       |                                                       |
       |                  Ship Hull (Top-Down)                 |
       |                                                       |
       | /---------------------------------------------------\ |
       |/                                                     \|
```

### 10.2 Technical Rendering Implementation

1. **Composite SVG Construction**:
   - The ship is generated as an inline SVG element or an HTML container combining:
     - **Hull Layer**: Top-down tapered boat hull silhouette with deck outlines and cargo bay indicators.
     - **Mast Layer**: Central circular mast base with a miniature upright pole.
     - **Flag Layer**: High-contrast, crisp national flag badge positioned above the mast center.
2. **Dynamic Rotation**:
   - The hull is wrapped in a container that applies CSS rotation:
     ```css
     transform: rotate(${headingDegrees}deg);
     transform-origin: center center;
     transition: transform 0.1s linear;
     ```
   - *Option A (Direction-locked Flag)*: The flag rotates with the ship.
   - *Option B (Upright Flag)*: The flag counter-rotates (`-headingDegrees`) so the national flag remains upright and readable to the user regardless of vessel direction. Option B is designated as the preferred default for maximum visual readability.
3. **Zoom-Compensated Scaling**:
   - Ship dimensions dynamically adapt based on Google Maps zoom level ($z$):
     $$\text{scale} = \max(0.6, \min(1.4, 1.0 + (z - 9) \times 0.15))$$
   - At zoom 8 (zoomed out), the marker scales slightly down to avoid crowding the narrow strait.
   - At zoom 11 (zoomed in), deck details and the mast become prominent.

---

## 11. Google Maps API Configuration & Fallback

### 11.1 Configuration Mechanism

- The application uses a designated placeholder in `js/config.js`:
  ```javascript
  const GOOGLE_MAPS_CONFIG = {
      API_KEY: "YOUR_GOOGLE_MAPS_API_KEY",
      DEFAULT_CENTER: { lat: 26.5667, lng: 56.2500 }, // Strait of Hormuz
      DEFAULT_ZOOM: 9,
      MAP_ID: "HORMUZ_OPERATIONS_MAP"
  };
  ```
- **Runtime Key Configuration Modal**:
  - The UI includes an "API Key" modal/button in the top navigation bar.
  - If `API_KEY` is set to the placeholder or is invalid, the user can paste their Google Maps API key into an input field at runtime.
  - The key is saved to browser `sessionStorage` and dynamically injects the Google Maps script tag:
    `https://maps.googleapis.com/maps/api/js?key=${apiKey}&callback=initMap`
- **Security Guidance**: API keys must not be committed to Git. For academic production, HTTP referrer restrictions (`http://localhost:8080/*`) should be enabled in Google Cloud Console.

### 11.2 Graceful Load Failure Handling

- If Google Maps fails to load (network offline, quota exceeded, or invalid key), `window.gm_authFailure` triggers a clear banner:
  > **Map Loading Notice**: Google Maps JavaScript API could not be initialized with the current API key. Please verify your internet connection or enter a valid key using the API Key button above.
- The UI maintains dashboard metrics and allows reviewing vessel manifests even if map tiles fail to render.

---

## 12. User Interface & Layout Boundaries

Visual styling is governed by the principles in `brain.md` (operations-dashboard aesthetic, asymmetric layout, selective glassmorphism, restrained accent palette). Detailed CSS variables, typography, and color codes will be finalized in `UI_UX_GUIDELINES.md`.

### 12.1 Layout Wireframe

```
+-----------------------------------------------------------------------------------------------+
| [HVS LOGO]  HORMUZ VASOOLI SYSTEM   | [SIMULATION DISCLAIMER BADGE] | [API Key] [Server Status]|
+-----------------------------------------------------------------------------------------------+
| +-------------------------+ +---------------------------------------------------------------+ |
| | SIMULATION CONTROLS     | |                       FINANCIAL OVERVIEW                      | |
| | [Start] [Pause] [Reset] | |  Iran Revenue: $27,000  |  Oman Revenue: $10,000  |  Total: $37K| |
| | Speed: [1x] [2x] [4x]   | +---------------------------------------------------------------+ |
| +-------------------------+ |                                                               | |
| | FLEET TRAFFIC SUMMARY   | |                                                               | |
| | Total: 6  Waiting: 2    | |                     GOOGLE MAPS WORKSPACE                     | |
| | Passing: 3  Passed: 1   | |                 (Centered on Strait of Hormuz)                | |
| +-------------------------+ |                                                               | |
| | SEARCH & FILTERS        | |   [Iran Checkpoint Zone]               [Vessel Marker w/ Flag]| |
| | [ Search by name... ]   | |                                                               | |
| | Country: [All        v] | |                                                               | |
| | Cargo:   [All        v] | |                                                               | |
| | Status:  [All        v] | |                  [Oman Checkpoint Zone]                       | |
| +-------------------------+ |                                                               | |
| | VESSEL REGISTRY LIST    | |                                                               | |
| | > MT Hormuz Pioneer     | +---------------------------------------------------------------+ |
| | > MV Yangtze Dragon     | | RECENT TOLL TRANSACTIONS (LIVE STREAM)                        | |
| | > MV Pacific Trader     | | [10:14:02] SHIP-001 charged $15,000 at Iran Checkpoint        | |
| | > MT Arabian Breeze     | | [10:14:15] SHIP-002 EXEMPT ($0) at Iran Checkpoint (Strategic)| |
| +-------------------------+ +---------------------------------------------------------------+ |
+-----------------------------------------------------------------------------------------------+
```

---

## 13. Error Handling, Validation, and Resilience

| Failure Scenario | Detection Point | Handling & Recovery Action |
| :--- | :--- | :--- |
| **Missing / Invalid API Key** | `window.gm_authFailure` / Script onerror | Display non-blocking notification banner; open API key configuration modal |
| **Backend Unreachable** | `fetch()` reject in `api-client.js` | Show visual offline indicator; queue non-critical events; do not crash movement |
| **Invalid `runId` on Toll Post** | `TollAssessmentServlet` | Server returns HTTP `400 Bad Request`; client triggers state resynchronization |
| **Unknown `vesselId` / `checkpointId`** | `TollPolicyService` | Server returns HTTP `404 Not Found`; logs warning; does not adjust revenues |
| **Duplicate Checkpoint Event** | `SimulationStateService` | Server returns HTTP `409 Conflict`; client logs debug note; ignores fee update |
| **Malformed JSON Payload** | `BaseServlet` | Returns HTTP `400 Bad Request` with structured error JSON: `{"error": "MALFORMED_JSON"}` |
| **Missing Config Resource** | `SimulationConfigService` | Fatal exception during startup logged via `java.util.logging`; deployment halts with clear log message |

---

## 14. Build, Packaging, and GlassFish Deployment

### 14.1 Prerequisites
- NetBeans IDE 8.2 installed.
- Oracle JDK 8 (`1.8.0_181`) configured in NetBeans (`Tools -> Java Platforms`).
- GlassFish Server 4.1.1 registered in NetBeans (`Services -> Servers`).

### 14.2 Build Sequence
1. **Clean and Compile**:
   - Execute `mvn clean compile` using NetBeans bundled Maven.
   - Compiles all classes under `src/main/java` targeting bytecode level 1.8.
2. **Package Web Archive**:
   - Execute `mvn package`.
   - Packages `target/HormuzVasooliSystem-1.0-SNAPSHOT.war`.
   - Copies `simulation-config.json` into `WEB-INF/classes/simulation-config.json`.
3. **GlassFish Deployment**:
   - Right-click project in NetBeans -> **Run** (or deploy via GlassFish Admin Console at `http://localhost:4848`).
   - Context root defaults to `/HormuzVasooliSystem`.
4. **Verification URL**:
   - Access the application at `http://localhost:8080/HormuzVasooliSystem/`.
   - Verify that `http://localhost:8080/HormuzVasooliSystem/api/simulation/config` returns `200 OK` with JSON configuration.

---

## 15. Testing & Verification Strategy

Detailed test cases will be specified in `TESTING_PLAN.md`. The technical architecture must support the following automated and manual test categories:

1. **Unit & Service Tests**:
   - `SimulationConfigServiceTest`: Verify JSON parsing of ships, checkpoints, and tariff rules from classpath.
   - `TollPolicyServiceTest`:
     - Test standard charge calculation (`BaseFee * CargoMultiplier`).
     - Test country exemption returning `0.0 USD` and status `"EXEMPT"`.
     - Test duplicate prevention returning duplicate status upon second invocation with identical idempotency key.
     - Test that a ship crossing an Oman checkpoint after an Iran checkpoint is correctly assessed at both.
2. **Integration & API Tests**:
   - `GET /api/simulation/config`: Verify JSON response schema and HTTP 200.
   - `POST /api/simulation/toll`:
     - Valid request updates revenue atomically.
     - Duplicate request returns HTTP 409 or duplicate acknowledgement without revenue increment.
   - `POST /api/simulation/reset`: Verify revenue zeroing and run ID regeneration.
3. **Frontend Visual & Motion Tests**:
   - Verify vessel orientation aligns with route direction vectors.
   - Verify country flag displays atop the mast and remains legible at different zoom levels.
   - Verify filter controls isolate specific vessels without stopping background simulation.

---

## 16. Extensibility for Professor Practical Modifications

During the college practical examination, the professor may request live modifications. Rather than requiring code rewrites, the system provides both **interactive UI management interfaces** (active in the running application) and seed configuration files:

| Potential Professor Request | Interactive UI Method | Seed Config File | Implementation Action |
| :--- | :--- | :--- | :--- |
| **Add a New Country** | Country Management Drawer | `simulation-config.json` | Specify ISO code, name, SVG flag, default tariff, exemption status. |
| **Modify a Country's Toll Rate** | Toll Policy Drawer | `simulation-config.json` | Edit tariff for Iran or Oman. Future transits billed at new rate; past ledger immutable. |
| **Grant an Exemption** | Toll Policy Drawer | `simulation-config.json` | Toggle exemption switch. Future transits billed at $0.00 EXEMPT. |
| **Add a New Ship** | Vessel Management Drawer | `simulation-config.json` | Specify vessel name, IMO, nationality, cargo, and route assignment. |
| **Modify Checkpoint Coordinates** | Checkpoint Drawer | `simulation-config.json` | Update latitude, longitude, or radius. Map overlay updates immediately. |
| **Add a Route Waypoint** | Route Management Drawer | `simulation-config.json` | Add waypoint coordinates to adjust vessel corridor path. |
| **Export Transaction / Revenue Report** | Revenue / Ledger Panel | N/A | Click "Export CSV" to trigger client-side RFC 4180 download. |
| **Adjust Speed / Density Threshold** | Dashboard Controls | `config.js` | Toggle 1x/2x/4x speed presets; adjust congestion alert thresholds. |

---

## 17. Technical Decisions and Open Items

### 17.1 Approved Technical Decisions
- **Decision TRD-01 (CONFIRMED)**: Single-page architecture using vanilla HTML, CSS, and ES6 JavaScript.
- **Decision TRD-02 (CONFIRMED)**: Java EE 7 Web with native `javax.json` for all backend JSON operations.
- **Decision TRD-04 (CONFIRMED)**: In-memory thread-safe state for local examination demonstration (no database required for MVP).
- **Decision TRD-05 (CONFIRMED)**: Client-side coordinate interpolation with SVG ship models featuring top-down hulls, masts, and local SVG national flags.
- **Decision TRD-06 (CONFIRMED)**: Idempotency latching mechanism to prevent duplicate billing during checkpoint encounters.
- **Decision TRD-07 (CONFIRMED)**: Interactive UI management interfaces for countries, toll tariffs, vessels, routes, checkpoints, CSV export, congestion alerts, and layer toggles (DEC-026).
- **Decision TRD-08 (CONFIRMED)**: Historical transaction ledger immutability and configuration preservation across simulation resets.

### 17.2 Proposed Decisions (Awaiting Explicit Approval)
- **Decision TRD-03 (PROPOSED - DEC-010)**: Backend-authoritative toll assessment servlet pipeline. Note: Until formally approved, server-authoritative tolling remains proposed; the baseline constraint is strictly decoupling toll policy calculation from animation loops.
- **Decision TRD-P2 (PROPOSED - DEC-012)**: `simulation-config.json` exact seed schema on classpath.
- **Decision TRD-P3 (PROPOSED - DEC-013)**: Exact REST servlet endpoint naming structure.
- **Decision TRD-P4 (PROPOSED - DEC-014)**: Specific compound format for server idempotency key (`runId:vesselId:checkpointId:passageId`).

### 17.3 Open Items Requiring Verification During Implementation
- **Open Item TRD-O1**: Verification of NetBeans 8.2 Maven compiler plugin upgrade to Java 8 (`1.8`) without disrupting GlassFish endorsed libraries.
- **Open Item TRD-O2**: Specific CSS styling variables and glassmorphism panel blur performance across different local browsers (to be finalized in `UI_UX_GUIDELINES.md`).
- **Open Item TRD-O3**: Selection between Google Maps `OverlayView` vs inline SVG Data-URI Markers for optimal performance and event handling.

---

## 18. Document Authority & Ownership

- **`docs/brain.md`**: Owns overall project identity, environmental constraints, and working rules.
- **`docs/PRD.md`**: Owns functional requirements, user stories, and acceptance criteria.
- **`docs/TRD.md`** (This Document): Owns technical architecture, component responsibilities, API contracts, and implementation design.
- **`docs/UI_UX_GUIDELINES.md`**: Will own visual design, layout specifications, and style tokens.
- **Status**: Draft for Review. Implementation must not commence until design guidelines and implementation planning are approved.
