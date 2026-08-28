# Buzzy's Balls 💛

A custom, mobile-first brick-breaker in the style of **Brick Blast / Ball Breaker**:
aim, launch a stream of bouncing balls, and clear the numbered bricks before the
stack marches down to the bottom. Every level starts you with the same stream of
**60 balls**, and brick toughness climbs each level. **Special power-up blocks**
are mixed right into the grid.

It's a **progression**, not a high-score run: every level is a fixed, hand-
repeatable puzzle that is identical every single time you play it. Clear it and
you move on; lose it and you retry *that same level*, not the whole game. Your
place is saved on the device, so closing the tab picks up where you left off.

It's a single self-contained HTML file (no build step, no dependencies, no server
logic) designed to run great on a phone and be hosted for free on GitHub Pages.

## How to play

- **Drag** anywhere to aim — a dotted line previews the shot.
- **Release** to launch your whole stream of balls. Wherever the *first* ball
  lands is the collection point — every other ball rolls along the floor to it,
  and that becomes your next launch spot.
- Each ball knocks a brick's number down by 1; the brick shatters at 0.
- **Clear every brick to beat the level.** The next level brings more rows, denser
  packing, and higher-numbered bricks.
- **Your ball count resets at the start of every level.** Balls you pick up during
  a level are yours for that level only — they never carry over.
- **Drag far out to the side** for a flat, low shot along the wall — the aim goes
  down to about 5° above horizontal.
- After every shot the bricks drop one row. The bottom row is a **kill line**: a
  brick reaching it ends the run immediately. The board stays clean until bricks
  are one row above it, and then the kill row lights up with flashing warning
  triangles and a red border — that's your last turn to clear them.
- The **combo counter** above the board tracks hits for the turn and heats up from
  blue to pink to flaming red, with a **Super Combo!** banner at big streaks.
- Tap **⏩** to fast-forward, or **Return** to call all your balls home early.
- **Levels are fixed.** Level 7 is always the exact same board, so a level you
  keep losing is one you can learn and plan around.
- **Losing costs you the level, not the run.** You restart that level with a
  fresh 60 balls; you never get sent back to Level 1.
- A **Reset progress** button on the title screen wipes your saved level and
  furthest level and puts you back to Level 1. It asks for a yes/no confirmation
  first, and only appears once there's actually progress to wipe.

## Blocks

**Numbered bricks** are colour-coded by how tough they are, from cyan (easy) to
green, gold, and red (hardest). Some are **triangle wedges** that bounce balls off
at 45° — great for banking shots into awkward corners.

There is always at least one **empty row above the stack**, so you can always break
through into open space along the top. The stack's top row itself may be completely
closed off across every column.

**Special blocks** are mixed into the grid. There are no buttons to press: hit one
with a ball and it fires automatically.

| Block | Effect |
| --- | --- |
| **+5** (green ring) | Adds 5 balls to your stream for the rest of **this level**. |
| **Ball cluster + arrow** (orange ring) | A big burst of **bonus balls for this turn only**. They fly in pink so you can tell them apart, and they vanish when the turn ends — your permanent count is unchanged. |
| **Laser** (cyan ring) | Balls **pass straight through** it. Every ball that crosses it fires a beam taking **exactly 1 off every brick in its row**, so a dense stream racks up damage fast. It **burns out at the end of the turn it fires on**; an untouched one waits for a later turn. |
| **Scrambler** (purple ring) | Re-aims every ball in play in a fresh upward direction. It **never adds balls** — it only changes where they are going. |

## Cheers from the sidelines

Clear a pile of bricks in one turn and the game pipes up with a random cheeky
one-liner. The first fires at 6 bricks destroyed in a turn, then every 9 after
that, capped at 3 per turn so it never turns into a wall of text. The lines live
in the `MESSAGES` array at the top of the script — add, cut or rewrite them
freely, it's just a list of strings.

## Bonus turns

Some levels hand you a **bonus turn**. When it fires you get a huge one-off
stream of extra balls (they launch in pink behind your normal gold ones), and
the price is that the board is **pushed down three rows** at once.

- Whether a level offers one at all is fixed per level, like the board itself —
  roughly 60% of levels have one, and it can only fire **once per level**.
