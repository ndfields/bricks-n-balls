# Brick N Balls 💛

A custom, mobile-first brick-breaker game — the "Bricks n Balls" / "Ballz" style
where you aim, launch a stream of bouncing balls, and clear the numbered bricks
before the stack marches down to the bottom line. It's **level-based** and gets
tougher as you climb, with collectible **power-ups** (lasers, scramblers, bombs)
and juicy particle/glow effects.

It's a single self-contained HTML file (no build step, no dependencies, no server
logic) designed to run great on a phone and be hosted for free on GitHub Pages.

## How to play

- **Drag** anywhere to aim — a dotted line previews the shot.
- **Release** to launch all your balls in a stream.
- Each ball chips a brick's number down by 1; the brick breaks at 0.
- **Clear every brick to beat the level.** Each level celebration awards +2 balls,
  then the next level starts with more rows, denser packing, and tougher bricks.
- After every shot the bricks drop one row. **Don't let them reach the dashed
  line at the bottom** — that's game over.
- Tap the **⏩ button** (bottom-right) while balls are flying to fast-forward.
- Your best level is saved on the device.

## Power-ups

Power-ups are **special blocks mixed into the grid** among the numbered bricks.
There are no buttons to press — just hit a special block with a ball and it
triggers automatically, then it's consumed.

| Block | Effect |
| --- | --- |
| **＋1** (gold) | Adds a permanent extra ball to your stream. |
| **×2** (green) | Multiplier — spawns extra balls mid-flight for big chain hits. |
| **⇋ Laser** (cyan) | Fires a beam across its **row or column**, clearing every brick in that line. The arrow on the block shows which way it shoots. |
| **↑ Redirect** (orange) | Bends the ball that hits it off in a new direction to reach tricky angles. |

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

- `COLS` / `ROWS` — board width and height.
- `BASE_SPEED_FRAC` — ball speed.
- `levelRows(l)` — how many brick rows each level throws at you.
- `newBrick()` controls brick toughness per level.
- `rollPower()` sets which special blocks appear and how often each type comes
  up; the `puChance` line in `spawnTopRow()` controls how frequently any special
  block shows up at all.

You can also change the title and the "Made with 💛" line in the HTML, or swap the
accent color by editing the `--accent` value in the `:root` CSS.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire game (HTML + CSS + JS). |
| `manifest.webmanifest` | Makes it installable as a home-screen app. |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App icons. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |
