# 🌊 ORCA — Marine EcOsystem Reasoning with Collaborative Agents (SIH26176)

**ORCA** is an agentic-AI conversational marine intelligence platform designed for **fishermen, coastal authorities (Coast Guard & Fisheries Departments), and marine researchers**. It translates complex atmospheric, oceanographic, geospatial, and geofence data into **plain, direct, evidence-backed recommendations** with a bold **Go / Caution / No-Go verdict**, dynamic navigation routes, proactive border alarms, and family reassurance dispatching.

---

## 🧭 Live Demo & Quick Start

```bash
# 1. Install dependencies
npm install

# 2. Start the local Vite Development Server
npm run dev

# 3. Open in your browser
http://localhost:5173/
```

---

## 🗺️ Site Map & Role-Based Routes

| Route | View Name | Description |
|---|---|---|
| `/` | **Landing / Home Page** | Public portal with live verdict preview card, multi-agent engine explainer, and role entry points. |
| `/login` | **Role-Based Login** | **Fisherman**: Simulated biometric facial scan HUD + phone/OTP fallback. **Explorer/Researcher**: Email/Password. **Coastal Admin**: Restricted command terminal. |
| `/signup` | **Profile & Vessel Registration** | Captain registration, vessel MFID code, home harbor, and Indian language preferences. |
| `/dashboard/fisherman` | **Fisherman Bridge** | Voice-first co-pilot, 24-hr sea safety scores, nearest PFZ hotspots, passage route planner, proactive IMBL geofence alarm, survival kit ration calculator, nearby AIS boats, and voyage catch log. |
| `/dashboard/explorer` | **Explorer & Researcher Hub** | **Explorer**: Activity safety advisor (Sailing, Diving, Kayaking, Beach). **Researcher**: Dual layer comparison (SST vs Chlorophyll-a), historical ecological time-series, root-cause reasoning, and CSV/JSON export. |
| `/dashboard/admin` | **Guardian Command Center** | Fleet radar telemetry, emergency alert queue, SAR interceptor dispatching, **Family SMS Reassurance Log**, and policy/district risk analytics. |
| `/settings` | **System Preferences** | Vessel telemetry customization, 7 Indian voice synthesis languages, and emergency shore contacts. |

---

## 🎯 Architecture & Multi-Agent Reasoning Engine

ORCA utilizes a collaborative multi-agent architecture orchestrated by a **Master Planner**:

```
                              ┌─────────────────────────────┐
                              │  USER INPUT (Voice / Text)  │
                              └──────────────┬──────────────┘
                                             │
                                             ▼
                              ┌─────────────────────────────┐
                              │    MASTER PLANNER AGENT     │
                              └──────────────┬──────────────┘
                                             │
      ┌─────────────────┬────────────────────┼────────────────────┬─────────────────┐
      │                 │                    │                    │                 │
      ▼                 ▼                    ▼                    ▼                 ▼
┌──────────────┐ ┌──────────────┐  ┌───────────────────┐  ┌──────────────┐  ┌──────────────┐
│Weather Agent │ │ Ocean Agent  │  │ Geospatial Agent  │  │ Border Agent │  │  Risk Agent  │
│(IMD Squalls, │ │(SST fronts,  │  │ (INCOIS PFZs,     │  │ (IMBL bounds,│  │ (Composite   │
│Cyclones, Hs) │ │Chlorophyll-a)│  │  Bearing & Time)  │  │ Turn-Back)   │  │  Score & Veto│
└──────┬───────┘ └──────┬───────┘  └─────────┬─────────┘  └──────┬───────┘  └──────┬───────┘
       │                │                    │                   │                 │
       └────────────────┴────────────────────┼───────────────────┴─────────────────┘
                                             │
                                             ▼
                                   ┌───────────────────┐
                                   │    Route Agent    │
                                   │ (Waypoints & Fuel)│
                                   └─────────┬─────────┘
                                             │
                                             ▼
                              ┌─────────────────────────────┐
                              │  EXPLAINABLE VERDICT BADGE  │
                              │  (Go / Caution / No-Go)     │
                              │  + Agent Evidence Audit Log │
                              └─────────────────────────────┘
```

### 🤖 Modular Agent Breakdown
1. **Weather Agent (`src/services/agents/weatherAgent.ts`)**:
   - Evaluates wind speed, gusts, significant wave height ($H_s$), Beaufort sea state, precipitation, lightning strike probabilities, and cyclonic depression threats.
2. **Ocean Agent (`src/services/agents/oceanAgent.ts`)**:
   - Analyzes Sea Surface Temperature (SST) gradients, Chlorophyll-a concentration fronts, tidal heights (Spring/Neap), current drift, and thermocline depth.
3. **Geospatial Agent (`src/services/agents/geospatialAgent.ts`)**:
   - Computes great-circle/rhumb-line distance in Nautical Miles (NM), magnetic compass bearing (°), and estimated travel time to Potential Fishing Zones (PFZs).
