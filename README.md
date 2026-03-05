# FeedOracle Macro Intelligence

Real-time macroeconomic signals for AI trading agents. 13 MCP tools powered by 86 Federal Reserve (FRED) economic series.

Before your agent allocates capital, adjusts leverage, or rebalances a portfolio — it checks the macro environment.

## Quick Connect

```bash
npx -y mcp-remote https://feedoracle.io/mcp/macro/sse
```

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

---

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
| `macro_regime` | Deterministic regime: RISK_ON / NEUTRAL / RISK_OFF / STRESS. Score 0–100, 7 weighted signals, confidence, agent hint |

### Meta

| Tool | What it does |
|------|-------------|
| `ping` | Health, version, uptime |

---

## Example: `macro_regime` (Premium)

```json
{
  "regime": "NEUTRAL",
  "risk_score": 30,
  "confidence": 0.79,
  "engine_version": "macro-regime/1.0",
  "signals": {
    "vix":                   { "value": 18.63, "status": "NORMAL",      "weight": 0 },
    "yield_curve":           { "spread_2s10s": 0.6, "status": "FLAT",   "weight": 0 },
    "recession_probability": { "value": 10,    "status": "MODERATE",    "weight": 0 },
    "credit_spread_hy":      { "value": 2.98,  "status": "TIGHT",       "weight": -1 },
    "financial_stress":      { "value": -0.85, "status": "NORMAL",      "weight": 0 },
    "fed_stance":            { "status": "NEUTRAL", "rate": 3.64,       "weight": 0 },
    "consumer_sentiment":    { "value": 56.4,  "status": "PESSIMISTIC", "weight": 2 }
  },
  "agent_hint": "Mixed macro signals with no clear directional bias.",
  "next_catalyst": { "date": "2026-03-19", "type": "Meeting + SEP" }
}
```

---

## Regime Engine Scoring Rules

All rules are transparent and documented:

| Signal | RISK_ON | Normal | Elevated | STRESS |
|--------|---------|--------|----------|--------|
| VIX | < 15: −2 | 15–25: 0 | 25–35: +3 | > 35: +5 |
| Yield Spread 2s10s | > 1.0: −2 | 0–1.0: 0 | −0.5–0: +3 | < −0.5: +5 |
| Recession Probability | < 10%: −2 | 10–25%: 0 | 25–50%: +3 | > 50%: +5 |
| Credit Spread HY | < 3%: −1 | 3–5%: +1 | 5–7%: +3 | > 7%: +5 |
| Financial Stress | < −1: −2 | −1–0: 0 | 0–2: +3 | > 2: +5 |
| Fed Stance | CUT: −1 | HOLD: 0 | — | HIKE: +2 |
| Consumer Sentiment | > 80: −2 | 60–80: 0 | 40–60: +2 | < 40: +4 |

Regime mapping: 0–25 RISK_ON · 26–50 NEUTRAL · 51–75 RISK_OFF · 76–100 STRESS

---

## Data Sources

86 FRED series from: Federal Reserve · US Treasury · Bureau of Labor Statistics · Bureau of Economic Analysis · Coinbase (BTC/ETH reference)

Full Treasury yield curve (1M–30Y) · SOFR/EFFR/OBFR · CPI/PCE/PPI · VIX · Credit Spreads · 10 FX pairs · Gold/Silver/Oil/Copper · Housing · Consumer Sentiment

---

## Pricing

| Tier | Price | Calls/day | Regime Engine |
|------|-------|-----------|---------------|
| Free | €0 | 100 | ✗ |
| Developer | €49/mo | 5,000 | ✓ |
| Professional | €299/mo | 25,000 | ✓ + verified proofs |
| Enterprise | Custom | Unlimited | ✓ + SLA |

---

## The FeedOracle MCP Ecosystem

| Server | URL | Purpose |
|--------|-----|---------|
| **Compliance Oracle** | `https://feedoracle.io/mcp/` | MiCA/DORA regulatory data + AI Evidence Layer (22 tools) |
| **Macro Oracle** (this) | `https://feedoracle.io/mcp/macro/` | Fed/ECB economic indicators, 86 FRED series |
| **Stablecoin Risk** | `https://feedoracle.io/mcp/risk/` | 7-signal stablecoin operational risk (SAFE/CAUTION/AVOID) |

> "May your agent trade this?" → Compliance Oracle  
> "Should your agent trade right now?" → Macro Oracle (this server)  
> "Is this stablecoin safe for settlement?" → Stablecoin Risk

---

🌐 [feedoracle.io](https://feedoracle.io) · [API Docs](https://feedoracle.io/docs.html) · [Health](https://feedoracle.io/mcp/macro/health)
