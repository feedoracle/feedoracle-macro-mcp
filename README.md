<div align="center">

# FeedOracle Macro Intelligence MCP

**Real-time macroeconomic signals for AI agents.**

86 FRED + 20 ECB indicators. Signed. Verifiable. Regime classification.

[![Server](https://img.shields.io/badge/server-live-10B898?style=flat-square)](https://feedoracle.io/mcp/macro/health)
[![Tools](https://img.shields.io/badge/tools-13-3B82F6?style=flat-square)](https://feedoracle.io/mcp.html)
[![Free](https://img.shields.io/badge/free-100_calls%2Fday-10B898?style=flat-square)](https://feedoracle.io/pricing.html)

[Website](https://feedoracle.io) Â· [All MCP Tools](https://feedoracle.io/mcp.html) Â· [Docs](https://feedoracle.io/docs.html) Â· [Main Repo](https://github.com/feedoracle/feedoracle-mcp)

</div>

---

## What this does

Before your AI agent allocates capital, adjusts leverage, or rebalances â€” it checks the macro environment. Every response is cryptographically signed (ES256K) and independently verifiable.

## Quick start

```bash
claude mcp add --transport sse feedoracle-macro https://feedoracle.io/mcp/macro/sse
```

No API key needed. Then ask:

```
What is the current macro regime? Should my agent be risk-on or risk-off?
```

## 13 tools

| Tool | Description |
|------|-------------|
| `economic_health` | Health index 0-100: GDP, unemployment, inflation |
| `recession_risk` | Recession probability + yield curve signal |
| `inflation_monitor` | CPI, PCE, PPI, breakeven rates |
| `labor_market` | Unemployment, payrolls, jobless claims |
| `gdp_tracker` | GDP growth rate + release schedule |
| `fed_watch` | Fed Funds Rate, EFFR, FOMC calendar |
| `yield_curve` | Full Treasury curve 1M-30Y, spreads |
| `defi_rates` | SOFR, credit spreads, VIX, BTC/ETH, gold |
| `consumer_sentiment` | Michigan Sentiment + retail sales |
| `market_stress` | VIX, credit spreads, financial stress index |
| `safe_haven_flows` | Gold, USD index, BTC/ETH reference |
| `macro_regime` | RISK_ON / NEUTRAL / RISK_OFF / STRESS |
| `ping` | Health check |

## Regime scoring

| Score | Regime | Meaning |
|-------|--------|---------|
| 0-25 | RISK_ON | Favorable conditions |
| 26-50 | NEUTRAL | Mixed signals |
| 51-75 | RISK_OFF | Elevated caution |
| 76-100 | STRESS | Crisis conditions |

7 weighted signals: VIX, yield curve, recession probability, credit spreads, financial stress, Fed stance, consumer sentiment.

## The FeedOracle ecosystem

| Server | Tools | Purpose |
|--------|-------|---------|
| [Compliance Oracle](https://github.com/feedoracle/feedoracle-mcp) | 27 | MiCA, DORA, evidence packs, audit trail |
| **Macro Intelligence** (this) | 13 | Fed/ECB indicators, regime classification |
| [Stablecoin Risk](https://github.com/feedoracle/-feedoracle-mcp-risk) | 13 | 7-signal operational risk scoring |

---

<div align="center">

**FeedOracle turns compliance documents into compliance evidence.**

[feedoracle.io](https://feedoracle.io) Â· *Evidence by Design.*

</div>
