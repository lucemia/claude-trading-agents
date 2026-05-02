# claude-trading-agents

Multi-agent trading analysis as a Claude Code plugin — powered entirely by your Claude subscription, no external LLM API cost.

## Install

```
/plugin marketplace add lucemia/claude-trading-agents
/plugin install claude-trading-agents
```

Then install the Python data dependency (fetches market data via yfinance):

```bash
git clone https://github.com/lucemia/claude-trading-agents
cd claude-trading-agents
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
Phase 2 (sequential):  Bull makes case → Bear rebutts Bull directly
Phase 3:               Research Manager synthesizes debate
Phase 4:               Trader sets entry / stop / size
Phase 5:               Portfolio Manager → final BUY / SELL / HOLD
```

Each phase uses Claude Code's native `Agent` tool to run subagents. Data is fetched for free via [yfinance](https://github.com/ranaroussi/yfinance) — no OpenAI, Anthropic, or any other LLM API key required.

## Sample output

```
TICKER: NVDA
DATE: 2026-05-02
SIGNAL: BUY
RATING: Overweight
ENTRY: $199–201
STOP: $183
SIZE: 3–5% of portfolio, add in 2 tranches

BULL: Forward P/E of 17.8x for 96% earnings growth with 71% gross margins is cheap.
BEAR: Google and Amazon are building competing chips while being NVDA's largest customers.
VERDICT: Fundamentals overwhelm near-term noise. Buy in two tranches, stop below SMA200.
```

## Data sources

All data via [yfinance](https://github.com/ranaroussi/yfinance) — free, no API key required.

| Feed | Indicators |
|------|-----------|
| Technical | Price, EMA10, SMA50, SMA200, RSI14, MACD, Bollinger Bands, ATR |
| News | Latest headlines and summaries |
| Fundamentals | P/E, P/B, margins, ROE, FCF, analyst consensus, price targets |
| Macro | S&P 500, 10Y Treasury, Gold, Oil news headlines |

## Requirements

- [Claude Code](https://claude.ai/code) with an active subscription
- Python 3.10+, [uv](https://github.com/astral-sh/uv) (or pip)

## Manual install (alternative)

If you prefer not to use the plugin system:

```bash
git clone https://github.com/lucemia/claude-trading-agents
cd claude-trading-agents
uv sync
mkdir -p ~/.claude/commands
cp .claude/commands/trading-analysis.md ~/.claude/commands/
```

Then update the script path in `~/.claude/commands/trading-analysis.md` to match your local clone path.

## Inspiration

Inspired by [TradingAgents](https://github.com/TauricResearch/TradingAgents) — a multi-agent LLM trading framework by Tauric Research. This project adapts their analyst pipeline design as a native Claude Code plugin, replacing the LangGraph/OpenAI stack with Claude's built-in `Agent` tool and yfinance for free market data.

## License

MIT
