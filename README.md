# KAUNTI — Kenya Counties Quiz

A React + Vite + Tailwind v4 single-page app. All game logic lives in `src/App.jsx`.

## Prerequisites

- **Node.js 18 or newer.** Check with `node -v`. If you need to install it:
  - macOS: `brew install node`
  - Ubuntu/Debian: `curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt install nodejs`
  - Windows: download from https://nodejs.org

## Run it

```bash
cd kaunti-app
npm install
npm run dev
```

Vite will start on http://localhost:5173 and open the browser automatically.

## Build for production

```bash
npm run build
npm run preview        # serve the built files locally
```

The output is in `dist/`. Drop that on any static host (Netlify, Vercel, Nginx, S3+CloudFront, GitHub Pages).

## Project structure

```
kaunti-app/
├── index.html             # entry HTML
├── package.json
├── vite.config.js         # Vite + React + Tailwind v4 plugin
└── src/
    ├── main.jsx           # React mount
    ├── index.css          # `@import "tailwindcss";`
    └── App.jsx            # the entire game (47-county data + map + game loop)
```

## How the map works

`App.jsx` ships with the 47 Kenya county polygons inline as SVG paths. They were derived from the public `mikelmaron/kenya-election-data` GeoJSON, simplified to roughly 1.3 km tolerance and projected to a 700×900 SVG viewBox. Each county carries:

- `d` — SVG path
- `labelX`, `labelY` — representative point for labels
- `neighbors` — names of counties that geographically touch it, used to pick plausible distractor options

If you ever want to swap in higher-resolution boundaries, regenerate the `COUNTIES` array against any Kenya counties GeoJSON using the same projection (uniform equirectangular over the lon/lat bbox of all features, padded by 20 px inside a 700×900 box).

## Tech

- React 18
- Vite 5
- Tailwind CSS 4 (via `@tailwindcss/vite`)
- `lucide-react` for icons
- No state persistence — scores reset on refresh by design

## Troubleshooting

- **Port 5173 already in use** → `npm run dev -- --port 5174`
- **Fonts (Bricolage / DM Sans / JetBrains Mono) not loading** → they pull from Google Fonts at runtime; make sure you have network access. If you want them offline, swap the `@import url(...)` line in `App.jsx`'s `<style>` block for self-hosted `@font-face` declarations.
- **Sound doesn't play on mobile Safari** → tap the volume icon once after the Start screen; iOS requires a user gesture before AudioContext can resume.
