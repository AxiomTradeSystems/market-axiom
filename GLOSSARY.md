# Glossary

Market Axiom uses plain names so anyone can use it. Most of them stand for standard trading
techniques. This page maps each name to its standard term and says what it does.

Where something is Market Axiom's own design rather than an industry standard, it says
**proprietary**.

Every setting also has full help inside the indicator. Hover the ⓘ beside it.

---

## Strategy

**Signal Quality** → *entry threshold*
How strong a setup must be before the bot enters. `Aggressive` (default) takes more trades.
`Moderate` takes fewer, and takes trend trades only in the direction of the longer-term trend.

**Trade Direction** → *long / short bias*
`Long Only` (default, most tested), `Short Only` or `Long & Short`.

**Trading Style** → *holding-period profile*
One control for the whole timing profile: how fast the bot reads the market, how long it gives a
trade, and how long it waits between trades. Seven steps from `Day Trading` to `Slow Position`.
`Swing` is the default.

**How to Close Trades** → *exit method*
- `Follow the Trend` (default) = **trailing stop exit.** No fixed target. The stop moves up behind
  the price and the trade runs until the stop is hit.
- `Set Target & Stop` = **bracket exit.** A target at four times the stop distance, plus a stop.
  Both are set at entry and do not move.

**Close Trades Before Market Close** → *end-of-day flatten*
Day Trading style only. On a 15-minute chart it closes the trade in the last 30 minutes of the
session and opens no new trades then.

**Strategy Type** → *strategy family*
`Momentum`, `Trend Follow` and `Reversion` are standard families. `Auto` (default) lets the engine
pick one for each ticker. The picking is **proprietary**.

---

## Risk and sizing

**Account Balance** → *account equity for sizing*
The money the bot sizes from. You type it in; the indicator does not read your broker. Its meaning
depends on Share of Account (below). Minimum $500.

**Share of Account (%)** → *percent-of-equity sizing*
`0` (default) is off: Account Balance is this chart's money, and the bot sends your broker a share
count. From `1` to `100` it is on: Account Balance is your whole account, this chart's part is this
percent, and the bot sends your broker a percent that it turns into shares from your real balance.

**How to Size Trades** → *sizing method*
- `Same % Every Trade` (default) = **fixed-fractional sizing.** Each trade starts from the chart's
  full budget. Weaker setups are scaled down, to as little as a quarter.
- `Smart Sizing` = **risk-based sizing.** Each trade risks a set percent of the budget, scaled by
  setup quality from 0.25× to 2×.

**Risk Per Trade (%)** → *fractional risk per position*
The base percent risked when a stop is hit. Used by Smart Sizing only (LITE always uses Smart
Sizing). Default 1%, range 0.25% to 5%.

**Stop Width** → *ATR multiplier*
Stop distance measured in Average True Range (ATR), the instrument's typical price movement. So the
stop is wider on volatile tickers and tighter on calm ones. It scales a base that is calibrated per
ticker. It also scales the trailing stop on Trend Follow trades. Default 1.0, range 0.9 to 2.5.

**Add to Winning Trades (%)** → *pyramiding*
Adds to a trade that is already in profit. Default 0% (off). Not used on the TradersPost route.

**Capital Buffer (%)** → *execution reserve*
A small part of the chart's money held back to absorb fill differences. Default 0.25%.

**Auto Stop-Loss for Bad Days** → *daily loss limit*
If the day's loss, including an open trade, passes a limit, the bot closes the position and takes
no new trades until the next session. The limit is set from the ticker's own volatility, between
2% and 15% of the day's starting equity. The method is **proprietary**.

**Conviction scaling** *(automatic, not a setting)* → *signal-weighted position sizing*
Both sizing methods scale each trade by setup quality and market conditions. This is why share
counts change from trade to trade. The method is **proprietary**.

---

## Automation

**Broker Platform** → *order routing*
`Alerts Only (no broker orders)` (default) sends no orders. `TradersPost` sends orders through TradersPost.
`Standard (Custom JSON)` sends orders to your self-hosted receiver.

