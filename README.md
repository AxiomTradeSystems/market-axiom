# Market Axiom

A TradingView indicator that finds trades, manages them, and can place them at your broker for you.

Add it to a chart and you get buy and sell signals, a stop loss, position sizing and a live
performance dashboard. When you are ready, connect a broker and the same signals become real orders.

---

## Where to start

| You want to… | Read |
|---|---|
| Get it on a chart and understand what you see (10 minutes) | **[QUICK-START.md](QUICK-START.md)** |
| Automate it with TradersPost (no code) | **[TRADERSPOST.md](TRADERSPOST.md)** |
| Automate it with your own receiver and Alpaca (advanced) | **[SELF-HOSTED.md](SELF-HOSTED.md)** |
| Create an alert, manual or automated | **[CREATING-ALERTS.md](CREATING-ALERTS.md)** |
| Move from paper trading to real money | **[GOING-LIVE.md](GOING-LIVE.md)** |
| Find settings that suit your ticker | **[TUNING.md](TUNING.md)** |
| Fix something that looks wrong | **[TROUBLESHOOTING.md](TROUBLESHOOTING.md)** |
| Understand how it works | **[FAQ.md](FAQ.md)** |
| Look up what a setting or label means | **[GLOSSARY.md](GLOSSARY.md)** |

Every setting also has its own help inside the indicator. Hover the ⓘ beside it. That help ships
with each version, so it always matches the version you run.

**Bookmark this page instead of saving a copy.** It is the one address that stays current.

---

## Three ways to use it

The **Broker Platform** setting decides what the bot does with its signals.

| Broker Platform | What happens | Best for |
|---|---|---|
| **Alerts Only (no broker orders)** (default) | Signals on the chart and in TradingView alerts. No orders are sent. | Learning, and trading by hand |
| **TradersPost** | Each signal becomes an order at your broker through TradersPost. No code. | Most people who want automation |
| **Standard (Custom JSON)** | Each signal goes to your own receiver, which manages the trade at Alpaca. | Advanced users who want the most control |

### How the two automated routes compare

| | TradersPost | Self-hosted receiver |
|---|---|---|
| Setup | Sign up and paste a web address | Deploy a small Cloudflare service once |
| Brokers | Any broker TradersPost supports | Alpaca |
| Protective stop at the broker | Placed at entry and kept there | Placed at entry and moved up as the trailing stop tightens |
| Stop check | Your broker holds the stop | The receiver checks every 15 minutes that each position has a stop, and replaces a missing one |
| Add to Winning Trades | Not used on this route | Supported |
| Extended-hours trading | Not used on this route | Supported |

Both routes start the same way. Run the bot on a **paper (practice) account** for at least two
weeks, then follow [GOING-LIVE.md](GOING-LIVE.md).

---

## Three rules that apply everywhere

1. **An alert is a frozen copy.** When you create an alert, TradingView saves a copy of the script,
   its settings and the chart. Later changes do not reach it. To change an alert, delete it and
   create a new one. [CREATING-ALERTS.md](CREATING-ALERTS.md) explains this in full.
2. **Be flat before you delete an alert.** Close the position at your broker first, then delete the
   alert. A deleted alert can no longer manage the trade it opened.
3. **Your broker is the record.** If the chart and your broker ever disagree, the broker is right.

---

## Versions

Market Axiom updates through TradingView. Your chart uses the new version after you reload the page
or add the indicator again. The version number shows in the status line at the bottom of the
indicator. A running alert keeps the version it was created with until you create it again, while flat.
The release notes on the TradingView script page say what changed in each version.

---

## Support

Message me on TradingView to request access or to ask a question.
[TROUBLESHOOTING.md](TROUBLESHOOTING.md#still-stuck) lists what to include so I can answer in one reply.

---

*This repository holds the documentation and the optional self-hosted receiver. The indicator itself
is distributed only through TradingView. Nothing here is financial advice. Trading involves risk,
and past results do not guarantee future results.*