- It never lands in a level's **first three turns**, so a level always gets going
  under its own steam before the board takes the three-row drop.
- It waits for a safe moment. It only triggers when the stack is high enough
  that dropping three rows cannot put a brick on, or even next to, the kill
  line, so a bonus turn can never be what loses you the level.
- The extra balls scale with the level and last **that turn only** — afterwards
  you are back to exactly the ball count you had before.

## Haptics

The game buzzes on launch, power-ups, laser shots, combo milestones, the
last-turn danger warning, level clear and game over, via the standard
[Vibration API](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/vibrate).

**This works on Android and does nothing on iPhone.** WebKit has never shipped
the Vibration API, so `navigator.vibrate` is simply absent in every iOS browser.
The game checks for it and silently skips the buzz — nothing errors, nothing
changes about how it plays.

The old workaround (toggling a hidden `<input type="checkbox" switch>`, which
made iOS play a system tick) worked on iOS 17.4-26.4 but Apple patched it in
**iOS 26.5**: only a *direct finger tap* on a real, natively-styled switch fires
a haptic now, and script can't trigger one at all. That rules out the haptics
this game would actually want — brick hits, lasers, level clear — since those
are all script-driven. Set `bnb_haptics` to `"0"` in localStorage to turn the
buzzing off.

## Play locally

Just open `index.html` in any browser. On a computer you can also run a tiny
static server so it behaves exactly like it will on Pages:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Host it on GitHub Pages (free)

1. Push this repository to GitHub (a public repo is simplest for free Pages).
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Pick the branch that has these files and the folder **`/ (root)`**, then **Save**.
5. Wait a minute, then reload the Pages settings page — it shows the live URL,
   something like `https://<your-username>.github.io/bricks-n-balls/`.

Send that link to your wife's phone.

### Add it to her home screen (feels like a real app)

- **iPhone (Safari):** open the link → Share → **Add to Home Screen**.
- **Android (Chrome):** open the link → menu (⋮) → **Add to Home screen** /
  **Install app**.

The included `manifest.webmanifest` and app icons make it launch full-screen with
its own icon, just like an installed game.

## Make it yours

Everything lives in `index.html`. A few easy tweaks near the top of the `<script>`:

- `COLS` is the board width in columns; `ROWS` auto-fits the screen height.
- `START_BALLS` — how many balls every level starts with (default 60).
- `BONUS_TURN_CHANCE` / `BONUS_TURN_DROP` / `BONUS_TURN_MIN_TURN` — how often a
  level offers a bonus turn, how many rows it costs, and the earliest turn it may
  land on; `bonusTurnBalls(l)` sets how many extra balls it hands you at a given
  level.
- `BASE_SPEED_FRAC` — ball speed; the ⏩ button doubles it.
- `BONUS_BALLS` — how many one-turn bonus balls the orange block gives (default 30).
- `TOP_GAP` — how many rows are always kept clear above the stack.
- `GRID_SCALE` — shrinks the cells to fit more rows on screen (more play area).
- `MIN_AIM` — how flat a shot you can line up, in radians above horizontal.
- `levelRows(l)` — how many brick rows each level throws at you.
- `levelBase(l)` — the toughest fresh brick for a level. Since the ball count is
  the same every level, this is the main difficulty dial; `brickTier()` keys the
  brick colours off it. Later levels get harder mainly through **brick toughness**
  rather than packing more bricks in: the `density` line creeps up slowly and is
  capped at `COLS - 2`, so at least two columns of every row are always open and
  a row can never become a solid wall.
- `buildLevel(l)` generates a whole level from a seed made only from its number
  — brick values, triangles, which columns fill, and which special blocks appear
  and where. Change anything in here and **every level changes**, including ones
  already beaten. The `density` line controls how packed each row is, and the
  `hasSpecial` chance controls how often a special block shows up.

Progress lives in `localStorage` under `bnb_level` (the level to play next) and
`bnb_bestlvl` (the furthest reached).

You can also change the title and the "Made with 💛" line in the HTML, or swap the
colours by editing the `--bg1` / `--bg2` / `--accent` values in the `:root` CSS.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire game (HTML + CSS + JS). |
| `manifest.webmanifest` | Makes it installable as a home-screen app. |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App icons. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |
