# Frequently Asked Questions

Conceptual questions about how Market Axiom works. If something looks *wrong*, start with
**TROUBLESHOOTING.md** instead — this page explains behaviour, that one diagnoses symptoms.

Settings are documented inside the indicator itself. Hover the ⓘ beside any setting for its full
explanation — those tooltips ship with the build, so they can never fall out of date the way a web
page can.

---

## The basics

**What is Market Axiom?**
A TradingView indicator that generates buy and sell signals with a stop loss and position sizing,
and can send those signals to your broker as real orders. It reads each ticker's behaviour and routes
it to one of three strategy engines — Momentum, Trend Following, or Mean Reversion — rather than
applying one fixed approach to everything.

**Does it repaint?**
No. Entries are always evaluated on the close of a completed candle, regardless of any other setting.
That is what stops signals from appearing and disappearing mid-candle.

Exits can optionally react faster: the `Quick Exits` Signal Speed checks stops and targets as prices
arrive rather than waiting for the candle to finish. That affects when a trade *closes*, never
whether an entry existed.

**Do I need to leave TradingView open for it to trade?**
No. Alerts run on TradingView's servers, not in your browser. Once an alert is created you can close
the tab, shut the laptop, and it keeps firing.

**What timeframe should I use?**
15 minutes. Every trading style produces its most consistent results there. Other timeframes work,
but they are outside the tested configuration and the dashboard will say so.

**Does it work on crypto and forex?**
Yes, and on stocks and ETFs. The engine adapts to the instrument's behaviour rather than assuming a
market type. Note that `Trade Extended Hours` does nothing on markets that already trade 24/7.

---

## Getting it trading

**Do I need to build anything to automate it?**
No. Choose **TradersPost** as your Broker Platform — it connects TradingView alerts to brokers like
Alpaca and Tradier with no code at all. You create an account there, connect your broker inside it,
and paste the webhook address it gives you into your TradingView alert.

**What is the Standard (Custom JSON) route then?**
It is for people running their own webhook receiver — typically a small server they deployed
themselves. It sends the bot's native order format, including trailing-stop updates that the
TradersPost route does not push. **If you don't already know you need it, you don't.**

**Which broker should I use?**
Whichever your chosen platform supports and you already trust. Market Axiom never touches your
broker directly — it sends signals to the platform you configured, and that platform places the
orders.

**Why does the bot ship unable to trade?**
Broker Platform defaults to `Alerts Only`, which sends nothing to any broker. That is deliberate.
Handing order placement to software is a decision you should make on purpose, after watching it on a
chart and testing on a paper account — not something that happens because you added an indicator.

**How long should I test on paper before going live?**
At least two weeks. Both TradersPost and most brokers offer a free paper account. Two weeks is the
minimum needed to see the bot behave across a range of market conditions rather than one mood.

**Can I run several tickers at once?**
Yes — one chart and one alert per ticker. **Split your Account Balance between them.** Every chart
sizes as though it is the only one touching your account, so four charts each told they have $20,000
will each trade full size and you'll risk four times what you intended.

**Can I run two strategies on the same ticker?**
No. Two charts on one symbol will both try to steer a single position at your broker and fight over
its protective stop. One ticker, one chart, one alert.

---

## Understanding the dashboard

**Why is Est. Annual Rtn "estimated"?**
It annualises the result of a limited backtest window. It is not a forecast and it is not a promise
— it is what this configuration would have produced over the bars TradingView loaded, scaled to a
year. Treat it as a comparison tool between styles and tickers, not as an expected return.

**What is Stock Fit?**
A grade for how well this ticker suits the engine, from A to F. It is computed live on whatever
symbol you're looking at, so it works on any ticker rather than only a fixed list.

**What is Bot Score?**
An overall S/A/B/C/D grade combining return, risk and consistency for the current ticker and
settings. It is a summary of the dashboard, not an extra measurement.

**My ticker shows a D Tier. Is the bot broken?**
No — it's telling you this ticker and style don't suit each other. Not every ticker works. Try
another Trading Style first, then another ticker. See TROUBLESHOOTING.md.

**Why do the numbers change day to day?**
TradingView loads a limited number of historical bars and that window moves forward. Old trades fall
off the back as new ones arrive, so every statistic drifts slightly. This is normal and affects every
TradingView indicator that reports statistics.

---

## Risk and expectations

