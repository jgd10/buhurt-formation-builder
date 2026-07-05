# Buhurt Formation Tool

A single-file, no-build browser tool for recording and analysing Buhurt team formations as time-indexed snapshots on a to-scale list (field).

It does not simulate combat, physics, or outcomes — it's a recording and review tool. Where fighters stand, what weapon they're carrying, and whether they're down or disarmed at any moment is left entirely to what you record.

## Running it

There's no build step. Everything lives in `index.html`.

- **Locally:** just open `index.html` in a browser.
- **GitHub Pages (recommended for sharing with your club):**
  1. Create a new GitHub repo (e.g. `buhurt-formation-tool`).
  2. Add `index.html` (and this `README.md`) to the repo root and push.
  3. In the repo, go to **Settings → Pages**.
  4. Under **Build and deployment**, set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
  5. Save. GitHub gives you a URL like `https://<username>.github.io/buhurt-formation-tool/` within a minute or two.

No server, database, or API keys are needed — it's entirely client-side.

## How it works

- **Match** — title, date, location, notes, format (3v3/5v5/12v12/30v30 — each constrains the list's allowed length/width range per the rules), list length and width in metres, and the two teams (name + up to 3 colours each). Changing format snaps the list to that format's default size.
- **Fighters** — added per team, with name, optional belt number, type (grappler/striker/generalist/runner/other), and a default weapon from the fixed set: sword/axe/mace & shield, sword/axe/mace & buckler, solo sword/axe/mace, long axe, poleaxe, longsword, two-handed falchion.
- **Marshals & linesmen** — the gold dots. Add as many as you like from the match card; drag them into position directly on the list; click one on the list to remove it. They're shared across the whole match rather than reset per snapshot.
- **Timeline** — snapshots in the order you create them. Click **+** to add one (it starts as a copy of the previous snapshot, so you're not rebuilding everyone from scratch each time), click a chip to open it, **Duplicate** to branch a variation.
- **The list (snapshot editor)** — a to-scale top-down view of the field:
  - Fighters not yet on the list sit in the **bench** row below it; click a name to drop them onto their team's side.
  - Drag any fighter or marshal directly on the list to reposition them freely — there's no grid or slots, just real coordinates.
  - Each fighter is shown as concentric rings of their team's colours (colour 1 at the centre, ringed by colour 2, ringed by colour 3, for teams that use more than one), labelled with the first 3 letters of their name plus a symbol for their current weapon.
  - Click a fighter to open the **inspector**, where you can switch their weapon at any point mid-fight, and toggle **Down** (they stay on the list, greyed out, as part of the battlefield, and you pick a reason — fall, decision, or armour failure) and **Disarmed** independently.
  - Click **Remove from list** in the inspector to send a fighter back to the bench for that snapshot.

## Data & persistence

- Your current match autosaves to the browser's `localStorage` as you work, so a page refresh won't lose your place.
- `localStorage` is per-browser, per-device — it isn't shared or backed up anywhere. **Use Export JSON** to save a match file you can keep, share, or re-import later (on this device or any other).
- **Export JSON** downloads a versioned file (`{ "version": 3, "match": {...} }`).
- **Import JSON** loads a previously exported file, replacing whatever match is currently open. Older `version: 1` files (from the original numbered-slot layout) are auto-migrated: fighters are spread out in a rough line on their side of a default 5v5-sized list with a placeholder weapon, since there was nowhere to record weapon/status data in that format.

## Known constraints (by design)

- Exactly 2 teams per match, up to 3 colours per team.
- List length/width are clamped to the selected format's allowed range (e.g. 5v5 is 9–20m long, 7–15m wide).
- Weapons are drawn from the fixed 7-category list above (plus a generic "Other" symbol as a catch-all) — there's no free-text weapon field.
- No combat simulation, AI, scoring, or enforced tactical meaning — positions, weapons, and status are whatever you record; the app just shows it back to you.

## Extending it

Everything is vanilla HTML/CSS/JS in one file, so it's easy to fork. Natural next steps if you want them later: a read-only shareable match viewer, PNG/SVG export of a single snapshot for match reports, or CSV export of the fighter roster.
