# Melo — Deploy Notes

## Files in this drop

| File | Purpose |
|---|---|
| `index.html` | The full app (rename of pantryflow.html) — push to repo root |
| `manifest.json` | PWA manifest — declares app name, colors, icons, scope |
| `sw.js` | Service worker — caches assets for offline use, updates on each visit |
| `icon.svg` | App icon (vector source) — used for browser tabs, modern browsers |
| `icon-192.png` | 192×192 PNG icon — Android home screen, fallback |
| `icon-512.png` | 512×512 PNG icon — splash screens, large displays |

## How to deploy on your `Melo` repo

**One-time setup (you've done most of this):**

1. Clone the repo if you haven't:
   ```
   git clone https://github.com/komalxspace/Melo.git
   cd Melo
   ```

2. Drop all 6 files at the repo root (next to your existing `index.html`):
   ```
   Melo/
     ├── index.html       ← this replaces your current one
     ├── manifest.json
     ├── sw.js
     ├── icon.svg
     ├── icon-192.png
     └── icon-512.png
   ```

3. Commit & push:
   ```
   git add index.html manifest.json sw.js icon.svg icon-192.png icon-512.png
   git commit -m "Add PWA support: manifest, service worker, icons"
   git push
   ```

4. GitHub Pages will redeploy in ~30 seconds.

## What this gives users

- **Install to home screen** — on iPhone, open the site in Safari → Share → "Add to Home Screen". The Melo icon appears as a real app, opens fullscreen, no browser chrome.
- **Offline support** — once the user has visited once, the app keeps working even with no network. Their data already lived in localStorage so this just makes the shell load.
- **Faster repeat loads** — assets cached by service worker, app opens instantly on second visit.
- **Proper app metadata** — when shared via WhatsApp / Slack / etc., the link previews with "Melo — Meal Planner" instead of generic.

## On future deploys

Whenever you push a new `index.html`:

- The service worker will detect the new version and update on the user's next visit (it does network-first for HTML).
- If you want to force-bust the cache after a big change, bump `VERSION` in `sw.js` (e.g. `melo-v1` → `melo-v2`) — this triggers a full cache clear on every device.

## Verifying it works

1. Visit `https://komalxspace.github.io/Melo/` in Chrome
2. Open DevTools → Application tab → Manifest — should show your manifest details
3. Application → Service Workers — should show `sw.js` activated
4. On iPhone: Share sheet → "Add to Home Screen" → confirm "Melo" appears with the green icon

## Path notes

The manifest uses `/Melo/` as `start_url` and `scope` — this matches your GitHub Pages path. If you ever move the repo (custom domain, root deployment, etc.), update both fields in `manifest.json`.
