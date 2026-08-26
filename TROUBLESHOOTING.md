# Troubleshooting

Find your symptom. Most problems here are configuration or a misunderstanding of how TradingView
alerts work — not a fault in the indicator.

**One rule before anything else: your broker is the only authority.** If the chart and your broker
disagree about whether you hold a position, the broker is right. Always check there first.

---

## The bot isn't placing any trades

Work down this list in order. The first four cause the overwhelming majority of cases.

**1. Broker Platform is on `Alerts Only`.**
That is the default, and on it the bot sends nothing to any broker by design. Open the indicator
settings and set Broker Platform to `TradersPost` or `Standard (Custom JSON)`.

**2. You created the alert BEFORE you set Broker Platform.**
This is the single most common setup mistake, and it looks exactly like a broken bot.

An alert is a **frozen copy** of the script and its settings at the moment you created it. If Broker
Platform was on `Alerts Only` when you clicked Create, that alert will send plain text forever and
can never place a trade — no matter what you change on the chart afterwards.

**Fix:** delete the alert and create a new one with Broker Platform already set. There is no way to
repair the existing alert, and nothing on the chart can warn you about this, because the chart has no
way to know an alert exists.

**3. Check the footer says `armed`.**
The status line at the bottom of the indicator shows the build number and, when the bot can place
orders, the word `armed`. If `armed` is missing, the chart is not configured to trade — fix that
before you create any alert.

**4. Check the alert still exists and is running.**
Open the Alerts Manager (the 🔔 panel on the right). Your alert should show a green **Active**
status. Any other status means it is not firing:

| Status | Meaning |
|---|---|
| `Active` (green) | running normally |
| `Stopped — Expired` (red) | the alert's expiration passed — recreate it, and set Expiration to Open-ended if your TradingView plan offers it |
| `Stopped manually` (grey) | someone paused it — restart or recreate |
| `Stopped — Triggered` (orange) | it was created as a one-time alert — recreate it without "Only Once" |

**5. TradersPost users — check the strategy and subscription.**
Any one of these will stop orders silently:

- **Auto submit** must be ON. If it is off, trades sit waiting for manual approval.
- **Allowed tickers** must include the symbol, or be set to allow any.
- **Allowed IP addresses** must be set to **TradingView**. If it is set to specific IPs that don't
  match TradingView's servers, your signals are rejected before they become trades.
- **Short selling** must be ON if you trade shorts, otherwise short signals do nothing.
- The **strategy** and the **subscription** must both be enabled.

**6. Check the TradersPost Signals page, not just Notifications.**
TradersPost has three separate stages and they fail differently:

- **Signals** — did the webhook arrive and parse?
- **Trades** — did it become an order at the broker?
- **Notifications** — only fires on events your notification settings cover.

A signal can arrive successfully and never become a trade. If Notifications is set to "Only Failed
Signals," some rejections never notify you at all. **Always check Signals and Trades directly.**

---

## I changed a setting and nothing happened

**Expected.** An alert is a frozen copy of the script and its settings at the moment you created it.

- Changing a setting on your chart does **not** reach a running alert.
- Updating to a newer version of the indicator does **not** reach a running alert.
- The snapshot includes the **chart itself** — so changing the chart's session (regular hours vs
  extended hours) doesn't reach the alert either.

**The only way to change an alert is to delete it and create a new one.**

⚠️ **Never delete an alert while that ticker holds an open position at your broker.** Close the
position at your broker first, then delete the alert. Deleting it mid-trade can leave that position
with nothing managing it.

---

## My alert log says "Webhook delivery failed — request took too long and timed out"

**This usually does not mean your trade was missed.**

TradingView gives a webhook 3 seconds to respond. If your receiver is slow to answer — which happens
most often on the first alert of the day, when everything is cold — TradingView stops waiting and
logs a failure. The instruction very often arrived and executed anyway, seconds later.

**Check your broker before assuming anything was lost.** In extended testing, every order-bearing
timeout still executed at the broker.

**The one failure that IS real:** `couldn't find this domain`. That means the request never reached
your receiver at all. If you see it on an entry or exit, check your broker immediately — that
instruction was genuinely lost.

This affects the Standard (Custom JSON) route most, because your own server has to answer within
3 seconds.

---

## The dashboard shows a negative Est. Annual Rtn, or a D Tier Bot Score

**Nothing is broken. The bot is telling you this ticker and style don't suit each other.**

Not every ticker works. That is why **Stock Fit** and **Bot Score** exist — they grade the pairing
rather than pretending every symbol is a good one.

**What to do:**

1. Check **Stock Fit**. An A means the engine reads this ticker well; an F means avoid it.
2. Try a different **Trading Style**. Watch how the dashboard responds — the difference between
   styles on the same ticker is often large.
3. Try a **15-minute chart** if you aren't on one. Every style produces its most consistent results
   there.
4. If it still reads negative, **use a different ticker.** That is the correct outcome, not a
   failure.

Before trading real money, check all four of these on your ticker and style: Est. Annual Rtn
positive, Profit Factor above 1.5, Max Drawdown you could actually live through, and at least
20 trades in the backtest.

---

## Changing Broker Platform changed my backtest numbers

