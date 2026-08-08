# Weirdos — blind box collecting, after dark

A single self-contained HTML file (`blindbox-simulator.html`) that runs a whole
little blind-box collecting game: no build step, no server — just open it in
a browser.

## What it is

Tap a mystery box to "open" it and reveal a **Weirdo**: a sausage-shaped
little guy with a costume, hat/ears, and a face, randomly generated as an
inline SVG (no image assets — every figure is drawn by code).

- **8 series** (Family, Animal, Job, Party, Global, Bakery, Pop Star, School),
  8 figures each, 64 figures total.
- Each series has 5 **common**, 2 **rare**, and 1 **golden secret** figure.
  Odds per box: 78% common / 17% rare / 5% secret.
- Three tabs: **Open Boxes**, **My Collection** (per-series grid with
  owned/locked slots and duplicate counts), and **Stats** (totals, per-series
  progress bars, a golden-secrets shelf, a rares shelf).
- Your collection persists automatically — see **Storage** below.

## Gold coins (currency)

Boxes now cost **gold coins** to open — a coin icon with a tiny sausage face
pressed into the middle, shown top-right at all times.

- You start with **500 coins**.
- Each box costs **25 coins** to open (`BOX_COST` in the script).
- The balance is visible in the badge in the corner and as a price tag on
  every box on the shelf.
- If you don't have enough, clicking a box just shakes it and shows a "not
  enough gold coins" message instead of opening it — boxes you can't afford
  also dim slightly so it's obvious before you tap.
- "Reset collection" also resets your coin balance back to 500.

This is intentionally a minimal first pass — spend-only, no earning yet —
since the ask was specifically "pay to open blind boxes, and later for
shop." The economy is a fixed-size sink for now; see **Ideas for later**
below for how it'd naturally extend into a shop.

### Tuning it
Both numbers live right next to each other near the top of the `<script>`
block:

```js
var STARTING_COINS = 500;
var BOX_COST = 25;
```

Change either and reload — no other code needs to change.

## Storage

The game tries three backends, in order, and quietly falls back:

1. `window.storage` — used automatically when this file is embedded/opened
   inside a Claude.ai artifact.
2. `localStorage` — used when opened normally in a browser (e.g.
   double-clicked, or opened via `open blindbox-simulator.html`).
3. An in-memory object — last resort if even `localStorage` is blocked
   (some `file://` contexts). In that case progress won't survive a reload.

Your figure collection and your coin balance are stored under separate keys
(`weirdos-collection-v1` and `weirdos-coins-v1`) so the coin system could
change shape later without touching saved collections.

## Ideas for later (the "shop")

Not built yet, but the currency is set up to lead into it:

- A shop tab to spend coins on things like: a specific series' odds boost,
  cosmetic reskins, or buying a *specific* figure outright.
- A way to earn more coins — e.g. "melt down" a duplicate figure for a
  partial coin refund, a daily login bonus, or completing a series.
- Bundle discounts (e.g. open 5 boxes for less than 5× the price).

## Running it

No install needed:

```
open blindbox-simulator.html
```

or just double-click the file in Finder.
