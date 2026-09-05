# UnderCast — NFL Weather Totals

*Forecasting the under.*

A mobile-first web app that flags NFL point totals (over/unders) that look **inflated given the
weather** at the venue, and leans **UNDER** when the model disagrees with the sportsbook line.

Open `index.html` in a browser (or serve it) — it's a single, dependency-free file.

## Live data (no API key required)

When deployed to a real web host, UnderCast pulls live data from two free, key-free sources:

- **Schedule, venues & market totals** — [ESPN's public NFL scoreboard API](https://site.api.espn.com/apis/site/v2/sports/football/nfl/scoreboard)
  (real games, kickoff times, indoor/outdoor flags, live scores, current over/under).
- **Weather** — [Open-Meteo](https://open-meteo.com) forecast keyed to each stadium's coordinates,
  matched to the kickoff hour (temperature, sustained wind, gusts, precipitation).

**Optional exact book lines:** add a free [The Odds API](https://the-odds-api.com) key under
**⚙ Settings** to override the consensus total with the exact **FanDuel** or **DraftKings** number.
The key is stored only in your browser (`localStorage`), never hardcoded or transmitted anywhere
but the odds provider.

> Inside sandboxed previews (like the Claude Artifact) external calls are blocked, so the app falls
> back to a clearly-labeled **sample slate**. Deploy it to your own site (e.g. GitHub Pages) and it
> runs on live data automatically — same code, no config.

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
`computeEdge()` is a pure function — the same math runs on live and sample data.

## Features

- **Games tab** — slate sorted by weather impact, market vs. adjusted total, expandable breakdown.
- **Pre-game / Live** toggle — Live mode adds in-game score and a scoring-**pace** projection so you
  can watch a low-scoring weather game trend under in real time.
- **Locks & Parlays** — highest-conviction unders, plus 2- and 3-leg model parlays.
- **How It Works** — full methodology and data-source notes.
- **Settings** — optional odds-API key + preferred book.

## Deploy

Any static host works. For GitHub Pages: push, then enable Pages on the branch — the browser makes
the ESPN / Open-Meteo calls directly (both send permissive CORS headers).

## Disclaimer

For entertainment only. Not betting advice, not a guarantee. 21+. If gambling stops being fun,
call **1-800-GAMBLER**.
