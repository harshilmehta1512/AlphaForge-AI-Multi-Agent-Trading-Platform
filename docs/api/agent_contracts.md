# Agent Contracts

## Overview

Agent contracts define the structured data formats used for communication between agents in the AlphaForge AI multi-agent trading platform.

Each agent produces outputs that may serve as inputs for other agents.  
To ensure compatibility and maintain consistency across the system, agents exchange information using standardized data contracts.

These contracts represent the core data structures used throughout the system.

---

# Market Data Contract

Market data represents the price and volume information of financial assets.

Example structure:


{
"symbol": "AAPL",
"timestamp": "2024-01-01T10:30:00Z",
"price": 185.24,
"volume": 1200000
}


Fields:

- symbol: trading asset symbol
- timestamp: time of the market data point
- price: current asset price
- volume: traded volume

---

# News Article Contract

News analysis agents process financial news articles.

Example structure:


{
"source": "Reuters",
"title": "Apple Reports Strong Earnings",
"timestamp": "2024-01-01T08:00:00Z",
"summary": "Apple reported higher than expected earnings for the quarter."
}


Fields:

- source: news provider
- title: article title
- timestamp: time of publication
- summary: extracted or summarized article content

---

# Sentiment Score Contract

Sentiment analysis agents evaluate market sentiment.

Example structure:


{
"symbol": "AAPL",
"sentiment_score": 0.78,
"confidence": 0.85
}


Fields:

- symbol: associated financial asset
- sentiment_score: sentiment value (-1 to 1)
- confidence: confidence level of the sentiment model

---

# Technical Indicator Contract

Technical analysis agents compute indicators used by strategy agents.

Example structure:


{
"symbol": "AAPL",
"rsi": 62.3,
"macd": 1.5,
"moving_average": 182.4
}


Fields:

- symbol: trading asset
- rsi: relative strength index
- macd: moving average convergence divergence
- moving_average: computed moving average

---

# Trading Signal Contract

Strategy agents generate trading signals based on analysis results.

Example structure:


{
"symbol": "AAPL",
"signal": "BUY",
"confidence": 0.82,
"timestamp": "2024-01-01T10:35:00Z"
}


Fields:

- symbol: trading asset
- signal: BUY / SELL / HOLD
- confidence: probability score
- timestamp: time signal was generated

---

# Risk Evaluation Contract

The risk management agent evaluates the safety of executing trades.

Example structure:


{
"symbol": "AAPL",
"approved": true,
"max_position_size": 10000,
"stop_loss": 175.0
}


Fields:

- symbol: trading asset
- approved: whether the trade is allowed
- max_position_size: maximum allowed capital allocation
- stop_loss: recommended stop-loss level

---

# Portfolio Allocation Contract

Portfolio management agents determine asset allocation.

Example structure:


{
"symbol": "AAPL",
"allocation_percentage": 0.25
}


Fields:

- symbol: trading asset
- allocation_percentage: portfolio allocation weight

---

# Order Contract

Trade execution agents submit orders using a structured format.

Example structure:


{
"symbol": "AAPL",
"order_type": "MARKET",
"side": "BUY",
"quantity": 50
}


Fields:

- symbol: trading asset
- order_type: MARKET or LIMIT
- side: BUY or SELL
- quantity: number of shares or units

---

# Contract Validation

All agent contracts should follow consistent validation rules.

Validation may include:

- schema validation
- type checking
- required field verification

This ensures that incorrect data does not propagate through the system.

---

# Future Contract Extensions

Additional contracts may be added as the system evolves.

Examples include:

- macroeconomic indicators
- options trading signals
- arbitrage opportunities
- portfolio risk metrics