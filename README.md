# 🎟️ Scratch 100 — 25¢ a Square

A single-page scratch-card game. The board has **100 squares**. Each square costs
**25¢** to scratch. Cash prizes are hidden around the board — you see what you got
instantly. The catch: the hidden prize pool is worth **less** than the cost of
scratching the whole board, so **clearing all 100 squares always loses money**.
The skill is knowing when to **stop while you're ahead**.

## How to run

No build step, no dependencies, no backend. Just open the file:

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or double-click `index.html`. Works on any static host too.

### Play it online

Live on GitHub Pages: <https://connolyc-design.github.io/mines-game/>

## How to play

1. Tap any of the 100 squares — each tap costs **25¢** (taken from your balance).
2. The square reveals instantly: a **cash prize** (gold/green) or a **blank** (·).
3. Keep scratching as long as you like. Watch **Net (this board)** — it's your
   winnings minus what you've spent so far on this board.
4. Hit **New Board** any time to reshuffle and start fresh (free — you only pay
   per scratch). Run out of money and you'll get free play chips.

## The prize allocation (the "house edge")

Cost to scratch every square: 100 × $0.25 = **$25.00**.

Hidden prize pool (editable in `CONFIG.PRIZES`):

| Prize | Count | Subtotal |
|------:|------:|---------:|
| $5.00 |   1   | $5.00 |
| $2.00 |   3   | $6.00 |
| $1.00 |   5   | $5.00 |
| 50¢   |   8   | $4.00 |
| 25¢   |   6   | $1.50 |
| **Total** | **23 winners / 100** | **$21.50** |

Clearing the entire board: win **$21.50**, spend **$25.00** → net **−$3.50**
(86% payout / ~14% house edge). A lucky player who finds the big prizes early
and stops can still finish ahead — that's the whole game.

## Where to tweak things

All gameplay constants live in the `CONFIG` block at the top of the `<script>` in
[`index.html`](./index.html):

```js
const CONFIG = {
  COLS: 10,
  ROWS: 10,                 // 10x10 = 100 squares
  CLICK_COST: 0.25,         // cost to scratch one square
  STARTING_BALANCE: 25.00,  // player's starting money
  RESET_BALANCE: 25.00,     // free money given when broke
  PRIZES: [                 // [prize amount, how many squares]
    [5.00, 1],
    [2.00, 3],
    [1.00, 5],
    [0.50, 8],
    [0.25, 6]
  ]
};
```

- **Cost per scratch**: `CLICK_COST`.
- **Prize allocation**: `PRIZES` — keep the total below `100 × CLICK_COST` to keep
  the "clearing the board loses" rule. The legend in-game and the math update
  automatically from this table.
- **Starting / reset balance**: `STARTING_BALANCE`, `RESET_BALANCE`.

## Tech

Plain, self-contained `index.html` with vanilla JS and CSS. No backend, no
external accounts, no real money. All state in memory. Mobile-responsive, with an
optional WebAudio sound toggle (no asset files).
