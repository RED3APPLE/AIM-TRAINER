# Aim Trainer

A lightweight, browser-based aim/reaction trainer. No install, no build step, no dependencies.

## How to run (quick, no install)
Just open `index.html` in any modern browser (Chrome, Firefox, Edge). Double-click it, or drag it into a browser window. Works fully offline, no server needed.

## How to install as a desktop app (PWA)
This is now a Progressive Web App — it can be "installed" to get a real desktop icon and its own window, without any Electron bloat (this stays a few KB, not 150MB+).

**Important:** browsers only show the install option when the page is served over `http://` or `https://` — not when opened directly as a `file://` path. So:

1. Open a terminal in this folder.
2. Run a tiny local server (pick whichever you have installed):
   - Python: `python3 -m http.server 8000`
   - Node: `npx serve .`
3. Open `http://localhost:8000` in Chrome or Edge.
4. Click the install icon in the address bar (or menu → "Install Aim Trainer").
5. It now runs as a standalone windowed app with its own icon, and works offline after the first load.

If you want this installable without running a server yourself, host the folder anywhere (GitHub Pages, Netlify, Vercel — all free and take a couple minutes) and open that URL instead.

## What's new: branding + retention system
- **Logo/branding**: a simple crosshair mark used as the app icon and on the main menu header. This makes the app look legitimate the moment someone lands on it — it does not by itself bring in traffic (see the note below).
- **Daily streak**: visiting on consecutive days increments a streak counter shown on the main menu. Miss a day and it resets to 1. Stored in `localStorage`, so it survives closing the tab — the same core mechanic Duolingo/Wordle use to bring people back.
- **Personal bests**: best score and best accuracy ever achieved are now remembered and shown on the main menu.
- **Daily Challenge**: every day, all players get the same difficulty + session length (deterministically derived from the date, like Wordle's one-puzzle-per-day). Completing it marks that day done; it resets the next day.

**Honest limitation:** all of this lives in the browser's `localStorage`, tied to one browser on one device. Clearing browser data, incognito mode, or switching devices resets it — there's no account/login, so there's no cross-device sync without real backend infrastructure (database + auth), which is a bigger project than this.

**On actually getting users:** none of the above causes anyone to discover the app — it only affects whether people who *already* arrived come back. Getting people to arrive at all requires real distribution (posting where an audience exists, search ranking built over months, or paid ads) — see the earlier discussion for realistic traffic expectations in this specific market.

## Controls
- Mouse: click the target
- Esc: pause / resume
- Space: restart (from pause or results screen)
- Enter: start (from main menu)

## Known limitations (honest list)
- **Mouse sensitivity only has a real effect when Pointer Lock is active.** Pointer Lock is what gives raw, unscaled mouse-delta input (like real aim trainers use) instead of the OS cursor's absolute screen position. It's on by default and requests automatically when you start a session, but some browsers only grant it from a direct click gesture, and Esc always releases it (that's a browser-level security behavior, not something this app controls) — which is why losing lock mid-session pauses the game rather than leaving you with a desynced cursor. If lock fails or is turned off in Settings, the game still works fine, just with sensitivity having no effect (cursor follows your OS pointer 1:1, as before).
- Clicking empty space while a target is alive currently does nothing (no penalty). Not yet decided if that should count as a miss — flag if you want that changed.
- Session stats (score/accuracy this run) still reset when the tab closes — only streaks and all-time bests persist now, not full session history.
- Not tested across every browser/OS combination — mainly reasoned through and fixed based on reported bugs, not exhaustively QA'd.
- Phase 2 features not built yet: reaction-time graph, miss heatmap, full session history, CSV export. The stats object already logs per-event data (`{type, x, y, reactionMs, t}`) needed for these, so adding them later is additive, not a rewrite.

## Wrapping as a desktop app (optional, later)
If you want an actual `.exe`/`.app` instead of "open in browser," this file can be dropped into an Electron shell with minimal changes — the game code itself doesn't need to change, only a thin wrapper around it.
