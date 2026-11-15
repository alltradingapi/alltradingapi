<div align="center">

# All Trading API

**Market-data infrastructure for Quotex — candles, live prices, history, signals and analyzers over plain HTTP.**

<a href="https://alltradingapi.com">
  <img src="https://img.shields.io/badge/website-alltradingapi.com-1f6feb?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Website">
</a>
<img src="https://img.shields.io/badge/endpoints-14-3fb950?style=for-the-badge" alt="14 endpoints">
<img src="https://img.shields.io/badge/free_tier-yes-3fb950?style=for-the-badge" alt="Free tier">
<img src="https://img.shields.io/badge/auth-API_key-d29922?style=for-the-badge" alt="API key auth">
<img src="https://img.shields.io/badge/access-read--only-8b949e?style=for-the-badge" alt="Read-only">

</div>

---

## Why this exists

Quotex publishes no official public API and issues no developer keys. Getting clean, programmatic access to its market data means reverse-engineering a browser session, babysitting sockets and rebuilding the same candle parser on every project.

All Trading API does that once, and serves the result as ordinary JSON over HTTPS.

- **One endpoint, one product.** Pick only the data you need — candles, signals, screenshots, analyzers.
- **Plain HTTP + JSON.** Works from Python, PHP, Node, Go, a spreadsheet, or `curl`. No SDK lock-in.
- **Read-only by design.** The desk serves market data. It never connects to a broker account, places orders or holds positions.
- **Private keys, gated access.** Each product issues its own key after approval, passed as `?key=ap_your_key`.
- **Free tier included.** `/qx_timeframes` costs nothing, so you can wire up the auth flow before paying for anything.

---

## Live endpoints

Base URL — `https://alltradingapi.com`

| Product | Endpoint | Price |
| :--- | :--- | ---: |
| Quotex Timeframes | `/qx_timeframes` | **Free** |
| Quotex Candles | `/qx_candles` | $15/mo |
| Quotex Candle History | `/qx_history` | $15/mo |
| Quotex Saved Candles | `/qx_saved` | $20/mo |
| Quotex Assets | `/qx_assets` | $10/mo |
| Quotex Asset Check | `/qx_check` | $3/mo |
| Quotex Signals | `/qx_signal` | $30/mo |
| Quotex Future Signals | `/qx_future` | $30/mo |
| Quotex Results | `/qx_result` | $15/mo |
| Quotex Screenshots | `/qx_ss_pro_v2` | $10/mo |
| Quotex Analyzer | `/qx_analyzer` | $30/mo |
| Quotex Analyzer v4 | `/qx_analyzer_v4` | $30/mo |
| Quotex Analyzer v5 | `/qx_analyzer_v5` | $30/mo |
| Quotex Market Trend | `/qx_market_trend` | $30/mo |

---

## Quick start

**1. Get a key.** Choose a product on the desk and wait for approval — your key arrives in the dashboard.

**2. Call it.** Two candles of `1m` history, no SDK required:

```bash
curl "https://alltradingapi.com/qx_candles?asset=EURUSD_otc&hours=2&timeframe=1m&timezone=-3&key=YOUR_API_KEY"
```

**3. Read JSON.** Example from `/qx_market_trend`:

```json
{
  "success": true,
  "engine_version": "v6.0-unified-snr-safety",
  "asset_info": {
    "asset": "USDMXN_otc",
    "market_type": "otc",
    "payout": 91,
    "is_open": true,
    "current_price": 19.9165,
    "timeframes": ["1m", "5m", "15m"],
    "candles_analyzed": 300
  },
  "executive_summary": {
    "market_tradeability_score": 53.5,
    "can_trade_market": false
  }
}
```

Every response carries `success`, so a single check covers error handling:

```bash
curl -s "...&key=$KEY" | jq '.success, .asset_info.payout'
```

---

## Built for load

```
client ──HTTPS──> alltradingapi.com (API layer, key gating)
                        │
                        ├── HTTP/JSON market-data feed
                        └── stream centre (SSE + WebSocket)
```

<sub>

`PHP` · `Python` · `Nginx` · `WebSocket` · `SSE` · `JSON`

</sub>

---

## Roadmap

Quotex is live. The same product set is specified for four more brokers — catalogued, not yet connected:

| Broker | Quotes | Assets | Realtime | Screenshots |
| :--- | :--- | :--- | :--- | :--- |
| Binolla | Planned | Planned | Planned | Planned |
| Pocket Option | Planned | Planned | Planned | Planned |
| IQ Option | Planned | Planned | Planned | Planned |
| Deriv | Planned | Planned | Planned | Planned |

---

<div align="center">

### Start with the free endpoint

<a href="https://alltradingapi.com">
  <img src="https://img.shields.io/badge/Open_alltradingapi.com-1f6feb?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Open alltradingapi.com">
</a>

</div>

---

<sub>

**Disclaimer.** All Trading API is an independent service. It is not affiliated with, endorsed by or sponsored by Quotex, Binolla, Pocket Option, IQ Option or Deriv, and it is not a financial advisor. All market data is provided as-is for research and tooling purposes, without warranty of accuracy or fitness for trading. Nothing here constitutes financial advice. Trading involves substantial risk of loss.

</sub>
