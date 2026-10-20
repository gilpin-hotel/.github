<div align="center">

<img width="1200" alt="Gilpin Hotel & Lake House" src="profile/assets/gilpin-banner.png" />

# 🌲 Gilpin Hotel & Lake House
### *Enterprise Hospitality Architecture & Intelligence Platform*
**Windermere, The English Lake District · Relais & Châteaux**

[![Data Residency](https://img.shields.io/badge/Data_Residency-UK_%26_EU_Verified-1f513f?style=for-the-badge&logo=shield)](https://github.com/gilpin-hotel)
[![Snowflake GDL](https://img.shields.io/badge/Snowflake-GCP_London_europe--west2-29B5E8?style=for-the-badge&logo=snowflake)](https://github.com/gilpin-hotel/GDL_Merged)
[![Arrival Intelligence](https://img.shields.io/badge/Arrival_Tool-v10.6.0_Production-2ea44f?style=for-the-badge&logo=react)](https://gilpin-arrival-tool.vercel.app)
[![Vercel Infrastructure](https://img.shields.io/badge/Edge_Apps-London_lhr1-black?style=for-the-badge&logo=vercel)](https://vercel.com/)
[![UK GDPR](https://img.shields.io/badge/UK_GDPR-Article_28_Governed-gold?style=for-the-badge)](https://github.com/gilpin-hotel)

---

</div>

## 🏛️ System Architecture

The Gilpin technology platform is a unified, bi-directional hospitality intelligence network linking physical estate sensors, core property management, conformed analytics, and front-line operational dashboards across **Gilpin Hotel** and the **Lake House Sanctuary**.

```mermaid
flowchart TD
    subgraph SOURCES["1. Source Feeds & Physical Ingestion"]
        HART["Hart PMS API<br/>Stays, Dining, Spa Deltas<br/>(11:45 & 23:30 Slots)"]
        ARCH["Historical Archives<br/>ResLynxE (2004–2021)<br/>Hart Historic (2022–2026)"]
        PLATO_IN["Plato Operations<br/>8 Feeds: Guest Issues, Feedback,<br/>Handovers & Maintenance"]
        MKT["Marketing & Consent<br/>Dotdigital (93k+ Records)<br/>WordPress Web Forms"]
        ANPR["Lake House Gate Camera<br/>Hikvision ANPR PC<br/>HTTPS Lake Poster"]
        DESK["Arrivals Tool Write-Back<br/>Desk Edits & Guest ETAs"]
    end

    subgraph GDL["2. Gilpin Data Lake (GDL) — Snowflake (GCP London europe-west2)"]
        RAW["RAW Layer (22 Tables)<br/>Verbatim Payloads · Replay Floor · Zero Mutations"]
        BRONZE["BRONZE Layer (29 Tables)<br/>Typed Selections per Source Record"]
        SILVER["SILVER Layer (33 Tables, 18 Views)<br/>Data Quality · Canonical Identity Engine · Hubs"]
        GOLD["GOLD Layer (94 Tables, 122 Views)<br/>Conformed Star Schema (37 Dims, 45 Facts, 12 Bridges)<br/>Thin Consumer Faces (gld_*)"]
        CTRL[("Control Plane<br/>META (Standards, Contracts)<br/>AUDIT (~630 Gates/Build, Lineage)<br/>SECURITY & ENRICH")]
        
        RAW --> BRONZE --> SILVER --> GOLD
        CTRL -. Governs .-> RAW
        CTRL -. Governs .-> BRONZE
        CTRL -. Governs .-> SILVER
        CTRL -. Governs .-> GOLD
    end

    subgraph CONSUMERS["3. Downstream Consumers & Operational Tools"]
        AT["Gilpin Arrival Tool (v10.6.0)<br/>React 19 · Firebase RTDB (Belgium)<br/>Vercel (London) · VIP & Room Orchestration"]
        KD["Kitchen Dashboard<br/>React · Source at Gilpin<br/>Real-Time Allergen & Dietary Safety"]
        RH["Reviews Hub<br/>Google Places API Direct<br/>Gemini Sentiment Analytics"]
        AR["Amenities Register<br/>Hotel & Lake House Suites"]
        MCP["Claude MCP Servers<br/>Natural Language Lake Analytics<br/>Least-Privilege Service Roles"]
        PLATO_OUT["Plato Reports<br/>Read-Only GOLD Ingestion"]
    end

    HART --> RAW
    ARCH --> RAW
    PLATO_IN --> RAW
    MKT --> RAW
    ANPR --> RAW
    DESK --> CTRL

    GOLD -- "SERVING_NOTIFY Task" --> AT
    GOLD --> KD
    GOLD --> RH
    GOLD --> MCP
    GOLD --> PLATO_OUT
    AT -- "Plate Sync" --> ANPR
```

---

## 📦 The Technology Repositories

### 1. [GilpinArrivalTool](https://github.com/gilpin-hotel/GilpinArrivalTool) · `Production v10.6.0`
* **Purpose:** Real-time arrival intelligence, front-of-house orchestration, and guest recognition platform.
* **Architecture:**
  - **Frontend:** React 19, TypeScript 5.7, Vite 6, Tailwind CSS.
  - **Real-Time State:** Firebase Realtime Database in **Belgium (`europe-west1`)** providing millisecond cross-device synchronization for room readiness, guest arrivals, and duty manager shift diaries.
  - **Edge Execution:** Vercel Serverless Functions in **London (`lhr1`)** with HMAC-authenticated API routes.
  - **Lake Integration:** Automated sync triggered by GDL's `SERVING_NOTIFY` post-build task.
  - **PMS & Billing Integration:** Hart PMS token-on-file verification at check-in, ETA write-backs, and Room Confirmation 18 template management.
  - **AI & Reviews:** Gemini AI arrival briefing generation and direct Google Business Profile API review fetching without third-party scrapers.

### 2. [GDL_Merged](https://github.com/gilpin-hotel/GDL_Merged) · `Governed Lake Platform`
* **Purpose:** Enterprise guest data platform unifying 20+ years of hotel, dining, and spa history into conformed single-guest truth.
* **Architecture:**
  - **Core Engine:** Snowflake on **GCP `europe-west2` (London)**.
  - **Execution Graph:** 113 native Snowflake tasks (111 executed per build), scheduled twice daily at **11:45 and 23:30 Europe/London** (runtime ~42 min). Rebuilds deterministically from `RAW`.
  - **Lineage Governance:** Module `112_lineage_gate.sql` enforces rules `LIN-0` through `LIN-7` every build (no skipped layers, no backwards view dependencies, no view-on-view gold faces).
  - **Regression Harness:** Local DuckDB test suite (`qa/run_local.py`) running 50/50 unit and semantic checks in seconds on laptop without Snowflake compute cost.
  - **Privacy by Construction:** Single-commit scrubbed snapshot on GitHub with zero commit history, zero raw dumps, and pseudonymised test fixtures (Lake Standard C9).

### 3. [gilpin-kitchen-dashboard](https://github.com/gilpin-hotel/gilpin-kitchen-dashboard) · `Production`
* **Purpose:** Real-time food safety and culinary pacing dashboard for *Source at Gilpin Hotel* (Michelin-starred dining).
* **Architecture:**
  - **Frontend:** React, TypeScript, Vite, Tailwind CSS.
  - **Data Pipeline:** Subscribes to conformed guest dietary notes and UK Top 14 allergen alerts from the Arrivals tool feed and GDL Gold views.
  - **Operational Safety:** Cross-checked against head chef allergen runbooks; zero local PII storage.

### 4. [Gilpin-reviews-hub](https://github.com/gilpin-hotel/Gilpin-reviews-hub) · `Active`
* **Purpose:** Multi-channel reputation intelligence and sentiment analysis for Hotel & Lake House.
* **Architecture:**
  - **Frontend & Serverless:** React, TypeScript, Vercel Serverless Functions (London).
  - **API Connections:** Direct Google Places / Business Profile API, TripAdvisor, and Booking.com intake.
  - **AI Classification:** Gemini AI classifies feedback into department, sentiment polarity, and actionable operational insights.
  - **Storage:** Isolated Firebase RTDB path syncing live sentiment into the Arrivals Tool.

### 5. [gilpin-amenities](https://github.com/gilpin-hotel/gilpin-amenities) · `Operational`
* **Purpose:** Comprehensive room, suite, and back-of-house asset register.
* **Architecture:**
  - High-performance, zero-build static architecture (HTML5, Tailwind CSS, JavaScript) deployed to Vercel.
  - Covers all 38 bedrooms (Hotel 1–31 excl. 29, Lake House 51–58) and plant areas for Plato room-mapping integration.

---

## 🔒 Verified Governance, Security & Residency Standards

| Dimension | Architectural Fact & Verification |
| :--- | :--- |
| **Data Residency** | **100% UK & EU hosting verified.** Snowflake in London (`europe-west2`), Vercel apps in London (`lhr1`), Firebase RTDB in Belgium (`europe-west1`), Google Workspace in Europe, Plato in the UK. |
| **Snowflake AI Residency** | `CORTEX_ENABLED_CROSS_REGION` is explicitly set to **`DISABLED`**, preventing any analytical queries or LLMs from routing guest data outside the London region. |
| **Lineage Integrity** | Enforced by automated gate `AUDIT.gate_result` and module 112 every single build. |
| **Change Verification** | Every production schema migration is proven on a zero-copy clone of production before promotion (`lake standard C7`). |
| **Drift Verification** | `setup/check_stage_drift.py` asserts byte-level equivalence between Git commits, staged Snowflake code (`META.GDL_CODE`), task DAGs, and MCP hashes. |
| **UK GDPR Compliance** | Article 28 governed; Article 17 multi-layer subject erasure implemented via `META.SP_RAW_ERASE_SUBJECT` and module 103. |

---

<div align="center">

*Gilpin Hotel & Lake House · Crook Road, Windermere, Cumbria LA23 3NE*  
*Relais & Châteaux · Principal Architecture by Dimitrios Karakonstantinou*

</div>