**Expected, and the chart is being honest with you.**

The TradersPost route cannot do everything the other routes can. An exit that triggers outside
regular trading hours is held on the chart and only sent on the next regular-hours bar, and
pyramiding and extended-hours entries are disabled on that route entirely.

Those differences change *when* trades close, which changes the trade set, which changes every
statistic downstream. The dashboard models what that route will actually do rather than showing you
numbers from a configuration you aren't running.

If you switch routes, re-read the dashboard. It is describing a different thing.

---

## My share counts keep changing, even on "Same % Every Trade"

**Expected.** Both sizing modes apply an internal conviction multiplier — roughly 0.25× to 2.0× —
based on signal strength, volatility, market regime and current drawdown.

"Same % Every Trade" refers to the sizing *budget*, not to an identical share count on every trade.
Seeing 800 shares one day and 450 the next on the same ticker is the bot adapting to live risk, not
a fault.

---

## The numbers changed since yesterday and I didn't touch anything

**Expected.** TradingView loads a limited number of historical bars, and that window moves forward
every day. Old trades fall off the back as new bars arrive, so every statistic drifts.

TradingView also caches indicator results. If numbers look stuck or implausible, remove the indicator
from the chart and add it again for a clean calculation.

---

## The dashboard says "Need 14+ days" or shows dashes

**The bot is still warming up on this ticker.** Statistics need 14 trading days and at least 5
trades before they mean anything, and the bot would rather show nothing than show you a number built
on three trades.

This is most common on 4-hour and Daily charts with the Position styles, where there simply aren't
enough trades in the loaded history. Those styles work best on 1-hour charts and below.

---

## Settings are greyed out and won't unlock

**If you are on v1.0.91 (build 260826D) or later, this cannot happen any more.** Every setting is
editable straight away.

**On earlier builds:** pressing the Defaults / Reset settings button left TradingView's settings
dialog in a stale state where advanced settings stayed greyed out and unclickable, and re-ticking the
unlock switch did nothing. This was a TradingView bug, not an indicator fault.

**Fix on an older build:** remove the indicator from the chart and add it again. Reopening the
dialog, changing the timeframe and refreshing the page all fail to clear it. **Updating to v1.0.91 or
later removes the cause.**

Your alerts are never affected by this — an alert keeps its own frozen copy and carries on trading
normally.

---

## My broker shows a position the chart doesn't — or the chart shows one my broker doesn't

**The broker is right. Always.**

The chart maintains its own belief about what you hold. That belief can drift from reality in a few
ways: a webhook that never arrived, an order the broker rejected, a manual position change you made
yourself, or a partial fill.

**To confirm what you actually hold**, add up every filled order for that symbol in your broker's
order history. Do not rely on a dashboard summary widget — check the fills.

**Then fix it:**

1. Close the position at your broker, manually, so you are flat.
2. Only then, delete the alert and create a new one.

**The most common cause is extended-hours trading.** With `Trade Extended Hours` on, orders placed
outside regular hours are limit orders, and in thin pre-market they often don't fill. The chart may
book the outcome anyway. If you are seeing repeated mismatches, turn that setting off — it is off by
default and it is not part of the validated configuration.

---

## A trade closed and I don't know why

Every exit is labelled on the chart:

| Label | Meaning |
|---|---|
| 🏆 Target | hit the take profit level |
| 🛡️ Stopped | hit the stop loss or trailing stop |
| 📌 Changed | the engine's read on the ticker changed and it exited the thesis |
| ⏳ Too Slow | the trade wasn't progressing and was closed on time |
| ⚠️ Cut Early | an early-exit condition triggered |
| 💤 Dead Trade | the position stopped moving and was closed |
| 🔴 Closed | closed for another reason |
| ⏱ EOD Close | Day Trading style flattened before the market closed |

Hover the Strategy Report header for the full trade history including best and worst trades and
current streaks.

---

## My bot was working and then stopped

Check in this order:

1. **Is the alert still Active?** See the status table above. An alert that has expired or been
   stopped looks identical to a quiet market from everywhere except the Alerts Manager.
2. **Is your broker connection still authorised?** Broker connections expire and need reauthorising.
3. **Is there an open position it's waiting on?** The bot holds one position per ticker at a time.
4. **Did the market simply not signal?** Check Signals / Day on the dashboard for what's normal on
   this ticker and style.

---

## Two charts on the same ticker

**Don't do this.** Two charts on one symbol will both try to steer a single position at your broker
and will fight over its protective stop. One chart, one alert, one ticker.

If you want several tickers, use a separate chart and a separate alert for each — and split your
Account Balance between them, or every chart will trade as though it owns the whole account.

---

## LITE: why can't I see Profit Factor, Est. Annual Rtn or Bot Score?

Those are PRO features. LITE shows the signals, the stop line, the exit reasons and the Stock Fit
grade. The performance metrics and all automation are locked, and locked controls are marked 🔒 in
the settings so you can see what the full version adds.

---

## Still stuck?

Message me through my TradingView profile. It helps enormously if you include:

- The ticker and timeframe
- What Broker Platform is set to
- The build number from the indicator's footer
- What the footer says — especially whether `armed` is present
- What your broker shows, if a position is involved
