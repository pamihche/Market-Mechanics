# Market-Mechanics

Quantitative market-structure libraries and precision Pine Script v6 indicators for time-synchronized intermarket divergence, liquidity, and price delivery.

---

## Overview

**Market-Mechanics** is a modular TradingView ecosystem engineered in **Pine Script v6**. Rather than packing duplicate logic into standalone monolithic scripts, core price-action primitives — multi-asset synchronization, multi-timeframe candle aggregation, liquidity swings, imbalances, order blocks, and intermarket correlation — are isolated into reusable, strongly typed libraries orchestrated by a central execution engine.

Every component is built around **strict time alignment**: cross-asset and multi-timeframe structures are evaluated on synchronized bar timestamps across historical bars, live intrabar ticks (`varip`), and multi-asset Bar Replay.

---

## Architecture

```text
Market-Mechanics/
├── libraries/                  # Core Pine Script v6 Libraries (dependency order)
│   ├── HelperLibrary.pine      # Composite integer keys, lookback scopes & utilities
│   ├── AssetLibrary.pine       # Correlated triad / pair auto-detection & symbol resolution
│   ├── TimeFrameLibrary.pine   # Fractal timeframe hierarchy & calendar/intraday mapping
│   ├── CandleLibrary.pine      # OHLC state, LTF-to-HTF merging & bounded candle storage
│   ├── SwingLibrary.pine       # Structural swing detection, sweep/break state & extremes
│   ├── GapLibrary.pine         # FVG, Volume Imbalance (VI), NDOG & NWOG lifecycle + inversion
│   ├── OrderBlockLibrary.pine  # Order blocks, mitigation & breaker conversion
│   ├── SmtLibrary.pine         # Divergence candidates, ranking & diff-synced rendering
│   └── EngineLibrary.pine      # Multi-asset, multi-timeframe data & state orchestrator
│
├── indicators/                 # Production Indicators
│   └── Parallax.pine           # Multi-asset, fractal-timeframe intermarket divergence & PSP
│
├── sandbox/                    # Test harnesses & R&D prototypes
└── VERSIONS.md                 # Published TradingView library & indicator version registry
