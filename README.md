# Formation Ledger — Buhurt Tactical Formation Analysis Tool

A single-file, no-build browser tool for recording and analysing Buhurt team formations as time-indexed snapshots. Built to match the spec: match/team/fighter setup, a timeline of snapshots, a drag-and-drop (or click-to-place) formation editor with numeric slots, and versioned JSON import/export.

It does not simulate combat, physics, or outcomes — it's a recording and review tool. All tactical interpretation of slot numbers is left to you.

## Running it

There's no build step. Everything lives in `index.html`.

- **Locally:** just open `index.html` in a browser.
- **GitHub Pages (recommended for sharing with your club):**
  1. Create a new GitHub repo (e.g. `buhurt-formation-ledger`).
  2. Add `index.html` (and this `README.md`) to the repo root and push.
  3. In the repo, go to **Settings → Pages**.
  4. Under **Build and deployment**, set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
  5. Save. GitHub gives you a URL like `https://<username>.github.io/buhurt-formation-ledger/` within a minute or two.

No server, database, or API keys are needed — it's entirely client-side.

## How it works

- **Match** — title, date, location, notes, and the two teams (name + up to 3 colours each).
- **Fighters** — added per team, with name, optional belt number, type (grappler/striker/generalist/runner/other), stored per-match (no persistent global roster, per spec).
- **Timeline** — snapshots in the order you create them. Click **+** to add one, click a chip to open it, **Duplicate** to branch a variation.
- **Snapshot editor** — set timestamp, phase label, notes, and slot count, then assign fighters into numbered slots for each team:
  - Click a fighter card in the roster to "arm" it, then click a slot to place it (works well on touch/mobile).
  - Or drag a fighter card straight onto a slot (desktop).
  - A fighter can only occupy one slot per team per snapshot — placing it elsewhere moves it automatically.
  - Click the **×** on a filled slot to clear it.

## Data & persistence

- Your current match autosaves to the browser's `localStorage` as you work, so a page refresh won't lose your place.
- `localStorage` is per-browser, per-device — it isn't shared or backed up anywhere. **Use Export JSON** to save a match file you can keep, share, or re-import later (on this device or any other).
- **Export JSON** downloads a versioned file matching the spec's schema (`{ "version": 1, "match": {...} }`).
- **Import JSON** loads a previously exported file, replacing whatever match is currently open.

## Known constraints (by design, per spec)

- Exactly 2 teams per match, up to 3 colours per team.
- Slot keys are numeric strings ("1"–N"), consistent within a snapshot.
- No combat simulation, AI, scoring, or enforced tactical meaning — slot positions are whatever your team decides they mean.

## Extending it

Everything is vanilla HTML/CSS/JS in one file, so it's easy to fork. Natural next steps if you want them later: a read-only shareable match viewer, PNG export of a formation, or CSV export of the fighter roster.
