# Finding Settings That Suit Your Ticker

The dashboard is a tuning tool, not a scoreboard. This page shows how to use it well.

---

## The question you are asking

**Is this ticker and this style a good pairing?** The dashboard answers that in about ten minutes.

The goal is not the highest Est. Annual Rtn you can find. If you change settings until that number
peaks, you find the settings that best fit the exact bars TradingView loaded. That is called
over-fitting. It makes a beautiful backtest and a disappointing account.

**The test:** a good setting still works when you move it a little. If Stop Width 1.2 looks great
and 1.1 and 1.3 both look poor, 1.2 is luck. If 1.1, 1.2 and 1.3 all look good, you have found
something real.

---

## Before you change anything

- **Start from the defaults.** They are the settings the product was measured on.
- **Use a 15-minute chart.** The defaults were validated on that timeframe.
- **Check the trade count first.** Below 20 trades, the other numbers are noise.
- **Change one thing at a time.** With two changes at once, you learn nothing about either.
- **Use the route you will trade.** Broker Platform changes the numbers, because each route follows
  its own rules. Tune with it already set.

---

## The order to try things

Work down this list and stop when you have a good pairing. The first items matter most.

### 1. Trading Style — the biggest lever

```
Day Trading → Fast Swing → Swing → Slow Swing → Fast Position → Position → Slow Position
```

Start at **Swing** and step outward in both directions. Watch Est. Annual Rtn, Profit Factor and
Signals / Day.

Look for a **range** of styles that works, not one winner. If Swing and Slow Swing both do well,
that tells you something real about the ticker's rhythm. Faster styles winning means the ticker
moves in short bursts. Slower styles winning means it trends and needs room.

### 2. Bot Score

**Bot Score** grades how the bot has actually done on this ticker, from S to D. It only gives a
high grade when there is enough evidence, and it needs 30 days of history before it shows a grade.

- **S, A or B:** worth more of your time.
- **D** on every style: that is your answer. Pick a different ticker.
- **C** often means there is not enough evidence yet, however good the numbers look.

**Stock Fit** (A to F) is not a performance grade. It shows how busy and lively the stock is. Many
A-grade stocks still do poorly with the bot, so judge results by Bot Score.

### 3. How to Close Trades

- **Follow the Trend** (trailing stop) suits tickers that trend. Winners run, and each trade gives
  back part of its peak.
- **Set Target & Stop** suits tickers that chop. It takes profit at a fixed target, four times the
  stop distance.

With a slower style, start with Follow the Trend. With a faster style, try both.

### 4. Signal Quality

**Aggressive** takes more trades. **Moderate** takes fewer and only takes trend trades in the
direction of the longer-term trend.

If the trade count is healthy but Profit Factor is weak, try Moderate. If the trade count is
already low, stay on Aggressive. On the slower styles, Moderate also needs more chart history before
it can start.

### 5. Stop Width — the smallest lever

Stop Width scales the stop distance, from 0.9 to 2.5 (default 1.0). It scales the entry stop, and
the trailing stop on trades taken by the Trend Follow engine. Wider means fewer stop-outs and larger single losses.
Tighter means the reverse.

Because it is a fine dial, this is where over-fitting happens most. Move it in steps of 0.1 or more,
and only with a reason, for example "normal noise on this ticker keeps stopping me out".

---

## What a good pairing looks like

- **Est. Annual Rtn** positive, and positive at nearby settings too
- **Profit Factor** above 1.5
- **Max Drawdown** you could sit through
- **At least 20 trades**, ideally many more
- **Signals / Day** that fits your life
- **Bot Score** of B or better

Then follow [GOING-LIVE.md](GOING-LIVE.md).

---

## Signs of over-fitting

Go back to the defaults if you see any of these:

- **The result falls apart when you nudge a setting.**
- **You have changed four or five things.**
- **The trade count fell as the return rose.** You removed losing trades, not improved the system.
- **You cannot say why the change should work.**

---

## What the dashboard cannot tell you

- **Whether the future will look like the past.** Every number comes from bars that already
  happened.
- **Whether the sample is typical.** If the loaded history was all one kind of market, the numbers
  describe that market.
- **How a drawdown will feel.** −25% on screen and −25% in your account are different experiences.

---

## When to stop

Stop when you have a good pairing, not the best one you can find. The gap between good and
"optimal" is mostly noise. If you have tried the styles, checked Bot Score and tried both exits,
and nothing passes, that ticker is not a fit for this engine. That is a useful answer. Try another
ticker.
