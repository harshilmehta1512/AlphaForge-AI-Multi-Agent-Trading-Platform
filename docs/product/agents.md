# First-Phase Agents

This document defines every agent and supporting service required for the first-phase end-to-end AlphaForge product.

The purpose of this document is to remove ambiguity. A teammate should be able to read this file and know:
- which agents exist in phase one
- what each one is responsible for
- what data each one can use
- what each one must not do
- where each agent sits in the overall system

## Global Rules For Phase One

All phase-one agents and services must follow these project-level boundaries:
- asset universe: fixed basket of 20 US large-cap equities only
- data frequency: daily only
- decision horizon: 3-10 trading days
- final system outputs: `BUY`, `HOLD`, `SELL`
- execution mode: paper trading / simulated execution only

Tracked ticker basket:
- AAPL
- MSFT
- NVDA
- GOOGL
- AMZN
- META
- TSLA
- AMD
- NFLX
- JPM
- GS
- V
- MA
- XOM
- CVX
- UNH
- JNJ
- WMT
- COST
- KO

Out of scope for phase one:
- crypto
- forex
- options
- futures
- international stocks
- live broker APIs

## System Grouping

The first-phase system includes:
- four analysis agents
- one orchestration agent
- one risk management agent
- one portfolio management agent
- one trade execution agent in simulated mode
- one reporting agent

## 1. Market Data Analysis Agent

### Purpose
Analyze market structure for one tracked stock using daily market data and derived indicators.

### Allowed Inputs
- daily OHLCV data
- adjusted close
- daily returns
- rolling volatility
- short and medium moving averages
- momentum features such as RSI-style indicators if available

### Core Questions This Agent Must Answer
- Is the stock trending up, down, or sideways over the recent window?
- Is short-term momentum supportive or weak?
- Is recent volatility moderate enough to support a directional view?

### Required Output Style
This agent should return:
- `BUY`, `HOLD`, or `SELL`
- confidence from `0` to `1`
- rationale based on trend, momentum, and volatility

### What It Must Not Do
- use news data
- use sentiment text
- use company financial statements
- include crypto or any non-MVP asset class

## 2. News Analysis Agent

### Purpose
Evaluate recent ticker-specific news and determine whether the recent news flow is supportive, adverse, or neutral for the stock.

### Allowed Inputs
- cleaned news articles mapped to one of the tracked tickers
- source metadata
- publication timestamps
- article summaries or cleaned text

### Default Data Window
- last 7 calendar days unless a later config changes it

### Core Questions This Agent Must Answer
- Has there been materially positive or negative news for the company?
- Are recent stories likely to matter over the next 3-10 trading days?
- Is the current news flow concentrated around one event or several different events?

### What It Must Not Do
- infer price behavior from charts
- use social chatter as if it were formal news
- produce macro-only reasoning unless tied directly to the tracked company

## 3. Sentiment Analysis Agent

### Purpose
Estimate short-term market mood around one tracked stock from ticker-linked sentiment-bearing text.

### Allowed Inputs
- cleaned sentiment text mapped to one tracked ticker
- source metadata
- publication timestamps
- sentiment labels or scores if present in the dataset

### Default Data Window
- last 3-7 calendar days

### Core Questions This Agent Must Answer
- Is sentiment around the stock supportive, adverse, or mixed?
- Is sentiment getting stronger, weaker, or unstable?
- Does current sentiment support a 3-10 day directional view?

### What It Must Not Do
- summarize full news articles as its primary job
- infer chart trends from OHLCV data
- include untickered market chatter
- include crypto sentiment

## 4. Fundamental Analysis Agent

### Purpose
Evaluate company quality and valuation context for one tracked stock.

### Allowed Inputs
- revenue
- net income
- EPS
- PE ratio
- PB ratio
- debt to equity
- operating margin
- ROE
- market capitalization
- other documented company-level fundamental fields

### Core Questions This Agent Must Answer
- Does the company look fundamentally strong or weak?
- Does valuation look attractive, fair, or stretched?
- Does the company's financial profile support a constructive or cautious short-medium term view?

### What It Must Not Do
- react mainly to one-day price moves
- depend on news headlines as its primary evidence
- use non-equity assets or crypto data

## 5. Orchestration Agent

### Purpose
Collect outputs from the four analysis agents and create one combined recommendation.

### Responsibilities
- call the required analysis agents for a ticker
- validate that outputs match contract format
- combine the signals using a transparent method
- compute contribution percentages
- produce one preliminary final recommendation

### Why It Exists
This keeps the system understandable. Instead of agents calling each other directly, one orchestrator controls the flow.

## 6. Risk Management Agent

### Purpose
Prevent weak or unstable recommendations from passing through as strong trading calls.

### Phase-One Responsibilities
- review final confidence
- detect excessive recent volatility
- detect strong disagreement across agents
- downgrade or block a recommendation if needed

### Typical Behavior
Examples:
- downgrade `BUY` to `HOLD` if confidence is too low
- downgrade a strong call if recent volatility is extreme
- flag decisions where agents strongly conflict

### What It Is Not In Phase One
- not a full institutional risk engine
- not a full compliance system
- not a portfolio optimizer

## 7. Portfolio Management Agent

### Purpose
Convert an approved recommendation into a simple allocation or position-sizing decision for the paper-trading flow.

