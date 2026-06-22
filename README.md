# 💎 Mines — Risk-Free Edition

A single-page web game modeled on the casino game **Mines**, but with **no betting
and no way to lose your balance**. The player can only gain points — over time the
Balance is monotonically non-decreasing.

## How to run

No build step, no dependencies, no backend. Just open the file:

```bash
# from the project root
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or double-click `index.html` in a file browser. It also works fine served from any
static host (e.g. `python3 -m http.server` then visit `http://localhost:8000`).

## How to play

1. Pick a **mine count** (1–24) with the slider before flipping your first tile.
2. Flip tiles one at a time. Each **gem** 💎 raises your **round multiplier**.
3. Hit **Cash Out** to bank `roundStake × multiplier` into your Balance and start a
   new round. Cash Out is disabled until you've found at least one gem.
4. Flip a **mine** 💣 and the round ends — you forfeit **only this round's pending
   win**. Your Balance is never reduced.
5. Clear every gem on the board for an automatic perfect-clear payout.

### The "no losses" rule (design choice)

Hitting a mine forfeits the unbanked round multiplier only. The persistent Balance
never goes down — there's no stake deducted and no real money. This is the one
intentional deviation from real Mines, kept per the spec.

## Multiplier math

After revealing `k` gems with `M` mines on a 25-tile grid:

```
multiplier(k) = 0.99 × Π (i = 0 .. k-1) of (25 - i) / (25 - M - i)
round winnings = roundStake × multiplier(k)
```

The `0.99` is the house edge. The live multiplier and projected winnings update as
you reveal gems, and the side panel previews the next-gem multiplier.

## Where to tweak things

All gameplay constants live in the `CONFIG` block at the top of the `<script>` in
[`index.html`](./index.html):

```js
const CONFIG = {
  GRID_SIZE: 25,          // total tiles (5x5); the multiplier math assumes 25
  STARTING_BALANCE: 1000, // points the player starts with
  ROUND_STAKE: 100,       // virtual basis for round winnings (NOT deducted)
  DEFAULT_MINES: 3,       // initial mine count
  MIN_MINES: 1,           // slider lower bound
  MAX_MINES: 24,          // slider upper bound
  HOUSE_EDGE: 0.99        // the 0.99 factor in the multiplier formula
};
```

- **Mine count**: change in-game with the slider, or set `DEFAULT_MINES`.
- **Starting balance**: `STARTING_BALANCE`.
- **Round stake**: `ROUND_STAKE` (the fixed virtual basis for round winnings).

## Features

- 5×5 grid with flip animations; green gems, red bombs.
- Dark, modern casino aesthetic; fully mobile-responsive.
- Live Balance, multiplier, projected round winnings, gems-found counter.
- Mine-count slider (1–24), locked once you flip the first tile of a round.
- Satisfying Cash Out flash and a "boom" reveal of all tiles on a mine hit.
- Optional sound (synthesized via WebAudio — no asset files).
- In-memory "Best balance" tracker.

## Tech

Plain, self-contained `index.html` with vanilla JS and CSS. No backend, no external
accounts, no real money. All state is kept in memory.
