# Brick N Balls 💛

A custom, mobile-first brick-breaker in the style of **Brick Blast / Ball Breaker**:
aim, launch a stream of bouncing balls, and clear the numbered bricks before the
stack marches down to the bottom. It's **level-based**, gets tougher as you climb,
and mixes **special power-up blocks** right into the grid.

It's a single self-contained HTML file (no build step, no dependencies, no server
logic) designed to run great on a phone and be hosted for free on GitHub Pages.

## How to play

- **Drag** anywhere to aim — a dotted line previews the shot.
- **Release** to launch your whole stream of balls.
- Each ball knocks a brick's number down by 1; the brick shatters at 0.
- **Clear every brick to beat the level.** Clearing awards +2 balls, then the next
  level brings more rows, denser packing, and higher-numbered bricks.
- After every shot the bricks drop one row. When they reach the bottom row the
  board flashes **red warning triangles** — that's your last turn before game over.
- The **combo counter** above the board tracks hits for the turn and heats up from
  blue to pink to flaming red, with a **Super Combo!** banner at big streaks.
- Tap **⏩** to fast-forward, or **Return** to call all your balls home early.
- Your best level is saved on the device.

## Blocks

**Numbered bricks** are colour-coded by how tough they are, from cyan (easy) to
green, gold, and red (hardest). Some are **triangle wedges** that bounce balls off
at 45° — great for banking shots into awkward corners.

**Special blocks** are mixed into the grid. There are no buttons to press: hit one
with a ball and it fires automatically.

| Block | Effect |
| --- | --- |
| **+5** (green ring) | Adds 5 balls to your stream — **permanent**, for every turn from now on. |
| **Ball cluster + arrow** (orange ring) | A big burst of **bonus balls for this turn only**. They fly in pink so you can tell them apart, and they vanish when the turn ends — your permanent count is unchanged. |
| **Ball with spray arrows** (blue ring) | **Splitter** — the ball that hits it splits into extra balls fanning outward. |
| **Spinning bar** (pink ring) | A **rotating deflector** that stays on the board and knocks balls off at whatever angle it happens to be pointing. |

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

- `COLS` — how many columns wide the board is (`ROWS` auto-fits the screen).
- `BASE_SPEED_FRAC` — ball speed.
- `BONUS_BALLS` — how many one-turn bonus balls the orange block gives (default 24).
- `SPIN_RATE` — how fast the pink deflector spins.
- `levelRows(l)` — how many brick rows each level throws at you.
- `freshValue()` controls brick toughness per level; `brickTier()` sets the
  colour thresholds.
- `rollSpecial()` sets which special blocks appear and how often each type comes
  up; the `0.42` chance in `spawnTopRow()` controls how frequently any special
  block shows up at all.

You can also change the title and the "Made with 💛" line in the HTML, or swap the
colours by editing the `--bg1` / `--bg2` / `--accent` values in the `:root` CSS.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire game (HTML + CSS + JS). |
| `manifest.webmanifest` | Makes it installable as a home-screen app. |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App icons. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |
