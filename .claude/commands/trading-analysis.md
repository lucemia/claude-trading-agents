Extract the ticker symbol from $ARGUMENTS (e.g. "NVDA"). If no ticker is provided, ask the user for one before proceeding.

Set TODAY to the current date in YYYY-MM-DD format.

## Phase 1 — Parallel Data Analysis

Spawn these 3 subagents IN PARALLEL using the Agent tool (all at once, do not wait for one before starting the others). Each subagent has Bash access to fetch data.

**Subagent 1 — Technical Analyst:**
```
You are a technical analyst for $TICKER as of $TODAY.

Fetch data:
```bash
uv run --project /Users/davidchen/repo/TradingAgents python /Users/davidchen/repo/TradingAgents/scripts/fetch_market_data.py --ticker $TICKER --type technical --date $TODAY
```

Write a technical analysis report (150-200 words) covering:
- Trend: price vs EMA10, SMA50, SMA200 — bullish/bearish structure
- Momentum: RSI level, MACD direction and histogram
- Volatility: ATR relative to price, Bollinger band position
- Key levels: nearest support and resistance based on recent closes

End with exactly: TECHNICAL SIGNAL: BULLISH, BEARISH, or NEUTRAL
```

**Subagent 2 — News & Sentiment Analyst:**
```
You are a news and sentiment analyst for $TICKER as of $TODAY.

Fetch data:
```bash
uv run --project /Users/davidchen/repo/TradingAgents python /Users/davidchen/repo/TradingAgents/scripts/fetch_market_data.py --ticker $TICKER --type news --date $TODAY
```

Write a sentiment report (150-200 words) covering:
- Top 3 most impactful headlines and their market implications
- Overall sentiment: positive, negative, or mixed
- Any sector tailwinds/headwinds visible in the news
- Any earnings, guidance, or analyst signals

End with exactly: SENTIMENT SIGNAL: POSITIVE, NEGATIVE, or NEUTRAL
```

**Subagent 3 — Fundamentals Analyst:**
```
You are a fundamentals analyst for $TICKER as of $TODAY.

Fetch data:
```bash
uv run --project /Users/davidchen/repo/TradingAgents python /Users/davidchen/repo/TradingAgents/scripts/fetch_market_data.py --ticker $TICKER --type fundamentals --date $TODAY
```

Write a fundamentals report (150-200 words) covering:
- Valuation: trailing P/E, forward P/E, P/B vs sector norms
- Growth: revenue growth, earnings growth trajectory
- Quality: gross/operating margins, ROE, free cash flow
- Analyst consensus: mean recommendation (1=Strong Buy, 5=Sell), price target vs current price

End with exactly: FUNDAMENTAL SIGNAL: STRONG, FAIR, or WEAK
```

Wait for all 3 subagents to complete. Collect their full reports.

---

## Phase 2 — Parallel Bull/Bear Debate

Spawn these 2 subagents IN PARALLEL using the Agent tool.

**Subagent 4 — Bull Analyst:**
```
You are a bull analyst. Make the strongest BUY case for $TICKER using these research reports:

TECHNICAL REPORT:
[insert full technical report from Phase 1]

NEWS REPORT:
[insert full news report from Phase 1]

FUNDAMENTALS REPORT:
[insert full fundamentals report from Phase 1]

Write 150-200 words. Use specific data points. Address the most obvious bear objections head-on. Be direct and confident.

End with exactly: BULL CONVICTION: HIGH, MEDIUM, or LOW
```

**Subagent 5 — Bear Analyst:**
```
You are a bear analyst. Make the strongest SELL/AVOID case for $TICKER using these research reports:

TECHNICAL REPORT:
[insert full technical report from Phase 1]

NEWS REPORT:
[insert full news report from Phase 1]

FUNDAMENTALS REPORT:
[insert full fundamentals report from Phase 1]

Write 150-200 words. Use specific data points. Address the most obvious bull counterarguments. Be direct and skeptical.

End with exactly: BEAR CONVICTION: HIGH, MEDIUM, or LOW
```

Wait for both subagents to complete.

---

## Phase 3 — Portfolio Manager Decision

You are now the Portfolio Manager. Synthesize all 5 reports (technical, news, fundamentals, bull, bear) and output a decision in this EXACT format:

```
TICKER: [ticker]
DATE: [today]
SIGNAL: [BUY / SELL / HOLD]
RATING: [Overweight / Equal Weight / Underweight]
ENTRY: $[specific price or N/A]
STOP: $[specific price or N/A]
SIZE: [e.g. "3-5% of portfolio, add in 2-3 tranches" or "N/A"]

BULL: [one sentence — the single strongest bull argument]
BEAR: [one sentence — the single strongest bear argument]
VERDICT: [2-3 sentences explaining why the bull or bear case won and what specifically to do]
```

---

## Phase 4 — Post to Slack

Post the decision to both Slack channels using the mcp__claude_ai_Slack__slack_send_message tool.

Format the Slack message as:
```
*[TICKER]* — *[SIGNAL] ([RATING])*  |  Entry: [ENTRY]  |  Stop: [STOP]
  Size: [SIZE]
• 📈 [BULL one-liner from decision]
• ⚠️ [BEAR one-liner from decision]
• [VERDICT sentence 1]
• [VERDICT sentence 2]
```

Send to both channels:
1. `C0AH3D5P49J` — #investment (investment workspace)
2. `C0ATZ3N2X0S` — #gliaclaw-investment (livingbio workspace)

Finally, display the full PM decision to the user.
