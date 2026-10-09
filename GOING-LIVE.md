# Going Live

The path from a paper (practice) account to real money. Work through it once, in order.

---

## 1. Run on paper for two weeks

TradersPost, Alpaca and most brokers offer a free paper account. Run your exact setup there for at
least two weeks. That is long enough to see a trend, a choppy stretch and a gap.

Each day, check that:

- every entry on the chart has a filled order at the broker,
- every open position has a protective stop resting at the broker,
- every exit left the broker flat, with no orders left behind,
- the chart and the broker agree on what you hold.

When all four hold for two weeks, the setup is ready.

---

## 2. Check the numbers for your ticker

On the exact ticker, timeframe and settings you will run:

- [ ] **Est. Annual Rtn** is positive
- [ ] **Profit Factor** is above 1.5
- [ ] **Max Drawdown** is a drop you could sit through
- [ ] The backtest has at least **20 trades**

All four together make a pairing worth trading. These numbers come from the history TradingView
loaded. They show how the setup behaved, not what it will do next.

---

## 3. Know your worst case before you fund

Every rules-based system has losing streaks. Streaks of 8 to 14 losing trades in a row have
happened in testing on tickers that were profitable overall. Decide now what you will sit through.

**On the default sizing, `Same % Every Trade`**, the loss on a stopped trade depends on the
position size and the stop distance. The Strategy Report shows what that meant on your ticker:

- **Max Drawdown** — the deepest fall from a high point.
- Hover the **Strategy Report** header for the **Worst Loss Streak** and the worst trade.

If those numbers are larger than you can accept, use a smaller Account Balance (or Share of
Account) for this chart.

**To set the loss per trade directly**, switch **How to Size Trades** to **Smart Sizing**. Then
**Risk Per Trade** sets the base loss when a stop is hit. The bot scales it from 0.25× to 2× with
setup quality. On the base value alone, 14 losses in a row leave:

| Risk Per Trade | Account left after 14 losses |
|---|---|
| 0.5% | about 93% |
| 1% (default) | about 87% |
| 2% | about 75% |
| 3% | about 65% |

Pick the level whose worst case you could live through, then leave it alone.

---

## 4. Set the money each chart may use

Two settings work together: **Account Balance** and **Share of Account**.

### Share of Account on (recommended for automation)

The bot sends your broker a percent of the account, and the broker turns it into shares from the
money that is really there. Sizing follows your balance up and down on its own.

- **Account Balance:** your **whole** account balance, the same on every chart.
- **Share of Account:** this chart's part. One chart: `100`. Four charts: `25` each. Keep the total
  at 100 or less. A total under 100 leaves room while other charts hold open trades.

### Share of Account off (`0`, the default)

The bot sends your broker a share count, sized from Account Balance.

- **Account Balance:** the money **this chart** may use. Four charts on a $20,000 account: type
  `5000` into each.
- Every two to three weeks, while flat, type your current balance and recreate the alert.

### Either way

- The minimum is $500. On stocks and ETFs the bot buys whole shares, so a chart that cannot afford
  one share skips the trade.
- **Capital Buffer** (default 0.25%) keeps a small part of each chart's money aside for fill
  differences.
- One ticker per chart. Two charts on one ticker would steer one position from two places.

---

## 5. Switch the route to your live account

**TradersPost:** subscribe your live broker account to the strategy, and check every setting in
the [strategy settings table](TRADERSPOST.md#3-set-the-strategy-settings) again on the live
subscription.

**Self-hosted receiver:** set `ALPACA_BASE_URL` to the live address, store your **live** Alpaca keys
with `npx wrangler secret put`, deploy, and run the health check.
[SELF-HOSTED.md](SELF-HOSTED.md#3-choose-paper-or-live) has the details.

---

## 6. Create the live alerts

While flat on each ticker, delete the paper alert and create the live one.
[CREATING-ALERTS.md](CREATING-ALERTS.md#alerts-for-automated-trading) has the steps. Check that the
status line shows `armed` before you click Create.

---

## 7. Watch the first week

Check once a day, the same four things as on paper:

- [ ] Every entry has a filled order at your broker
- [ ] Every open position has a protective stop resting at your broker
- [ ] Every exit left your broker flat
- [ ] The chart and your broker agree

If they ever disagree, your broker is right. Close the position at the broker, then recreate the
alert while flat.

---

## What to expect

Market Axiom enters on rules, sizes every trade and always has an exit plan. It will have losing
streaks, it will sometimes exit a trade that later recovers, and it will skip some moves. That
discipline is the point: the rules run the same way on good days and bad ones.

Trade only with money you can afford to lose. Nothing here is financial advice, and past results,
backtested or live, do not guarantee future results.
