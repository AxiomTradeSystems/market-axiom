# Troubleshooting

Find your symptom below. Most problems come from a setting or from how TradingView alerts work, and
each one has a short fix.

**First rule: your broker is the record.** If the chart and your broker disagree about what you
hold, the broker is right. Always look there first.

---

## The bot is not placing trades

Work down this list in order.

**1. Broker Platform is on `Alerts Only (no broker orders)`.**
That is the default. It sends no orders by design. Set Broker Platform to `TradersPost` or
`Standard (Custom JSON)`.

**2. The alert was created before Broker Platform was set.**
The alert froze the old setting. Delete it and create a new one, while flat.
[CREATING-ALERTS.md](CREATING-ALERTS.md) explains why.

**3. The status line does not show `armed`.**
Then the chart is not set to send orders. Fix the settings, then recreate the alert.

**4. The alert is not Active.**
Open the alerts panel. Anything other than a green **Active** status means the alert is not running.
See [the status table](CREATING-ALERTS.md#check-that-it-works).

**5. TradersPost:** check that the strategy and the subscription are both enabled, that **Auto
submit** is on, and that **Allowed tickers** includes your symbol. Then check the **Signals** page
(did the alert arrive?) and the **Trades** page (did it become an order?).

**6. Self-hosted receiver:** run the [health check](SELF-HOSTED.md#6-check-its-health).

- `"status": "error"` names the missing piece in `degraded_reasons`.
- `last_ingress_rejected:unauthorized_secret` means the Webhook Secret Key in the indicator does not
  match your `WEBHOOK_SECRET`. Type the same value in both, then recreate the alert while flat.

---

## My bot was working and then went quiet

1. **Is the alert still Active?** On plans without open-ended alerts, an alert stops after at most
   two months. TradingView also stops any alert that fires more than 15 times in 3 minutes.
2. **Is your broker connection still authorized?** Broker connections in TradersPost expire and need
   to be renewed.
3. **Is it holding a position?** The bot holds one position per ticker at a time, and waits for
   that trade to close.
4. **Is the market quiet?** Signals / Day on the dashboard shows what is normal for this ticker.

---

## I changed a setting and nothing happened

That is expected. A running alert is a frozen copy and does not see chart changes. Delete the alert
and create a new one, while flat.

---

## The alert log says "request took too long and timed out"

TradingView waits 3 seconds for a reply. The order very often arrived and went through anyway, a few
seconds later. Check your broker before you act.

**"Couldn't find this domain"** is different: the message did not arrive. Check the webhook address
in the alert, then check your broker.

---

## A TradersPost trade closed early the next morning

That is the last-bar rule of the TradersPost route. A bar that closes at the market close counts as
after-hours, so an exit found on it goes through at the next session's first regular-hours bar.
[TRADERSPOST.md](TRADERSPOST.md#how-the-bot-trades-on-this-route) explains which exits carry over.

---

## The chart and my broker show different positions

The broker is right. The usual causes are an order the broker refused, a message that never
arrived, or a change you made by hand at the broker.

1. Add up the filled orders for that symbol in your broker's order history. That is what you hold.
2. Close the position at your broker, so you are flat.
3. Delete the alert and create a new one.

With **Trade Extended Hours** on, orders outside regular hours are limit orders and can go unfilled
in thin trading. If mismatches repeat, turn that setting off.

---

## The share counts change from trade to trade

That is expected. The bot scales every trade by setup quality and market conditions:

- **Same % Every Trade** (PRO default): from 0.25× to 1× of the chart's budget. Weaker setups get smaller
  trades. No trade is larger than the budget.
- **Smart Sizing** (always used in LITE): from 0.25× to 2× of the Risk Per Trade base.

With **Share of Account** on, your broker turns the percent into shares from your real balance, so
its share count can differ a little from the chart's.

---

## The dashboard shows a negative Est. Annual Rtn or a D Tier Bot Score

Nothing is broken. The bot is telling you this ticker and these settings are not a good pairing.

1. Try another **Trading Style**.
2. Use a **15-minute chart**.
3. If it still reads negative on every style, use a different ticker.

[TUNING.md](TUNING.md) has the full method.

---

## The numbers changed and I did not touch anything

That is expected. TradingView loads a limited window of history, and it moves forward every day.
Old trades drop off as new bars arrive, so every number drifts a little.

Changing **Broker Platform**, **Account Balance** or **Share of Account** also changes the numbers,
because the chart models the route and the money you set.

If numbers look stuck or strange, remove the indicator from the chart and add it again.

---

## The dashboard shows "Need … trading days" or "Need … trades"

The bot is still collecting history on this ticker. It shows its numbers once it has 14 trading
days and 5 trades, and the message counts down until then. Bot Score has its own countdown, such as
"18/30 days", because it needs 30 days. Slower styles on 4-hour and daily charts can take a long time to get there. They
work best on 1-hour charts and below.

---

## A warning says this chart's money "cannot buy one share"

On stocks and ETFs the bot buys whole shares. The chart's money is less than one share's price, so
it will skip the trade. Raise **Account Balance**, or **Share of Account** if it is on.

---

## A trade closed and I do not know why

The exit label names the reason:

| Label | Meaning |
|---|---|
| 🛡️ Stopped | The stop loss or trailing stop was hit |
| 🏆 Target | The take-profit target was hit (Set Target & Stop) |
| 📌 Changed | The engine's read on the ticker changed, so it left the trade |
| ⏳ Too Slow | The trade did not make progress in time |
| ⚠️ Cut Early | A stalled trade at or below its entry was closed early |
| 💤 Dead Trade | The price stopped moving, so the trade was closed |
| 🎯 Stretch Take | The price stretched far above normal while in profit, so the bot took it (Follow the Trend) |
| ⛔ Halt Triggered | The day's loss passed the Auto Stop-Loss for Bad Days limit |
| ⏱ EOD Close | Day Trading closed the trade before the market close |
| 🔴 Closed | Closed for another reason |

Hover any label for details of that trade.

---

## LITE: some numbers and settings show 🔒

Those are PRO features. LITE shows the signals, the exit reasons, the trend cloud, the price numbers
and Stock Fit. The stop line, prices on the labels, the performance numbers and all automation are
part of PRO.

---

## Still stuck?

Message me on TradingView and include:

- the ticker and timeframe,
- the version from the status line, for example `v1.0.114 (build 261008F)`,
- whether the status line shows `armed`,
- your Broker Platform setting,
- what your broker shows, if a position is involved.

Please never send your webhook URL, secret keys or broker keys.
