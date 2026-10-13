# Hormuz Vasooli System
# UI/UX Design Guidelines & Visual Specifications

Version: 1.0  
Status: Draft for User Review  
Project Type: College Practical Examination Project  
Primary Visual Reference: `docs/references/visual-reference.png` (Ledgerix Maritime Operations Dashboard)

---

## 1. Visual Style & Aesthetic Foundation

The visual design of **Hormuz Vasooli System (HVS)** is inspired directly by the user-selected visual reference image ([visual-reference.png](file:///c:/Users/godbl/OneDrive/Documents/NetBeansProjects/HormuzVasooliSystem/docs/references/visual-reference.png)).

The application is an **interactive maritime operations and hypothetical toll-simulation dashboard**, built for a college practical examination. It is neither a generic commercial landing page nor a brightly colored toy admin template. The central visual hero is the **interactive Google Map showing the Strait of Hormuz and animated vessels**, framed by sophisticated, translucent operational cards.

```
+----------------------------------------------------------------------------------------------------+
|                                    AESTHETIC PILLARS                                               |
+----------------------------------------------------------------------------------------------------+
| 1. Deep Maritime Foundation : Oceanic slate, midnight charcoal, and dark obsidian bases.           |
| 2. Controlled Glassmorphism : Selective translucent panels (backdrop-filter: blur(14px)) that let  |
|                               underlying map routes remain visible without sacrificing text contrast.|
| 3. High-Energy Warm Coral   : Vivid orange-red (#FF5520 / #FF4500) accents inspired by the        |
|    / Electric Orange Accents  reference image for active controls, toll checkpoints, and key KPIs.  |
| 4. Clean Asymmetric Layout  : Floating modular panels with disciplined spacing and subtle borders. |
| 5. Functional Restraint     : Every indicator, graph, and badge represents live simulation data.   |
|                               Zero decorative charts, zero fake metrics, zero purple neon glow.    |
+----------------------------------------------------------------------------------------------------+
```

### 1.1 Reference Image Analysis (`visual-reference.png`)
Detailed visual inspection of `docs/references/visual-reference.png` reveals several key patterns that form our design foundation:
- **Atmosphere & Depth**: Deep navy-slate background overlaid with floating translucent panels. The aerial maritime perspective provides a natural visual anchor.
- **Surface Treatment**: Translucent dark charcoal surfaces (`rgba(22, 28, 36, 0.72)`) with delicate, crisp boundary borders (`1px solid rgba(255, 255, 255, 0.08)` to `0.12`).
- **Signature Accent**: A vibrant, punchy orange-red (`#FF5520` / `#FF4500`) applied strategically to primary navigation pills, active buttons, metric callouts, and upward trend vectors.
- **Header & Navigation**: A compact, floating horizontal pill-bar header with rounded segmented navigation icons, search pill, and operational status indicators.
- **Card Topology**: Smooth rounded corners (`16px` to `20px` radius), subtle inner drop shadows, and top-right directional icon markers (`↗`) on expandable summary cards.
- **Data Visualization**: Clean, high-contrast numeric KPI typography paired with minimalist vertical bar charts, timeline activity sparks, and segment progress pills.

---

## 2. Desktop Dashboard Layout Architecture

The desktop view utilizes a **map-centered, asymmetric floating panel composition** designed for 1920x1080 and 1366x768 display environments (typical of college lab monitors and laptops).

```
+---------------------------------------------------------------------------------------------------------------------------------------+
| [LOGO: HVS] Hormuz Vasooli System | [NAV: Map | Fleet | Policies | Countries | Ledger | Reports] | [1x 2x 4x] [Alert: OK] [API Key]    |
+---------------------------------------------------------------------------------------------------------------------------------------+
| +-----------------------------+ +---------------------------------------------------------------------------------------------------+ |
| | SIMULATION CONTROLS         | |                                       FINANCIAL METRICS BAR                                       | |
| | [► Start] [❚❚ Pause] [⟲ Reset]| | Iran Revenue: $27,000 USD   |   Oman Revenue: $10,000 USD   |   Combined Total: $37,000 USD       | |
| | Speed: [1x] [2x] [4x]       | +---------------------------------------------------------------------------------------------------+ |
| +-----------------------------+ | [LAYERS: Routes 👁 | Checkpoints 👁 | Labels 👁]                                                        | |
| | TRAFFIC METRICS (KPIs)      | |                                                                                                   | |
| | Active Vessels: 6           | |                                        GOOGLE MAPS CANVAS                                         | |
| | - Waiting: 2                | |                                 (Centered on Strait of Hormuz)                                    | |
| | - Passing: 3                | |                                                                                                   | |
| | - Passed:  1                | |      [IRAN CHECKPOINT ZONE]                              [VESSEL MARKER: SHIP-001]                | |
| +-----------------------------+ |       (Northern Green Corridor)                            (Hull + Mast + Indian Flag)            | |
| | SEARCH & FILTER PANEL       | |                                                                                                   | |
| | [ 🔍 Search vessel/IMO... ] | |                                                                                                   | |
| | Nation: [All Nations    v]  | |                                                                                                   | |
| | Cargo:  [All Cargo Types v] | |      [OMAN CHECKPOINT ZONE]                                                                       | |
| | Status: [All Statuses   v]  | |       (Southern Coral Corridor)                                                                   | |
| +-----------------------------+ |                                                                                                   | |
| | FLEET REGISTRY & ACTIONS    | |                                                                                                   | |
| | [+ Add Vessel] [+ Manage]   | +---------------------------------------------------------------------------------------------------+ |
| | > MT Hormuz Pioneer (IN)    | | LIVE TOLL TRANSACTION STREAM & AUDIT LEDGER                                    [ ⬇ Export CSV ]   | |
| | > MV Yangtze Dragon (CN)    | | [10:14:02] SHIP-001 (India) -> Charged $15,000 USD at Iran Northern Sector                           | |
| | > MV Pacific Trader (PA)    | | [10:14:15] SHIP-002 (China) -> EXEMPT ($0 USD) at Iran Northern Sector (Friendly)                    | |
| +-----------------------------+ +---------------------------------------------------------------------------------------------------+ |
+---------------------------------------------------------------------------------------------------------------------------------------+
```

### 2.1 Workspace Allocation
- **Central Map Workspace (70% - 75% Viewport Area)**: The Google Map is rendered edge-to-edge behind translucent floating panels, keeping maritime geography, shipping lanes, and ship movements persistently visible.
- **Top Application Bar (`h: 56px - 64px`)**: Floating rounded pill header containing application identity, view switcher (Map, Fleet, Policies, Countries, Ledger, Reports), simulation speed toggles (configurable demonstration presets: 1x, 2x, 4x), congestion status badge, API key configuration, and server status.
- **Left Control & Filter Stack (`w: 320px - 360px`)**: Floating vertical stack housing simulation execution controls, traffic counts, multi-attribute search/filter controls, and fleet registry with quick-action management triggers.
- **Top-Right Financial Summary (`w: auto`, `h: 72px`)**: Horizontal floating card cluster displaying Iran Revenue, Oman Revenue, and Combined Total in large, crystal-clear monospace typography.
- **Map Layer Visibility Bar (Floating, Top-Left of Map)**: Compact pill toggles to show/hide route polylines, checkpoint circles, and vessel labels.
- **Bottom Transaction Ledger (`h: 130px`, Collapsible)**: Stream of processed toll assessments with color-coded outcome badges and an integrated `[⬇ Export CSV]` button.
- **Slide-In Management Drawers & Modals (`w: 420px`)**: Glassmorphic slide-out panels for dynamic CRUD operations on countries, toll policies, vessels, routes, and checkpoints.
- **Inspection Modal / Detail Drawer (`w: 380px`, Slide-in)**: Displays itemized manifests and receipts upon selecting any vessel or transaction.

---

## 3. Colour System & Design Tokens

The color palette is derived directly from `visual-reference.png`, featuring a deep oceanic charcoal base contrasted against high-visibility warm coral/electric orange accents.

```
  #0B0E14       #161C24       #212B36       #FF5520       #00E5FF       #00E676       #FF3D57
[Base Dark]  [Panel Glass] [Panel Hover] [Accent Orange][Route Cyan]  [Success Green][Alert Red]
```

### 3.1 Design Tokens (CSS Custom Properties)

```css
:root {
    /* Base Foundations */
    --hvs-bg-base: #0B0E14;             /* Canvas background behind map tiles */
    --hvs-bg-surface: rgba(22, 28, 36, 0.78); /* Floating panel glass background */
    --hvs-bg-surface-elevated: rgba(33, 43, 54, 0.88); /* Dropdowns, modals, drawers */
    --hvs-bg-surface-subtle: rgba(255, 255, 255, 0.04); /* Inset inputs, metric blocks */

    /* Glassmorphism & Borders */
    --hvs-glass-blur: blur(14px);
    --hvs-border-subtle: rgba(255, 255, 255, 0.08); /* Card outer boundaries */
    --hvs-border-strong: rgba(255, 255, 255, 0.16); /* Interactive focus, active pills */
    --hvs-border-accent: rgba(255, 85, 32, 0.45);   /* Active card highlight */

    /* Typography & Contrast */
    --hvs-text-primary: #FFFFFF;        /* Headings, primary numeric values */
    --hvs-text-secondary: #919EAB;      /* Subtitles, labels, descriptions */
    --hvs-text-muted: #637381;          /* Inactive icons, timestamps, watermarks */
    --hvs-text-inverse: #0B0E14;        /* Text on bright accent badges */

    /* Signature Accent Palette (Inspired by Reference) */
    --hvs-accent-orange: #FF5520;       /* Primary CTA, toll checkpoints, active pill */
    --hvs-accent-orange-hover: #FF6E40;
    --hvs-accent-orange-subtle: rgba(255, 85, 32, 0.15);

    /* Authority Distinction Palette */
    --hvs-iran-primary: #00C853;        /* Iran authority theme (maritime green) */
    --hvs-iran-subtle: rgba(0, 200, 83, 0.18);
    --hvs-oman-primary: #FF5520;        /* Oman authority theme (maritime coral/red) */
    --hvs-oman-subtle: rgba(255, 85, 32, 0.18);

    /* Operational & Semantic States */
    --hvs-route-cyan: #00E5FF;          /* TSS shipping route corridors */
    --hvs-route-cyan-subtle: rgba(0, 229, 255, 0.12);
    --hvs-status-success: #00E676;      /* Charged toll, passing status */
    --hvs-status-exempt: #FFD600;       /* Exempt friendly nation passage */
    --hvs-status-duplicate: #B0BEC5;    /* Duplicate passage ignored */
    --hvs-status-warning: #FF9100;      /* Near boundary, caution */
    --hvs-status-danger: #FF3D57;       /* API failure, collision, invalid event */

    /* Elevation & Shadows */
    --hvs-shadow-panel: 0 12px 32px 0 rgba(0, 0, 0, 0.45);
    --hvs-shadow-accent: 0 4px 16px 0 rgba(255, 85, 32, 0.35);
    --hvs-radius-sm: 8px;
    --hvs-radius-md: 14px;
    --hvs-radius-lg: 20px;
    --hvs-radius-pill: 9999px;
}
```

*Note: This color palette is proposed based on `visual-reference.png` and remains subject to user review.*

---

## 4. Typography System

The typography hierarchy balances industrial precision with high legibility.

### 4.1 Recommended Font Families
- **Display & Headings**: `Manrope`, system fallback `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`.
- **Interface & Body**: `Inter` or `Plus Jakarta Sans`, system fallback `Segoe UI, Arial, sans-serif`.
- **Financial Figures, Coordinates & IDs**: `IBM Plex Mono` or `Consolas`, monospace fallback `monospace`.

Fonts will be loaded cleanly via standard Google Fonts `<link>` in `index.html` with robust local system fallbacks so the application operates seamlessly even in an offline college lab environment.

### 4.2 Type Scale & Hierarchy

| Role | Font Family | Size | Weight | Line Height | Usage |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Hero Metric** | IBM Plex Mono | 28px | 700 Bold | 34px | Total Combined Revenue figure ($37,000) |
| **Section KPI** | IBM Plex Mono | 22px | 600 SemiBold | 28px | Iran / Oman separate revenue figures |
| **Header Title** | Manrope | 16px | 700 Bold | 22px | Application branding ("Hormuz Vasooli System") |
| **Card Header** | Manrope | 14px | 600 SemiBold | 20px | Panel titles ("Simulation Controls", "Traffic Summary") |
| **Body Standard** | Inter | 13px | 400 Regular | 18px | Descriptive labels, vessel manifests, table entries |
| **Caption / Meta** | Inter | 11px | 500 Medium | 14px | Checkpoint coordinates, cargo weights, disclaimer text |
| **Badge / Pill** | Inter | 10px | 700 Bold | 12px | All-caps status pills (`CHARGED`, `EXEMPT`, `PASSING`) |

---

## 5. Map Design & Overlay Aesthetics

### 5.1 Google Maps Custom Styling
To harmonize with the dark maritime dashboard, Google Maps will initialize with a custom JSON style array:
- **Water Bodies**: Deep obsidian navy (`#0d141f` to `#101926`).
- **Landmasses (Iran & Oman)**: Muted slate-charcoal (`#192231`), creating strong contrast with the coastal boundaries.
- **Boundaries & Borders**: Crisp, thin borders (`#2c3b52`) clearly demarcating Iranian and Omani territory.
- **Labels**: Subdued grey labels (`#637381`) preventing visual clutter beneath ship paths.
- **Roads / Urban Clutter**: Hidden or minimized (`visibility: "off"`).

### 5.2 Navigation Channels (Traffic Separation Scheme - TSS)
- **Inbound Lane (Southern Channel)**: Rendered as a styled semi-transparent polyline (`strokeColor: "#00E5FF"`, `strokeOpacity: 0.6`, `strokeWeight: 3`, dashed or dotted).
- **Outbound Lane (Northern Channel)**: Rendered with distinct directional arrows showing movement toward the Gulf of Oman (`strokeColor: "#29B6F6"`, `strokeWeight: 3`).

### 5.3 Hypothetical Checkpoint Zones
- **Visual Distinction**:
  - **Iran Northern Sector Checkpoint**: Circular geographic zone (`radius: 12km`) with translucent emerald fill (`rgba(0, 200, 83, 0.12)`), solid border (`#00C853`, `2px`), and an anchored circular authority beacon.
  - **Oman Musandam Sector Checkpoint**: Circular geographic zone (`radius: 12km`) with translucent coral fill (`rgba(255, 85, 32, 0.12)`), solid border (`#FF5520`, `2px`), and an anchored authority beacon.
- **Disclaimer Enforcement**: Checkpoint markers must feature the badge text: `"HYPOTHETICAL TOLL ZONE — SIMULATION ONLY"` in tooltip and detail drawers.

---

## 6. Custom Ship Models, Masts, and Country Flags

In strict compliance with **Requirement 4.6** and **TRD Section 10**, ships must not be represented as generic colored arrows. Every vessel is an individual composite vector entity.

```
                         +-----------------------------+
                         | [Flag: assets/flags/in.svg] |  <-- Crisp, rectangular vector flag (18x12px)
                         +--------------+--------------+
                                        |
                                   [Mast: 8px]            <-- Centered vertical mast pole
                                        |
                         +--------------+--------------+
                         |       Bridge / Cabins       |
        Bow              +-----------------------------+               Stern
       / \                    |   |   |   |   |                      |
      /   \-------------------+---+---+---+---+----------------------|
     |      Cargo Deck (Containers / Tanks / Bulk Holds)             |
      \   /----------------------------------------------------------|
       \ /                                                           |
```

### 6.1 Composite Marker Specifications
1. **Ship Model (Top-Down Silhouette)**:
   - Rendered via SVG path representing an authentic ship hull viewed from above: tapered bow, parallel midbody cargo deck, and squared stern.
   - Distinct hull color accents by cargo category:
     - *Crude Oil / Tanker*: Deep slate hull with rust-red deck tanks.
     - *LNG Carrier*: Slate hull with insulated dome outlines.
     - *Container Ship*: Structured grid of multi-colored container cells.
     - *Food Grains / Bulk*: Neutral beige/charcoal deck hatches.
     - *Medicines / General*: Clean maritime slate with medical green or cross insignia.
2. **Mast / Aerial Pole**:
   - A visible vertical pole centered on the vessel's superstructure.
   - Rendered in high-contrast silver/white (`#E0E6ED`) with a subtle masthead beacon dot.
   - Firmly anchors the national flag above the deck without detaching during movement.
3. **National Flag Rendering (SVG Assets — No Unicode Emojis)**:
   - **No Unicode Emojis**: National flags must never be rendered using system Unicode flag emojis (which vary wildly across operating systems, lack consistent rectangular proportions, and blur at non-standard sizes).
   - **Local SVG Assets**: Flags must be rendered using dedicated, crisp vector SVG assets (or equivalent local raster assets) stored within the web application: `src/main/webapp/assets/flags/{flagIcon}.svg`.
   - **Heading-Aware Body Rotation**: The ship hull rotates dynamically based on its forward azimuth heading angle ($\theta$).
   - **Upright, Legible Flag (Counter-Rotation)**: The flag badge is counter-rotated ($-\theta$) relative to the vessel hull, ensuring the flag remains permanently horizontal, upright, and readable to the user regardless of whether the ship is travelling north, south, east, or west.
   - **Rigid Coordinated Motion**: The ship hull, mast, and country flag share a single coordinate anchor point on the Google Map overlay, ensuring the components never drift, separate, or jitter during animation.

### 6.2 Configuration-Driven Flag Resolution & Extensibility
- **Dynamic Country Mapping**: The renderer must never hard-code a closed list of nationalities.
- **Config-Driven Asset Binding**: Each vessel in `simulation-config.json` specifies a `nationality` and a corresponding `flagIcon` token (e.g., `"flagIcon": "in"`, `"flagIcon": "cn"`, `"flagIcon": "pa"`, `"flagIcon": "lr"`, `"flagIcon": "om"`, `"flagIcon": "ir"`). The renderer dynamically constructs the asset URI:
  ```javascript
  const flagUri = `assets/flags/${ship.flagIcon || 'fallback'}.svg`;
  ```
- **Seamless Extensibility for Practical Exam**: If the professor requests adding a new country during the exam (e.g., Japan, Germany, Greece, Singapore), the operator merely adds the country to `simulation-config.json` and places `jp.svg` into `assets/flags/`. The ship-rendering engine requires zero code changes or recompilation.
- **Sensible Fallback Mechanism**: If a vessel's nationality is missing, unrecognized, or the target SVG file fails to load:
  - The renderer falls back gracefully to `assets/flags/fallback-maritime.svg` (a clean neutral nautical ensign or generic blue maritime flag with the vessel's 2-letter country code stamped on it).
  - The map simulation continues unhindered without throwing JavaScript errors, console exceptions, or displaying broken image icons.

### 6.3 Zoom-Compensated Proportions & Visual Consistency
To prevent the mast or flag from obscuring the ship hull at different zoom levels, strict bounded scaling rules apply:

| Map Zoom Level | Hull Length / Width | Mast Height | Flag Dimensions (W x H) | Visual Behavior & Proportions |
| :--- | :--- | :--- | :--- | :--- |
| **Zoom 8 (Overview)** | `36px x 10px` | `6px` | `14px x 9px` | Compact overview; flag remains legible; hull remains primary anchor. |
| **Zoom 9–10 (Standard)** | `48px x 14px` | `8px` | `18px x 12px` | Ideal demonstration scale; deck features, mast, and crisp flag fully balanced. |
| **Zoom 11+ (Close Inspection)**| `60px x 18px` | `10px` | `22px x 15px` | Detailed view; cargo contours and wake foam visible; flag never covers the hull. |

- **Strict Boundary Constraint**: The flag's surface area must never exceed 35% of the hull's surface area, guaranteeing that the flag complements the ship model rather than masking it.


---

## 7. Information Panels & Visual Hierarchy

### 7.1 Financial Overview Bar (Top Right)
The revenue counters represent the core practical demonstration output and must command immediate attention:
- **Combined Total Revenue Card**:
  - Background: Gradient glow inspired by reference (`linear-gradient(135deg, rgba(255, 85, 32, 0.25), rgba(22, 28, 36, 0.85))`).
  - Label: `"COMBINED SIMULATED REVENUE"` in `10px` uppercase tracking.
  - Figure: High-visibility monospace `$37,000 USD` in bold `24px` white.
- **Iran Revenue Card**:
  - Subdued emerald left accent stripe (`#00C853`).
  - Figure: `$27,000 USD`.
- **Oman Revenue Card**:
  - Subdued coral left accent stripe (`#FF5520`).
  - Figure: `$10,000 USD`.

### 7.2 Fleet Traffic Status (Left Panel)
Inspired by the active shipments widget in `visual-reference.png`:
- **Active Fleet KPI**: Large `1,780` style count showing total ships (`6`).
- **Segmented Horizontal Bar**: Three-part proportional color bar:
  - Yellow segment = Waiting (`2`).
  - Cyan segment = Passing (`3`).
  - Green segment = Passed (`1`).
- Below the bar: Micro-metrics with indicator arrows showing vessel breakdown.

### 7.3 Selected Vessel Inspection Drawer (Slide-In)
When any vessel on the map or registry is clicked:
- A glassmorphic drawer slides in smoothly from the right edge (`380px` width).
- **Header**: Vessel Name (`MT Hormuz Pioneer`), IMO number, and large national flag.
- **Manifest Table**: Origin port, Destination port, Cargo classification, Gross Tonnage, Current Speed.
- **Toll Status Section**:
  - Iran Checkpoint Status (`PASSED & CHARGED: $15,000 USD` with receipt ID).
  - Oman Checkpoint Status (`APPROACHING`).
- **Action**: `"Center Map on Vessel"` button styled in primary coral.

### 7.4 Management Drawers & Dynamic Configuration Modals
To support live additions and edits during practical examinations without recompilation:
- **Surface Styling**: Glassmorphic modal dialog or right-anchored drawer (`w: 420px`, `background: rgba(33, 43, 54, 0.94)`, `backdrop-filter: blur(16px)`, `border: 1px solid rgba(255, 255, 255, 0.14)`).
- **Form Controls**:
  - Inputs & Dropdowns: Inset dark background (`rgba(255, 255, 255, 0.05)`), border (`1px solid rgba(255, 255, 255, 0.12)`), text (`#FFFFFF`), focus highlight (`--hvs-border-accent`).
  - Validation Alerts: Inline error text in red (`--hvs-status-danger: #FF3D57`) displayed directly beneath invalid inputs (e.g., negative tariff, duplicate ISO code, empty name).
- **Specific Management Panels**:
  1. *Country Registry Modal*: Form fields for Country Name, 3-letter ISO Code, SVG Flag Asset selector/path, Default Toll Tariff ($USD), and Exemption Toggle.
  2. *Toll Policy Editor*: Form fields to adjust Iran and Oman base rates, cargo multipliers, and friendly nation exemption toggles. Includes clear disclaimer: *"Policy updates apply strictly to future transits; past ledger records remain immutable."*
  3. *Vessel Management Drawer*: Form fields to create or edit simulated ships (Name, IMO, Nationality dropdown, Cargo dropdown, Assigned Route).
  4. *Checkpoint & Route Coordinate Editor*: Numeric coordinate inputs for Latitude $[-90, 90]$, Longitude $[-180, 180]$, and Detection Radius ($km$).
- **Action Buttons**: Primary action (`"Save & Apply"`) in solid coral (`#FF5520`); secondary action (`"Cancel"`) in neutral glass outline.

### 7.5 Revenue Reports & RFC 4180 CSV Export UI
The revenue dashboard provides direct auditability and reporting:
- **Tabular Revenue Breakdown**:
  - Breakdown by Authority: Iran Revenue, Oman Revenue, Combined Total with percentage bars.
  - Breakdown by Nationality: Itemized revenue generated per country with vessel transit counts.
  - Breakdown by Cargo: Itemized revenue generated per cargo category.
- **Export Action Triggers**:
  - High-visibility `[⬇ Export Transaction Log (CSV)]` and `[⬇ Export Revenue Summary (CSV)]` buttons styled in coral outline with hover fill.
  - Generates instant client-side RFC 4180 CSV download via native browser Blob.
  - Displays transient green toast notification: *"Report downloaded: transactions-export.csv"*.

### 7.6 Traffic Congestion Alert Badges & Sector Highlighting
To demonstrate active traffic corridor monitoring:
- **Configurable Thresholds & Academic Disclaimer**: Parameters include waiting-vessel count threshold and proximity radius. Illustrative defaults (e.g. $<3$ normal, $3$–$4$ moderate, $\ge 5$ congested) are academic simulation defaults and do not represent official maritime traffic regulations.
- **Header / Panel Status Badge**:
  - `FLUID`: Green indicator badge (`rgba(0, 230, 118, 0.15)`), text: *"Corridor: Normal (<3 vessels)"*.
  - `MODERATE`: Amber indicator badge (`rgba(255, 145, 0, 0.2)`), text: *"Corridor: Moderate (3-4 vessels)"*.
  - `CONGESTED`: Pulsing coral-red alert badge (`rgba(255, 61, 87, 0.3)`), text: *"Corridor: High Density (≥5 vessels)"*.
- **Map Visual Highlighting**: When density exceeds caution thresholds, the corresponding checkpoint circle or corridor segment gains a pulsating accent ring.

### 7.7 Map Layer Visibility Controls
A floating, segmented glass pill bar anchored to the top-left of the map canvas provides instant layer toggles:
- `[Routes 👁]` (Cyan accent): Toggles visibility of Traffic Separation Scheme polylines.
- `[Checkpoints 👁]` (Emerald / Coral accents): Toggles visibility of checkpoint circles and detection zones.
- `[Labels 👁]` (White text): Toggles visibility of floating vessel name and flag tags.

---

## 8. Interaction States & Motion Design

Every state transition must provide unambiguous visual feedback without disruptive screen jitter.

| Element | Idle State | Hover / Focus State | Active / Operating State |
| :--- | :--- | :--- | :--- |
| **Start Button** | Coral outline, white icon | Solid coral fill, subtle glow (`box-shadow: 0 0 16px rgba(255,85,32,0.4)`) | Green "Running" pulse badge |
| **Pause Button** | Dark glass, grey icon | White icon, border highlight | Amber paused indicator |
| **Reset Button** | Dark glass, grey icon | Red outline warning highlight | Smooth spin animation on icon |
| **Speed Toggle (1x, 2x, 4x)** | Dark glass pill, muted text | Accent border highlight | Solid coral pill background, white bold text |
| **Ship Marker** | Normal opacity (1.0), upright flag | Soft white halo around hull | Thick cyan border, glowing wake trail, selected drawer opens |
| **Toll Crossing** | Checkpoint circle idle | Mouse hover shows authority & toll rate card | **Pulse Wave Animation**: Expanding concentric ring emits from checkpoint; financial counter numbers roll upward |
| **Exempt Passage** | Standard transit | N/A | Gold shimmer ring over ship; ledger logs `"EXEMPT - $0"` |
| **Duplicate Hit** | Standard transit | N/A | Muted grey bounce; notification toast `"Duplicate event prevented"` |
| **CSV Export Button** | Coral border, download icon | Solid coral fill, icon bounce | Transient checkmark icon with success toast |

---

## 9. Navigation & Supporting Views

The top floating pill bar provides quick switching between operational modes:

1. **Operations Map (Primary View)**: Full interactive Google Maps view with live ships, routes, floating KPI overlays, speed selector, and layer toggles.
2. **Fleet Registry & Vessel Management View**: Grid/list of all configured simulated vessels with search, multi-attribute filters, flag badges, and `[+ Add Vessel]` action.
3. **Toll Policies & Country Management View**: Clean matrix displaying country tariffs, cargo multipliers, exemption toggles, and `[+ Add Country]` configuration drawer.
4. **Collection Ledger View**: Full-width chronological audit trail of all generated toll receipts with transaction IDs, timestamps, authority breakdowns, and `[⬇ Export CSV]`.
5. **Revenue Reports & Analytics View**: Aggregated financial charts, authority splits, nationality breakdowns, and summary CSV export.
6. **Settings & Diagnostics**: Google Maps API key entry, simulation speed presets (1x, 2x, 4x), congestion threshold configuration, and GlassFish connection monitor.

---

## 10. Responsive Design & Screen Adaptation

The application is optimized for desktop presentation during practical examinations, with graceful adaptation down to 1024px tablet screens:

- **Desktop (1366px - 1920px+)**: Multi-panel asymmetric floating layout. Left control stack, top-right revenue bar, bottom ticker.
- **Laptop / Compact Lab Monitor (1024px - 1365px)**:
  - Left panel collapses into an icon sidebar with a toggle drawer.
  - Revenue bar compacts into a single combined pill with expandable dropdown for Iran/Oman splits.
  - Bottom ledger collapses into a single-line live ticker with an expandable full sheet.
- **Touch / Mobile (> 768px)**:
  - Map retains primary touch gestures (pinch-to-zoom, pan).
  - Floating panels convert to bottom sheets (swipe-up to inspect vessel or view revenue).

---

## 11. Accessibility, Contrast & Legibility Standards

- **Color Contrast (WCAG 2.1 AA)**: All text tokens (`--hvs-text-primary: #FFFFFF`, `--hvs-text-secondary: #919EAB`) maintain a contrast ratio greater than `4.8:1` against the translucent dark panel surfaces (`rgba(22, 28, 36, 0.85)`).
- **Non-Color Dependent Indicators**: Every status color (green, yellow, coral) is strictly paired with text labels (`CHARGED`, `WAITING`, `EXEMPT`) and recognizable geometric icons so information is never conveyed by hue alone.
- **High-Readability Numbers**: Monospaced tabular numerals (`font-variant-numeric: tabular-nums`) prevent numbers from jumping horizontally during live revenue accumulation.

---

## 12. Anti-Template Strict Prohibitions

To ensure the project maintains an authentic, high-grade maritime operations aesthetic and avoids looking like a generic AI template, the following are strictly prohibited:

- ❌ **No Generic Neon Purple Gradients**: No bright purple or magenta cyberpunk accents. Accents are restricted to the maritime coral/orange (`#FF5520`) and marine cyan (`#00E5FF`).
- ❌ **No Overly Rounded "Bubble" Cards**: Border radii must not exceed `20px`. No 40px bubble shapes.
- ❌ **No Decorative "Dummy" Graphs**: Do not draw SVG wave graphs or bar charts that display random numbers. Every visual element must map to actual simulation state.
- ❌ **No Fake Marketing Testimonials or Hero Headlines**: The dashboard is an operational tool, not a SaaS product pitch.
- ❌ **No Emoji Icons in Interface Elements**: Use SVG vector icons (navigation arrows, compasses, play/pause symbols) rather than consumer emojis like 🚢 or 💰 in UI buttons and headers (flags on ships remain standard SVG national flags).
- ❌ **No Excessive Blur that Obscures Content**: Backdrop blur is constrained to `14px`, and panel background opacity remains at least `0.75` so text is razor-sharp.

---

## 13. Summary of Design Tokens

| Token Category | Token Variable | Value | Purpose |
| :--- | :--- | :--- | :--- |
| **Canvas** | `--hvs-bg-base` | `#0B0E14` | Map canvas backplate |
| **Surface** | `--hvs-bg-surface` | `rgba(22, 28, 36, 0.78)` | Main operational cards |
| **Border** | `--hvs-border-subtle` | `rgba(255, 255, 255, 0.08)` | Outer boundaries |
| **Accent 1** | `--hvs-accent-orange`| `#FF5520` | Checkpoints, primary buttons, Oman |
| **Accent 2** | `--hvs-iran-primary` | `#00C853` | Iran authority indicators |
| **Route** | `--hvs-route-cyan` | `#00E5FF` | Shipping lanes & waypoints |
| **Font Body** | `--hvs-font-body` | `Inter, sans-serif` | General interface copy |
| **Font Data** | `--hvs-font-mono` | `IBM Plex Mono, monospace`| Toll sums, coordinates, IDs |
| **Radius** | `--hvs-radius-md` | `14px` | Standard card border radius |

---

## 14. Implementation & Architecture Boundary

- **Design Contract Only**: This document provides the UI/UX contract and visual specifications. It does not authorize writing HTML/CSS code or modifying project files.
- **Technical Restrictions**: Implementation must strictly adhere to Vanilla HTML5, CSS3, and ES6 JavaScript running on GlassFish 4.1.1. No CSS preprocessors (SASS/LESS), no CSS-in-JS, and no UI libraries (Bootstrap, Tailwind, Material-UI) will be introduced.

---

## 15. Document Status & Approval Checklist

- [x] **Established Requirements (from `brain.md` & `PRD.md`)**:
  - Interactive Google Maps centered on Strait of Hormuz.
  - Custom ship markers with top-down hulls, masts, and country flags.
  - Visible Iran and Oman hypothetical checkpoints.
  - Start, pause, resume, reset simulation controls.
  - Separate Iran, Oman, and Total revenue counters.
  - Clear simulation disclaimer.
- [x] **Visual Design Recommendations (from `visual-reference.png`)**:
  - Dark oceanic slate foundation with electric coral-orange accents.
  - Translucent floating panels (`backdrop-filter: blur(14px)`).
  - High-contrast monospace metric displays.
  - Top floating pill navigation header.
- [ ] **Awaiting User Review**:
  - Final confirmation of color tokens (`#FF5520`, `#00C853`, `#00E5FF`).
  - Confirmation of panel positioning and drawer widths.

**Document Status**: **Draft for User Review**
