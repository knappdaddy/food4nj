# WindEdge — NFL Weather Totals

A mobile-first web app that flags NFL point totals (over/unders) that look **inflated given the
weather** at the venue, and leans **UNDER** when the model disagrees with the sportsbook line.

Open `index.html` in a browser (or serve it) — it's a single, dependency-free file.

## The model

For every game:

```
Adjusted Total = Market Total − Weather Penalty
```

The weather penalty is the sum of four transparent components (indoor / roof-closed venues = 0):

| Factor | Rule |
| --- | --- |
| **Wind** | 0 below 10 mph · 0.18 pt/mph from 10–20 · +0.32 pt/mph above 20 |
| **Precip** | rain 0.6 / 1.6 / 2.6 · snow 1.4 / 2.4 / 3.6 (light / moderate / heavy) |
| **Cold** | 0.6 (20–31°F) · 1.1 (10–19°F) · 1.7 (<10°F) |
| **Heat** | 0.5 (85–92°F) · 1.0 (>92°F) |

A penalty ≥ 1.5 pts triggers an **UNDER** lean. Every number is shown in each game's "Why" panel.

## Features

- **Games tab** — slate sorted by weather impact, market vs. adjusted total, expandable breakdown.
- **Pre-game / Live** toggle — same model against pre-game or in-game lines.
- **Locks & Parlays** — highest-conviction unders, plus 2- and 3-leg model parlays.
- **How It Works** — full methodology and data-source notes.

## Going live

Ships with a realistic sample slate. To run on real games, feed two APIs into the `GAMES` array
in `index.html`:

- **Lines** — an odds API (e.g. The Odds API) for FanDuel / DraftKings pre-game and live totals.
- **Weather** — a forecast API (OpenWeather, Tomorrow.io, NWS) keyed to each venue + kickoff.

`computeEdge()` is a pure function — swap the data source and the rest works unchanged.

## Disclaimer

For entertainment only. Not betting advice, not a guarantee. 21+. If gambling stops being fun,
call **1-800-GAMBLER**.
