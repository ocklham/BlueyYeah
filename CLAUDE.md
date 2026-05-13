# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project intent

Sandbox for "vibe coding" experiments — a meeting point for unrelated tests, demos and prototypes. There is **no overarching product, no shared build, no test suite, no package manager**. Each experiment lives under its own folder and is meant to stand alone. The user (David, it.manager@arkeero.com) writes copy in Spanish; preserve Spanish strings when editing UI text.

Do not try to unify these experiments under a shared framework or extract "shared utilities" across folders unless the user explicitly asks. Cross-experiment abstraction is anti-goal here — each folder is a disposable playground.

## Repository layout

- `Bluey/` — Bluey-themed mini-games and assets.
  - `el_patio_de_bluey.html` (~1.8k lines) — single-file landing + voxel/canvas game, custom CSS, Spanish copy, references `Backyard_Chase.mp3` for audio.
  - `voxel_fox_forest_game_v2.html` (~310 lines) — isometric voxel scene built with Three.js r128 loaded from cdnjs.
  - `Backyard_Chase.mp3` — audio asset used by `el_patio_de_bluey.html`. Keep this filename stable.
- `Tests/testflights/flight_intel_live.html` (~2.8k lines) — live global ADS-B flight tracker on a Three.js globe (three.module from cdnjs v0.160.0), pulling state vectors from `api.adsb.lol` and OpenSky-shaped adapters. Reads live external APIs at runtime.

## Running

Everything is a static `.html` you open directly. Two options:

- Double-click the file (`file://` works for the Bluey pages; Three.js is loaded via CDN so internet is required on first load).
- For pages that use ES modules or strict CORS (e.g. `flight_intel_live.html` uses `<script type="importmap">` + `import`), serve the folder over HTTP. From the repo root:
  ```powershell
  python -m http.server 8000
  ```
  then visit `http://localhost:8000/Tests/testflights/flight_intel_live.html`.

There is no build step, no linter, no test runner — do not invent one.

## Editing conventions specific to this repo

- **Single-file pages**: HTML, CSS and JS are intentionally inlined in the same file. Resist the urge to split into `.css`/`.js` siblings unless asked — the self-contained format is part of the experiment.
- **CDN dependencies are pinned by URL**: Three.js r128 in `voxel_fox_forest_game_v2.html`, Three.js 0.160.0 (module form) in `flight_intel_live.html`. Don't silently bump versions; the APIs differ between r128 and 0.160.
- **External data sources** in `flight_intel_live.html` include `api.adsb.lol` and OpenSky-style adapters (see line ~1820, `OPENSKY FETCH` section, and the adapter list near line ~1855). Network calls happen client-side at runtime — there is no backend.
- **Spanish UI copy** is the default for the Bluey pages and most user-facing strings. When adding text, match the existing tone.
