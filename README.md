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

The list (the field) is the first thing on screen, followed by Match, Fighters, and Timeline.

- **The list** — a to-scale top-down view, Team A at the bottom, Team B at the top:
  - Fighters not yet on the list sit in the **bench** row below it; click a name to drop them onto their team's side. Adding or removing a fighter from a roster updates the list immediately.
  - Drag any fighter or marshal directly on the list to reposition them freely — there's no grid or slots, just real coordinates.
  - Each fighter is shown as concentric rings of their team's colours (colour 1 at the centre, ringed by colour 2, ringed by colour 3, for teams using more than one), labelled with up to 3 consonants from their name (vowels are only pulled back in if that leaves fewer than 3 characters — JACK → JCK, JOE → JOE, MAX → MAX, MAXY → MXY) plus a symbol for their current weapon. The selected fighter's label turns cyan instead of being ringed/highlighted.
  - Click a fighter to open the **inspector**: switch their weapon at any point mid-fight, toggle **Down** (they stay on the list, greyed out, as part of the battlefield — pick a reason: fall, decision, or armour failure) and **Disarmed** independently, or remove them from the list.
  - **Draw on list** toggles freehand annotation mode — sketch arrows or notes straight onto the field in a colour of your choice; **Undo stroke** and **Clear annotations** clean it up. Annotations are stored per snapshot.
- **Match** — title, date, notes, and format (3v3/5v5/12v12/30v30). Picking a format snaps the list to that format's standard size automatically (not manually adjustable). Team names and up to 3 colours each. **Generate dummy teams** fills both rosters with randomised names, weapons, and colour schemes sized to the current format — handy for testing without real fighter data.
- **Fighters** — added per team, with name, optional belt number, type (grappler/striker/generalist/runner/other), and a default weapon from the fixed set: sword/axe/mace & shield, sword/axe/mace & buckler, solo sword/axe/mace, long axe, poleaxe, longsword, two-handed falchion. New fighters default to poleaxe (the closest match to "halberd" in this list — see note below).
- **Marshals & linesmen** — the gold dots. Add as many as you like from the match card; drag them into position directly on the list; click one on the list to remove it. Shared across the whole match rather than reset per snapshot.
- **Timeline** — snapshots in the order you create them. Click **+** to add one (starts as a copy of the previous snapshot's positions and annotations), click a chip to open it, **Duplicate** to branch a variation.

## Data & persistence

- Your current match autosaves to the browser's `localStorage` as you work, so a page refresh won't lose your place.
- `localStorage` is per-browser, per-device — it isn't shared or backed up anywhere. **Use Export JSON** to save a match file you can keep, share, or re-import later (on this device or any other).
- **Export JSON** downloads a versioned file (`{ "version": 3, "match": {...} }`).
- **Import JSON** loads a previously exported file, replacing whatever match is currently open. Older `version: 1` files (from the original numbered-slot layout) are auto-migrated.

## Known constraints (by design)

- Exactly 2 teams per match, up to 3 colours per team.
- List size is fixed to the selected format's standard dimensions — there's no manual length/width override.
- Weapons are drawn from the fixed 7-category list above (plus a generic "Other" symbol as a catch-all) — there's no free-text weapon field.
- No combat simulation, AI, scoring, or enforced tactical meaning — positions, weapons, and status are whatever you record; the app just shows it back to you.

## A note on "Halberd"

The weapon list uses "Poleaxe" rather than "Halberd" (they're often used interchangeably in HMB circles, and the list doesn't have room for both). New fighters default to Poleaxe. If you'd rather have a separate Halberd category, that's a quick addition — just say the word.

## Extending it

Everything is vanilla HTML/CSS/JS in one file, so it's easy to fork. Natural next steps if you want them later: a read-only shareable match viewer, PNG/SVG export of a single snapshot for match reports, or CSV export of the fighter roster.
