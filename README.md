# grid_strike strategy builder

A single-file, offline-capable dashboard for designing and pressure-testing a **Hyperliquid grid trading strategy** — with a live BTC chart, draggable grid range, per-cycle economics, risk projection, and one-click config export for [Hummingbot](https://hummingbot.org)'s `grid_strike` controller.

**▶ Live:** https://learn57130.github.io/grid-dashboard/
**📖 Guide:** https://learn57130.github.io/grid-dashboard/guide.html

---

## What it does

Two pages, cross-linked:

- **Builder** (`index.html`) — the interactive tool.
- **Guide** (`guide.html`) — a plain-English reference for every parameter (what it is + how to tune it).

### Builder features

- **Live BTC/USDC chart** (Hyperliquid data) — candlesticks, volume, zoom/pan, 1m–1D timeframes, lazy-loaded history.
- **Draggable grid** — drag the `start` / `end` / `limit` lines to resize, or drag the band to shift the whole range. Config updates live.
- **Per-cycle economics** — net/cycle, gross, fee + funding cost, break-even TP, level spacing, margin use, with live validation flags.
- **`take_profit` ⛓ linked to spacing** by default (a coherent grid); unlink for fixed-TP "rung scalping".
- **Risk views** — a trend-down **drawdown** worst-case and a **cumulative-PnL projection**, both hoverable.
- **Live account** — paste your **public** wallet address (with or without the `HL:` prefix) for a read-only balance lookup (perp + spot).
- **Config export** — generates the `grid_strike` YAML; copy or download for `conf/controllers/`.
- **Per-parameter help** — hover the `?` next to any field.

---

## How it works

- **100% client-side.** No backend, no build step, no accounts. All market and balance data is fetched directly from Hyperliquid's public `info` API from your browser (CORS-open, HTTPS).
- **Single self-contained file.** `lightweight-charts` is vendored inline, so the tool loads and runs even offline (only the *live data* needs a connection). No CDN dependency.
- **Nothing is stored or sent anywhere** except the read-only market/balance requests to Hyperliquid.

## Safety

- The tool **only ever uses your public wallet address** for a read-only balance lookup. It never asks for, handles, or transmits a private key.
- The **"deploy strategy"** button is a UI state toggle (draft ↔ live) — it does **not** place any orders. Actual deployment happens in Hummingbot.
- When you do go live: use a Hyperliquid **`api_wallet`** key (trade-only, cannot withdraw), keep **leverage at 1×**, and remember BTC trades on the **perp** (transfer USDC spot → perp first).

> This is a planning/education tool, not financial advice. Grid trading can lose money — size accordingly.

## Run locally

It's one static file. Either open it directly:

```bash
open index.html          # macOS  (or just double-click)
```

or serve it:

```bash
python3 -m http.server 8000    # then visit http://localhost:8000
```

## Deploying the actual bot

The exported `.yml` is a [Hummingbot](https://hummingbot.org) V2 `grid_strike` controller config. Drop it in `conf/controllers/`, connect `hyperliquid_perpetual`, and start. See the [guide](https://learn57130.github.io/grid-dashboard/guide.html) for the parameter details.

---

Charting by [TradingView Lightweight Charts](https://github.com/tradingview/lightweight-charts) (Apache-2.0). Not affiliated with Hyperliquid or Hummingbot.
