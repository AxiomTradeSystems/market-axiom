# Creating Alerts

Alerts are how Market Axiom reaches you or your broker. TradingView runs them on its own servers,
so they keep working when your browser and computer are off.

---

## The one idea to understand first

**An alert is a frozen copy.** When you click Create, TradingView saves a copy of the script, its
settings and the chart (symbol, timeframe and session). The alert runs from that copy.

- Changing a setting on the chart does not reach the alert.
- A new version of the indicator does not reach the alert.
- To change an alert, delete it and create a new one, **while you are flat on that ticker**.

This is how TradingView works for every indicator. Once you know it, alerts are simple: set
everything first, then create the alert.

---

## Before you open the alert dialog

1. Open the indicator settings and set everything the way you want it.
2. For automation, set **Broker Platform** first. It decides what the alert sends.
3. Click **OK**.
4. Check the status line at the bottom of the indicator:

```
@AxiomTradeSystems v1.0.114 (build 261008F) · armed
```

`armed` shows when the bot is set to send orders to a broker. For automation it must be there.
For notifications only, it should not be.

---

## Alerts for manual trading

Leave **Broker Platform** on `Alerts Only (no broker orders)`. You get a message for each signal and you place the
orders yourself.

1. Click **Alert** in the top toolbar.
2. **Condition:** choose **Axiom** (the indicator), then **`Any alert() function call`**.
3. **Expiration:** choose **Open-ended** if your plan offers it.
4. **Notifications:** choose any mix of app, email and sound.
5. Click **Create**.

Messages look like this:

```
🟢 BUY TSLA | Qty: 54 @ $385.42 | Stop: $372.10 | Risk: $719.28
🛡️ Stopped TSLA | Sold 54 @ $380.55 | P/L: -1.26% (-$262.98)
```

LITE alerts name the signal and the ticker, and show 🔒 PRO in place of the prices.

---

## Alerts for automated trading

Set up your route first: [TRADERSPOST.md](TRADERSPOST.md) or [SELF-HOSTED.md](SELF-HOSTED.md).
Then, with `armed` showing in the status line:

1. Click **Alert** in the top toolbar.
2. **Condition:** choose **Axiom** (the indicator), then **`Any alert() function call`**.
3. **Expiration:** choose **Open-ended** if your plan offers it.
4. **Notifications:** turn on **Webhook URL** and paste your TradersPost strategy URL or your
   receiver's address.
5. Click **Create**.

Do this once for each ticker: **one chart, one alert, one ticker.**

---

## The dialog, field by field

**Condition.** Always **Axiom** (the indicator) and **`Any alert() function call`**. The script decides what
each message says and when it is sent.

**Interval.** Leave it on `Same as chart`.

**Expiration.** On plans with open-ended alerts, choose **Open-ended**. On other plans an alert
stops on its own after at most two months. Put the expiry date in your calendar and recreate the
alert, while flat, before that date.

**Message.** Leave it as it is. For this condition, the Message box only names the alert in your
alert list. The script writes the message that is sent.

**Webhook URL.** Your TradersPost strategy URL or your receiver's address. Your Webhook Secret Key
never goes here. On the self-hosted route the script puts it inside the message.

---

## Check that it works

1. **Alerts list.** Open the alerts panel on the right. Your alert should show a green **Active**
   status.

   | Status | Meaning |
   |---|---|
   | Active (green) | Running |
   | Stopped — Expired (red) | The expiration date passed. Recreate it. |
   | Stopped manually (gray) | Paused. Restart it or recreate it. |
   | Stopped — Triggered (orange) | It was set to fire once. Recreate it. |

2. **Wait for a signal.** The bot acts at bar close when a condition is met. On a 15-minute chart
   the first signal can take hours, or days on a quiet ticker.
3. **Check the alert log** after the first signal. Each fired alert is listed with its delivery
   result.
4. **For automation, check your broker.** You should see the filled entry and a stop resting under
   it.

---

## Reading the alert log

**Webhook successfully delivered** — your platform received it. Your broker shows what happened
next.

**Request took too long and timed out** — TradingView waits 3 seconds for a reply. The order very
often arrived and went through anyway, a few seconds later. Check your broker.

**Couldn't find this domain** — the message did not reach your platform. Check the webhook address,
then check your broker.

Deleting an alert also deletes its log. Save what you need first.

---

## When to recreate an alert

Always while flat on that ticker:

- **After you change a setting** that you want the alert to use.
- **After a new version is published**, to run the new version.
- **Every two to three weeks**, if **Share of Account** is off. Type your current balance into
  Account Balance first, so sizing keeps up with your account.
- **Before its expiration date**, on plans without open-ended alerts.

---

## How often alerts fire

TradingView stops any alert that fires more than 15 times in 3 minutes. Market Axiom sends far fewer
messages than that on its supported timeframes. If you run **Quick Exits** on a 1-minute or seconds
chart, check the alerts list now and then for a stopped alert.
