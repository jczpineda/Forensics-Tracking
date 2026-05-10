# Forensics Tracking

A single-file, browser-based sports tracking visualization tool for analyzing player and ball positional data. No installation or server required — just open `index.html` in a modern browser.

---

## Features

- **2D Pitch Visualization** — Renders player positions on a customizable pitch image with color-coded home/away teams.
- **Voronoi Diagram** — Displays each player's territorial control zone on the pitch.
- **Pitch Control (Spearman-style)** — Generates a heatmap showing which team controls each area of the pitch at a given moment.
- **3D Drone View** — Interactive 3D scene built with Three.js showing players (~1.80m tall) and the ball on a full 105×68m pitch.
- **Player Perspective / Follow Cam** — Select any player from the roster panel or by clicking their dot on the 2D canvas, then enable **Follow Cam** to see the game from their point of view in 3D.
- **Playback Controls** — Step through frames at 0.10s intervals (10 Hz) or use Play/Pause for animated playback.
- **Label Modes** — Toggle player labels between: Name, Speed, Acceleration, and Direction Angle.
- **Video Attachment** — Attach a local match video file alongside the tracking data (note: video is not synchronized with coordinates).
- **Data Export**
  - PDF snapshot via jsPDF
  - CSV of player positions
  - XLSX (positional data)
  - XLSX (physical metrics)
  - Video export for the 2D, Voronoi, and Pitch Control views
  - 3D scene recording

---

## Input Files

| Input | Description |
|---|---|
| `match.json` | Match metadata including team rosters and player info |
| `tracking .jsonl` | Positional tracking data at 10 Hz (one JSON object per line) |
| Pitch image (PNG/JPG) | Optional background image of the pitch |
| Video file | Optional match video (MP4, etc.) — manual attach, not synced |

### Coordinate Convention

Raw coordinates are transformed before rendering:

$$x' = x + 52.5, \quad y' = y + 34$$

This maps the origin to the center of a standard 105×68m pitch.

---

## Usage

1. Open `index.html` in a modern browser (Chrome recommended for video recording features).
2. Upload your `match.json` and tracking `.jsonl` files.
3. Optionally upload a pitch image and/or a video file.
4. Set the period, minute, second, and tenth-of-second to the moment you want to inspect.
5. Click **OK → Make PDF** to render that frame and generate a PDF snapshot.
6. Use **▶ Play / ⏸ Pause** and **±0.10s** buttons to scrub through the data.
7. Select a player from the **Player Perspective** panel (or click a dot on the 2D canvas) to highlight them and enable the **Follow Cam** in the 3D view.

---

## Dependencies (CDN — no install needed)

| Library | Version | Purpose |
|---|---|---|
| [jsPDF](https://github.com/parallax/jsPDF) | 2.5.1 | PDF export |
| [d3-delaunay](https://github.com/d3/d3-delaunay) | 6.0.4 | Voronoi diagram computation |
| [SheetJS (xlsx)](https://sheetjs.com/) | 0.18.5 | XLSX export |
| [Three.js](https://threejs.org/) | r128 | 3D rendering |
| Three.js OrbitControls | r128 | 3D camera mouse controls |

All libraries are loaded via CDN. An internet connection is required on first load.

---

## Browser Compatibility

- **Chrome / Edge** — Full feature support including video recording (`MediaRecorder`).
- **Firefox** — Supported; some video export formats may differ.
- **Safari** — Partial support; video recording may be limited.

---

## Notes

- Tracking data is streamed line-by-line from the `.jsonl` file during playback to keep memory usage low.
- The attached video player has its **own independent time** and is not synchronized with the tracking coordinates.
- If a selected player is not on the pitch (e.g., on the bench), the 3D camera moves to a bench-side view automatically.
- Pitch Control computation uses a Spearman-style model with configurable parameters (reaction time, max speed, integration horizon, etc.) defined in the `PC_PARAMS` object in the source.

Official Link: https://jczpineda.github.io/Forensics-Tracking/
