# Buzzy's Balls 💛

A custom, mobile-first brick-breaker in the style of **Brick Blast / Ball Breaker**:
aim, launch a stream of bouncing balls, and clear the numbered bricks before the
stack marches down to the bottom. Every level starts you with the same stream of
**60 balls**, and brick toughness climbs each level, so the later levels really
bite. **Special power-up blocks** are mixed right into the grid.

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

There is always at least one **empty row above the stack**, so you can always break
through into open space along the top. The stack's top row itself may be completely
closed off across all seven columns.

**Special blocks** are mixed into the grid. There are no buttons to press: hit one
with a ball and it fires automatically.

| Block | Effect |
| --- | --- |
| **+5** (green ring) | Adds 5 balls to your stream for the rest of **this level**. |
| **Ball cluster + arrow** (orange ring) | A big burst of **bonus balls for this turn only**. They fly in pink so you can tell them apart, and they vanish when the turn ends — your permanent count is unchanged. |
| **Laser** (cyan ring) | Balls **pass straight through** it. Every ball that crosses it fires a beam taking **exactly 1 off every brick in its row**, so a dense stream racks up damage fast. It **burns out at the end of the turn it fires on**; an untouched one waits for a later turn. |
| **Scrambler** (purple ring) | Re-aims every ball in play in a fresh upward direction. It **never adds balls** — it only changes where they are going. |

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

- `COLS` — how many columns wide the board is (`ROWS` auto-fits the screen).
- `START_BALLS` — how many balls every level starts with (default 60).
- `BASE_SPEED_FRAC` — ball speed; the ⏩ button doubles it.
- `BONUS_BALLS` — how many one-turn bonus balls the orange block gives (default 30).
- `TOP_GAP` — how many rows are always kept clear above the stack.
- `levelRows(l)` — how many brick rows each level throws at you.
- `levelBase()` — the toughest fresh brick for a level. Since the ball count no
  longer grows run-to-run, this is the main difficulty dial; `brickTier()` keys
  the brick colours off it.
- `rollSpecial()` sets which special blocks appear and how often each type comes
  up; the `hasSpecial` chance in `spawnTopRow()` controls how frequently any
  special block shows up at all.

You can also change the title and the "Made with 💛" line in the HTML, or swap the
colours by editing the `--bg1` / `--bg2` / `--accent` values in the `:root` CSS.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire game (HTML + CSS + JS). |
| `manifest.webmanifest` | Makes it installable as a home-screen app. |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App icons. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |
