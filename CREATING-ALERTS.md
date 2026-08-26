# Creating Alerts

Alerts are how Market Axiom reaches the outside world — to your phone, or to your broker. They are
also where almost every setup problem happens, and nearly all of those problems come from one
misunderstanding.

Read the first section before you touch anything.

---

## The one thing to understand first

**A TradingView alert is a frozen copy of the script, its settings, and the chart it was created on,
at the exact moment you clicked Create.**

That means:

- Changing a setting on your chart does **not** reach a running alert.
- Updating to a newer version of the indicator does **not** reach a running alert.
- Changing the chart's session — regular hours to extended hours — does **not** reach it either.

**The only way to change an alert is to delete it and create a new one.**

This is how TradingView works for every indicator. It isn't something Market Axiom chose, and there
is no setting that changes it.

**The consequence that catches people:** the chart has no way to know an alert exists, what settings
it froze, or whether it's still running. So the chart cannot warn you when an alert is
misconfigured — you have to get it right at creation time.

---

## Order of operations

**Configure the indicator completely, then create the alert. Never the other way round.**

1. Open the indicator settings and set everything the way you want it.
2. Set **Broker Platform** — this is the one that decides whether the bot can trade at all.
3. Close settings with **OK**.
4. Check the footer at the bottom of the indicator.
5. *Only then* create the alert.

### The footer check

The status line at the bottom of the indicator reads something like:

```
@AxiomTradeSystems v1.0.91 (build 260826D) · armed
```

**`armed` appears only when the bot is configured to send orders.** If you are setting up automation
and `armed` is missing, stop — the alert you're about to create would be unable to place a trade,
permanently.

If you only want notifications and no broker orders, `armed` should be absent. That's correct.

---

## Alerts for manual trading

Use these when you want to be told what to do and you'll place the orders yourself. Leave Broker
Platform on **Alerts Only**.

1. Click the **Alert** button (🔔) in the TradingView toolbar
2. **Condition** — change `Price` to **`Axiom`**
3. In the second dropdown, choose **`Any alert() function call`**
4. **Interval** — leave on `Same as chart`
5. **Expiration** — set to **Open-ended** if your TradingView plan offers it, so the alert doesn't
   stop on its own
6. **Message** — leave it. See below.
7. **Notifications** — pick what you want: app notification, email, sound, or any combination
8. Click **Create**

You'll receive messages like:

```
🟢 BUY TSLA @ $185.42 | Stop: $182.10 | Qty: 54
🛡️ Stopped TSLA @ $183.55 | P/L: -1.01%
```

---

## Alerts for automated trading

Same dialog, two differences: Broker Platform must be set first, and you turn on the Webhook
notification.

**Before you open the alert dialog:**

1. Set **Broker Platform** to `TradersPost` or `Standard (Custom JSON)`
2. If your platform needs one, enter your **Webhook Secret Key**
3. Click **OK**
4. **Confirm the footer shows `armed`**

**Then:**

5. Click the **Alert** button (🔔)
6. **Condition** — `Axiom`, then **`Any alert() function call`**
7. **Interval** — `Same as chart`
8. **Expiration** — **Open-ended** if available
9. **Notifications** — turn **Webhook** ON and paste your platform's webhook address into the URL box
10. Click **Create**

Repeat for every ticker you want to automate. **One chart, one alert, one ticker.**

---

## The dialog, field by field

**Condition.** Must be the indicator (`Axiom`) and **`Any alert() function call`**. Any other
condition sends the wrong thing or nothing at all.

**Interval.** `Same as chart`. The indicator is designed around the chart's timeframe; overriding it
here decouples the alert from what you tested.

**Expiration.** Set **Open-ended** if your plan offers it. Otherwise the alert stops on its own at
the expiration date and your bot goes quiet with no error anywhere — it simply stops appearing in the
log. If you can't set open-ended, note the date somewhere you'll see it.

