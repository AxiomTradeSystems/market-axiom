# Frequently Asked Questions

How Market Axiom works and what to expect. If something looks wrong, start with
[TROUBLESHOOTING.md](TROUBLESHOOTING.md).

---

## The basics

**What is Market Axiom?**
A TradingView indicator that finds trades, sets a stop loss, sizes each position and can send the
orders to your broker. It reads each ticker's behavior and routes it to one of three strategy
engines (Momentum, Trend Following or Mean Reversion) instead of using one approach for everything.

**Does it repaint?**
No. Entries are always decided at the close of a finished candle. A signal on the chart does not
appear and then vanish.

The **Quick Exits** Signal Speed lets exits react during a candle, on the TradersPost and
self-hosted routes. That changes when a trade closes, never whether an entry happened.

**Do I need to keep TradingView open?**
No. Alerts run on TradingView's servers. Once an alert exists, you can close the browser and turn
off the computer.

**What timeframe should I use?**
15 minutes. The default settings were validated on that timeframe, with the Swing style.

**What markets does it work on?**
Stocks, ETFs, crypto and forex. It adapts to how each instrument behaves. Automation is built and
tested on US stocks and ETFs.

---

## Automation

**Do I need to write code?**
No. Choose **TradersPost** as your Broker Platform. It connects TradingView to brokers such as
Alpaca and Tradier. See [TRADERSPOST.md](TRADERSPOST.md).

**What is the self-hosted route?**
Your own receiver, running in your Cloudflare account, manages each trade at Alpaca. It moves the
broker stop up with the trailing stop, checks every 15 minutes that each position has a stop, and
supports Add to Winning Trades and extended hours. Setup takes one command-line session. See
[SELF-HOSTED.md](SELF-HOSTED.md).

**Which route should I pick?**
TradersPost if you want the fastest setup or a broker other than Alpaca. The self-hosted receiver
if you use Alpaca and want the broker stop to follow the trailing stop.

**Why does the bot start on Alerts Only?**
So that nothing trades until you choose it. Watch it on a chart, run it on paper, then connect a
broker on purpose.

**How long should I paper trade?**
At least two weeks. [GOING-LIVE.md](GOING-LIVE.md) has the checklist.

**Can I run several tickers?**
Yes. Use one chart and one alert for each ticker. Turn on **Share of Account** to divide one
account between them. See [GOING-LIVE.md](GOING-LIVE.md#4-set-the-money-each-chart-may-use).

**Can I run two charts on the same ticker?**
No. Both would steer the same position at your broker.

---

## Money and risk

**How much money do I need?**
At least $500 per chart. On stocks and ETFs the bot buys whole shares, so a chart needs enough money
for at least one share. On high-priced stocks, more money gives finer sizing.

**How does it size trades?**
Two ways, under **How to Size Trades**:

- **Same % Every Trade** (default): each trade starts from the chart's full budget. Weaker setups
  get smaller trades, down to a quarter of the budget.
- **Smart Sizing:** each trade risks a set percent of the budget, chosen with **Risk Per Trade**.
  Setup quality scales it from 0.25× to 2×.

**What does Risk Per Trade do?**
It sets the base risk per trade when **How to Size Trades** is **Smart Sizing**. On the PRO default,
Same % Every Trade, it is not used. LITE always uses Smart Sizing, so there it always applies.

**What is Share of Account?**
With it on, the bot sends your broker a percent of the account, and the broker works out the shares
from the money that is really there. Sizing then follows your balance up and down. It is the
recommended setting for automation. See [GOING-LIVE.md](GOING-LIVE.md#4-set-the-money-each-chart-may-use).

**How bad do the losing streaks get?**
Streaks of 8 to 14 losing trades in a row have happened in testing, on tickers that were profitable
overall. Losing streaks are normal for a trend-following system. Before you fund an account, check
the Worst Loss Streak and Max Drawdown for your ticker. [GOING-LIVE.md](GOING-LIVE.md#3-know-your-worst-case-before-you-fund)
shows how.

**Will it beat buy-and-hold?**
Not on every ticker or in every period. Market Axiom is built for managed risk: rules-based entries,
a stop on every trade and disciplined exits. Those are the parts most traders find hardest to do by
hand.

**Does it trade short?**
Yes, with **Trade Direction** set to `Short Only` or `Long & Short`, if your broker allows short
selling. **Long Only** is the default and the most tested mode.

**What happens overnight?**
The Swing and Position styles hold overnight. Day Trading closes its trade before the market close.
When automated, the protective stop rests at your broker overnight.

**What is Auto Stop-Loss for Bad Days?**
A daily brake. If the day's loss, including an open trade, passes a limit set from the ticker's own
volatility, the bot closes the position and takes no new trades until the next session.

---

## The dashboard

**Why is Est. Annual Rtn an estimate?**
It scales the backtest result to one year. Use it to compare styles and tickers, not as a forecast.

**What are Stock Fit and Bot Score?**
**Bot Score** (S to D) grades how the bot has actually done on this ticker: mostly yearly return,
then how much it beats buying and holding, its profit-to-loss ratio and its worst dip. It gives a
high grade only when the evidence supports it. **Stock Fit** (A to F) shows how busy and lively the
stock itself is. It is not a forecast; judge results by Bot Score.

**Why do the numbers change from day to day?**
TradingView loads a limited window of history, and it moves forward each day. Old trades drop off as
new ones arrive.

---

## Alerts

**Why must I recreate an alert to change a setting?**
TradingView saves a frozen copy of the script, its settings and the chart when you create an alert.
Later changes do not reach it. This is true for every TradingView indicator.

**How often should I recreate my alerts?**
When you change a setting, when a new version is published, before the alert's expiration date,
and every two to three weeks if Share of Account is off. Always while flat on that ticker.

**Can I edit the alert's message?**
There is no need. For `Any alert() function call`, the Message box only names the alert. The script
writes the message that is sent.

---

## LITE and PRO

**What does LITE include?**
The same signal engine: buy and sell signals, exit reasons, the trend cloud, the price numbers on
the dashboard and the live Stock Fit grade. You can also get alerts for each signal.

LITE always sizes with Smart Sizing and Risk Per Trade, and always keeps Auto Stop-Loss for Bad
Days and the Day Trading close on.

**What does PRO add?**
The stop line, prices and share counts on the trade labels and in alerts, the performance numbers
(Profit Factor, Avg Trade, Max Drawdown, Reward/Risk, Signals / Day, Est. Annual Rtn, Bot Score),
the full Strategy Report, all automation, Set Target & Stop, the choice of sizing method, Share of
Account, Add to Winning Trades, the option to turn off Auto Stop-Loss for Bad Days or the Day
Trading close, and dashboard sizes.

**Are LITE's signals the same as PRO's?**
Yes. LITE runs the same engine. It shows less about each trade and cannot send orders.

---

## Data and privacy

**Does Market Axiom see my broker login?**
No. The indicator sends signals to the address you put in your alert. Your broker connection lives
in TradersPost or in your own receiver.

**Is my webhook address private?**
Treat it like a password. On TradersPost, set **Allowed IP addresses** to **TradingView**. On the
self-hosted route, the receiver accepts only messages that carry your secret key.

**Does anything leave TradingView on Alerts Only?**
No. TradingView delivers the alerts to you, and nothing else is sent anywhere.

---

## Getting help

Message me on TradingView. [TROUBLESHOOTING.md](TROUBLESHOOTING.md#still-stuck) lists what to
include.
