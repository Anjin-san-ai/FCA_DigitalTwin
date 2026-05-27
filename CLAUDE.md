# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

A static HTML prototype for an **FCA (Financial Conduct Authority) Digital Twin** — a visual intelligence platform for understanding the FCA's structure, regulatory objectives, and AI agent network. There is no build step, no package manager, and no framework.

## Deployment

Pushes to `main` auto-deploy to **Azure Static Web Apps** via `.github/workflows/azure-static-web-apps.yml`. The secret `AZURE_STATIC_WEB_APPS_API_TOKEN` must be set in GitHub repo settings. To test locally, open any HTML file directly in a browser.

## File structure

| File | Purpose |
|---|---|
| `index.html` | Main enterprise platform hub — tabbed SPA with animated canvas background, knowledge graph, agent orchestration panels |
| `fca-digital-twin.html` | Reference card view of FCA organisation — divisions, objectives, headcount stats; supports light/dark mode via `prefers-color-scheme` |
| `fca-agent-network.html` | Animated multi-agent network diagram — dark navy theme, live orchestration simulation |
| `staticwebapp.config.json` | Azure routing config — SPA fallback to `index.html`, security headers |

## Architecture

All CSS and JavaScript live **inline inside each HTML file** — there are no external `.css` or `.js` assets. Each file is independently self-contained and can be opened standalone.

### Design tokens

Each file defines its own CSS custom properties in `:root`. The conventions differ slightly across files:

- `index.html` — light theme only; uses an FCA blue family (`--navy`, `--blue`, `--cyan`, `--teal`) plus semantic colours; fonts: **Plus Jakarta Sans**, **JetBrains Mono**, **Fraunces**
- `fca-digital-twin.html` — warm neutral palette (`--bg`, `--text`) with full dark-mode overrides; fonts: **DM Sans**, **Fraunces**
- `fca-agent-network.html` — dark navy palette (`--navy`, `--cyan`, `--white{N}%` opacity scale); fonts: **Plus Jakarta Sans**, **JetBrains Mono**

All fonts are loaded from Google Fonts CDN.

### Navigation

`index.html` implements a tabbed single-page experience with `.nav-tab` buttons toggling `.panel` sections via JavaScript. `fca-agent-network.html` uses its own internal section/step orchestration. `fca-digital-twin.html` is a long-scroll document with no in-page routing.

### Canvas animation

`index.html` includes a `<canvas id="bg-canvas">` with a particle/graph animation loop running via `requestAnimationFrame`. The animation is purely decorative — avoid changing it without testing in the browser, as visual regressions are easy to introduce.

## Working with these files

- **Edit in place** — no compile step needed; save and refresh.
- When adding a new section or card to `fca-digital-twin.html`, follow the existing `.section` / `.grid` / `.card` pattern; colour-code using the existing pill and dot classes.
- When adding nodes or connections to `fca-agent-network.html`, update the data arrays at the top of the `<script>` block (nodes, edges, agent steps) rather than hard-coding HTML.
- `staticwebapp.config.json` excludes `/images/`, `/css/`, and `/js/` from the SPA fallback — create assets in those paths if you ever add external files.