4. **Border Agent (`src/services/agents/borderAgent.ts`)**:
   - Performs continuous geofence distance checks against International Maritime Boundary Lines (IMBL).
   - Computes safe turn-back escape bearings and vectors to the nearest safe harbor.
5. **Risk Agent (`src/services/agents/riskAgent.ts`)**:
   - Synthesizes all domain signals into a single **Composite Safety Score (0–100)** and strict **Go / Caution / No-Go** verdict.
   - Holds strict **veto authority** (e.g., even if fish potential is 95%, severe weather or border proximity forces an immediate NO-GO).
6. **Route Agent (`src/services/agents/routeAgent.ts`)**:
   - Generates passage plans (Safest vs Shortest vs Fuel-Efficient), safe departure & mandatory return windows, and estimated diesel fuel requirements.

---

## 🔍 Real Logic vs. Simulated Data Transparency

To adhere to hackathon and engineering evaluation standards, here is a transparent breakdown of what logic is real versus what data is simulated:

| Feature | Real Code / Algorithmic Logic | Simulated Data Source |
|---|---|---|
| **Multi-Agent Orchestration** | **Real**: Parallel agent pipeline, composite scoring matrix, veto state machine, and reasoning generation. | Structured JSON models mimicking INCOIS & IMD live API feeds. |
| **Biometric Face Login** | **Real**: Camera feed integration, laser scanning animation, and biometric registry matching state machine. | Camera canvas capture + mock fisherman credential matching. |
| **Voice Interface (STT & TTS)** | **Real**: Browser-native Web Speech API (`SpeechRecognition` & `SpeechSynthesisUtterance`) with multi-lingual voice recognition and speech readout. | Browser native speech engines for English, Tamil, Hindi, Telugu, Malayalam, Bengali, Gujarati. |
| **Geospatial & Boundary Geofencing** | **Real**: Haversine distance, perpendicular point-to-polyline calculations, and magnetic rhumb-line bearing mathematics. | Real-world coordinates of IMBL (Palk Strait India-Sri Lanka, Gujarat-Pakistan, Bay of Bengal). |
| **Border Proximity Alarm System** | **Real**: 3-stage state machine (Advisory $\rightarrow$ Warning $\rightarrow$ Critical), Web Audio API emergency siren synthesizer, 30s countdown snooze timer. | Simulated GPS beacon (draggable marker on Leaflet map). |
| **Interactive Nautical Map** | **Real**: Leaflet interactive chart with custom SVG ship markers, PFZ hotspots, boundary buffers, AIS fleet radar, and click-to-probe ocean soundings. | Carto Voyager nautical base tiles + mock AIS telemetry. |
| **Emergency Survival Kit** | **Real**: Metabolic hydration/calorie survival calculator, daily rationing budgets, and dehydration risk assessments. | User form inputs (crew size, water liters, food packets, stranded days). |
| **Voyage Catch Log** | **Real**: Browser `localStorage` persistence, automatic GPS coordinate stamping, species cataloging, and CSV report export. | Local device storage. |
| **Guardian Command Center** | **Real**: Interactive fleet telemetry grid, SAR interceptor dispatching, all-ships VHF broadcast triggers, and incident audit stream. | Simulated Class-B AIS transponder vessels. |
| **Family Reassurance SMS Log** | **Real**: Algorithmic translation of raw emergency telemetry into gentle, non-panicking family status notifications. | Shore family contacts registry. |
| **Researcher Ecological Hub** | **Real**: Dynamic split-slider comparison tool, multi-factor root cause analyzer, and live dataset CSV/JSON export. | INCOIS satellite SST/chlorophyll time-series models. |

---

## 🎨 Nautical Instrument Design System

Designed specifically to avoid generic SaaS / AI aesthetics:
- **Palette**: Deep Ocean Navy (`#071520`, `#0c2233`), Weathered Marine Teal (`#00a896`, `#02c39a`), Instrument Parchment, Alert Amber (`#f59e0b`), and Distress Red (`#ef4444`).
- **Typography**: Functional, high-contrast `Plus Jakarta Sans` paired with `JetBrains Mono` for navigational coordinates, knots, bearings, and depth soundings.
- **Fisherman Phrasing**: Direct, practical instructions (e.g., *"Safe to sail south-east. Steer 142° for Mandapam Ridge. Return before 15:00 IST."*).

---

## 📱 Multi-lingual Support
Supports 7 Indian languages with native scripts and audio readout:
- 🇬🇧 English
- 🇮🇳 தமிழ் (Tamil)
- 🇮🇳 हिन्दी (Hindi)
- 🇮🇳 తెలుగు (Telugu)
- 🇮🇳 മലയാളം (Malayalam)
- 🇮🇳 বাংলা (Bengali)
- 🇮🇳 ગુજરાતી (Gujarati)

---

## 🛡️ License
Built for the Smart India Hackathon (SIH-26176) — Prototype Demonstration.
