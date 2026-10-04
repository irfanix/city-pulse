[README.md](https://github.com/user-attachments/files/33026954/README.md)
# City Pulse 

**Which broken street should the city fix first?**
Citizen photos in. A ranked, forecast-backed repair plan out.

 **2nd Runner-Up, SYNK SOLVE One Ideathon 2026** (Synk-F Community, 24 to 25 September 2026)
Challenge: *SMART-01, Predictive Maintenance from Citizen-Captured Infrastructure Data*

🔗 **Live demo:** https://fancy-marigold-90da44.netlify.app/
 Also packaged as an Android APK. Open the **Stats** tab and tap **▶ Quick tour** for a 1-minute walkthrough.

---

## The Problem

Cities fix what gets reported most, not what will fail first.

The same pothole gets reported 11 times and jumps the queue, while a blocked drain next to a hospital, reported once, floods with the next rain. Current systems rank by complaint count instead of risk, don't forecast which problems will turn critical, and never check whether a fix actually held.

## The Solution

City Pulse is a single-page web app (also an Android APK) with two views: one for **citizens** and one for the **city team**.

### For citizens
- **Report in seconds**: photo, description, and location (GPS or tap on the map)
- **Smart assist**: suggests the problem type and severity from the description (works offline, understands English and Indonesian keywords)
- **Duplicate check**: warns if the same problem was already reported within 150 m and merges it as a confirmation
- **"I see this too"**: confirm someone else's report to raise its priority
- **My Reports**: follow each report through Received → Visit booked → Crew sent → Fixed → Confirmed
- **Close the loop**: see before and after photos and confirm whether the fix really held; a failed fix reopens the problem and moves it up the queue

### For the city team
- **Interactive map** with clustered pins and four layers: Pins, Risk, Heat, and busy Places
- **Priority queue** sorted by most urgent, getting worse, most severe, waiting longest, most reported, or most overdue
- **Emergency forecast**: estimated days until each problem reaches emergency level, with a decay chart
- **Explainable score**: every problem shows exactly why it ranks where it does
- **Cost of waiting**: repair cost now vs as an emergency
- **Today's Crew Plan**: picks the jobs with the most priority per kilometre for one shift, filtered by crew type, and compares it side by side with a "most reported first" plan
- **Weather-aware**: pulls the 7-day rain forecast from Open-Meteo; a storm simulation shows how rain pushes drains, potholes and trees up the queue
- **Field checks that teach the model**: when a crew records the real severity, the app compares it with its forecast and adjusts how fast that type of damage is assumed to grow
- **Blind spots**: flags busy places (hospitals, stations, markets) with no recent reports and no recent inspection
- **Fix targets (SLA)** per priority level, with overdue tracking

## How the Ranking Works

**Priority score** (0 to 100):

| Factor | Weight |
|---|---|
| Severity | 34% |
| People affected (distance to hospitals, stations, schools, markets) | 24% |
| Getting worse (forecast risk) | 20% |
| Confirmations from other reports | 12% |
| Waiting time | 10% |

A problem whose previous fix failed gets +8 points per failed fix.

| Priority | Fix target |
|---|---|
| P1 | 24 hours |
| P2 | 3 days |
| P3 | 7 days |
| P4 | 30 days |

**Severity of a merged problem** uses the latest crew field check if there is one, otherwise the field worker's rating, otherwise the median of citizen reports, so one exaggerated report can't inflate the whole problem.

**Emergency forecast**: each of the ten problem types has its own decay speed, increased by local impact and by forecast rain for rain-sensitive types, and corrected over time by field checks.

## Screenshots

| Map | Problems | Crew Plan | Details |
|-----|----------|-----------|---------|
| ![Map](screenshots/map.png) | ![Problems](screenshots/problems.png) | ![Crew Plan](screenshots/crew-plan.png) | ![Details](screenshots/details.png) |

## Tech Stack

| Layer | Tools |
|---|---|
| App | Single HTML file with vanilla JavaScript and CSS |
| Maps | Leaflet 1.9.4, Leaflet.markercluster, Leaflet.heat, CARTO basemap tiles (OpenStreetMap data) |
| Weather | Open-Meteo 7-day precipitation API |
| Storage | Browser localStorage (reports, statuses and fix history survive restarts) |
| AI (optional) | `CONFIG.AI_ENDPOINT` can point to a small proxy that sends the photo to a vision model; without it, the built-in keyword assistant is used |
| Android | Packaged as an APK with AppMint / WebToApk |

Leaflet, its plugins and the Inter font are bundled inside the file, so the app itself loads without a CDN. Map tiles and the weather forecast need an internet connection.

## Run Locally

```bash
git clone https://github.com/[username]/city-pulse.git
cd city-pulse
```

Open `index.html` in a browser. No build step and no dependencies.

To use your own city, edit the `CONFIG` block near the top of the main script (city center, currency, AI endpoint).

> **Note:** the app starts with simulated demo reports around Bengaluru so the ranking, forecast and crew plan can be explored right away. The "GPS location" button needs HTTPS or localhost.


## Team

**Irfan Dawood** (Team Lead), BCA student at Enlight Degree College, Bengaluru ·

## Credits

Built with [Leaflet](https://leafletjs.com) (BSD-2-Clause), [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster) (MIT), [Leaflet.heat](https://github.com/Leaflet/Leaflet.heat) (BSD-2-Clause), the [Inter](https://rsms.me/inter/) font (SIL Open Font License), map data © OpenStreetMap contributors, tiles © CARTO, and weather data from [Open-Meteo](https://open-meteo.com).
