# WebSocket Events

## Overview

The AlphaForge AI platform supports real-time communication using WebSockets.

WebSockets allow the backend to push updates directly to the frontend without requiring repeated HTTP requests.

This is particularly useful for applications that require live updates such as:

- market data streaming
- trading signals
- portfolio updates
- system alerts
- agent status monitoring

---

# WebSocket Connection

Clients connect to the backend WebSocket endpoint.

Example connection:


ws://backend-server/api/v1/ws


After connecting, the client can subscribe to different event streams.

---

# Event Structure

All WebSocket messages follow a common structure.

Example message format:


{
"event": "event_type",
"data": { ... }
}


Fields:

- event: identifies the type of message
- data: contains the event payload

---

# Market Data Events

These events deliver live market data updates.

Example event:


{
"event": "market_update",
"data": {
"symbol": "AAPL",
"price": 185.32,
"volume": 1223000,
"timestamp": "2024-01-01T10:30:05Z"
}
}


Triggered when:

- new market price arrives
- volume changes significantly
- new candle closes

---

# Trading Signal Events

These events notify the frontend when new trading signals are generated.

Example:


{
"event": "trading_signal",
"data": {
"symbol": "AAPL",
"signal": "BUY",
"confidence": 0.83
}
}


Triggered by:

- Strategy Agent
- signal generation events

---

# Portfolio Update Events

These events notify the dashboard when the portfolio changes.

Example:


{
"event": "portfolio_update",
"data": {
"symbol": "AAPL",
"quantity": 50,
"avg_price": 170.50
}
}


Triggered when:

- a new trade is executed
- a position changes
- a rebalance occurs

---

# Agent Status Events

These events provide real-time status updates for system agents.

Example:


{
"event": "agent_status",
"data": {
"agent": "sentiment_analysis_agent",
"status": "running"
}
}


Possible statuses include:

- running
- idle
- processing
- error

---

# System Alert Events

System alerts notify users about important events.

Example:


{
"event": "system_alert",
"data": {
"level": "warning",
"message": "High market volatility detected"
}
}


Alerts may include:

- risk alerts
- system errors
- market anomalies

---

# Backtesting Result Events

When backtesting tasks complete, results can be sent to the frontend.

Example:


{
"event": "backtest_result",
"data": {
"strategy": "momentum_strategy",
"return": 0.18,
"sharpe_ratio": 1.4
}
}


---

# Connection Handling

If the WebSocket connection is interrupted:

- the client should attempt reconnection
- missed events may be retrieved via REST APIs

---

# Security Considerations

WebSocket connections should be secured using authentication.

Possible approaches include:

- JWT authentication
- API key validation
- secure WebSocket connections (wss)

Example:


wss://backend-server/api/v1/ws


---

# Future Improvements

Future real-time capabilities may include:

- streaming order book updates
- high-frequency market feeds
- distributed event streaming
- advanced alert systems