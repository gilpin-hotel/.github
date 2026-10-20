<div align="center">

<a href="#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-org-banner-light.png" alt="Gilpin Hotel & Lake House" width="100%" /></a><a href="#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-org-banner-dark.png" alt="Gilpin Hotel & Lake House" width="100%" /></a>

<br/><br/>

<a href="https://thegilpin.co.uk#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-wordmark-pine.png" alt="Gilpin Hotel & Lake House" height="46" /></a><a href="https://thegilpin.co.uk#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/gilpin-wordmark-bone.png" alt="Gilpin Hotel & Lake House" height="46" /></a>

### *Enterprise Hospitality Operations & Conformed Data Architecture*
**Windermere, The English Lake District · Relais & Châteaux**

<br/>

[![Data Residency](https://img.shields.io/badge/Data_Residency-UK_%26_EU_Verified-183028?style=for-the-badge&logo=shield&logoColor=c5a065)](https://github.com/gilpin-hotel)
[![Snowflake GDL](https://img.shields.io/badge/Snowflake-GCP_London_europe--west2-29B5E8?style=for-the-badge&logo=snowflake)](https://github.com/gilpin-hotel/GDL_Merged)
[![Arrival Intelligence](https://img.shields.io/badge/Arrival_Platform-v10.6.0_Production-2ea44f?style=for-the-badge&logo=react)](https://gilpin-arrival-tool.vercel.app)
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
  • Marketing & Consent: Dotdigital (93,927 Records) & WordPress Web Form Intake
  • Physical Security: Lake House Gate Hikvision Camera ──▶ HTTPS Lake Poster (RAW)
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
  │  CONTROL PLANE              │ META (Standards) · AUDIT (~630 Gates/Build) · GOVERNANCE · ENRICH  │
  └─────────────────────────────┴────────────────────────────────────────────────────────────────────┘
  • Rebuilt deterministically from RAW twice daily (113-task graph, 111 active tasks, ~42 min runtime)
  • Lineage Gate (module 112) enforces rules LIN-0 through LIN-7 on every single build
  • Cortex cross-region routing explicitly DISABLED (zero guest analytics outside London europe-west2)
                                                │
                                                ▼
════════════════════════════════════════════════════════════════════════════════════════════════════════
                       3. OPERATIONAL SERVING & REAL-TIME DASHBOARDS
════════════════════════════════════════════════════════════════════════════════════════════════════════
        │                                       │                                       │
        ▼ (SERVING_NOTIFY Event)                 ▼ (Real-Time Sync)                      ▼ (Direct API)
  ┌───────────────────────────┐           ┌───────────────────────────┐           ┌───────────────────────────┐
  │   Gilpin Arrivals Tool    │           │ Kitchen Safety Dashboard  │           │   Review Intelligence     │
  │   v10.6.0 (Production)    │           │ Source at Gilpin Hotel    │           │   Google Business API     │
  │   React 19 · Firebase RTDB│           │ Real-Time Allergen Board  │           │   Gemini Sentiment Engine │
  │   VIP Alert & Room Sync   │           │ Dietary Pacing & Covers   │           │   Multi-Channel Analytics │
  └───────────────────────────┘           └───────────────────────────┘           └───────────────────────────┘
```

---

## 📦 Technology Repositories

### 🏨 [GilpinArrivalTool](https://github.com/gilpin-hotel/GilpinArrivalTool) · `v10.6.0 Production`
* **Real-Time Front-of-House Orchestration:** Multi-department arrivals board, VIP recognition, room readiness tracking, and Duty Manager shift diary.
* **Stack:** `React 19` `TypeScript 5.7` `Vite 6` `Tailwind CSS`.
* **State Sync:** Firebase Realtime Database in **Belgium (`europe-west1`)** provides sub-second cross-device synchronization across 9 hotel departments.
* **Edge Infrastructure:** Vercel Serverless Functions in **London (`lhr1`)** with HMAC-authenticated API routes.
* **PMS Integration:** Hart PMS token-on-file verification at check-in, ETA write-back, and Room Confirmation 18 templates.
* **Reviews:** Native Google Business Profile API integration without third-party scrapers.

### 🏛️ [GDL_Merged](https://github.com/gilpin-hotel/GDL_Merged) · `Governed Lake Platform`
* **Enterprise Guest Data Lake:** Ingests Hart PMS, Plato, and Dotdigital into a single conformed guest truth.
* **Engine:** Snowflake on **GCP `europe-west2` (London)**.
* **Build Cadence:** 113 native Snowflake tasks running twice daily at **11:45 and 23:30 London** (~42 min median runtime). Rebuilds from `RAW`.
* **Lineage Gates:** Module `112_lineage_gate.sql` enforces rules `LIN-0` through `LIN-7` every build.
* **Test Harness:** Local DuckDB suite (`qa/run_local.py`) running 50/50 regression tests in seconds on laptop.
* **Clean Snapshot:** Zero historical PII, zero credentials, and single-commit compliance on GitHub.

### 🍳 [gilpin-kitchen-dashboard](https://github.com/gilpin-hotel/gilpin-kitchen-dashboard) · `Production`
* **Kitchen Safety & Dining Recognition:** Real-time food safety and culinary pacing dashboard for *Source at Gilpin Hotel* (Michelin-starred dining).
* **Stack:** `React` `TypeScript` `Vite` `Tailwind CSS` deployed on Vercel London (`lhr1`).
* **Data Flow:** Subscribes to conformed guest dietary notes and UK Top 14 allergen alerts from the Arrivals tool feed and GDL Gold views.
* **Safety Protocols:** Cross-checked against head chef allergen runbooks; zero local PII storage.

### ⭐ [Gilpin-reviews-hub](https://github.com/gilpin-hotel/Gilpin-reviews-hub) · `Active`
* **Multi-Channel Reputation Platform:** Guest sentiment analysis and feedback analytics across Hotel and Lake House.
* **Stack:** `React` `TypeScript` `Vercel Serverless (London)`.
* **Data Intake:** Direct Google Places / Business Profile API, TripAdvisor, and Booking.com intake.
* **AI Engine:** Gemini AI classifies guest reviews by department, sentiment polarity, and actionable operational insights.

### 🌿 [gilpin-amenities](https://github.com/gilpin-hotel/gilpin-amenities) · `Operational`
* **Suite & Bedroom Register:** Asset and amenity inventory register across all 38 hotel bedrooms (Hotel 1–31 excl. 29, Lake House 51–58) and plant areas.
* **Stack:** Zero-build static architecture (`HTML5`, `Tailwind CSS`, `JavaScript`) deployed to Vercel London for Plato room-mapping integration.

---

## 🎨 Estate & Culinary Venues

<div align="center">

| The Lake House Sanctuary | Source at Gilpin Hotel | Gilpin Spice |
| :---: | :---: | :---: |
| <a href="https://thegilpin.co.uk/lake-house/"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/lake-house-exterior.jpg" width="260" alt="The Lake House Sanctuary" /></a> | <a href="https://thegilpin.co.uk/eat-drink/source/#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/source-logo-pine.svg" width="190" alt="SOURCE at Gilpin Hotel" /></a><a href="https://thegilpin.co.uk/eat-drink/source/#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/source-logo.svg" width="190" alt="SOURCE at Gilpin Hotel" /></a> | <a href="https://thegilpin.co.uk/eat-drink/spice/#gh-light-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/spice-logo-pine.svg" width="125" alt="Gilpin Spice" /></a><a href="https://thegilpin.co.uk/eat-drink/spice/#gh-dark-mode-only"><img src="https://raw.githubusercontent.com/gilpin-hotel/.github/main/profile/assets/spice-logo.svg" width="125" alt="Gilpin Spice" /></a> |
| **Private Lake & Spa Lodges**<br/><sub>6 Exclusive Suites on Knipe Tarn<br/>Private Treatment Rooms & Sanctuary Host</sub> | **Michelin-Starred Dining**<br/><sub>Executive Chef Ollie Bridgwater<br/>Hyper-Local Lake District Produce & Technique</sub> | **Pan-Asian Culinary Journey**<br/><sub>Vibrant Open Kitchen<br/>Tapas & Sharing Menus along the Spice Trail</sub> |

</div>

---

## 🔒 Verified Governance, Security & Residency Standards

| Dimension | Architectural Fact & Verification |
| :--- | :--- |
| **Data Residency** | **100% UK & EU hosting verified.** Snowflake in London (`europe-west2`), Vercel apps in London (`lhr1`), Firebase RTDB in Belgium (`europe-west1`), Google Workspace in Europe, Plato in the UK. |
| **Snowflake AI Residency** | `CORTEX_ENABLED_CROSS_REGION` is explicitly set to **`DISABLED`**, preventing any analytical queries or LLMs from routing guest data outside the London region. |
| **Lineage Integrity** | Enforced by automated gate `AUDIT.gate_result` and module 112 every single build (`LIN-0` through `LIN-7`). |
| **Change Verification** | Every production schema migration is proven on a zero-copy clone of production before promotion (`lake standard C7`). |
| **Drift Verification** | `setup/check_stage_drift.py` asserts byte-level equivalence between Git commits, staged Snowflake code (`META.GDL_CODE`), task DAGs, and MCP hashes. |
| **UK GDPR Compliance** | Article 28 governed; Article 17 multi-layer subject erasure implemented via `META.SP_RAW_ERASE_SUBJECT` and module 103. Zero live guest PII in repositories. |

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
