# Market-Mechanics

Quantitative market-structure libraries and precision Pine Script v6 indicators
for time-synchronized intermarket divergence, liquidity, and price delivery.

---

## Overview

**Market-Mechanics** is a modular TradingView suite engineered in
**Pine Script v6**.

Instead of duplicating logic inside monolithic indicators, every core
price-action primitive is isolated into its own strongly typed library and
orchestrated by a central multi-asset engine:

- **Strict Time Alignment** — Cross-asset and multi-timeframe structures are
  evaluated on exact synchronized bar timestamps.
- **Live & Replay Safe** — State management is built with `varip` persistence
  to behave consistently across historical bars, live intrabar ticks, session
  boundaries, and multi-asset Bar Replay.
- **Modular Primitives** — Asset pairing, fractal timeframe mapping, candle
  aggregation, liquidity swings, imbalances (`FVG` / `VI` / `NDOG` / `NWOG`),
  order blocks, and intermarket divergence each live in dedicated libraries.
- **Diff-Synced Rendering** — Chart drawings update in place rather than
  deleting and recreating objects on every tick.

---

## Architecture

```text
Market-Mechanics/
├── libraries/                  # Core Pine Script v6 Libraries (dependency order)
│   ├── HelperLibrary.pine      # Composite keys, lookback scopes & utilities
│   ├── AssetLibrary.pine       # Triad / pair auto-detection & symbol resolution
│   ├── TimeFrameLibrary.pine   # Fractal timeframe hierarchy & calendar mapping
│   ├── CandleLibrary.pine      # OHLC state, LTF-to-HTF merging & storage
│   ├── SwingLibrary.pine       # Structural swings, sweep/break state & extremes
│   ├── GapLibrary.pine         # FVG, VI, NDOG & NWOG lifecycle + inversion
│   ├── OrderBlockLibrary.pine  # Order blocks, mitigation & breaker conversion
│   ├── SmtLibrary.pine         # Divergence candidates, ranking & rendering
│   └── EngineLibrary.pine      # Multi-asset, multi-timeframe orchestrator
│
├── indicators/                 # Production Indicators
│   └── Parallax.pine           # Intermarket divergence & PSP indicator
│
├── sandbox/                    # Test harnesses & R&D prototypes
└── VERSIONS.md                 # Published TradingView version registry
```

---

## Core Modules

### Libraries (`libraries/`)

- **`HelperLibrary`**
  Provides zero-dependency foundational utilities, including collision-free
  composite integer key encoding for multi-dimensional maps and rolling
  calendar/bar lookback window guards (`1D` through `6M`) to scope historical
  processing cleanly.

- **`AssetLibrary`**
  Handles instrument identification and automatic pairing across index futures,
  micro futures, energies, metals, crypto, and FX. Supports both 3-asset triads
  and 2-asset pairs alongside custom ticker overrides and symbol normalization.

- **`TimeFrameLibrary`**
  Models the institutional timeframe hierarchy (`1m` through `3M`), converting
  between Pine timeframe strings, seconds, and human-readable labels while
  resolving valid higher-timeframe monitoring chains for any active chart.

- **`CandleLibrary`**
  Defines `varip`-safe OHLC candle primitives, bounded chronological storage
  holders, and real-time lower-to-higher timeframe candle merging across open
  sessions.

- **`SwingLibrary`**
  Detects 3-bar structural swing highs and lows, distinguishes liquidity wick
  sweeps from candle-body breaks with exact bar/timestamp attribution, and
  tracks monotonic structural extremes and active dealing ranges (`BSL` / `SSL`
  / `EQ`).

- **`GapLibrary`**
  Manages price-delivery imbalances across their full lifecycle: Fair Value
  Gaps (`FVG`), standalone Volume Imbalances (`VI`), New Day Opening Gaps
  (`NDOG`), and New Week Opening Gaps (`NWOG`), including partial fills, full
  mitigation, inversion (`IFVG`), and diff-synced rendering.

- **`OrderBlockLibrary`**
  Identifies bullish and bearish Order Blocks anchored to structural
  displacement, tracking touch/mitigation state, body invalidation, automatic
  conversion into Breaker Blocks, and dynamic right-edge chart rendering.

- **`SmtLibrary`**
  Provides the data structures, candidate storage, label placement geometry,
  and zero-flicker diff-synced drawing holders for Swing, Gap-Fill, Session,
  and Sequential (`SSMT`) intermarket divergences plus Precision Swing Point
  (`PSP`) markers.

- **`EngineLibrary`**
  The central multi-asset, multi-timeframe orchestrator. Coordinates scoped
  `request.security` feeds across up to 3 correlated assets, maintains
  synchronized runtime snapshots per timeframe, drives real-time candle/swing/
  gap/divergence state machines, and ranks active non-crossing divergences for
  display.

### Indicators (`indicators/`)

- **`Parallax`**
  Flagship intermarket divergence and Precision Swing Point (`PSP`) indicator
  powered by `EngineLibrary`. Reveals structural cracks in correlation across
  paired and triad instruments across aligned chart and higher fractal
  timeframes.

---

## Versioning & Workflow

- **`main`** always reflects verified code live on TradingView.
- Every TradingView publish is logged in `VERSIONS.md` and tagged in Git
  (e.g., `EngineLibrary-v1`, `Parallax-v1.0`) so any live TradingView build
  can be reproduced at any time.
