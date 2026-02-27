# FeedOracle Macro Intelligence

Real-time macroeconomic signals for AI trading agents. 13 MCP tools powered by 86 Federal Reserve (FRED) economic series.

Before your agent allocates capital, adjusts leverage, or rebalances a portfolio — it checks the macro environment.

## Quick Connect

```bash
npx -y mcp-remote https://feedoracle.io/mcp/macro/sse
```

### Claude Desktop

```json
{
  "mcpServers": {
    "feedoracle-macro": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://feedoracle.io/mcp/macro/sse"]
    }
  }
}
```

## 13 Tools

### Core Economic Signals

| Tool | What it does |
|------|-------------|
| `economic_health` | Health index 0–100: GDP, unemployment, inflation, confidence |
| `recession_risk` | Recession probability 0–100 + yield curve signal |
| `inflation_monitor` | CPI, PCE, PPI, rent, breakeven rates (5Y/10Y) |
| `labor_market` | Unemployment, payrolls, jobless claims |
| `gdp_tracker` | GDP growth rate + release schedule |

### Rates & Yield Curve

| Tool | What it does |
|------|-------------|
| `fed_watch` | Fed Funds Rate, EFFR, rate prediction, FOMC calendar |
| `yield_curve` | Full Treasury curve (1M–30Y), 2s10s spread, inversion signal |
| `defi_rates` | SOFR, EFFR, credit spreads, VIX, BTC/ETH, gold — one call |

### Markets & Sentiment

| Tool | What it does |
|------|-------------|
| `consumer_sentiment` | Michigan Sentiment, retail sales, consumer credit |
| `market_stress` | VIX, credit spreads (HY + IG), financial stress index |
| `safe_haven_flows` | Gold, USD index, BTC/ETH — risk-on vs risk-off |

### Decision Layer (Premium)

| Tool | What it does |
|------|-------------|
| `macro_regime` | **Deterministic regime classification:** RISK_ON / NEUTRAL / RISK_OFF / STRESS. Risk score 0–100, 7 weighted signals, confidence, agent hint. Engine: `macro-regime/1.0` |

### Meta

| Tool | What it does |
|------|-------------|
| `ping` | Health, version, uptime |

## Example: `defi_rates`

Everything a DeFi agent needs in one call:

```json
{
  "lending_rates": {
    "sofr": 3.67,
    "sofr_30d_avg": 3.67,
    "sofr_90d_avg": 3.73,
    "effr": 3.64
  },
  "yield_curve": {
    "spread_2s10s": 0.6,
    "recession_signal": false,
    "treasury_10y": 4.05
  },
  "volatility_stress": {
    "vix": 18.63,
    "credit_spread_high_yield": 2.98,
    "financial_stress_index": -0.85
  },
  "crypto_reference": {
    "btc_usd": 67068.86,
    "eth_usd": 2008.77
  },
  "safe_haven": {
    "gold_usd": 3250.20,
    "usd_index": 117.99
  }
}
```

## Example: `macro_regime` (Premium)

Deterministic macro classification — transparent rules, no ML blackbox:

```json
{
  "regime": "NEUTRAL",
  "risk_score": 30,
  "confidence": 0.79,
  "engine_version": "macro-regime/1.0",
  "signals": {
    "vix": { "value": 18.63, "status": "NORMAL", "weight": 0 },
    "yield_curve": { "spread_2s10s": 0.6, "status": "FLAT", "weight": 0 },
    "recession_probability": { "value": 10, "status": "MODERATE", "weight": 0 },
    "credit_spread_hy": { "value": 2.98, "status": "TIGHT", "weight": -1 },
    "financial_stress": { "value": -0.85, "status": "NORMAL", "weight": 0 },
    "fed_stance": { "status": "NEUTRAL", "rate": 3.64, "weight": 0 },
    "consumer_sentiment": { "value": 56.4, "status": "PESSIMISTIC", "weight": 2 }
  },
  "agent_hint": "Mixed macro signals with no clear directional bias.",
  "next_catalyst": { "date": "2026-03-19", "type": "Meeting + SEP" },
  "disclaimer": "Deterministic macro classification based on public economic data. Not investment advice."
}
```

### Regime Engine Scoring Rules

All rules are transparent and documented:

| Signal | < Threshold | Normal | Elevated | Extreme |
|--------|------------|--------|----------|---------|
| VIX | < 15: −2 | 15–25: 0 | 25–35: +3 | > 35: +5 |
| Yield Spread 2s10s | > 1.0: −2 | 0–1.0: 0 | −0.5–0: +3 | < −0.5: +5 |
| Recession Probability | < 10%: −2 | 10–25%: 0 | 25–50%: +3 | > 50%: +5 |
| Credit Spread HY | < 3%: −1 | 3–5%: +1 | 5–7%: +3 | > 7%: +5 |
| Financial Stress | < −1: −2 | −1–0: 0 | 0–2: +3 | > 2: +5 |
| Fed Stance | CUT: −1 | HOLD: 0 | — | HIKE: +2 |
| Consumer Sentiment | > 80: −2 | 60–80: 0 | 40–60: +2 | < 40: +4 |

Raw score → normalized 0–100 → Regime mapping:
- **0–25:** RISK_ON
- **26–50:** NEUTRAL
- **51–75:** RISK_OFF
- **76–100:** STRESS

## Built For

- **Trading agents** needing macro context before execution
- **DeFi yield agents** comparing SOFR vs on-chain rates
- **Portfolio rebalancing bots** monitoring recession risk
- **Risk management systems** tracking financial stress
- **AI copilots** in quantitative finance

## Data Sources

86 FRED series from: Federal Reserve · US Treasury · Bureau of Labor Statistics · Bureau of Economic Analysis · Coinbase (BTC/ETH reference)

Coverage: Full Treasury yield curve (1M–30Y) · SOFR/EFFR/OBFR · CPI/PCE/PPI · VIX · Credit Spreads · 10 FX pairs · Gold/Silver/Oil/Copper · Housing · Consumer Sentiment

## Pricing

| Tier | Price | Calls/day | Regime Engine |
|------|-------|-----------|---------------|
| Free | €0 | 100 | ✗ |
| Developer | €49/mo | 5,000 | ✓ |
| Professional | €299/mo | 25,000 | ✓ + verified proofs |
| Enterprise | Custom | Unlimited | ✓ + SLA + custom thresholds |

## Links

- [API Docs](https://feedoracle.io/docs.html#macro-oracle)
- [Health Endpoint](https://feedoracle.io/mcp/macro/health)
- [FeedOracle Compliance MCP](https://github.com/feedoracle/feedoracle-mcp) — the regulatory counterpart

## Also by FeedOracle

**[FeedOracle Compliance](https://github.com/feedoracle/feedoracle-mcp)** — MiCA regulatory preflight for AI agents. 18 tools covering 12 MiCA articles. PASS/WARN/BLOCK verdicts with signed evidence packs. *"May your agent trade this?"*

**FeedOracle Macro Intelligence** (this server) — *"Should your agent trade right now?"*

---

*FeedOracle — Evidence Infrastructure for Tokenized Markets*