**Message.** **Leave this alone.** For the `Any alert() function call` condition, the Message row
only sets the alert's **name** — the text you'd see in the Alerts Manager. It never leaves
TradingView and it has no effect on what gets sent. The actual payload comes from the script.

You can rename it if you find it useful for organising a long list. It changes nothing functional.

**Notifications.** For manual trading, whatever suits you. For automation, **Webhook ON** with your
platform's address in the URL box.

⚠️ **Never put your Webhook Secret Key into the alert.** It belongs in the indicator's settings,
where the script includes it in the payload itself.

---

## Verifying it worked

**1. Check the Alerts Manager.** Open the 🔔 panel on the right. Your alert should show a green
**Active** status. Anything else means it isn't firing:

| Status | Meaning |
|---|---|
| `Active` (green) | running normally |
| `Stopped — Expired` (red) | expiration passed — recreate it |
| `Stopped manually` (grey) | paused — restart or recreate |
| `Stopped — Triggered` (orange) | created as one-time — recreate without "Only Once" |

**2. Wait for a real signal.** Alerts fire on bar close, so nothing happens until a candle finishes
and a condition is met. On a 15-minute chart that can be a while, and on a quiet ticker it can be
days. Silence on the first afternoon is not evidence of a problem.

**3. Check the Log tab** once something has fired. Every triggered alert appears there with its
delivery status.

**4. For automation, check your broker.** Delivery to your platform is not the same as an order at
your broker. Look at the actual position and the actual resting stop.

---

## One alert per ticker

Never run two alerts on the same symbol, and never put two charts on one ticker.

Both would try to steer a single position at your broker and fight over its protective stop. The
result is unpredictable and can leave a position unprotected.

Several tickers is fine — a separate chart and a separate alert for each. Remember to **split your
Account Balance** between them, or every chart will size as though it owns the whole account.

---

## When to recreate an alert

**Every two to three weeks**, while you're **flat** on that ticker. Your Account Balance is frozen
into the alert at creation, so it drifts as your account grows or shrinks; recreating re-aligns it.
The bot also flags this once it's overdue.

**Whenever you change any setting** you want the bot to actually use. The running alert is still
using the settings it froze.

**After updating the indicator**, if you want the new build's behaviour on that lane.

## When NOT to recreate an alert

⚠️ **Never delete an alert while that ticker holds an open position at your broker.**

Close the position at your broker first, confirm you're flat, and only then delete and recreate.
Deleting an alert mid-trade can leave that position with nothing managing it — no trailing stop
updates, no exit signal, nothing.

Note also that **deleting an alert deletes its log history.** If you're investigating something, save
what you need before you delete.

---

## Reading the alert log

The Log tab shows every alert that fired and what happened to the delivery.

**`Webhook successfully delivered`** — your platform accepted it. This is not the same as an order
existing at your broker; check there for that.

**`Webhook delivery failed — request took too long and timed out`** — TradingView allows 3 seconds
for a response and gave up waiting. **This usually does not mean the instruction was lost.** It very
often arrived and executed anyway, seconds later. Check your broker before assuming anything.

**`Webhook delivery failed — couldn't find this domain`** — this one is real. The request never
reached your platform. If it was an entry or an exit, check your broker immediately.

---

## Common mistakes, in the order people make them

1. **Creating the alert before setting Broker Platform.** The alert freezes on `Alerts Only` and can
   never trade. No error, no warning, nothing in the log to explain it.
2. **Changing a setting and expecting the alert to follow.** It won't. Delete and recreate.
3. **Choosing the wrong condition.** It must be `Any alert() function call`.
4. **Editing the alert message box** expecting it to matter. It only sets the name.
5. **Two charts on one ticker.**
6. **Not splitting Account Balance** across multiple charts.
7. **Deleting an alert while a position is open.**
8. **Assuming a delivery failure means a lost trade.** Check the broker first.