**Signal Speed** → *exit timing*
`Wait for Bar Close` (default) = **bar-close evaluation**: exits are checked when each candle
finishes. `Quick Exits` = **intrabar evaluation**: exits are checked as prices arrive. Entries
always wait for the candle to finish. Signal Speed applies on the TradersPost and self-hosted
routes. On Alerts Only, exits always wait for the bar close.

**Webhook Secret Key** → *shared secret*
A password the indicator puts in each message to your self-hosted receiver, so the receiver knows
the message is yours. TradersPost does not use it.

**Alert Tag** → *strategy label*
An optional label sent with each message on the self-hosted route.

**Custom Ticker Symbol** → *symbol override*
Sends a different symbol name to your broker. Leave it blank unless your broker uses a different
name for the symbol than TradingView does.

**Trade Extended Hours** → *extended-hours trading*
Trades pre-market and after-hours on stocks and ETFs. The chart must also show extended hours.
Not used on the TradersPost route.

**armed** → *automation on*
Shown in the status line when Broker Platform is set to send orders.

---

## Dashboard

**Est. Annual Rtn** → *annualized return*
The backtest result scaled to one year. Use it to compare styles and tickers.

**Profit Factor** → *gross profit ÷ gross loss*
Above 1.0 is profitable. Above 1.5 is good. Above 2.0 is strong.

**Avg Trade** → *expectancy per trade*
The average return per trade, wins and losses together.

**Max Drawdown** → *largest peak-to-trough decline*
The deepest fall from a high point.

**Reward/Risk** → *Calmar ratio*
Annualized return divided by Max Drawdown.

**Signals / Day** → *signal frequency*
Entry and exit signals per day.

**Exp. Range** → *average true range (daily)*
The stock's typical full-day move, in dollars.

**Stock Today / Stock Week** → *instrument return*
The ticker's own move, not the bot's.

**Target Price** → *analyst consensus target*
From TradingView's data, not from the engine.

**Stock Fit** → *activity grade* — **proprietary**
How busy and lively the stock is, from A to F: how much money trades in it and how far it moves in
a day. It is not a forecast. Judge results by Bot Score.

**Bot Score** → *composite performance grade* — **proprietary**
How the bot has actually done on this ticker, from S to D: mostly yearly return, then how much it
beats buying and holding, its profit-to-loss ratio and its worst dip. High grades need enough
evidence, and it needs 30 days of history.

---

## Exit reasons

| On the chart | Standard term | Meaning |
|---|---|---|
| 🛡️ Stopped | stop loss / trailing stop | The stop was hit |
| 🏆 Target | take profit | The target was hit (Set Target & Stop) |
| 📌 Changed | thesis exit | The engine's read on the ticker changed |
| ⏳ Too Slow | time stop | The trade did not progress in time |
| ⚠️ Cut Early | early exit | A stalled trade at or below entry was closed |
| 💤 Dead Trade | time stop | The price stopped moving |
| 🎯 Stretch Take | profit take into strength | The price stretched far from normal while in profit (Follow the Trend) |
| ⛔ Halt Triggered | daily loss limit | Auto Stop-Loss for Bad Days closed the trade |
| ⏱ EOD Close | end-of-day flatten | Day Trading closed before the market close |
| 🔴 Closed | other | Closed for another reason |

---

## The engine

**Tri-Core** → *strategy family routing*
Three engines (Momentum, Trend Following, Mean Reversion). Each ticker is routed to one of them,
not a blend. The families are standard. The routing is **proprietary**.

**Auto-Personality Engine** → *instrument profiling* — **proprietary**
Profiles each ticker and adapts the bot to it.

**Regime detection** → *market state classification*
Trending or choppy. The idea is standard. The method is **proprietary**.

**Composite signal score** → *multi-factor signal*
Several kinds of evidence are combined into one score, and a trade needs that score to pass a
threshold. No single indicator triggers a trade.