**Will this beat buy-and-hold?**
On many tickers over long periods, no — and the honest answer matters more than the flattering one.
Market Axiom is built for managed risk, defined stops and disciplined exits: the parts most retail
traders struggle to do consistently by hand. It is not a market-beating machine and does not claim
to be.

**How bad do the losing streaks get?**
Streaks of eight to fourteen consecutive losing trades have occurred during testing on tickers that
were profitable overall. **This is the single most important thing to understand before you connect
real money.** A run of losses is a normal feature of a trend-following system, not a sign it has
stopped working — but you have to be able to sit through one without intervening.

Before you go live, work out what fourteen consecutive losses at your Risk Per Trade would do to your
account. If that number would make you quit, lower your Risk Per Trade until it wouldn't.

**What Risk Per Trade should I use?**
1% is the default and a sensible starting point. Higher is more aggressive in both directions. The
test above is the real answer: pick the number whose worst-case run you could actually live through.

**How much money do I need?**
Minimum $500, because the bot only buys whole shares — a balance too small to afford one share skips
the trade rather than placing it. On expensive tickers the practical minimum is much higher: at a
$900 share price, one share is a large fraction of a small account, and sizing becomes coarse.

**Does it short?**
Yes, if you set Trade Direction to `Short Only` or `Long & Short`, and if your broker and platform
both allow short selling. Long Only is the default and the most thoroughly tested mode.

**What happens overnight and at weekends?**
Swing and Position styles hold overnight by design. Day Trading flattens before the close. Nothing
fires when the market is closed, and stops resting at your broker do not trigger outside regular
hours unless your broker explicitly supports that.

**What happens if my internet drops?**
Nothing changes. Alerts run on TradingView's servers and orders are placed by your broker platform.
Your computer is not in the path once an alert exists.

---

## Alerts

**Why do I have to delete and recreate an alert to change a setting?**
Because TradingView freezes a copy of the script, its settings and the chart's context at the moment
you create the alert. Nothing you change afterwards reaches it. This is how TradingView works for
every indicator, not something Market Axiom chose.

**How often should I recreate my alerts?**
Every two to three weeks, and **only while you are flat on that ticker.** Your Account Balance is
frozen into the alert, so it drifts as your account grows or shrinks; recreating re-aligns it. The
bot also flags this for you once it is overdue.

**Never recreate an alert while a trade is open** — close the position at your broker first.

**Can I edit the alert's message?**
There is nothing useful to edit. For the `Any alert() function call` condition the Message row only
sets the alert's **name**, which never leaves TradingView. The actual payload comes from the script.

**Why did my alert log show a delivery failure but the trade happened anyway?**
TradingView allows a webhook 3 seconds to respond and logs a failure if it takes longer. The
instruction usually arrived and executed regardless. See TROUBLESHOOTING.md for which failures are
real and which aren't.

---

## LITE vs the full version

**What does LITE include?**
The same signal engine: buy and sell markers with entry price and stop loss, the trailing stop line,
exit reasons, the trend cloud and sub-panes, and the live Stock Fit grade.

**What is locked in LITE?**
All automation — LITE cannot send an order to any broker under any configuration. Also the
performance metrics (Profit Factor, Est. Annual Rtn, Max Drawdown, Bot Score and the full Strategy
Report), the fixed target-and-stop exit, Smart Sizing, the daily-loss circuit breaker, the
before-the-close flatten, pyramiding, and dashboard size. Locked controls show a 🔒.

**Are the signals the same in LITE?**
Yes. LITE runs the same engine — it shows you less about the outcome and it cannot act on the
signals for you.

---

## Data and privacy

**Does Market Axiom see my broker credentials?**
No. The indicator sends a signal to the webhook address you configured. Your broker connection lives
inside TradersPost or your own server, and the indicator never has access to it.

**Is my webhook address safe?**
Treat it like a credential — anyone who has it can send orders to your account. Never post it
publicly. On TradersPost, set **Allowed IP addresses** to **TradingView**, which makes the address
useless to anyone who obtains it.

**Does anything get sent anywhere if I use Alerts Only?**
No. On `Alerts Only` the indicator sends nothing to any external service. Alerts are delivered by
TradingView to you, in whatever form you chose when creating them.

---

## Getting help

Message me through my TradingView profile. If you're reporting a problem, TROUBLESHOOTING.md lists
the five things to include that make it solvable in one reply instead of five.