### Phase-One Responsibilities
- decide whether the trade should be taken in the simulated portfolio
- assign a simple position size based on confidence and risk constraints
- prevent unrealistic over-allocation in the paper portfolio

### What It Is Not In Phase One
- not an advanced optimizer
- not a rebalancing engine for many asset classes
- not a factor model system

## 8. Trade Execution Agent
### Purpose
Simulate trade execution after a portfolio decision has been approved.

### Phase-One Responsibilities
- accept paper-trade instructions from the portfolio layer
- create a deterministic simulated execution record
- return execution status and metadata
- update the trade log or trade history source used by the dashboard

### Minimum Output Expectations
At minimum, the execution result should contain:
- ticker
- side
- quantity or position size
- execution mode set to `paper`
- status
- execution timestamp

### What It Must Not Do
- connect to a real broker
- place real orders
- require live exchange connectivity
- depend on intraday matching engines or complex market simulation

## 9. Reporting Agent

### Purpose
Summarize what the system decided, how it decided it, and what simulated action followed.

### Phase-One Responsibilities
- summarize the final recommendation
- summarize agent contribution and confidence context
- record whether the risk layer changed the decision
- record whether a paper trade was created
- expose dashboard-friendly summaries

### Why Reporting Is Included In Phase One
Reporting is part of the end-to-end story. Without it, the product can think and act, but it cannot clearly communicate what happened after the decision.

### What It Must Not Do
- try to become a full business intelligence system
- generate highly complex analytics that delay the MVP
- replace the dashboard with report-only output

## Allowed Interactions Between Agents And Services

To keep the system explainable and easy to debug, the allowed flow is:

1. cleaned data layer provides validated input data
2. analysis agents run independently on their own domain data
3. orchestration agent combines those outputs
4. risk management agent reviews the combined result
5. portfolio management agent decides the paper allocation
6. trade execution agent simulates the trade
7. reporting agent summarizes the result

Analysis agents should not directly call:
- each other
- trade execution
- reporting
- dashboard components

## Ownership Guidance For Team Members

If a teammate owns one of these agents, they should know exactly what is theirs to solve:

### Market Data Agent Owner
Owns:
- price and volume feature logic
- trend and momentum interpretation
- volatility-aware signal generation

Does not own:
- news understanding
- company fundamentals
- social sentiment
- trade execution

### News Agent Owner
Owns:
- article relevance to a ticker
- recent news tone
- news-based signal interpretation

Does not own:
- price chart analysis
- social sentiment streams
- valuation logic

### Sentiment Agent Owner
Owns:
- ticker-linked market mood interpretation
- sentiment window choice within documented limits
- sentiment stability and directional support

Does not own:
- article summarization as primary logic
- chart-based reasoning
- company fundamentals

### Fundamental Agent Owner
Owns:
- company quality interpretation
- financial metric-based signal generation
- valuation context

Does not own:
- fast reaction to daily chart moves
- social or news-driven logic

### Orchestrator Owner
Owns:
- combining agent outputs
- contribution scoring
- preliminary final recommendation logic

Does not own:
- UI behavior
- route handling
- live data collection

### Risk Owner
Owns:
- downgrade logic
- disagreement handling
- confidence filtering

Does not own:
- portfolio optimization beyond the documented phase-one needs

### Portfolio Owner
Owns:
- paper position-sizing rules
- allocation sanity checks

Does not own:
- execution engine internals
- recommendation generation

### Execution Owner
Owns:
- simulated order record creation
- execution status handling

Does not own:
- live brokerage integration

### Reporting Owner
Owns:
- readable summary generation
- reporting output format

Does not own:
- dashboard layout decisions

## Phase-One Flow Summary

For one user request on one ticker, the intended order is:
1. load the latest cleaned data for that ticker
2. run the four analysis agents
3. combine their outputs in the orchestration agent
4. pass the result to the risk management agent
5. pass approved results to the portfolio management agent
6. simulate the trade in the trade execution agent
7. record the result through the reporting agent
8. return the explained output to the dashboard

## Non-Phase-One Agents

The repository may contain folders for other future agents, but they are not required to complete the first-phase product. Those may include:
- compliance agent
- backtesting agent
- macro analysis agent
- order management agent
- separate technical analysis agent as an independent service

Those should not block the MVP unless the project is intentionally re-scoped.
### Purpose
Simulate trade execution after portfolio and risk approval.

### Phase-One Responsibilities
- accept approved paper-trade instructions
- record a simulated order
- return an execution result
- update the paper portfolio state or trade log

### Important Boundary
This is simulated execution only. No real money, no live broker, no exchange connectivity.

## 9. Reporting Agent

### Purpose
Create readable summaries of what the system decided and what happened in the simulated flow.

### Phase-One Responsibilities
- log the final recommendation
- log per-agent contributions
- log whether risk changed the output
- log whether a simulated trade was executed
- generate a simple dashboard-friendly summary

### Why It Matters
Reporting is part of the end-to-end story. It shows that the system not only thinks, but also records and presents its actions clearly.

## Phase-One Flow Summary

For one user request on one ticker, the intended order is:
1. load the latest cleaned data for that ticker
2. run the four analysis agents
3. combine their outputs in the orchestration agent
4. pass the result to the risk management agent
5. pass approved results to the portfolio management agent
6. simulate the trade in the trade execution agent
7. record the result through the reporting agent
8. return the explained output to the dashboard
