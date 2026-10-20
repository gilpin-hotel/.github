<div align="center">

<img width="1200" alt="Gilpin Hotel & Lake House" src="profile/assets/gilpin-banner.png" />

# 🌲 Gilpin Hotel & Lake House
### *Luxury Hospitality Orchestration & Intelligence Platform*
**Windermere, The English Lake District · Relais & Châteaux**

[![Data Residency](https://img.shields.io/badge/Data_Residency-UK_%26_EU_Only-1f513f?style=for-the-badge&logo=shield)](https://github.com/gilpin-hotel)
[![GDL Lake](https://img.shields.io/badge/Snowflake-London_eu--west--2-29B5E8?style=for-the-badge&logo=snowflake)](https://github.com/gilpin-hotel/GDL_Merged)
[![Arrival Intelligence](https://img.shields.io/badge/Arrival_Tool-v10.6.0_Production-2ea44f?style=for-the-badge&logo=react)](https://gilpin-arrival-tool.vercel.app)
[![GDPR Compliant](https://img.shields.io/badge/UK_GDPR-Article_28_Governed-gold?style=for-the-badge)](https://github.com/gilpin-hotel)

---

</div>

## 🏨 The Technology Suite

The bespoke technology ecosystem powering operations, culinary excellence, and guest recognition across **Gilpin Hotel** and the **Lake House Sanctuary**:

```
                       ┌───────────────────────────────┐
                       │     Gilpin Data Lake (GDL)    │
                       │   Snowflake · London AWS      │
                       │   Resolved 360° Guest Identity │
                       └──────────────┬────────────────┘
                                      │
              ┌───────────────────────┼───────────────────────┐
              │ SERVING_NOTIFY        │ Real-Time Sync        │ Direct API
              ▼                       ▼                       ▼
   ┌─────────────────────┐ ┌─────────────────────┐ ┌─────────────────────┐
   │    Arrival Tool     │ │  Kitchen Intelligence │ │    Reviews Hub     │
   │  v10.6.0 (React 19) │ │  Allergens & Dining │ │ Sentiment Analytics │
   └─────────────────────┘ └─────────────────────┘ └─────────────────────┘
              ▲                       ▲                       ▲
              │                       │                       │
   ┌──────────┴──────────┐ ┌──────────┴──────────┐ ┌──────────┴──────────┐
   │   ANPR Gate Camera  │ │   Shift Handovers   │ │   Amenities Hub     │
   │   Lake House Gate   │ │   Plato Operations  │ │   Suite Register    │
   └─────────────────────┘ └─────────────────────┘ └─────────────────────┘
```

---

### 📦 Platform Repositories

| Repository | Purpose & Architecture | Stack & Status |
| :--- | :--- | :--- |
| **[GilpinArrivalTool](https://github.com/gilpin-hotel/GilpinArrivalTool)** | **Real-Time Arrival Orchestration:** Multi-department arrivals board, VIP recognition, room readiness, card token at check-in, and DM shift diary. | `React 19` `TypeScript` `Vite` `Firebase RTDB` `Tailwind` · **v10.6.0 Production** |
| **[GDL_Merged](https://github.com/gilpin-hotel/GDL_Merged)** | **Enterprise Data Lake:** Ingests Hart PMS, Plato, and Dotdigital; conformed dimensional Star Schema (`GOLD`), automated lineage gates (`LIN-0..7`), and zero PII repository snapshot. | `Snowflake London` `DuckDB` `Python 3.14` `SQL` · **Governed** |
| **[gilpin-kitchen-dashboard](https://github.com/gilpin-hotel/gilpin-kitchen-dashboard)** | **Kitchen Intelligence & Food Safety:** Live dietary and allergen screen for *Source at Gilpin Hotel*, dining recognition, and service pacing. | `React` `TypeScript` `Vite` `Vercel` · **Production** |
| **[Gilpin-reviews-hub](https://github.com/gilpin-hotel/Gilpin-reviews-hub)** | **Guest Sentiment Intelligence:** Direct Google Business Profile API sync, multi-platform sentiment classification, and department feedback explorer. | `React` `TypeScript` `Vercel Serverless` `Google API` · **Active** |
| **[gilpin-amenities](https://github.com/gilpin-hotel/gilpin-amenities)** | **Suite & Room Register:** Live asset and amenity register across Hotel bedrooms (1–31) and Lake House sanctuaries (51–58). | `HTML5` `Tailwind CSS` `Vanilla JS` · **Operational** |

---

### 🔒 Architectural Guarantees & Standards

1. **Strict Data Residency:**  
   100% of guest data processing and storage resides in the **UK and EU** (Snowflake London `eu-west-2`, real-time cache in Belgium `europe-west1`, apps in London `lhr1`, Google Workspace in Europe). Cross-region model routing is strictly disabled.
2. **Lineage-Gated Integrity:**  
   Every lake build is verified by an automated lineage gate module (`112_lineage_gate.sql`) preventing backwards dependencies or unapproved cross-layer queries.
3. **Privacy by Construction:**  
   All code repositories are scrubbed under Lake Standard C9 — zero live guest personal data, zero unencrypted credentials, and pseudonymised test fixtures.

---

<div align="center">

*Gilpin Hotel & Lake House · Crook Road, Windermere, Cumbria LA23 3NE*  
*Relais & Châteaux · Crafted by Dimitrios Karakonstantinou*

</div>
