# Glossary — plain English to standard terms

Market Axiom uses plain names so a beginner can use it without a finance degree. Underneath, most of
them are standard techniques with standard names.

This page maps one to the other. If you already know the vocabulary, this tells you what the controls
actually are. If you don't, this is a decent way to learn it.

**Where something is proprietary rather than standard, it says so.** No point pretending an
in-house score is an industry measure.

---

## Strategy settings

**Signal Quality** → *entry threshold*
How high the composite score has to go before a trade triggers. `Aggressive` uses the base threshold.
`Moderate` raises it and adds a longer-term trend filter to trend-following entries, so those are
only taken in the direction of the larger trend. Mean-reversion and chop-regime entries bypass that
filter.

**Trade Direction** → *long / short bias*
Standard. `Long Only` is the most thoroughly tested.

**Trading Style** → *holding-period profile*
A single control over the whole timing regime. Each style sets its own lookbacks, cooldowns, exit
patience and detection thresholds — internally it moves the ADX, RSI, Bollinger Band, MACD and SMA
parameters together as a matched set. Faster styles hold minutes to hours; slower styles hold days
to weeks.

**How to Close Trades** →
- `Follow the Trend` = **trailing stop exit.** No fixed target; the stop advances behind price and
  the trade runs until it's hit.
- `Set Target & Stop` = **bracket exit.** Both levels calculated at entry, so risk and reward are
  known before the trade opens — a fixed R-multiple.

**Close Trades Before Market Close** → *EOD flatten*
Standard intraday practice. Applies to the Day Trading style; the Swing and Position styles hold
overnight by design.

**Strategy Type** → *strategy family selection*
`Momentum`, `Trend Follow` and `Reversion` are the three standard families. `Auto` lets the engine
choose per ticker — that routing is the proprietary part.

---

## Risk and sizing

**Account Balance** → *notional account equity*
The capital base every size is computed from. Not read from your broker — you type it, and each
chart uses its own figure.

**Risk Per Trade (%)** → *fractional risk per position*
Standard. The percentage of the account intended to be lost if the stop is hit.

**How to Size Trades** →
- `Same % Every Trade` = **fixed-fractional sizing.** Size comes from a constant share of the sizing
  budget.
- `Smart Sizing` = **volatility-adjusted sizing.** Size scales with the instrument's volatility so
  the money at stake stays comparable across different tickers.

**Stop Width** → *ATR multiplier*
Stop distance measured in Average True Range units rather than a fixed percentage or dollar amount,
so the stop widens on volatile instruments and tightens on calm ones. The value multiplies against a
per-ticker calibrated base rather than being the raw multiple.

**Add to Winning Trades (%)** → *pyramiding / scaling in*
Standard. Adds to a position that is already profitable. Ships off, because that is the configuration
the performance work was done on.

**Capital Buffer (%)** → *execution reserve*
A ceiling that holds back part of the sizing budget to absorb slippage, partial fills and drift
between what the chart believes and what the broker filled. Binds only on the largest entries.

**Auto Stop-Loss for Bad Days** → *daily loss limit / circuit breaker*
Standard risk control. Halts new trades for the rest of the session once the day's loss passes a
threshold. The threshold is dynamic — derived from the instrument's own volatility rather than a
fixed percentage — which is the proprietary part.

**Conviction multiplier** *(not a setting — internal)* → *signal-weighted position sizing*
Both sizing modes scale the final size between **0.25× and 2.0×** based on signal strength, ADX,
volatility, time of session, current drawdown and market regime. This is why share counts vary even
on `Same % Every Trade`, and it is the single most common "is this a bug?" question. It isn't.

---

## Automation

**Broker Platform** → *order routing / signal destination*
`Alerts Only` sends nothing anywhere. `TradersPost` formats for that service. `Standard (Custom
JSON)` emits the native payload for a self-hosted receiver.

**Signal Speed** → *exit evaluation granularity*
`Wait for Bar Close` = **bar-close evaluation**; stops and targets are checked only when a candle
completes. `Quick Exits` = **intrabar evaluation**; they're checked as prices arrive. Entries are
always bar-close gated regardless — that is what prevents repainting.

**Webhook Secret Key** → *shared authentication phrase*
Included in the payload on the Standard route so your own server can verify the request came from
your indicator. TradersPost payloads never carry it.

**Alert Tag** → *strategy identifier / routing label*
Passed through on the Standard route only, for distinguishing lanes inside a multi-strategy receiver.

**Trade Extended Hours** → *ETH session participation*
Pre-market and after-hours. Note that outside regular hours orders become limit orders and fills are
unreliable — this is off by default and outside the tested configuration.

---

## Dashboard

**Est. Annual Rtn** → *annualised return*
The backtest result scaled to a year. A comparison tool between styles and tickers, not a forecast.

**Profit Factor** → *gross profit ÷ gross loss*
Standard. Above 1.0 is profitable; above 1.5 is generally considered good; above 2.0 strong.

**Avg Trade** → *per-trade expectancy*
Standard. The average return per trade, wins and losses combined.

**Max Drawdown** → *maximum peak-to-trough decline*
Standard. The deepest fall from a high-water mark.

**Reward/Risk** → *Calmar ratio*
Annualised return divided by maximum drawdown. Standard, though usually computed over 36 months
rather than a backtest window.

**Signals / Day** → *trade frequency*
Standard.

**Exp. Range** → *projected daily range*
An estimate of today's high and low from recent volatility.

**Stock Today / Stock Week** → *instrument return*
The **ticker's** move, not the strategy's. Included for context.

**Target Price** → *analyst consensus target*
Sourced from TradingView, not computed by the engine.

**Stock Fit** → *instrument suitability score* — **proprietary**
How well the ticker's measured behaviour suits the engine, A to F. There is no industry equivalent.

**Bot Score** → *composite performance grade* — **proprietary**
An S/A/B/C/D roll-up of return, risk and consistency for the current ticker and settings.

---

## Exit reasons

| On the chart | Standard name |
|---|---|
| 🏆 Target | take-profit hit |
| 🛡️ Stopped | stop loss or trailing stop hit |
| 📌 Changed | regime or thesis exit — the engine's read on the ticker changed |
| ⏳ Too Slow | time-based exit (time stop) |
| ⚠️ Cut Early | early exit on deteriorating conditions |
| 💤 Dead Trade | time-based exit on a non-progressing position |
| ⏱ EOD Close | end-of-day flatten |

---

## Engine internals

**Tri-Core architecture** → *strategy family routing*
Three engines — Momentum, Trend Following, Mean Reversion — selected per ticker rather than blended.
The families are standard; the routing is proprietary.

**Auto-Personality Engine** → *instrument characterisation* — **proprietary**
Scores every ticker on trend strength, volatility profile and liquidity signature, and derives the
thresholds, stop distances and exit behaviour from those scores.

**Regime Detection** → *market state classification*
Trending versus choppy. The concept is standard; the classifier is proprietary.

**Composite Signal Scoring** → *multi-factor signal aggregation*
Momentum, trend, volume and mean-reversion inputs combined into one score that must cross a
threshold, rather than any single indicator triggering a trade.
