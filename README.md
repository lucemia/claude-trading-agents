# claude-trading-agents

Multi-agent trading analysis as a Claude Code slash command — powered entirely by your Claude subscription, no external LLM API cost.

## How it works

Type `/trading-analysis NVDA` in Claude Code and it spawns a full analyst pipeline:

```
Phase 1 (parallel):  Technical Analyst + News Analyst + Fundamentals Analyst
Phase 2 (parallel):  Bull Analyst + Bear Analyst
Phase 3:             Portfolio Manager → BUY / SELL / HOLD decision
Phase 4:             Post to Slack
```

Each phase uses Claude Code's native `Agent` tool to run subagents in parallel. Data is fetched for free via [yfinance](https://github.com/ranaroussi/yfinance). No OpenAI, Anthropic, or any other LLM API key required.

## Installation

**1. Clone and install dependencies**

```bash
git clone https://github.com/lucemia/claude-trading-agents
cd claude-trading-agents
uv sync
```

**2. Install the slash command**

```bash
mkdir -p ~/.claude/commands
cp .claude/commands/trading-analysis.md ~/.claude/commands/
```

**3. Update the data script path in the skill file**

Edit `~/.claude/commands/trading-analysis.md` and replace `/Users/davidchen/repo/claude-trading-agents` with your local path.

**4. (Optional) Configure Slack**

Copy `.env.example` to `.env` and add your Slack bot token. The skill posts via Claude's Slack MCP connector — update the channel IDs in the skill file to match your workspace.

## Usage

```
/trading-analysis NVDA
/trading-analysis GOOG
/trading-analysis SOXL
```

## What you get

```
TICKER: NVDA
DATE: 2026-05-02
SIGNAL: BUY
RATING: Overweight
ENTRY: $199-201
STOP: $183

BULL: Forward P/E of 17.8x for 96% earnings growth with 71% gross margins is cheap.
BEAR: Google and Amazon are building competing chips while being NVDA's largest customers.
VERDICT: Fundamentals overwhelm near-term noise. Buy in two tranches, stop below SMA200.
```

## Daily Cron (optional)

Run analysis automatically every weekday morning and post to Slack:

```bash
cp scripts/run_daily.sh.example scripts/run_daily.sh
# edit tickers and paths
chmod +x scripts/run_daily.sh
crontab -e
# add: 0 9 * * 1-5 /path/to/claude-trading-agents/scripts/run_daily.sh >> /tmp/trading.log 2>&1
```

## Data sources

All data is fetched via [yfinance](https://github.com/ranaroussi/yfinance) — free, no API key required.

- **Technical**: price, EMA10, SMA50, SMA200, RSI14, MACD, Bollinger Bands, ATR
- **News**: latest headlines and summaries
- **Fundamentals**: P/E, P/B, margins, ROE, FCF, analyst consensus

## Requirements

- [Claude Code](https://claude.ai/code) with an active subscription
- Python 3.10+
- [uv](https://github.com/astral-sh/uv)

## License

MIT
