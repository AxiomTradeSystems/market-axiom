# MarketAxiom

A TradingView indicator that turns its signals into real broker orders.

---

## Most people don't need anything on this page yet

If you're using **TradersPost** — the default and recommended route — everything you need is in the
indicator's own settings plus the TradersPost setup below. There's nothing to install, nothing to
deploy, and no code to write.

| If you want to… | Read |
|---|---|
| Set up an alert, manual or automated | **[CREATING-ALERTS.md](CREATING-ALERTS.md)** |
| Connect real money | **[GOING-LIVE.md](GOING-LIVE.md)** |
| Find settings that suit your ticker | **[TUNING.md](TUNING.md)** |
| Fix something that looks wrong | **[TROUBLESHOOTING.md](TROUBLESHOOTING.md)** |
| Understand how it works | **[FAQ.md](FAQ.md)** |
| Know what a setting is really called | **[GLOSSARY.md](GLOSSARY.md)** |

Every setting is documented inside the indicator — hover the ⓘ beside it. Those tooltips ship with
the build, so they can never fall out of date.

The self-hosted advanced route and setup videos are in progress and will appear here as they're
finished. **Bookmark this page rather than saving a copy** — it's the one address that stays
current.

---

## Before you turn anything on

**Run it on a PAPER (fake money) account for at least two weeks.** Both TradersPost and most brokers
offer one free. This is not a formality — you are handing order placement to software, and two weeks
is the minimum needed to see it behave across a full range of market conditions.

The indicator can lose money. It has losing streaks. Nothing here is financial advice, and past
results never guarantee future ones. Read the full disclaimer in the script description before you
connect a live account.

---

## Setting up TradersPost

1. Create a TradersPost account and connect your broker inside it.
2. Create a **strategy** — one per symbol you want to trade.
3. Copy that strategy's **webhook URL**.
4. In the indicator settings, set **Broker Platform** to `TradersPost`.
5. Create the TradingView alert (see below) and paste the webhook URL into it.

### Required strategy settings

**The indicator cannot set these for you.** Each one has a specific failure mode if it's wrong.

| Setting | Value | What goes wrong otherwise |
|---|---|---|
| Duplicate / repeated-signal protection | **ON** | one signal can place two orders |
| Allow adding to positions | **OFF** | an unintended second entry on the same symbol |
| Short selling | **ON only if you trade shorts** | short signals silently do nothing |
| Cancel open orders on exit | **ON** | protective orders left resting on a flat account |
| Stop order time in force | **GTC** | your protective stop expires at the end of the day |
| Failure notifications | **ON** | **a rejected order is invisible to the chart — you would never know** |
| Allowed IP addresses | **TradingView** | anyone who obtains your webhook URL can trade your account |

That last row matters more than it looks. **The webhook URL is a credential.** Anyone holding it can
place orders in your account, and restricting the strategy to TradingView's own servers is what makes
a leaked URL useless. Leave it on `TradingView`.

### One strategy per symbol

Never point two strategies or two charts at the same ticker. They will both try to steer one position
and fight over its protective stop.

---

## Creating the TradingView alert

**Set `Broker Platform` in the indicator settings BEFORE you create the alert.** An alert created
while it is on `Alerts Only` sends plain text forever and can never place a trade, no matter what you
change on the chart afterwards.

Then:

- **Condition:** this indicator, **`Any alert() function call`**
- **Frequency:** Once Per Bar Close
- **Notifications:** paste your webhook URL into the Webhook URL box
- **Leave the Message row alone.** It only sets the alert's *name*, which never leaves TradingView.
  The payload comes from the script.

Before you click Create, check the status line at the bottom of the indicator. It should show the
build number and the word **`armed`**. If `armed` is missing, the alert would be created unable to
place orders.

---

## The most important thing to understand

**A TradingView alert is a frozen copy of the script and its settings at the moment you created it.**

- Changing a setting on your chart does **not** reach a running alert.
- Updating to a newer version of the indicator does **not** reach a running alert.
- The only way to change an alert is to **delete it and create a new one**.

And the safety rule that follows from it:

> **Never delete an alert while that symbol holds an open position at your broker.**
> Close the position at your broker first, then delete the alert. Deleting it while a trade is open
> can leave that position with nobody managing it.

---

## What the TradersPost route does and doesn't do

Worth knowing before you're surprised by it:

- Your broker holds the **entry stop**. The tightening trailing stop is shown on the chart but is
  not pushed to the broker on this route.
- Pyramiding (adding to winning trades) and extended-hours entries are **disabled** on this route.
- An exit that triggers pre-market or after-hours is held on the chart and sent on the next
  regular-hours bar.
- If TradingView restarts the alert while a trade is open, the bot sends a protective market exit to
  close the position, because it cannot re-attach to a trade in progress on this route.

---

## Everything else

- **[CREATING-ALERTS.md](CREATING-ALERTS.md)** — the full alert walkthrough, why order of operations
  matters, and the eight mistakes people make in the order they make them.
- **[GOING-LIVE.md](GOING-LIVE.md)** — the pre-flight checklist for connecting real money, including
  the losing-streak arithmetic you should do before you fund anything.
- **[TROUBLESHOOTING.md](TROUBLESHOOTING.md)** — symptom-first. The bot isn't trading, my numbers
  changed, my broker and the chart disagree, and the rest.
- **[TUNING.md](TUNING.md)** — how to use the dashboard to find out whether a ticker and style suit
  each other, and how to avoid fooling yourself while you do it.
- **[FAQ.md](FAQ.md)** — how it works, what to expect, risk and losing streaks, LITE vs full.
- **[GLOSSARY.md](GLOSSARY.md)** — plain-English names mapped to the standard industry terms, and an
  honest note about which parts are proprietary.
- **Settings** — documented in the indicator itself, on every ⓘ.

## Coming soon

- **Self-hosted route** — run your own webhook receiver instead of using TradersPost. Advanced;
  requires deploying a Cloudflare Worker yourself.
- **Setup walkthrough video**

---

## Support

Access and support are handled through TradingView. Request access on the script's page and message
me there.

---

*This repository contains documentation and, in future, the optional self-hosted receiver. It does
not contain the indicator itself, which is distributed only through TradingView.*
