# Diablo IV Achievement Tracker — PWA

Bilingual (RU/EN) installable web app based on the 45-entry catalogue in the companion iOS project.

## Run locally
A PWA requires HTTP(S), not `file://`.
```bash
cd DiabloIVTracker-PWA
python3 -m http.server 8080
```
Open `http://localhost:8080`. For installation on iPhone, publish the folder to an HTTPS host (GitHub Pages, Netlify, Cloudflare Pages, etc.), open it in Safari, tap Share → Add to Home Screen.

## Features
- 45 bundled Steam achievement records and Russian guides.
- Search, category filter, status filter and sorting.
- Progress dashboard.
- Manual marks stored in localStorage.
- Offline shell and catalogue via service worker.
- Optional Steam sync through a small Node backend. Steam API key remains on the backend, not in browser storage.

## Steam sync backend
1. Deploy `server/` to a server with Node.js 18+.
2. Set `STEAM_API_KEY` using the hosting provider's secret/environment settings. Do not commit a real key.
3. Set `ALLOWED_ORIGIN` to the exact HTTPS origin hosting the PWA.
4. Run `npm start` (or `node server.mjs`).
5. In PWA Settings enter SteamID64 and backend base URL, e.g. `https://steam-proxy.example`.
6. Tap Synchronize Steam.

The proxy calls Steam `GetSchemaForGame` and `GetPlayerAchievements` for appid 2344520. If profile game details are private, Steam may not return the player's achievements.

## Progress precedence
- Manual toggle overrides Steam state for that achievement.
- If there is no manual override, Steam state is displayed.
- Manual marks are local to the current browser/device. This starter version does not cloud-sync manual marks across devices.

## Important
- HTTPS is required for service workers and Add to Home Screen (localhost is an exception for development).
- The catalogue and guide wording are carried over from the iOS companion project; review game-specific conditions in-game if patches change them.
- Steam sync is read-only. It does not unlock or modify achievements.
