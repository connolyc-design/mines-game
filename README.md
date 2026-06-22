# 🍀 Lucky 5 — Pick a Square

A simple single-page betting game. There are **25 squares; 5 of them are lucky
winners**. Place a bet, tap one square, and find out instantly whether you won.

- **Win** (you tapped a lucky square): your bet pays **5×**.
- **Lose** (you tapped a blank): you lose your bet.

You're told the result right away, and the lucky squares are revealed.

## How to run

No build step, no dependencies, no backend. Just open the file:

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or double-click `index.html`. It also works on any static host
(`python3 -m http.server`, GitHub Pages, etc.).

### Play it online

Live on GitHub Pages: <https://connolyc-design.github.io/mines-game/>

## How to play

1. Set your **bet** with the − / + buttons or the quick chips (10 / 50 / 100 / Max).
2. Tap any square.
3. **💎 lucky square** → you win your bet × 5, shown immediately.
   **✕ blank** → you lose your bet, shown immediately.
4. Tap **Play Again** for a new round. If you ever hit 0 points, Play Again
   gives you free chips so you can keep playing.

## Odds & payout

5 winners out of 25 squares is a **1-in-5** chance. A **5× payout** is fair
(no house edge): over many plays your balance trends roughly flat, with swings.
Lower the payout in `CONFIG` if you want a house edge.

## Where to tweak things

All gameplay constants live in the `CONFIG` block at the top of the `<script>` in
[`index.html`](./index.html):

```js
const CONFIG = {
  GRID_SIZE: 25,           // total tiles (5x5)
  WINNERS: 5,              // number of lucky/correct squares
  PAYOUT: 5,               // a winning pick returns bet × PAYOUT (5 = fair odds)
  STARTING_BALANCE: 1000,  // points the player starts with
  DEFAULT_BET: 100,        // starting bet amount
  BET_STEP: 10,            // +/- button increment
  MIN_BET: 10,             // smallest allowed bet
  RESET_BALANCE: 1000      // free chips given if the player hits 0
};
```

- **Number of winners**: `WINNERS`.
- **Payout multiplier**: `PAYOUT`.
- **Starting balance / bet**: `STARTING_BALANCE`, `DEFAULT_BET`.

## Tech

Plain, self-contained `index.html` with vanilla JS and CSS. No backend, no
external accounts, no real money. All state is kept in memory. Mobile-responsive,
with an optional WebAudio sound toggle (no asset files).
