# Automating with TradersPost

TradersPost connects TradingView alerts to your broker. You do not write or deploy any code.
Market Axiom formats its orders for TradersPost, so the setup is a few settings and one web address.

---

## What you need

- A TradingView plan that supports webhook alerts
- A TradersPost account
- A broker account that TradersPost supports. Start with its **paper (practice)** account.

---

## Setup

### 1. Connect your broker in TradersPost

Create your TradersPost account and connect your broker's paper account inside it.

### 2. Create one strategy per ticker

In TradersPost, create a **strategy** for each ticker you will trade, and subscribe your broker
account to it. Copy the strategy's **webhook URL**.

Treat the webhook URL like a password. Anyone who has it can send orders to that strategy.

### 3. Set the strategy settings

Market Axiom cannot change these from TradingView. Set each one in the TradersPost strategy and its
subscription:

| Setting | Value | Why |
|---|---|---|
| Auto submit | **On** | Orders go through without waiting for your approval |
| Allowed tickers | **Your ticker** (or allow any) | Orders for other symbols are refused |
| Allowed IP addresses | **TradingView** | Only TradingView can use your webhook URL |
| Duplicate / repeated-signal protection | **On** | One signal places one order |
| Allow adding to positions | **Off** | Each ticker holds one position at a time |
| Short selling | **On only if you trade shorts** | Short signals need it |
| Cancel open orders on exit | **On** | No orders stay behind after a trade closes |
| Stop order time in force | **GTC** | The protective stop stays in place overnight |
| Failure notifications | **On** | You hear about any order the broker refuses |
| Enable trade locking (Retry settings) | **On** | Two signals seconds apart cannot both act on the same position |
| Cancel open order wait time | **120 seconds** (the default) | The exit cancels your stop before it sells; a longer wait can delay it |

### 4. Set up the indicator

Open the indicator settings on the chart for this ticker:

1. **Broker Platform** → `TradersPost`
2. **Account Balance** and, if you use it, **Share of Account** → see
   [GOING-LIVE.md](GOING-LIVE.md#4-set-the-money-each-chart-may-use)
3. Leave **Webhook Secret Key** and **Alert Tag** blank. TradersPost does not use them.
4. Click **OK** and check that the status line at the bottom of the indicator ends in `armed`.

### 5. Create the alert

Follow [CREATING-ALERTS.md](CREATING-ALERTS.md#alerts-for-automated-trading) and paste the
strategy's webhook URL into the alert's Webhook URL box.

### 6. Watch the first signals

When the first trade comes, check TradersPost's **Signals** page and **Trades** page, then your
broker. You should see the entry filled and a stop order resting under it.

---

## How the bot trades on this route

**Entries** are market orders, sent at the close of the signal bar.

**The protective stop** is sent with the entry and rests at your broker. It stays at the level set
at entry. With **Set Target & Stop**, the target also rests at your broker, as a limit order.
With **Follow the Trend**, you will also see a take-profit limit far above the price (about 10× the
entry price). TradersPost requires a target with every stop, so this one is a placeholder. Leave it
in place.

**The trailing stop** with **Follow the Trend** is managed by the chart. When the price reaches the
chart's trailing stop, the bot sends a market exit and TradersPost cancels the resting stop. The
stop at the broker is the floor under every trade.

**Exits** are market orders. With the default **Signal Speed**, `Wait for Bar Close`, they are sent
at the close of the bar that triggered them.

**The last bar of the day.** A bar that closes at the market close (4:00 PM Eastern, or 1:00 PM on
early-close days) counts as after-hours on this route, because an order sent then cannot fill until
the next session. An exit found on that bar is handled at the next session's first regular-hours
bar (at its close with `Wait for Bar Close`, which is 9:45 AM Eastern on a 15-minute chart):

- 🛡️ Stopped, 📌 Changed, ⛔ Halt Triggered and 🏆 Target go through then.
- ⏳ Too Slow, ⚠️ Cut Early, 💤 Dead Trade and 🎯 Stretch Take are checked again, and go through
  only if they are still true.

The stop (and target) resting at your broker keep working through the last bar as usual. If one
fills then, your broker shows the trade closed at once, and the chart records it the next morning.

**Add to Winning Trades** and **Trade Extended Hours** are not used on this route. The chart and
the dashboard follow these route rules, so the numbers you see describe this route.

**When the alert starts, and each time TradingView restarts it,** the bot sends one protective
exit to TradersPost, whether a trade is open or not. This route cannot pick up a trade in progress,
so it closes any position the bot may have lost track of. On New York stocks, funds and indexes the
exit waits for regular market hours (Monday to Friday, 09:30 to 15:58 New York time). The bot then
takes no new entry for 5 minutes, so your broker can finish the exit, and waits for its next signal.

This exit closes every share of that ticker in the account and cancels all its open orders, also
ones you placed by hand. Keep shares you trade by hand in a different account. On a flat account
TradersPost shows the exit as a rejected signal. That is normal.

---

## Several tickers

Use one chart, one alert and one TradersPost strategy for each ticker. Never put two charts on the
same ticker. Read [GOING-LIVE.md](GOING-LIVE.md#4-set-the-money-each-chart-may-use) to divide the
money between charts.

---

## When something looks wrong

TradersPost has three pages, and each one answers a different question:

- **Signals** — did the alert arrive?
- **Trades** — did it become an order at the broker?
- **Notifications** — only the events your notification settings cover.

A signal can arrive and still not become a trade. Check **Signals** and **Trades** directly.
[TROUBLESHOOTING.md](TROUBLESHOOTING.md) covers the common causes.
