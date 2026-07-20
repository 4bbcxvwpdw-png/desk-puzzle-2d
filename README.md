> **Desk Puzzle — a 2D pathology desk sorting game.**
> Play: https://4bbcxvwpdw-png.github.io/desk-puzzle-2d/
> Built with Claude Code.

# Desk Puzzle — Paper Edition

A calm grouping puzzle played on a desk seen from above: sixteen physical
clues, four hidden groups, no timer, no pressure.

## The game

You look straight down at a desk. Sixteen clue pieces start as a messy pile
strewn about the desk — drag them around freely to spread out and read them,
then drag four that share something into one of the four trays along the
bottom and press its **Lock In** button. Right, and the tray locks with the
group's name (colored by difficulty tier). Wrong costs one of four mistakes;
"One away!" means three of the four belonged together. Four locked trays
wins; four mistakes loses (unless Casual mode is on).

Each item's `zone` field decides what kind of physical piece it becomes:

| `zone`      | Piece            | Readable?                                    |
| ----------- | ---------------- | -------------------------------------------- |
| `corkboard` | sticky note      | yes, on its face                             |
| `folder`    | paper sheet      | yes, on its face                             |
| `photo`     | photograph       | yes — its image (or captioned print) is the clue |
| `rx`        | prescription     | yes, on its face                             |
| `rack`      | microscope slide | no — put it on the **microscope stage** (each slide carries an anonymous letter A, B, C…) |
| `tubes`     | X-ray film       | no — lay it over the **light box**           |

(The retired index-card kind, `deskCards`, still loads from old files and is
shown as a paper sheet.)

**Microscope.** The stage sits on the desk; the viewer is a permanent
square panel on the left of the play screen, sized to match the desk.
Drag the zoom knob between the 4×, 10×, and 40× detents (it clicks at
each stop; arrow keys step too) and pan with the buttons or by dragging
inside the view. Slides with a `scope.image` show that image; slides
without one show their label as an etched-glass specimen, so text-only
puzzles stay solvable.

**Settings.** The gear (menu or play header) opens Settings: theme
(Light / Dark / System, persisted; the dark theme is a warm walnut room
under the same plum accent), sound, Casual mode, and a developer toggle
for the experimental drag-audio bed. A quick Mute lives in the play
header. The default drag sound is a gated scrape: silent at rest and
during small adjustments, sparse ticks on slow drags, a fused slide on
fast ones (all tunable in the `?layout` Sound section).

**Light box.** Always lit, and films never attach to it — light simply
shines through whatever part of a film physically lies on the glass. Half
on, half lit. A film's lit content is its `info.image` if the puzzle has
one, otherwise its label in glowing bone-white lettering.

**Label printer** (the hint system). Drop any piece on the printer (or focus
it and press L) and it prints the piece's name on a small stuck-on label —
permanent for the rest of that puzzle. Three blank labels per puzzle,
unlimited in Casual mode. The results screen counts the hints you used.

## Puzzle format

Plain JSON in `puzzles/` (see `sample-001.json`): 16 `items` in 4 `groups`
of 4, each item typed by its piece kind. A per-puzzle `machines` list
(e.g. `"machines": ["scope", "lightbox"]`) declares which desk machines the
puzzle uses — only those render, and validation refuses combinations that
would leave clues unreadable (slides with no microscope, films with no
light box). Older files without a `machines` field get all three machines;
legacy piece-kind ids (`corkboard`/`folder`/`rack`/`tubes`/`deskCards`) are
still read but never shown — the UI always says sticky note, paper sheet,
index card, slide, X-ray film.

