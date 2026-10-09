# Automating with Your Own Receiver (Alpaca)

The self-hosted route sends each signal to a small service that you run in your own Cloudflare
account. That service, the **Axiom receiver**, places and manages the orders at Alpaca.

This is the route with the most control. You deploy the receiver once, and after that it runs on
its own.

---

## What the receiver adds

- **The stop at your broker follows the trailing stop.** With **Follow the Trend**, the receiver
  moves the protective stop at Alpaca up as the chart's trailing stop tightens.
- **A stop check every 15 minutes, day and night.** The receiver confirms that every open position
  has a protective stop at Alpaca, and places one if it is missing.
- **Add to Winning Trades** and **Trade Extended Hours** both work on this route.
- **Only your alerts can trade.** The receiver refuses any request that does not carry your secret
  key.
- **One receiver serves all your charts** on one Alpaca account.

---

## What you need

- A TradingView plan that supports webhook alerts
- An **Alpaca** account. Start with its **paper (practice)** account.
- A **Cloudflare** account on the **Workers Paid** plan. The receiver uses features that need it.
- A computer with **Node.js** (the current LTS version) to run Cloudflare's `wrangler` tool
- The receiver files from the `receiver` folder of this repository: `worker.js` and `wrangler.toml`

You need a command line for about ten minutes. Every command is below.

---

## Setup

### 1. Sign in to Cloudflare from the command line

Open a terminal in the `receiver` folder and run:

```
npx wrangler login
```

A browser window opens. Approve the access.

### 2. Create the storage space

```
npx wrangler kv namespace create WEBHOOK_KV
```

The command prints an `id`. Open `wrangler.toml`, find the `[[kv_namespaces]]` block, and paste
that id into its `id = ""` line.

### 3. Choose paper or live

In `wrangler.toml`, under `[vars]`, the line `ALPACA_BASE_URL` picks the account:

| Account | Value |
|---|---|
| Paper (start here) | `https://paper-api.alpaca.markets/v2` |
| Live | `https://api.alpaca.markets/v2` |

Paper and live accounts have different API keys. Use the keys that match this line.

### 4. Store your keys

Run each command and type the value when it asks. The values are stored in Cloudflare, encrypted,
and never appear in any file.

```
npx wrangler secret put ALPACA_API_KEY
npx wrangler secret put ALPACA_API_SECRET
npx wrangler secret put WEBHOOK_SECRET
npx wrangler secret put ADMIN_TOKEN
```

| Secret | What to type |
|---|---|
| `ALPACA_API_KEY`, `ALPACA_API_SECRET` | Your Alpaca API key pair, from the Alpaca dashboard |
| `WEBHOOK_SECRET` | A long password you make up. Letters and numbers only. You will type the same value into the indicator. |
| `ADMIN_TOKEN` | A second, different long password. Letters and numbers only. It unlocks the health check. |

The receiver refuses every order until `WEBHOOK_SECRET` is set.

### 5. Deploy

```
npx wrangler deploy
```

It prints the receiver's web address, for example
`https://alpaca-bridge.your-name.workers.dev`. Keep it for the alert.

**Always deploy from the command line.** The Cloudflare dashboard editor skips a setup step that the
receiver needs.

### 6. Check its health

```
curl -H "Authorization: Bearer YOUR_ADMIN_TOKEN" https://alpaca-bridge.your-name.workers.dev/health
```

The reply starts with `"status"`:

| Status | Meaning |
|---|---|
| `ok` | Ready |
| `degraded` | Working, with a note in `degraded_reasons`. Right after the first deploy, `audit_absent`, `audit_stale` and `ingress_never_recorded` are normal until the first 15-minute check runs and the first alert arrives. |
| `error` | A required piece is missing. `degraded_reasons` names it: `alpaca_keys_missing`, `webhook_secret_missing`, `kv_unbound`, `kv_rw_failed` or `do_unbound`. Fix it and deploy again. |

### 7. Set up the indicator

On each chart you will automate:

1. **Broker Platform** → `Standard (Custom JSON)`
2. **Webhook Secret Key** → the same value as your `WEBHOOK_SECRET`
3. **Account Balance** and **Share of Account** → see
   [GOING-LIVE.md](GOING-LIVE.md#4-set-the-money-each-chart-may-use)
4. **Alert Tag** (optional) → a short label, for example `acct1-tsla`, that the receiver records
   with each order
5. Click **OK** and check that the status line ends in `armed`.

### 8. Create the alert

Follow [CREATING-ALERTS.md](CREATING-ALERTS.md#alerts-for-automated-trading) and paste the
receiver's web address into the alert's Webhook URL box.

Your secret key travels inside the order message, which the script builds. It never goes in the
alert dialog.

---

## How the bot trades on this route

**Entries** are sent at the close of the signal bar. The receiver places the order at Alpaca, waits
for the fill, then places the protective stop.

**The protective stop** rests at Alpaca for the whole trade. With **Follow the Trend**, the receiver
moves it up as the chart's trailing stop tightens. It never moves down.

**Exits** close the position at Alpaca and cancel its resting orders.

**Extended hours.** With **Trade Extended Hours** on, and extended hours shown on the chart, the
bot also trades pre-market and after-hours. Orders outside regular hours are limit orders, as Alpaca
requires. Those sessions are thin, so some orders do not fill.

**Several tickers.** One receiver handles every chart you point at it. Use one chart and one alert
per ticker, and turn on **Share of Account** so each chart sizes from the real Alpaca balance.

---

## Updating the receiver

When a new receiver version is published here:

1. Replace `worker.js` in your `receiver` folder. Keep your own `wrangler.toml`.
2. Run `npx wrangler deploy`.
3. Run the health check. It reports the new version.

A receiver update does not touch your alerts. You do not need to recreate them, unless the release
notes say so.

Keep a copy of the previous `worker.js`. To go back, put it in place and deploy again.

---

## Keeping it secure

- Your `WEBHOOK_SECRET`, `ADMIN_TOKEN` and Alpaca keys live only in Cloudflare's secret store. Never
  put them in a file, a screenshot or a chat.
- If a secret may have leaked, set a new value with `npx wrangler secret put`. For
  `WEBHOOK_SECRET`, also type the new value into the indicator and recreate each alert while flat.
- The receiver's web address is safe to have in your alerts. Without the secret key, it accepts
  nothing.
