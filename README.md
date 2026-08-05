================================================================
OROBORO CLINICAL SIMULATION PLATFORM - README
================================================================
Version: Prototype 0.3 - Visual Demo Edition
Date: 2026-05-13
Purpose: Agent-based infectious disease modeling with visual-first pitch

----------------------------------------------------------------
1. THE CORE IDEA
----------------------------------------------------------------
> A short demo video beats a written pitch.

Oroboro is a visual product. Every agent is a sphere. Blue -> Yellow -> Red
tells the story faster than any paper. Lead with the visual everywhere
(Twitter/X, LinkedIn, Reddit).

This repo contains two versions:
- backend/ : FastAPI + Three.js (original, requires server)
- demo/    : Standalone browser version (oroboro-demo_*.html) - NO BACKEND,
             built specifically for screen recording.

For your 30-60s demo, USE THE STANDALONE HTML FILE. Just open it in Chrome.

----------------------------------------------------------------
2. QUICK START - RECORDING THE VIRAL CLIP
----------------------------------------------------------------
File: oroboro-demo_agentic_artifact_1_57566a407bfd.html

Open fullscreen (F11). Follow this script:

[0-5s]   Population is all blue (Susceptible) + 90 Yellow (Exposed per GAMESHEET)
[5s]     Click APPLY on GAMESHEET panel -> logs "GAMESHEET loaded"
[6s]     Click ● REC (top bar) -> Red dot blinks, timer starts
[7s]     Click "Add Outbreak" -> Inject 10 Infectious
[7-30s]  Watch red spread. Event readout scrolls on right.
[30s]    Scroll to zoom in, R-Drag to rotate, L-Drag to PAN
[35-50s] Slow pan across outbreak, show telemetry climbing
[50s]    Click ■ STOP REC -> OS Save As dialog appears

Save As:
- Tries window.showSaveFilePicker() first (Chrome/Edge). You choose path.
  Example: C:\Users\You\Videos\oroboro-outbreak-1715600000.webm
- Fallback: Browser download -> Downloads folder

Recording tech: canvas.captureStream(60) + MediaRecorder (vp9/webm, 60fps)

Recommended tools if you want external capture: OBS, Screen Studio, Loom

