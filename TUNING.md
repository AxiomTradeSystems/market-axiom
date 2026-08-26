# Finding Settings That Suit Your Ticker

The dashboard is a tuning instrument, not a scoreboard. This page is how to use it.

**Read this first, because it governs everything below.**

---

## What you are doing, and what you are not

**You are asking: is this ticker and this style a viable pairing at all?**

That is a yes-or-no question with an honest answer, and the dashboard can give it to you in about
ten minutes.

**You are not trying to maximise Est. Annual Rtn.**

If you keep changing settings until the number peaks, you have not found a better configuration —
you have found the settings that best fit the specific bars TradingView happened to load. That is
called over-fitting, and it produces a beautiful backtest and a disappointing live account. It is the
single most common way traders lose money with tools like this one.

**The tell:** a configuration that only works at one exact value, and falls apart if you nudge it,
is fitted to noise. A configuration that works across a range of nearby values has probably found
something real.

Test that deliberately. If Stop Width 1.2 looks great and 1.1 and 1.3 both look terrible, don't
trade 1.2 — you found an artefact.

---

## Before you change anything

**Start from the defaults.** They are the configuration the product was measured on. Anything you
change is a departure from tested ground, and you should know why you're making it.

**Use a 15-minute chart.** Every style produces its most consistent results there. Tuning on another
timeframe means tuning outside the tested configuration and the dashboard will flag it.

**Get a real sample.** Below 20 trades, the numbers are noise and you are reading tea leaves. Check
the trade count before you read anything else.

**Change one thing at a time.** Two changes at once and you learn nothing about either.

---

## The order to try things

Work down this list. Stop as soon as you have something viable — earlier changes matter far more
than later ones.

### 1. Trading Style — the biggest lever by far

This is the setting that moves the dashboard most, and it's the first thing to try when a ticker
looks bad.

```
Day Trading → Fast Swing → Swing → Slow Swing → Fast Position → Position → Slow Position
```

Start at **Swing** and walk outward in both directions. Watch how Est. Annual Rtn, Profit Factor and
Signals/Day respond.

**What you're looking for:** a *neighbourhood* that works, not a single winning value. If Swing and
Slow Swing are both decent, that's a real signal about the ticker's rhythm. If only Fast Swing works
and everything either side is poor, be suspicious.

**Reading it:** if faster styles do better, the ticker moves in short bursts. If slower ones do, it
trends and needs room. If nothing works at any speed, the ticker doesn't suit the engine — move on.

### 2. Check Stock Fit before spending more time

**Stock Fit** grades how well the ticker's measured behaviour suits the engine, A to F.

A **D or an F is your answer.** No amount of setting-changing rescues a ticker the engine can't read,
and time spent trying is time spent fitting noise. Pick a different ticker.

An **A or B** with poor numbers means the pairing is worth more attention — keep going.

### 3. How to Close Trades

Two exits, and they suit different markets:

- **Follow the Trend** (trailing stop) — better when the ticker trends. Lets winners run, gives back
  some of the peak on every trade.
- **Set Target & Stop** (bracket) — better when the ticker chops. Takes profit at a level instead of
  waiting for a reversal.

If your best Trading Style is one of the slower ones, try Follow the Trend. If it's one of the
faster ones, try both.

### 4. Signal Quality

**Aggressive** takes more trades. **Moderate** takes fewer and adds a longer-term trend filter to
trend-following entries.

If your trade count is healthy but Profit Factor is weak, Moderate may be filtering out the marginal
setups that are costing you. If your trade count is already thin, Moderate will make the sample too
small to read — leave it on Aggressive.

### 5. Stop Width — smallest lever, most over-fitted

Wider stops mean fewer stop-outs and bigger individual losses. Tighter means the reverse.

**This is where over-fitting happens most**, because it's a continuous number and there is always
some value that flatters the backtest. Move it in meaningful steps, not fine increments, and only
if you can articulate why — "this ticker's normal noise keeps stopping me out" is a reason,
"1.3 scored higher than 1.2" is not.

---

## What good actually looks like

Not "the highest number you found." A configuration worth trading:

- **Est. Annual Rtn positive** — and positive at nearby settings too, not just this one
- **Profit Factor above 1.5**
- **Max Drawdown you could sit through** without turning it off
- **At least 20 trades**, ideally many more
- **Signals / Day that matches your life** — a strategy firing six times a day is not viable if you
  check your phone twice
- **Stock Fit of A or B**

The full pre-flight is in **[GOING-LIVE.md](GOING-LIVE.md)**.

---

## Warning signs you've over-fitted

Stop and reset to defaults if you notice any of these:

- **The result collapses when you nudge a setting.** Real edges are robust to small changes.
- **You've changed four or five things.** Each one narrows the configuration onto this specific
  history. Go back to defaults and change one.
- **You're on an unusual Trading Style with a specific Stop Width on a ticker with mediocre Stock
  Fit.** That combination is a backtest artefact almost every time.
- **The trade count fell as the return rose.** You have probably excluded losing trades rather than
  improved anything. Twelve trades with a spectacular return tells you nothing.
- **You can't say why the change should work.** If there's no story about the ticker's behaviour
  behind it, it's a coincidence.

---

## Things the dashboard cannot tell you

**Whether the future resembles the past.** Every number is computed on bars that already happened.

**Whether you'll hold on.** A −25% max drawdown is a number on screen and an entirely different
experience in your account. Be honest about which one you're evaluating.

**Whether the sample is representative.** TradingView loads a limited window. If that window was all
bull market, you are looking at how the strategy does in a bull market.

**Anything about a route you aren't running.** Broker Platform changes the numbers, because the
TradersPost route holds outside-hours exits until the next regular session and that changes the trade
set. Tune on the route you'll actually use.

---

## When to stop

**Stop when you have a viable configuration, not when you have the best one you can find.**

The difference between a good configuration and the theoretically optimal one is mostly noise. The
difference between trading a robust one and abandoning an over-fitted one three weeks in is your
entire account.

If you've walked the Trading Styles, checked Stock Fit, tried both exits, and nothing clears the
bar — **that ticker isn't for this engine.** That is a real, useful answer, and it took you ten
minutes instead of three months. Try another symbol.
