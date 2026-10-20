<div align="center">

<a href="#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-org-banner-light.png" alt="Gilpin Hotel & Lake House" width="100%" /></a><a href="#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-org-banner-dark.png" alt="Gilpin Hotel & Lake House" width="100%" /></a>

<br/><br/>

### *Enterprise Hospitality Operations & Conformed Data Architecture*
**Windermere, The English Lake District · Relais & Châteaux**

</div>

<br/>

> [!NOTE]
> **Working Draft (Pre-Production UAT):** All platform descriptions, architecture specifications, and operational registers are currently a **Working Draft** in active pre-production review. Real-time integrations with **Plato Operations** (live operational feeds, guest handovers, maintenance, guest issues, dietary follows, and morning briefing sync) are actively running in UAT across the estate platform alongside Hart PMS and the Gilpin Data Lake (Snowflake London). Final documentation, full governance reviews, and descriptions will be completed with Claude on Sunday.

<br/>

<div align="center">

[![Snowflake GDL](https://img.shields.io/badge/Snowflake-GCP_London_europe--west2-29B5E8?style=for-the-badge&logo=snowflake)](https://github.com/gilpin-hotel/GDL_Merged)
[![Arrival Platform](https://img.shields.io/badge/Arrival_Platform-v10.6.0_UAT-2ea44f?style=for-the-badge&logo=react)](https://gilpin-arrival-tool.vercel.app)
[![Kitchen Intelligence](https://img.shields.io/badge/Kitchen_Safety-v7.1.0_UAT-e05d44?style=for-the-badge&logo=vitest)](https://gilpin-kitchen.vercel.app)
[![Vercel Edge](https://img.shields.io/badge/Vercel_Edge-London_lhr1-000000?style=for-the-badge&logo=vercel)](https://vercel.com/)
[![UK GDPR](https://img.shields.io/badge/UK_GDPR-Article_28_Governed-c5a065?style=for-the-badge)](https://github.com/gilpin-hotel)

---

</div>

## 🏛️ System Architecture & Data Flow

The Gilpin technology platform provides a conformed, single source of guest truth linking 20+ years of hotel, culinary, and spa history with real-time physical estate sensing and front-line operational tools across **Gilpin Hotel** and the **Lake House Sanctuary**.

```
════════════════════════════════════════════════════════════════════════════════════════════════════════
                             1. SOURCE INGESTION & PHYSICAL SENSORS
════════════════════════════════════════════════════════════════════════════════════════════════════════
  • Hart PMS API: Stays, Dining & Spa Deltas (Scheduled 11:45 & 23:30 London Pulls)
  • Historical Archives: ResLynxE (2004–2021) & Hart Historic (2022–July 2026)
  • Plato Operations: 8 Feeds to RAW (Guest Issues, Feedback, Handovers, Maintenance)
  • Marketing & Consent: Dotdigital (Landed subscribers, lists, suppressions) & Web Form Archive
  • Physical Estate Sensing: Lake House Gate Hikvision Camera via On-Premises Gate PC (GIL10620)
  • Arrivals Tool Write-Back: Front Desk Room Edits & Guest Arrival Window Selections
                                                │
                                                ▼
════════════════════════════════════════════════════════════════════════════════════════════════════════
                  2. GILPIN DATA LAKE (GDL) — SNOWFLAKE (GCP LONDON europe-west2)
════════════════════════════════════════════════════════════════════════════════════════════════════════
  ┌─────────────────────────────┬────────────────────────────────────────────────────────────────────┐
  │  RAW LAYER (22 Tables)      │ Verbatim landing floor · Replay boundary · Never mutated           │
  ├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
  │  BRONZE LAYER (29 Tables)   │ Typed, conformed selections per source record                      │
  ├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
  │  SILVER LAYER (33T, 18V)    │ Canonical identity engine (v3) · Data quality · Shared hub tables  │
  ├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
  │  GOLD LAYER (94T, 122V)     │ Conformed Dimensional Star (37 Dims, 45 Facts) + Thin gld_* Faces │
  ├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
  │  CONTROL PLANE              │ META (Standards & Budgets) · AUDIT (~630 Gates/Build) · GOVERNANCE │
  └─────────────────────────────┴────────────────────────────────────────────────────────────────────┘
  • Rebuilt deterministically from RAW twice daily (113-task graph, 111 active tasks, ~42 min runtime)
  • Lineage Gate (module 112) enforces rules LIN-0 through LIN-7 on every single build
  • Least-privilege consumers read through thin GOLD faces; MCP server connects Claude to lake
                                                │
                                                ▼
════════════════════════════════════════════════════════════════════════════════════════════════════════
                       3. OPERATIONAL SERVING & REAL-TIME DASHBOARDS
════════════════════════════════════════════════════════════════════════════════════════════════════════
        │                                       │                                       │
        ▼ (SERVING_NOTIFY Event)                 ▼ (Real-Time Sync)                      ▼ (Direct API)
  ┌───────────────────────────┐           ┌───────────────────────────┐           ┌───────────────────────────┐
  │   Gilpin Arrivals Tool    │           │ Kitchen Safety Dashboard  │           │   Review Intelligence     │
  │      v10.6.0 (UAT)        │           │       v7.1.0 (UAT)        │           │       v1.1.0 (UAT)        │
  │   React 19 · Firebase RTDB│           │ React 19 · Vite 8 · TW 4  │           │ React 19 · Gemini 2.5     │
  │   10 Department Dashboards│           │ Real-Time Allergen Board  │           │ Google Places API         │
  │   AI Live Assistant (lhr) │           │ Karpathy Procurement Loop │           │ 3 Views (Inbox/Reviews)   │
  └───────────────────────────┘           └───────────────────────────┘           └───────────────────────────┘
```

---

## 📦 Technology Repositories

### 🏨 [GilpinArrivalTool](https://github.com/gilpin-hotel/GilpinArrivalTool) · `v10.6.0 UAT`
* **Real-Time Front-of-House Orchestration:** Master arrivals manifest and 10 role-specific operational dashboards (Arrivals, Housekeeping, Maintenance, Front of House, Handover Hub, Turndown, Checkouts, In House, Duty Manager Diary, and Analytics).
* **Stack:** `React 19` `TypeScript 5.7` `Vite 6` `Tailwind CSS 3.4` `Framer Motion 12`.
* **State & Edge:** Firebase Realtime Database in **Belgium (`europe-west1`)** provides sub-second multi-device sync across all 10 departments. Edge compute runs on Vercel Serverless in **London (`lhr1`)**.
* **Authentication:** Firebase Auth RBAC (role-based access across departments and HODs; legacy HMAC token auth completely removed).
* **AI Live Assistant:** Voice & text conversational assistant powered by Gemini Flash Live via a dedicated server-side WebSocket relay in **London (`lhr`)**.
* **PMS & Estate Integrations:** Column-aware PDF briefing parser (38-room estate map), Hart PMS check-in tokens, ETA timeline write-back, Room Confirmation templates, and Lake House ANPR gate plate matching.

### 🏛️ [GDL_Merged](https://github.com/gilpin-hotel/GDL_Merged) · `Governed Lake Platform (UAT)`
* **Enterprise Guest Data Lake:** Unifies 20+ years of Hart PMS, Plato operations, and Dotdigital marketing into a single conformed guest truth.
* **Engine:** Snowflake on **GCP `europe-west2` (London)**.
* **Build Cadence:** 113 native Snowflake tasks (111 active) running twice daily at **11:45 and 23:30 London** (~42 min median runtime). Deterministically rebuilds layers from `RAW`.
* **Lineage & Quality Gates:** Module `112_lineage_gate.sql` enforces rules `LIN-0` through `LIN-7` every build. Automated gate runner asserts ~630 quality checks per build into `AUDIT.gate_result`.
* **Test Harness:** Local DuckDB suite (`qa/run_local.py`) running 50/50 regression tests in seconds over synthetic fixtures.
* **Clean Snapshot:** Zero historical PII, zero credentials, and single-commit compliance on GitHub.

### 🍳 [gilpin-kitchen-dashboard](https://github.com/gilpin-hotel/gilpin-kitchen-dashboard) · `v7.1.0 UAT`
* **Culinary Safety & Real-Time Kitchen Intelligence:** Real-time food safety and operations platform serving both *Source at Gilpin Hotel* and *Gilpin Spice*.
* **Stack:** `React 19.2` `TypeScript 5.9` `Vite 8` `Tailwind CSS 4.2` deployed on Vercel **London (`lhr1`)**.
* **Food Safety Protocol:** Immediate, un-gated visibility of UK Top 14 allergens, dietary requirements, and last-minute booking modifications (<60 min); cover counts split by Main Hotel and Lake House.
* **Karpathy Auto-Research Loop:** Autonomous procurement agent utilizing an Exponential Moving Average (EMA) feedback loop that learns from daily post-service consumption (ordered vs. used) to accurately scale future ingredient orders.

### ⭐ [Gilpin-reviews-hub](https://github.com/gilpin-hotel/Gilpin-reviews-hub) · `v1.1.0 UAT`
* **Multi-Channel Reputation Platform:** Internal review-intelligence tool for Gilpin Hotel & Lake House.
* **Stack:** `React 19` `TypeScript 5.7` `Vite 6` `Tailwind CSS 3.4` `Framer Motion 12` on Vercel Functions **London (`lhr1`)**.
* **Data Flow & AI:** Dedicated partition on Firebase RTDB (`reviewLake/reviews/*`). Pulls reviews on demand from Google Places API (New), TripAdvisor, and Booking.com; Gemini 2.5 Flash classifies department, sentiment polarity, and actionable operational insights.
* **Views & Reporting:** 3 operational views (Inbox reply queue, Reviews list with filter chips, Insights trend analytics) and 2-sheet XLSX export via `exceljs`.

### 🌿 [gilpin-amenities](https://github.com/gilpin-hotel/gilpin-amenities) · `UAT`
* **Suite & Bedroom Register:** Live inventory register across all 38 estate bedrooms (Main Hotel 1–31 excl. 29, Lake House 51–58) and Back-of-House areas (plant rooms, boiler rooms).
* **Stack:** Zero-build static single-page architecture (`index.html`) deployed to Vercel **London (`lhr1`)**.
* **Storage & Plato Bridge:** Room records and changelog stored in Cloud Firestore in **EU (`eur3`)**; room and plant hardware photos stored in Cloud Storage (`US-EAST1`, hardware photos only, strictly zero guest data). Provides automated zip export (`survey.json` + `survey.csv` + photos) for Plato room-mapping integration.

---

## 🎨 Estate & Culinary Venues

<div align="center">

| The Lake House Sanctuary | Source at Gilpin Hotel | Gilpin Spice |
| :---: | :---: | :---: |
| <a href="https://thegilpin.co.uk/lake-house/"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/lake-house-exterior.jpg" width="260" alt="The Lake House Sanctuary" /></a> | <a href="https://thegilpin.co.uk/eat-drink/source/#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/source-logo-pine.svg" width="190" alt="SOURCE at Gilpin Hotel" /></a><a href="https://thegilpin.co.uk/eat-drink/source/#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/source-logo.svg" width="190" alt="SOURCE at Gilpin Hotel" /></a> | <a href="https://thegilpin.co.uk/eat-drink/spice/#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/spice-logo-pine.svg" width="125" alt="Gilpin Spice" /></a><a href="https://thegilpin.co.uk/eat-drink/spice/#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/spice-logo.svg" width="125" alt="Gilpin Spice" /></a> |
| **Private Lake & Spa Lodges**<br/><sub>6–8 Exclusive Suites set in 100 Acres on Knipe Tarn<br/>Private Boathouse, Pool &amp; The Jetty Spa</sub> | **Michelin-Starred Fine Dining**<br/><sub>Head Chef Kevin Scanlan<br/>Hyper-Local Cumbrian Produce &amp; Modern Technique</sub> | **Pan-Asian Culinary Journey**<br/><sub>Head Chef Ralph Rojas<br/>Vibrant Open Kitchen &amp; Spice Trail Sharing Menus</sub> |

</div>

---

## 🔒 Verified Governance, Security & Residency Standards

| Dimension | Architectural Fact & Verification (per Hosting Register 12 Oct 2026) |
| :--- | :--- |
| **Guest Data Lake** | **Snowflake in GCP London (`europe-west2`).** All 6 stages internal, no replication, no external storage integrations. Full guest records held strictly within London. |
| **Operational State Sync** | **Firebase RTDB in Belgium (`europe-west1`).** Both live and preview instances pinned to Belgium; Upstash Redis rate-limiter primary in `europe-west1`. |
| **Serverless Compute** | **Vercel Serverless in London (`lhr1`).** Arrivals API, Kitchen API, and Reviews API pinned to London edge instances. |
| **In-Transit Assistant Relay** | **Fly.io in London (`lhr`).** Live voice/text WebSocket relay holds guest context in memory only. |
| **Hardware & Plant Storage** | **Cloud Storage (`US-EAST1`).** Amenities dashboard photos (radiators, boiler plates, room furnishings) held in `US-EAST1`; strictly zero guest PII. Firestore room data in EU (`eur3`). |
| **AI Inference** | **Google Gemini & Claude Enterprise.** Gemini batch calls pseudonymised (Guest A/B, registrations stripped); Claude MCP queries executed via local subprocess reading London Snowflake. |
| **Lineage Integrity** | Enforced by automated gate `AUDIT.gate_result` and module 112 every single build (`LIN-0` through `LIN-7`). |
| **Change Verification** | Every production schema migration is proven on a zero-copy clone of production before promotion (`Lake Standard C7`). |
| **Drift Verification** | `setup/check_stage_drift.py` asserts byte-level equivalence between Git commits, staged Snowflake code (`META.GDL_CODE`), task DAGs, and MCP hashes. |
| **UK GDPR Compliance** | Article 28 governed; Article 17 multi-layer subject erasure implemented via `META.SP_RAW_ERASE_SUBJECT` (module 103). Zero live guest PII in repositories. |

---

<div align="center">

<a href="#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-g-pine.png" width="44" alt="Gilpin Crest" /></a><a href="#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-g-bronze.png" width="44" alt="Gilpin Crest" /></a>

<br/><br/>

<a href="https://www.relaischateaux.com/gb/united-kingdom/gilpin-cumbria-windermere">
  <img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/relais-chateaux-logo.png" width="110" alt="Relais & Châteaux" />
</a>

<br/><br/>

*Gilpin Hotel & Lake House · Crook Road, Windermere, Cumbria LA23 3NE*  
*Relais & Châteaux · Principal Architecture by Dimitrios Karakonstantinou*

</div>
