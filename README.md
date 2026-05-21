# Vitals — Personal Health Dashboard

A **100% private, browser-based** Apple Health data visualizer. Your data never leaves your device.

## Quick Start

1. **Export your Apple Health data:**
   - Open Health app on iPhone
   - Tap your profile icon (top-right corner)
   - Scroll down, tap "Export All Health Data"
   - Wait for zip to generate, then Save

2. **Open this dashboard** and drag the `export.zip` onto the page
3. Wait 30–60 seconds for first parse (on-device, no server)
4. Explore 6 tabs: **Recovery**, Cardio, Body, Training, Activity, Environment

## What You Get

### Recovery (Your Priority)
- **Composite Recovery Score** (0–100) from sleep + HRV vs baseline + RHR vs baseline
- Sleep duration, stages (Deep/REM/Core), consistency
- Trending HRV and RHR with 30-day rolling baselines
- Month-over-month deltas

### Cardio
- VO2 Max, Resting HR, Walking HR, HR Recovery
- **Zone 2 minutes per week** (estimated from daily avg HR on workout days)
- HR zone distribution across all workouts

### Body
- Weight + Body Fat % dual-axis recomp chart with 7-day smoothing
- Lean Body Mass, TDEE estimate (active + basal energy)
- Note: Xiaomi scale data only from Mar 2025 onward

### Training
- Workouts per week, stacked by type (Strength / Cardio / Walking / Other)
- **ACWR** (Acute/Chronic Workload Ratio) — flags injury risk if >1.5
- Weekly strength session count vs 6-day PPL target
- Recent workout table

### Activity
- Steps, distance, flights climbed, daylight time
- Charts with 7-day rolling averages

### Environment
- Headphone audio exposure (flags days >80 dB)
- Environmental audio, daylight for circadian rhythm

## How It Works

- **Streaming XML parser:** Processes 1.5GB export.xml in ~30–60 seconds without freezing the UI (web worker)
- **IndexedDB cache:** Stores parsed summary so next load is instant
- **No servers:** All parsing happens in your browser. Zero data transmission.
- **Offline capable:** Works entirely offline once cached

## Caveats

### Zone 2 Minutes (Heuristic)
Apple Health aggregates HR to daily averages; it doesn't export per-second HR data. I estimate Zone 2 time by:
- Taking each day's mean HR (from all HR records)
- On workout days, if mean HR falls in 60–70% of HRmax (117–137 bpm for you), I count that workout's duration as Z2 time
- **This is directional, not surgical.** Real Z2 detection would need per-sample HR tied to each workout.

### Body Fat & Weight Data
Your Xiaomi scale data begins in March 2025, so body composition charts before then are sparse or missing. Apple Watch weight records were imported from your health app earlier.

### Sleep Before May 2023
Early sleep records (Nov 2020 – May 2023) are iPhone "InBed" records without stage breakdown. From May 2023 onward (when your Apple Watch connected), full sleep architecture (Deep/REM/Core/Awake) is available.

### Recovery Score Baselines
The HRV vs baseline and RHR vs baseline calculations are most meaningful after 3+ weeks of consistent Watch wear (you have 3+ years, so ✓).

## Local Development

If you want to modify the dashboard:

```bash
# Clone or download the repo
git clone https://github.com/yourusername/vitals-dashboard.git
cd vitals-dashboard

# Open in browser (file:// works, or use a simple server)
open health_dashboard.html

# For a server:
python3 -m http.server 8000
# Then visit http://localhost:8000/health_dashboard.html
```

## Tech Stack

- **Single HTML file** (~80 KB) with embedded CSS + JavaScript
- **Chart.js 4.4** for visualizations
- **JSZip 3.10** for reading zip exports
- **Web Worker** for streaming XML parsing (doesn't freeze UI)
- **IndexedDB** for local caching

## Privacy

100% of processing happens in your browser. No analytics, no tracking, no data upload. The code is plaintext HTML/CSS/JS — inspect the source if you want to verify.

## License

Public domain. Use, modify, fork freely.

---

**Built for:** Personal health tracking, recovery monitoring, training adherence  
**Data source:** Apple Health (iPhone export)  
**Last updated:** May 2026
