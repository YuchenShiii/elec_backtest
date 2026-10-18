# China Power Sector Drawdown Strategy & Backtest Lab

**Live demo:** [English](https://yuchenshiii.github.io/elec_backtest/en.html) · [中文](https://yuchenshiii.github.io/elec_backtest/)

A research dashboard and in-browser backtesting tool for China's listed power sector (thermal, hydro, nuclear). It asks one question: **when a power stock falls sharply relative to the market, is it a mispricing that will recover, or a value trap?**

## What's inside

**① Research framework**

- **Policy analysis**: how the 13th–15th Five-Year Plans, contract coal price caps and tariff reform turned thermal power from a cyclical stock into a dividend stock, and what that implies for factor selection.
- **A/B attribution framework**: every sector drawdown is classified as *Type B (mispriced)*, *Type A cyclical* or *Type A permanent (value trap)* using fundamentals: coal prices, tariffs, utilisation hours, demand, renewables squeeze and hydro inflows.
- **4-layer screening funnels** for thermal and hydro: timing → valuation → risk removal → final selection.
- **Factor log**: every timing and cross-sectional factor tested, with means *and* medians reported side by side.

**② Backtest Lab**

- **Data check (human in the loop)**: row counts, date ranges, abnormal-move warnings and column previews for 13 input files, which must be confirmed before a backtest can run.
- **19 candidate signals** across valuation, price, relative strength, flows and cross-asset groups, each with adjustable parameters.
- **Two tests**:
  - Quintile forward-return **IC test** (Spearman rank IC)
  - Dual-threshold **stepwise-position backtest**, with NAV vs. buy & hold vs. CSI 300, CAGR, max drawdown, Sharpe, Calmar and a trade log
- **Regime split** around 24 Sep 2024 to check whether a signal holds across market regimes.
- Interactive charts with hover details, built in plain SVG.

## Key findings (in-sample)

| Finding | Result |
|---|---|
| Pure thermal power does not recover after drawdowns | t+60 excess vs. CSI 300: **+0.14%** across 7 events |
| Thermal stock selection: means are flattering, medians are weak | Best factor (high FCF): mean +6.2%, median **+1.8%** |
| Hydro depends on water inflow | Wet-year drops recovered; the 2025 dry-year drop lost **−13.8%** |
| Strongest result: Yangtze Power drawdown signal | **+5.2%** t+60 excess, 83% hit rate, **p = 0.03**, median = mean |

The research's main value is diagnostic: separating mispricings from value traps, and showing where "buy the dip" fails. It did not find a strong standalone alpha. All results are in-sample and have not yet been checked with walk-forward or hold-out testing.

## Methodology safeguards (no look-ahead bias)

1. Signals use T-1 data; trades fill at the T+1 open.
2. Rolling percentiles use only past data, never full-sample percentiles.
3. IC forward returns use `shift(-N)`; the last N days are dropped automatically.
4. Financial data is lagged to its announcement date.
5. One IC variable basis (percentile or raw value) is applied to all signals, so signals can be compared directly.

## Running it

A single `index.html` / `en.html` with no dependencies or build step. Open either file in a browser, or use the live demo links above.

The public demo ships with **synthetic sample data** (clearly labelled MOCK) so the workflow and charts can be explored. The findings above come from the original research on real market data. To run on real data, drop CSVs with the names and columns below into the Data Check page. All computation runs locally in the browser; nothing is uploaded.

| Files | Columns |
|---|---|
| `600900.csv` `601985.csv` `003816.csv` `600011.csv` `601088.csv` (stocks) | `date,open,close,dv_ttm,pb,pe,amount,turnover` (adjusted prices) |
| `801161.SWI.csv` `H30199.CSI.csv` `000688.SH.csv` `H30184.CSI.csv` `000922.SH.csv` `000300.SH.csv` (indices) | `date,close` |
| `国债10Y.csv` (10-year government bond yield) | `date,yield` |
| `两市总成交额.csv` (total A-share market turnover) | `date,total_amount` |

Dates must be ascending, UTF-8, with units stripped from numbers and missing values left empty.

## Notes

- Forward-return windows overlap, so p-values are optimistic and shown for reference only.
- Signals 4 and 14 are off by default and signals 17 and 19 are retired; these four are not implemented.

**Tech:** HTML, CSS and vanilla JavaScript; hand-built SVG charts; statistics (Spearman IC, quintile analysis, drawdown and Sharpe metrics) implemented from scratch.
