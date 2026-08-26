# Before You Trade Real Money

Read this once, in full, at the point where you're about to connect a live account. Everything here
exists because skipping it is expensive.

Work top to bottom. Don't skip the arithmetic in step 3 — it's the step that decides whether you'll
still be running this in six months.

---

## 1. Run on paper for at least two weeks

Both TradersPost and most brokers offer a free paper account. Use one.

Two weeks is a minimum, not a target. It is roughly what it takes to see the bot behave across more
than one market mood — a trend, a chop, a gap, a bad day. One good week teaches you nothing you can
rely on.

**What you're watching for:**

- Orders arriving at the broker when the chart says they should
- Stops appearing at the broker alongside entries
- Exits closing positions cleanly, with nothing left resting afterwards
- The chart and the broker agreeing about what you hold, every time you look

**If any of those are inconsistent on paper, they will be inconsistent with real money.** Fix it
before you fund anything.

---

## 2. Check the four numbers on your ticker and style

On the exact ticker, timeframe and settings you intend to run:

- [ ] **Est. Annual Rtn** is positive
- [ ] **Profit Factor** is above 1.5
- [ ] **Max Drawdown** is a number you could sit through without intervening
- [ ] At least **20 trades** in the backtest

All four, or don't run it live on that pairing. Three out of four is not a pass — a great return over
six trades is noise, and a profit factor of 1.8 next to a drawdown that would make you quit is a
system you will abandon at the worst moment.

These are historical results over a limited window. They are evidence, not a forecast.

---

## 3. Do the losing-streak arithmetic

**This is the step people skip, and it's the one that matters most.**

Streaks of **eight to fourteen consecutive losing trades** have occurred in testing on tickers that
were profitable overall. A run of losses is a normal feature of a trend-following system, not a sign
that it has broken.

**Work out what fourteen consecutive losses at your Risk Per Trade would do to your account.**

| Risk Per Trade | Account after 14 consecutive losses |
|---|---|
| 0.5% | about 93% remaining |
| 1% | about 87% remaining |
| 2% | about 75% remaining |
| 3% | about 65% remaining |
| 5% | about 49% remaining |

**If that number would make you turn the bot off, lower your Risk Per Trade until it wouldn't.**

The trader who quits during a normal drawdown loses more than the trader who sized smaller and sat
through it. Pick the risk level whose worst case you can actually live with, then leave it alone.

---

## 4. Set your Account Balance honestly

**One chart:** type in your real account balance.

**Several charts:** split it. Four charts on a $20,000 account means $5,000 typed into each.

Every chart sizes as though it is the only thing touching your account. Tell all four they have
$20,000 and all four will trade full size, and you will risk four times what you intended — at the
exact moment several tickers signal together, which is when correlated markets move.

**Never put two charts on the same ticker.** They will both try to steer one position at your broker
and fight over its protective stop.

---

## 5. Verify your broker platform settings

For **TradersPost**, check every one of these on the strategy and its subscription. Each has a
specific silent failure mode:

- [ ] **Auto submit** — ON, or trades wait for manual approval
- [ ] **Allowed tickers** — includes your symbol
- [ ] **Allowed IP addresses** — set to **TradingView**
- [ ] **Short selling** — ON only if you trade shorts
- [ ] **Cancel open orders on exit** — ON, or protective orders are left resting on a flat account
- [ ] **Stop order time in force** — GTC, or your stop expires at the end of the day
- [ ] **Failure notifications** — ON, or a rejected order is invisible to you
- [ ] **Duplicate / repeated-signal protection** — ON

**Assume these can revert.** Re-read them after any change to the strategy, and re-read them once
more before you fund the account.

`Allowed IP addresses` is the one people leave open. Your webhook address is a credential — anyone
who obtains it can send orders to your account. Restricting the strategy to TradingView's servers is
what makes a leaked address useless.

---

## 6. Create the alert in the right order

**Set Broker Platform BEFORE you create the alert.** An alert is a frozen copy of the script and its
settings — one created while Broker Platform was on `Alerts Only` will send plain text forever and
can never place a trade, and nothing on the chart can warn you.

Before clicking Create, check the footer at the bottom of the indicator shows the word **`armed`**.
If it doesn't, stop and fix the settings first.

Full walkthrough: **[CREATING-ALERTS.md](CREATING-ALERTS.md)**

---

## 7. Watch the first week actively

Once real money is connected, check daily for the first week:

- [ ] Every entry on the chart has a matching filled order at your broker
- [ ] Every open position has a **protective stop resting** at the broker
- [ ] Every exit on the chart left the broker **flat**, with no orders still resting
- [ ] Nothing is holding a position the chart doesn't know about, or vice versa

**The broker is the only authority.** If the chart and the broker ever disagree, the broker is right.
Close the position at your broker manually, get flat, and only then recreate the alert.

---

## What you're signing up for

Market Axiom manages risk and enforces exits. It does not predict markets and it does not beat
buy-and-hold on most tickers over long periods.

It will have losing streaks. It will exit trades that would have recovered. It will sit out moves you
would have taken by hand. That is what a rules-based system does, and the discipline is the point —
but you have to want that trade-off before you fund it, not discover it afterwards.

**Never trade with money you cannot afford to lose.** Nothing here is financial advice, and past
results — backtested or live — do not guarantee anything about the future.
