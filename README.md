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

Collect these off the board by touching them with a ball:

| Pickup | Effect |
| --- | --- |
| **＋ orb** (gold) | Adds a permanent extra ball to your stream. |
| **⚡ Laser** (cyan) | Banks a charge. Tap the **⚡** button to fire vertical beams up every column that punch *through* all the stacked rows. |
| **🌀 Scramble** (purple) | Banks a charge. Tap the **🌀** button mid-flight to fling all your balls off in fresh upward directions to catch stragglers. |
| **💥 Bomb** (orange) | Explodes on contact, chain-damaging every nearby brick. |

The **⚡** and **🌀** buttons appear at the bottom-left and glow when they're ready
to use; the little badge shows how many charges you've banked.

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
- `LASER_DMG` / `BOMB_DMG` — how much punch the laser and bomb pack.
- `levelRows(l)` — how many brick rows each level throws at you.
- `newBrick()` controls brick toughness per level; `rollPickup()` and the
  `puChance` line in `spawnTopRow()` control how often power-ups appear.

You can also change the title and the "Made with 💛" line in the HTML, or swap the
accent color by editing the `--accent` value in the `:root` CSS.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire game (HTML + CSS + JS). |
| `manifest.webmanifest` | Makes it installable as a home-screen app. |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App icons. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |
