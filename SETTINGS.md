# Settings reference

The full help for the settings whose in-chart help is short. Since v1.0.117 the help beside each setting (the ⓘ) is
short enough for any screen, and the long explanations live here. If this page and the in-chart help ever disagree,
the in-chart help matches the version you run.

Contents: [Broker Platform](#broker-platform) · [Signal Speed](#signal-speed) · [Webhook Secret Key](#webhook-secret-key) · [Custom Ticker Symbol](#custom-ticker-symbol) · [Trade Direction](#trade-direction) · [How to Close Trades](#how-to-close-trades) · [Account Balance ($)](#account-balance-) · [Share of Account (%)](#share-of-account-) · [How to Size Trades](#how-to-size-trades) · [Stop Width](#stop-width) · [Strategy Report](#strategy-report)

---

## Broker Platform

What the bot does with its signals. This is the main control for automation. There is no other on/off switch.

⚠️ SET THIS BEFORE YOU CREATE THE ALERT. An alert is a frozen copy of the script, its settings and its chart. Changes you make later do not reach it.

⚠️ Run on a PAPER account for at least two weeks before you use real money.

⚠️ Do not delete a live alert while a trade is open. Your broker position can lose its protection. Close the position at your broker first.

REQUIRED TRADERSPOST STRATEGY SETTINGS (the bot cannot set them for you): Enable trade locking ON · Cancel open order wait time 120 seconds · allow adding to positions OFF · short selling ON only if you trade shorts · cancel open orders on exit ON · stop order time in force GTC · failure notifications ON (the chart cannot see a rejected order) · Allowed IP addresses set to TradingView (then a leaked webhook address is useless).

THE THREE ROUTES.

ALERTS ONLY (the default): the bot draws on the chart and sends normal TradingView alerts. It sends NO orders. Start here.

TRADERSPOST: a separate service that connects TradingView alerts to brokers such as Alpaca and Tradier. You do not write code. Most beginners should pick this.

STANDARD (Custom JSON): for advanced users who run their own server that reads the bot's own order format. Pick this only if you built that server yourself. If you are not sure, pick TradersPost.

SETUP, IN ORDER: (1) Pick TradersPost or Standard here. (2) In that platform, create a webhook and connect it to your broker. (3) Copy the webhook address. (4) If your platform needs a Webhook Secret Key, type it in the field below. Only the Standard route sends it. Do not put it in the alert. (5) Create a TradingView alert: condition = this indicator, 'Any alert() function call'. Paste the webhook address into the Webhook URL box. The Message box only sets the alert's name. It does not change what is sent.

⚠️ The two trading routes use different order formats. If you pick the wrong one, your broker cannot read the orders and no trades happen.

TRADERSPOST LIMITS: your broker holds only the ENTRY stop. The trailing stop, the halt and stop updates stay on the chart. Adding to trades and extended-hours entries are off. Exits are sent only in regular hours. An exit that triggers outside them waits for the next regular-hours bar, and some early exits are dropped if they no longer apply then.

⚠️ EACH TIME THE ALERT STARTS, AND EACH TIME TRADINGVIEW RESTARTS IT, the bot sends one market 'exit' for this ticker, also when no trade is open, and one more about 2.5 minutes later. It waits for regular hours. This exit closes EVERY share of this ticker in the account and cancels EVERY open order for it, also shares and orders that you placed by hand. Do not trade this ticker by hand in the same account. If the account holds no shares, TradersPost shows the exit as rejected. That is normal. No new entry is sent for 5 minutes after the first exit.

---

## Signal Speed

How quickly the bot reacts when a trade needs to CLOSE. Entries always wait for the candle to finish — that never changes, and it is what stops signals from flickering mid-candle. What this setting changes is EXITS: stop-losses, take-profits and early exits. 'Wait for Bar Close' checks them only when a candle finishes — slower to react, and the order leaves at the end of that candle. The candle's high and low still count, so a wick that touches your stop and recovers before the close still closes the trade. 'Quick Exits' checks them as prices arrive, so the first price that touches your stop closes the trade — faster protection, but it needs a stable internet connection and it can exit on a brief spike.

⚠️ This setting only takes effect once Broker Platform is set to something other than Alerts Only. On Alerts Only the bot always behaves as 'Wait for Bar Close', and the statistics shown on the chart are bar-close based no matter what is selected here.

---

## Webhook Secret Key

A password that proves an order really came from you.

WHEN YOU NEED IT: only if the service receiving your orders requires one. If you use TradersPost, TradersPost handles security its own way and you can usually LEAVE THIS BLANK (follow TradersPost's own webhook instructions). If you run your own Standard server that you set up to require a secret, type that exact same secret here so the two match.

HOW IT WORKS: on the STANDARD route whatever you type here is included with every order; TradersPost payloads never carry it, so filling this in adds no protection on that route; your server checks it and rejects anything that doesn't match — this stops a stranger who finds your webhook address from placing orders.

⚠️ If your server REQUIRES a secret and this box is blank (or the two don't match exactly), every order is silently rejected — you'll see NO trades and may not realize why. Check your broker; if nothing is filling, this mismatch is the first thing to verify.

⚠️ This is shown in plain text on your screen — don't screen-share or screenshot while it's filled in.

---

## Custom Ticker Symbol

Overrides the symbol name sent to your broker. LEAVE THIS BLANK unless you have a specific reason — by default the bot sends the chart's own symbol, which is what you want almost every time.

WHEN TO USE IT: the symbol on your TradingView chart doesn't exactly match the symbol your broker uses. Example: your chart shows an exchange-prefixed symbol but your broker expects just the plain ticker (e.g. broker wants 'MU' with nothing extra). Type the EXACT symbol your broker expects here and the bot will send that instead.

⚠️ What is sent is NOT exactly what you type: the bot keeps only the part AFTER the last colon and strips quotes, backslashes, tabs and line breaks, so 'NASDAQ:MU' sends 'MU'. If what is left is empty (a trailing colon, or only stripped characters) the field is ignored and the chart's own symbol is sent with no warning. A typo means orders for the wrong symbol, or rejected orders. Double-check it against your broker's symbol list. If your trades are firing on the wrong instrument, this field is the first place to look.

---

## Trade Direction

Long Only = buy expecting price to rise (RECOMMENDED — the most thoroughly tested mode). Long & Short and Short Only were reworked in v1.0.12+ (short cash accounting now reconciles with margin-account reality), but Long & Short still underperforms Long Only in backtest: most signals in the training set are long-favorable, so the short side is fighting the regime the rules were learned in. Use Long Only until a regime-filtered shorts sweep is published.

SEPARATELY, and it is a different thing: CALIBRATED means the symbol carries at least one bespoke swept parameter instead of the defaults. The calibrated set is MU, GOOGL, ENPH, AAPL, COIN, LCID, RKLB, XLK, AFRM, TSLA, PLTR, QQQ, IONQ and OKLO. Any other symbol runs on defaults. On stocks, funds and index symbols the dashboard flags that on the chart; on crypto, forex and futures it does not, so check the list above yourself.

---

## How to Close Trades

How the bot closes a winning trade. Follow the Trend (recommended) = a trailing stop rides winners, NO fixed target. Set Target & Stop = a fixed target at 4x your stop distance PLUS a stop, both set at entry and never moved.

WHERE THE STOP LIVES: on a broker route it rests at your broker; on Alerts Only (the default) nothing is sent anywhere and it is tracked on the chart only. BOTH modes still use the safety exits — thesis change, dead-trade, Cut Early, time gate, end-of-day close, and the daily loss halt where enabled.

⚠️ Cut Early closes a stalled trade still at or below its entry, ahead of the full dead-trade timeout.

🎯 Stretch Take sells into extreme strength: price far above VWAP while in profit, taking the highs instead of waiting for the stop -- FOLLOW THE TREND ONLY, it is never armed in Set Target & Stop, where the fixed target is the only profit exit. On Set Target & Stop the target is watched on the chart and closed at market when reached; on the TradersPost route it instead rests at your broker as a limit order and fills at the target price.

⚠️ ON THE TRADERSPOST ROUTE THE TRAILING STOP IS CHART-ONLY: the broker holds the stop set at entry and never receives the updates, so the line tightening on your chart is not what protects the position at the broker.

---

## Account Balance ($)

How much money the bot may use.

It has two meanings. Share of Account (the next setting) picks one.

SHARE OF ACCOUNT = 0 (off, the default): this is the money THIS CHART may trade with. One chart: type in your real account balance. Several charts: split it. Four charts on a $20,000 account means you type $5,000 into each one. Every chart trades as if it is the only one touching your account, so telling all four they have $20,000 risks four times what you meant.

SHARE OF ACCOUNT = 1 to 100 (on): this is your WHOLE account balance. Type the same full balance on every chart and do not split it. The chart's money is Account Balance x Share of Account.

NEVER put two charts on the same stock. They would both try to steer one position at your broker and fight over its stop.

Minimum $500. The bot only buys whole shares, so a chart whose money cannot afford one share skips the trade instead of placing it.

WHAT IT DRIVES: the Strategy Report, the share counts on the chart, and the share count in the alert text. With Share of Account off it also sets the real order size.

KEEPING IT ACCURATE: the bot starts from this chart's money, then adds its own wins and subtracts its own losses. TradingView sometimes restarts an alert without telling you. After a restart the bot forgets those wins and losses and starts again. So every two to three weeks, and after any large loss — WHILE YOU ARE FLAT on that ticker — type your current balance here, delete the alert and create it again. Never recreate an alert while a trade is open.

---

## Share of Account (%)

OFF at 0, the default. Then the bot sizes each order from Account Balance and sends your broker a share count.

Set it from 1 to 100 to size orders from the money that is really in your broker account. This is the safer way when the bot places orders for you (Broker Platform = TradersPost or Standard), because the size follows your real balance up and down.

WHEN YOU TURN IT ON, do two things on every chart:

1. Type your WHOLE account balance into Account Balance. Do not split it.

2. Type this chart's part here. One chart: 100. Four charts: 25 on each one. Keep the total at 100 or less.

What it does. The chart's own money becomes Account Balance x this percent. And the bot sends your broker a percent, not a share count. Your broker connection turns that percent into shares from the money that is really in the account at that moment.

Capital Buffer is taken off first, and a weaker signal sends less. So 100 with the default buffer sends at most 99.75.

WARNING: if you turn this on and leave a split balance in Account Balance, the chart gets a share of a share and trades far too small. If you set 100 on several charts, each one asks for the whole account.

With several charts, your broker can still borrow a little while another chart holds an open profit. Keep the total under 100 to leave room.

---

## How to Size Trades

Smart Sizing = risk-adjusted per trade based on stop distance. Same % Every Trade is the shipped default and the configuration the published performance figures were measured on: every trade starts from the same base — your full sizing budget — which the conviction multiplier then scales and the safety caps (capital buffer, position cap) trim.

⚠ Note: both modes apply an internal conviction multiplier that scales trade size on signal strength, volatility, drawdown state, session conditions, and 8 other safety factors. Smart Sizing uses the full 0.25× to 2.0× range. Same % Every Trade reaches only the 0.25× to 1.0× half: your base allocation is already the entire sizing budget, so conviction can shrink a trade below it on a weak setup but can never grow one above it. "Same %" trades therefore still vary in actual size — downward only, never upward — which is the bot adapting to live risk conditions, not a bug. To see exactly how much capital is deployed at any moment, use Chart Detail → Capital View.

---

## Stop Width

Fine-tune your stop distance. 1.0 = recommended default. The entry stop uses the per-ticker auto-calibrated base; the Trend Follow trailing stop uses a fixed daily-ATR base, and this setting scales that too. Lower = tighter stops (e.g. 0.9 = 10% tighter, more stop-outs but smaller losses). Higher = wider stops (e.g. 2.0 = double-distance, fewer stop-outs but bigger losses). Range 0.9–2.5.

⚠️ Only 0.9–1.1 is validated — measured long only on TSLA 15m. 1.0 scored best; 0.9 scored about 4 points of yearly return lower and 1.1 about 7 lower. The 0.9 floor exists because 0.7 was the WORST value tested: it puts the stop inside normal price noise, so trades stop out early. Above 1.1 results fall off unevenly. Test any value outside 0.9–1.1 in replay before going live.

⚠ Scope: this scales the stop on EVERY entry type. Momentum and chop stops are a multiple of intraday ATR; Trend Follow frames its stop on daily ATR and Reversion on 3x ATR, and this setting scales those two by the same factor, so 2.0 here roughly doubles them as well. With Strategy Type on Auto, the bot can pick Trend Follow or Reversion by itself, so this applies even if you never picked them. Stop distance changes POSITION SIZE only in Smart Sizing; in 'Same % Every Trade' (the shipped default) size comes from your sizing budget and this setting does not change it.

---

## Strategy Report

Hover the **Strategy Report** title on the dashboard to see the full scorecard. Notes on reading it:

**Alpha vs Hold** is this bot's return MINUS buy-and-hold, both measured over the WHOLE window, in percentage POINTS. It is NOT annualised, so do NOT subtract it from Estimated Annual Return, which IS annualised. The two are on different clocks: they agree ONLY when the window is exactly one year, and the gap widens the further your window is from that. Compare like with like or not at all. Positive = the bot earned its complexity.

**Sizing in the report.** The stats use chart-side trade sizing including modeled slippage (no commissions modeled). Actual broker fills may be smaller when account-level caps trigger.

**Bar Replay.** Use a 1-minute replay step. With a step equal to the chart timeframe (for example 15 minutes on a 15-minute chart), the replay can miss trades and show fewer than the normal chart.

**These figures depend on the Broker Platform you picked.** Picking TradersPost changes real behaviour, so it changes this report: same settings, same chart, different numbers. Alerts Only and Standard (Custom JSON) produce identical figures ONLY while Signal Speed is "Wait for Bar Close". On "Quick Exits" they do not: Signal Speed takes effect the moment you leave Alerts Only, so exits move from bar close to intrabar and every figure moves with them. On TradersPost: adding to winning trades is off, extended-hours trading is off, exits are held while the market is closed, and a fixed target books at the target price rather than the bar close.

**How big is the gap on your stock? Measure it.** Note the figures, switch Broker Platform to the other route, let the chart reload, and read them again: same stock, same settings, one change. That gap is your own route cost, and it is the only number that describes your ticker. It is bigger on stocks that trade actively outside regular hours, and near zero on stocks that do not. This is a chart-only comparison: an existing alert is a frozen copy and is not affected. After that one comparison, track each route against itself: a figure from one route is not a scorecard for the other.
