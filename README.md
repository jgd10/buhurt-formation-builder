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

## Layout

Screen order, top to bottom: a dismissible **What is this?** intro (first-time only), **Timeline**, **The list**, **Match**, **Fighters**.

## How it works

- **Timeline** — snapshots in the order you create them. Click **+** to add one (starts as a copy of the previous snapshot's positions and annotations), click a chip to open it, **Duplicate formation** to branch a variation.
- **The list** — a to-scale top-down view, Team A at the bottom, Team B at the top:
  - Fighters not yet on the list sit in the **bench** row below it; click a name to drop them onto their team's side. Adding or removing a fighter from a roster updates the list immediately.
  - Drag any fighter or marshal directly on the list to reposition them freely, or turn on **Snap to 0.5m grid** for tidier, measured placement.
  - Each fighter is shown as concentric rings of their team's colours, labelled with up to 3 consonants from their name plus a symbol for their current weapon. A name starting with a vowel always keeps that vowel as the first letter of its label (AARON → ARN, ANDREW → AND); otherwise vowels are dropped unless there aren't 3 consonants to use (JOE → JOE, MAX → MAX, MAXY → MXY). The selected fighter's label turns cyan.
  - Click a fighter to open the **inspector**: switch weapon mid-fight, toggle **Down** (with a reason — fall, decision, or armour failure) and **Disarmed**, or remove them.
  - **Draw on list** sketches freehand arrows or notes over the field in a colour of your choice; **Undo stroke** / **Clear annotations** clean it up. Annotations are stored per snapshot.
  - **Arrange** applies a formation template (line, wedge, echelon left/right, refused flank) to a whole team's roster in one click.
  - A stats line shows how many of each roster are on the list, and flags any pairs of fighters that look like they're overlapping.
- **Match** — title, date, notes, and format (3v3/5v5/12v12/30v30, which fixes the list's standard size — not manually adjustable). Team names and up to 3 colours each. **Generate both dummy teams** (or the per-team **Generate dummy team** button) fills rosters with randomised names, weapons, and colours sized to the current format — handy for building your own team and testing against a placeholder opponent.
- **Fighters** — name, optional belt number, type, and a default weapon from the fixed set: sword/axe/mace & shield, sword/axe/mace & buckler, solo sword/axe/mace, long axe, poleaxe, longsword, two-handed falchion. New fighters default to poleaxe (see the note on "Halberd" below).
- **Marshals & linesmen** — the gold dots, added/positioned from the Match card, shared across the whole match.

## Shortcuts

- `Ctrl+Z` / `Ctrl+Shift+Z` (or `Ctrl+Y`) — undo / redo. Covers roster, placement, status, weapon, marshal, snapshot, template, and annotation changes. Undo/redo buttons are also in the header.
- `D` — toggle Draw mode.
- `Delete` / `Backspace` — remove the currently selected fighter from the list.
Shortcuts are ignored while typing in a text field.

## Data & persistence

- Your current match autosaves to the browser's `localStorage` as you work, so a page refresh won't lose your place.
- `localStorage` is per-browser, per-device — it isn't shared or backed up anywhere. **Save formation file** downloads a JSON file you can keep, share, or re-import later.
- **Export as image** rasterises the current list to a PNG — good for dropping into Discord or WhatsApp.
- **Load formation file** replaces whatever match is currently open. Older `version: 1` files (from the original numbered-slot layout) are auto-migrated.

## Known constraints (by design)

- Exactly 2 teams per match, up to 3 colours per team.
- List size is fixed to the selected format's standard dimensions.
- Weapons are drawn from the fixed 7-category list above (plus a generic "Other" symbol as a catch-all).
- No combat simulation, AI, scoring, or enforced tactical meaning.

## A note on "Halberd"

The weapon list uses "Poleaxe" rather than "Halberd" (often used interchangeably in HMB circles). New fighters default to Poleaxe. Happy to add Halberd as its own category if you'd rather keep them distinct.

## What's not in here yet

A few requested ideas didn't make this round, either because they need infrastructure this tool intentionally doesn't have, or to keep the change set reviewable:

- **Shareable link** — this app has no backend by design (nothing to host, nothing to pay for, works from a static file). A real link-sharing feature needs somewhere to store the state server-side, or a URL long enough to encode a whole match, which gets unreliable fast with bigger rosters and annotations. **Save/Load formation file** is the supported way to share a match for now; happy to revisit if you want to add lightweight hosting.
- **Distance measurement tool** and a **dedicated attack-direction arrow control** — freehand drawing already covers both informally (draw a line and eyeball it, or draw an arrow), but neither has a purpose-built, precise control yet.
- **Experience-level colour coding** — there's no "experience" attribute on a fighter yet; weapon type already gets its own symbol.

## Extending it

Everything is vanilla HTML/CSS/JS in one file, so it's easy to fork. Natural next steps if you want them later: a read-only shareable match viewer, CSV export of the fighter roster, or a real distance/measurement tool.
