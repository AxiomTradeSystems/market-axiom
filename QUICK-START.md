# Quick Start

Ten minutes from nothing to a chart you can read. Nothing on this page sends an order anywhere.

---

## 1. Add the indicator

1. Open a TradingView chart.
2. Click **Indicators** in the top toolbar.
3. **Market Axiom (PRO):** open **Invite-only scripts** and click **Market Axiom**.
   **Market Axiom LITE:** search for "Market Axiom LITE" and click it.
4. Set the chart to **15 minutes**. The default settings were validated on that timeframe.

Try it first on **TSLA**, the product's default test ticker. Then try your own tickers.

---

## 2. Read the chart

**Trade labels.** Every trade the bot would have taken in the loaded history has two labels.

| Label | What it says |
|---|---|
| 🟢 **BUY 342 sh** · $380.29 · S:$359.38 | Bought 342 shares at $380.29. The protective stop was $359.38. |
| 🛡️ **Stopped** · −0.63% · ≈$378.01 | The trade closed at about $378.01, for −0.63%. |

The first word of an exit label is the reason the trade closed. [GLOSSARY.md](GLOSSARY.md#exit-reasons)
lists every reason. In LITE, the labels show 🔒 PRO in place of the prices.

**The stop line** (PRO) follows each open trade. With the default exit, **Follow the Trend**, it moves up
behind the price as the trade gains, and it never moves down.

**The cloud** shows the short-term trend. Green means up. Red means down. A wider cloud means a
stronger trend.

**The ⓘ chip** on the chart holds the chart key. Hover it to see what every color and mark means.

**The status line** at the bottom of the indicator shows the version, for example
`@AxiomTradeSystems v1.0.114 (build 261008F)`. When the bot is set to send orders to a broker,
it also shows the word `armed`.

---

## 3. Read the dashboard

The Strategy Report table grades this ticker with these settings, over the history TradingView
loaded. The four numbers to read first:

| Number | Good sign |
|---|---|
| **Est. Annual Rtn** | Positive |
| **Profit Factor** | Above 1.5 |
| **Max Drawdown** | A drop you could sit through without turning the bot off |
| **Bot Score** | B or better |

Hover the **Strategy Report** header for the full trade history, the best and worst trades, and
streaks. Hover any number for its explanation.

In LITE, the performance numbers show 🔒 PRO. The signals, the price numbers and Stock Fit stay
visible.

---

## 4. Set the three settings that matter first

Open the indicator settings (double-click the indicator name).

1. **Account Balance ($)** — the money this chart may trade with. Type your real paper or live
   balance. If you will run several charts, read [GOING-LIVE.md](GOING-LIVE.md#4-set-the-money-each-chart-may-use).
2. **Trading Style** — leave it on **Swing** to start. It is the validated default.
3. **Signal Quality** — leave it on **Aggressive** to start.

Leave everything else on its default for now. The defaults are the settings the product was
measured on.

---

## 5. Compare a few tickers

Change the symbol and watch the dashboard. Some tickers suit the engine and some do not.
**Bot Score** grades how the bot has actually done on this ticker. A D is a clear answer: use a different
ticker. A C can also mean there is not enough history yet. **Stock Fit** is different: it grades how busy and lively the stock itself is,
not how the bot does on it.

When you want to go further, [TUNING.md](TUNING.md) shows how to test settings without fooling
yourself.

---

## 6. Get notified of signals

Create a manual alert and TradingView will tell you about every entry and exit, on your phone or by
email. You place the orders yourself. [CREATING-ALERTS.md](CREATING-ALERTS.md#alerts-for-manual-trading)
has the steps.

---

## Next steps

- **Automate it:** [TRADERSPOST.md](TRADERSPOST.md) (no code) or [SELF-HOSTED.md](SELF-HOSTED.md)
  (your own receiver and Alpaca).
- **Practice first:** run any automation on a paper account for at least two weeks.
- **Questions:** [FAQ.md](FAQ.md).
