# trading-agents-plugin

Multi-agent trading analysis as a Claude Code plugin — powered entirely by your Claude subscription, no external LLM API cost.

## Install

```
/plugin marketplace add lucemia/trading-agents-plugin
/plugin install trading-agents-plugin
```

Then install the Python data dependency (fetches market data via yfinance):

```bash
git clone https://github.com/lucemia/trading-agents-plugin
cd trading-agents-plugin
uv sync   # or: pip install yfinance pandas
```

## Usage

```
/trading-analysis NVDA
/trading-analysis GOOG
/trading-analysis SOXL
```

## Pipeline

```
Phase 1 (parallel):    Technical + News + Fundamentals + Macro analyst
Phase 2 (sequential):  Bull → Bear rebuts Bull directly → Risk Analyst stress-tests both
Phase 3:               Research Manager synthesizes all three voices
Phase 4:               Trader sets entry / stop / size anchored to technicals
Phase 5:               Portfolio Manager → final BUY / SELL / HOLD
```

7 subagents total. Each phase uses Claude Code's native `Agent` tool. Data is fetched for free via [yfinance](https://github.com/ranaroussi/yfinance) — no OpenAI, Anthropic, or any other LLM API key required.

## What each analyst covers

| Analyst | Coverage |
|---------|---------|
| **Technical** | EMA10/SMA50/SMA200 trend, RSI14, MACD, Bollinger Bands, ATR, key support/resistance |
| **News** | Top headlines, sentiment, sector tailwinds/headwinds, earnings signals |
| **Fundamentals** | P/E, forward P/E, PEG, P/B, revenue/earnings growth, margins, ROE, FCF, D/E ratio, current ratio, beta, short ratio, quarterly income/balance sheet statements, analyst consensus |
| **Macro** | S&P 500, 10Y Treasury, gold, oil news — geopolitical/monetary policy signals |
| **Bull** | Strongest buy case using all Phase 1 data |
| **Bear** | Directly rebuts Bull's specific claims with data |
| **Risk** | Stress-tests both sides — crowding risk, beta drawdown scenarios, tail risks neither Bull nor Bear addressed |

## Sample output

```
TICKER: NVDA
DATE:   2026-05-02
SIGNAL: BUY
RATING: Overweight
ENTRY:  $197–$199 (tranche 1); $188–$190 (tranche 2 if macro deteriorates)
STOP:   $183 (below SMA200)
SIZE:   2–3% of portfolio initial; reserve 2% for second tranche

BULL: PEG 0.18, FCF $58.1B, net cash $51B, and 57 analysts pricing in 35% upside —
      NVDA's AI dominance is structurally mispriced.
BEAR: Beta 2.33 in RISK-OFF macro with crowded longs means a guidance miss could
      trigger a 20%+ unwind against a 30.7x P/B backdrop.
VERDICT: Bull wins on fundamentals — no operational weakness identified. Risk Analyst
         flagged crowding and ~23% beta drawdown scenario, reducing sizing to 2–3%
         initial with staged second entry near SMA50.
```

## Requirements

- [Claude Code](https://claude.ai/code) with an active subscription
- Python 3.10+, [uv](https://github.com/astral-sh/uv) (or pip)

## Manual install (alternative)

If you prefer not to use the plugin system:

```bash
git clone https://github.com/lucemia/trading-agents-plugin
cd trading-agents-plugin
uv sync
mkdir -p ~/.claude/commands
cp .claude/commands/trading-analysis.md ~/.claude/commands/
```

Then update the script path in `~/.claude/commands/trading-analysis.md` to match your local clone path.

## Inspiration

Inspired by [TradingAgents](https://github.com/TauricResearch/TradingAgents) — a multi-agent LLM trading framework by Tauric Research. This project adapts their analyst pipeline design as a native Claude Code plugin, replacing the LangGraph/OpenAI stack with Claude's built-in `Agent` tool and yfinance for free market data.

## License

MIT