----------------------------------------------------------------
3. CONTROLS REFERENCE
----------------------------------------------------------------
CAMERA SETTINGS (Strategy Game Mode):
- L-Drag / Left Click + Drag : PAN camera (move across map)
- R-Drag / Right Click + Drag: ROTATE / ORBIT around center
- Scroll Wheel : ZOOM IN/OUT (clamped 15 - 300 so you can't lose map)
- Middle Mouse : DOLLY
- O Key : Toggle Auto-Orbit ON/OFF
  When auto-orbit is ON, camera slowly orbits unless you are dragging.
  Dragging pauses auto-orbit for 3 seconds.

TIME CONTROL MECHANICS - Timeline Slider Functionality:
- History buffer: Every 200ms snapshot of all agents (positions+states) +
  events + telemetry. Capped at 1000 snapshots (~3 minutes).
- Timeline scrubber: Drag to instantly jump to any point in history.
  When scrubbing backwards, you are in REPLAY mode. When you drag to
  the end, you return to LIVE.
- Buttons: << REWIND TO START | < -5s | Play/Pause (Space) | +5s > | LIVE
- Speed: 0.25x / 0.5x / 1x (default) / 2x / 4x
- Timecode display: 00:12 / 02:45
- Indicator: LIVE (green) vs REPLAY (yellow)

GAMESHEET - USER DEFINED PARAMETERS:
Think like a board game setup screen.

Panel: "GAMESHEET — Start With"
- Start with [___] SUSCEPTIBLE (default 200)
- Start with [___] EXPOSED (default 90)  <- your requested 90
- Start with [___] INFECTIOUS (default 10)
- Start with [___] RECOVERED (default 0)
- Start with [___] HOSPITALIZED (default 0)
- Auto Total = sum, validated 10-3000

Mechanics:
- Transmission Rate: 0.01 - 0.8 (default 0.25, higher = faster spread for demo)
- Vaccinated %: 0 - 100 (default 30, reduces infection prob *0.2)
- Hospital Beds: 50 - 5000
- Movement Speed: 0.1x - 5x
- Infection Radius: 1 - 15 (default 6)

Click APPLY to regenerate population exactly as defined.

----------------------------------------------------------------
4. ORIGINAL BACKEND ARCHITECTURE (FastAPI Version)
----------------------------------------------------------------
If you want to run the original python version:

pip install fastapi uvicorn
python main.py  # runs on 127.0.0.1:8000

Architecture:
Patient Agent -> State Machine -> Risk Engine -> Transmission Model ->
Resource Forecasting -> Clinical Dashboard

API Endpoints:
GET /telemetry -> steps sim + returns population, states, resources
GET /visual_agents -> positions + colors for Three.js
GET /events?limit=40 -> rolling event log (deque maxlen 200)
GET /generate_test_population?size=500 -> reset population
GET /patient/{pid} -> single patient + risk score
GET / -> basic HTML telemetry panel

Patient Agent:
- id, x,y (0-100), state (SUSCEPTIBLE, EXPOSED, INFECTIOUS, RECOVERED, HOSPITALIZED)
- age 1-90, vaccinated bool, comorbidities 0-3, memory[], timer

RiskEngine:
- +0.30 if age>65, +0.25 if comorbidities, -0.35 if vaccinated
- clamped 0-1, confidence 0.82

Transmission:
- distance = sqrt((x1-x2)^2 + (y1-y2)^2)
- if distance>10: 0 chance, else 0.15 * (0.2 if vaccinated)

Resources:
- hospital_beds 500, used_beds = count(HOSPITALIZED), capacity = used/total

STATES and COLORS:
SUSCEPTIBLE: 0x3399ff blue
EXPOSED:     0xffcc00 yellow
INFECTIOUS:  0xff3333 red
RECOVERED:   0x33ff88 green
HOSPITALIZED: 0xdddddd white/grey

Frontend original: Three.js r128, OrbitControls, scroll zoom 15-300

----------------------------------------------------------------
5. HOW TO PITCH WITH VIDEO
----------------------------------------------------------------
Twitter/X (280 chars + video):
"We built an agent-based outbreak simulator. Every dot is a patient. Blue->Yellow->Red. Watch what happens when we drop 10 infectious into 300. Oroboro Clinical Sim - open source [VIDEO]"

LinkedIn:
"Text descriptions don't convey infectious disease dynamics. Visuals do. This is Oroboro - 300 autonomous patient agents with memory, risk, transmission. 30 seconds tells you more than a whitepaper. Built for clinical researchers, not dashboards. [VIDEO]"

Reddit r/bioinformatics, r/Simulated, r/threejs:
"I built a real-time infectious disease sim where you can *see* R0 happening - spheres change state, event log scrolls. Three.js + FastAPI. Here's 40s of an outbreak. GitHub in comments."

----------------------------------------------------------------
6. FILES IN THIS PACKAGE
----------------------------------------------------------------
- oroboro-demo_agentic_artifact_1_57566a407bfd.html : RECORD-READY standalone
- main.py (embedded in your message) : Original FastAPI backend
- README.txt : This file

----------------------------------------------------------------
7. NEXT STEPS FOR YOU
----------------------------------------------------------------
- Record 60s clip using standalone HTML
- Post natively to X/LinkedIn (don't link YouTube, upload video direct)
- Add GitHub link in first comment, not main post (algorithm)
- For longer demos: Use Timeline + Save Snapshot PNG for thumbnails

If you get stuck, think GAMESHEET: every parameter should be a knob a
researcher can turn. Start with counts, then tune rates.

================================================================
Built for visual storytelling. Lead with spheres.
================================================================