Each `group` can also carry an optional `article`: an array of
`{type, ...}` blocks — `{"type":"heading","text":"..."}`,
`{"type":"text","text":"..."}`, or `{"type":"image","src":"...","caption":"..."}`
(`src` is a data URI, same as an item's `info.image`) — rendered on the
results screen under that group's one-line `explanation`. It's optional
and backward compatible; groups (and whole puzzle files) without it render
exactly as before.

## Keyboard play

Every piece is focusable:

- **1–4** sends the focused piece to that tray (first empty slot)
- **0** or **Backspace** returns it to the desk
- **V** puts a slide on the microscope, or slides a film onto the light box
- **L** prints a name label on it (spends a hint)
- **Arrow keys** nudge a desk piece around

## How to run

No build step — plain HTML/CSS/JS. Either double-click `index.html` (an
embedded copy of the sample puzzle covers `file://` fetch limits), or serve
it:

```
python3 -m http.server 4607 --directory "projects/Desk-Puzzle-2D"
```

then open `http://localhost:4607`. A `desk-puzzle-2d` entry for this is in
the workspace's `.claude/launch.json`. Deep link a puzzle with
`?puzzle=<id>`.

**Dev pages** (URL-gated, not linked from the menu): `?layout` opens Layout
Mode: collapsible sections for machines, piece sizes, scatter, and a live
Sound editor (master, per-cue gains with audition buttons, drag-scrape
thresholds), a side tab hides the whole panel, and Export bundles it all
as layout JSON.
`?editor` opens the Puzzle Creator — group-by-group authoring with piece-type
chips, machine toggles (with inline warnings when a piece kind needs a
machine you turned off), field-level validation as you type, draft
autosave, and export of both the puzzle JSON and an updated `index.json`.
Each group also has a collapsible **Article** section: an ordered list of
heading/paragraph/image blocks (reorder or remove any block, add more with
the buttons underneath) that becomes the group's full write-up on the
results screen — the one-line `explanation` field stays and becomes the
lede above it. It's optional; a group with no article renders on results
exactly as it always has.

The live preview renders the real game at a fixed size captured when the
editor opens, then scales with the drawer: full screen when the drawer is
tucked away (edge tab to show/hide, same pattern as `?layout`), shrunk to
fit beside it when open, and the drawer's left edge is a drag handle to
resize it (persisted, so it stays where you left it). Edits reach the
preview in ~150ms — typing a field, adding an article block, or flipping a
machine toggle all repaint it live, no Test Play required. A **Preview
results** button shows the results screen exactly as it'll look once
someone solves the puzzle, articles and all, without playing it out. Test
Play still opens a standalone full-size run.

### Publishing your layout

`?layout`'s Export button downloads `layout.json`. Drop that file in the
project root, next to `index.html` (same folder this README lives in), and
every player picks it up automatically on their next load — no code change,
no rebuild. Precedence, lowest to highest: the built-in defaults, then
`layout.json` if one is present, then whatever you're still live-editing in
`?layout` in your own browser (that layer lives in `localStorage` and only
applies to you, so it always wins locally until you clear it with "Reset to
defaults"). If there's no `layout.json` file, nothing changes — the fetch
for it fails silently and the built-in defaults stand.

## Drop-in assets (all optional, all auto-detected)

**Textures — `assets/textures/`.** Drop files with these exact names and
the game uses them on the next load; anything missing keeps the built-in
CSS look. No manifest, no registration step (`manifest.json` in that folder
is a leftover from an older setup and is ignored):

```
desk.jpg  blotter.png  sticky.png  sticky-pink.png  sticky-green.png
sticky-orange.png  card.png  paper.png  slide.png  film.png
```

Object textures should be alpha-transparent cutouts (the piece shadow
follows the cutout); every file is auto-trimmed to its visible pixels at
load, so margins and resolution don't matter — pieces always render at
their standard size. Numbered alternates (`sticky-2.png`, `paper-2.png`, …)
join the per-piece variety pool when present. Piece variety otherwise comes
from a seeded flip/hue-brightness jitter per piece (the corner-fold and tape
overlay decorations were removed in round 9 — they read as clutter, not
realism; `assets/textures/overlays/` is retired, see `CHATGPT_PROMPTS.md`).
When textures are on, clue labels render in a handwritten style over them;
printed hint labels stay machine-set so hints never read like clues.
`CHATGPT_PROMPTS.md` in that folder has copy-paste-ready generation
prompts (mostly retired now, kept for reference).

**Sounds — `assets/sounds/`.** Every cue is synthesized in WebAudio (no
files needed). To replace one, list it in `assets/sounds/manifest.json`
(`{"present": ["correct.mp3", …]}`) and drop the file in. Cue names:
`pickup-paper, drop-paper, pickup-glass, drop-glass, dock-glass,
film-rustle, dial-tick, pan-tick, print, shuffle, correct, wrong, one-away,
win, lose`.

## How to add a puzzle

1. Author it in the `?editor` page (Export Puzzle JSON + Export updated
   index.json do the packaging for you), or write the JSON by hand.
2. Drop `<id>.json` into `puzzles/`.
3. Add its entry to `puzzles/index.json` (`{id, title, date, file}`) and
   point `current` at it — or use the editor's exported `index.json`.
4. Reload. Invalid puzzles fail loudly with a readable error screen.

## Persistence

Progress saves to `localStorage` under `dp2d:save3:<puzzle-id>` after every
move — tray contents, locked groups, mistakes, attempt history, printed
labels and hints used, plus the exact desk: every piece's position,
rotation, and stacking order, and what's on the microscope stage. Reloading
restores the desk exactly; a finished puzzle reopens on its results. Saves
are healed on load (staging, solved groups, machines, and hint counts are
cross-checked), so a stale or hand-edited save can't restore an impossible
game. Settings (Casual mode, sound, display size) live under
`dp2d:settings`; dev layout overrides under `dp2d:layout`.
