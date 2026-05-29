# SafeRoute

A single-file web tool that ranks Google Maps driving alternatives by how many **tricky intersections** they force on you — penalising unprotected turns across oncoming traffic and U-turns, rewarding turns at traffic signals — and finds **gas stations on your entry side of the road** with live fuel prices.

No build step. One HTML file. Runs entirely in the browser.

## Deploy to GitHub Pages

1. Create a repo (e.g. `saferoute`) and add **`index.html`** and **`.nojekyll`** to the root.
   ```bash
   git init
   git add index.html .nojekyll
   git commit -m "SafeRoute"
   git branch -M main
   git remote add origin https://github.com/<you>/saferoute.git
   git push -u origin main
   ```
2. Repo → **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main** / **/ (root)** → Save.
3. Wait ~1 minute. Your site is at:
   ```
   https://<you>.github.io/saferoute/
   ```

`.nojekyll` is optional here (the app has no underscore-prefixed files), but it's good insurance against Jekyll touching anything.

## API key setup

The app asks for a Google Maps Platform key at runtime and stores it only in your browser's localStorage. **Never commit a key to the repo** — the published Pages site is public.

In [Google Cloud Console](https://console.cloud.google.com/google/maps-apis):

- **Enable** these APIs on the project: Maps JavaScript API, Directions API, Places API (New).
- **Restrict the key** (the key is always visible in browser network requests; the referrer restriction is the real protection):
  - Application restrictions → **HTTP referrers** → add `https://<you>.github.io/saferoute/*`
  - API restrictions → allow only the three APIs above.

The key-entry screen shows the exact referrer string to paste, based on the URL you're actually running on.

## Install on a phone

Open the Pages URL on your phone → browser menu → **Add to Home Screen**. It launches fullscreen (standalone). Because Pages is HTTPS, the 📍 *Use my location* button and standalone mode both work (they don't on a local `file://` copy).

## Notes / limits

- **Tricky-intersection scoring** uses Google's per-step maneuvers + OpenStreetMap traffic-signal positions. "Unprotected" is a heuristic — divided highways, medians and dedicated turn phases can fool it.
- **Right-side gas** is judged from the route's heading vs. bearing to the station. Strong filter, not a guarantee (frontage roads, far-edge driveways).
- **Fuel prices** come straight from Google's `fuelOptions` (its priciest Places tier) — fetched only for the filtered right-side stations to keep cost down. US coverage is good but not every station reports.
- `DirectionsService`/`DirectionsRenderer` log deprecation warnings (Feb 2026) but still work and aren't scheduled for removal.
