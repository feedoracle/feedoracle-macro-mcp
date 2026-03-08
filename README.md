# FeedOracle Macro Intelligence MCP

Real-time macroeconomic signals for AI trading agents. 13 MCP tools powered by 86 Federal Reserve (FRED) economic series — cryptographically signed, on-chain anchored, independently verifiable.

Before your agent allocates capital, adjusts leverage, or rebalances — it checks the macro environment.

## Quick Connect

```bash
# Claude Code
claude mcp add --transport http feedoracle-macro https://feedoracle.io/mcp/macro/sse

# Any MCP client
npx -y mcp-remote https://feedoracle.io/mcp/macro/sse
```

**No API key required** — public access, no registration.

---

## Agent M2M Auth (client_credentials)

Agents can authenticate without a browser:

```bash
# Register once
curl -s -X POST https://feedoracle.io/mcp/register \
  -H "Content-Type: application/json" \
  -d '{"client_name":"macro-agent","grant_types":["client_credentials"],"redirect_uris":["https://localhost"]}'

# Get token
curl -s -X POST https://feedoracle.io/mcp/token \
  -d "grant_type=client_credentials&client_id=fo_client_...&client_secret=fo_secret_...&scope=mcp:read"

# Connect with Bearer token
curl -N -H "Authorization: Bearer fo_cc_..." https://feedoracle.io/mcp/macro/sse
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

### Decision Layer

| Tool | What it does |
|------|-------------|
| `macro_regime` | Deterministic regime: RISK_ON / NEUTRAL / RISK_OFF / STRESS |

### Meta

| Tool | What it does |
|------|-------------|
| `ping` | Health, version, uptime |

---

## Response Schema — ES256K Signed

Every tool response is independently verifiable:

```json
{
  "tool": "macro_regime",
  "regime": "NEUTRAL",
  "risk_score": 30,
  "confidence": 0.79,
  "signature": {
    "alg": "ES256K",
    "kid": "feedoracle-mcp-es256k-1",
    "sig": "..."
  },
  "verify_url": "https://feedoracle.io/verify"
}
```

Verify without trusting FeedOracle:
```bash
curl -s https://feedoracle.io/.well-known/jwks.json
# → public key for independent verification
```

---

## Macro Regime Scoring

| Signal | RISK_ON | Normal | Elevated | STRESS |
|--------|---------|--------|----------|--------|
| VIX | < 15: −2 | 15–25: 0 | 25–35: +3 | > 35: +5 |
| Yield Spread 2s10s | > 1.0: −2 | 0–1.0: 0 | −0.5–0: +3 | < −0.5: +5 |
| Recession Probability | < 10%: −2 | 10–25%: 0 | 25–50%: +3 | > 50%: +5 |
| Credit Spread HY | < 3%: −1 | 3–5%: +1 | 5–7%: +3 | > 7%: +5 |
| Financial Stress | < −1: −2 | −1–0: 0 | 0–2: +3 | > 2: +5 |
| Fed Stance | CUT: −1 | HOLD: 0 | — | HIKE: +2 |
| Consumer Sentiment | > 80: −2 | 60–80: 0 | 40–60: +2 | < 40: +4 |

Score: 0–25 RISK_ON · 26–50 NEUTRAL · 51–75 RISK_OFF · 76–100 STRESS

---

## Data Sources

86 FRED series — Federal Reserve, US Treasury, BLS, BEA, Coinbase reference rates.

Full Treasury yield curve (1M–30Y) · SOFR/EFFR · CPI/PCE/PPI · VIX · Credit Spreads · 10 FX pairs · Gold/Silver/Oil · Housing · Consumer Sentiment

---

## Pricing

| Tier | Price | Calls/day |
|------|-------|-----------|
| Free | €0 | 100 |
| Starter | $99/mo or 99 USDC | 5,000 |
| Pro | $299/mo or 299 USDC | 25,000 |
| Enterprise | Custom | Unlimited |

Agents can upgrade autonomously via USDC on Polygon. See [Compliance Oracle README](https://github.com/feedoracle/feedoracle-mcp) for payment flow.

---

## The FeedOracle MCP Ecosystem

| Server | URL | Purpose |
|--------|-----|---------|
| **Compliance Oracle** | `https://feedoracle.io/mcp/` | MiCA/DORA + AI Evidence Layer (22 tools) |
| **Macro Oracle** (this) | `https://feedoracle.io/mcp/macro/` | 86 FRED series, macro regime engine |
| **Stablecoin Risk** | `https://feedoracle.io/mcp/risk/` | SAFE/CAUTION/AVOID operational risk |

> "May your agent trade this?" → Compliance Oracle  
> "Should your agent trade right now?" → **Macro Oracle**  
> "Is this stablecoin safe for settlement?" → Stablecoin Risk

---

🌐 [feedoracle.io](https://feedoracle.io) · ✅ [Verify](https://feedoracle.io/feedoracle_agent_verify.py) · 🏥 [Health](https://feedoracle.io/api/v1/macro/health)

**License:** Proprietary — © 2026 FeedOracle.
