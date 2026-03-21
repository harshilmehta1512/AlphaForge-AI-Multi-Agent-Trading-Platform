# Agent Contracts

This document defines the minimum contract that all MVP agents must follow.

This file is one of the most important technical documents in the repository. If contributors violate these contracts, integration will become unreliable even if individual components work locally.

## Common Analysis Agent Output

Every analysis agent must return:

```json
{
  "ticker": "AAPL",
  "agent_name": "market_data_analysis_agent",
  "signal": "BUY",
  "confidence": 0.74,
  "rationale": "Short-term trend and momentum are positive while volatility remains moderate.",
  "key_factors": ["trend_up", "momentum_positive", "volatility_moderate"],
  "data_window": "2025-03-01 to 2025-03-19",
  "generated_at": "2025-03-19T12:00:00Z"
}
```

Rules:
- `ticker` must belong to the fixed 20-stock universe
- `signal` must be `BUY`, `HOLD`, or `SELL`
- `confidence` must be a number from `0` to `1`
- `rationale` must be short and human-readable
- `key_factors` must list the main reasons behind the signal

Additional expectations:
- `agent_name` must match the real agent identifier used across the backend
- `data_window` must describe the time range the agent evaluated
- `generated_at` must be a valid timestamp
- the output should be deterministic for the same input snapshot wherever possible

## Final Recommendation Contract

The orchestrator plus explainability layer must return:

```json
{
  "ticker": "AAPL",
  "final_signal": "BUY",
  "final_confidence": 0.79,
  "agent_outputs": [
    {
      "agent_name": "market_data_analysis_agent",
      "signal": "BUY",
      "confidence": 0.74
    },
    {
      "agent_name": "news_analysis_agent",
      "signal": "BUY",
      "confidence": 0.71
    },
    {
      "agent_name": "sentiment_analysis_agent",
      "signal": "HOLD",
      "confidence": 0.58
    },
    {
      "agent_name": "fundamental_analysis_agent",
      "signal": "BUY",
      "confidence": 0.83
    }
  ],
  "agent_contributions": {
    "market_data_analysis_agent": 30,
    "news_analysis_agent": 25,
    "sentiment_analysis_agent": 15,
    "fundamental_analysis_agent": 30
  },
  "risk_adjustment": {
    "applied": false,
    "reason": null
  },
  "final_rationale": "Three agents are constructive and the strongest support comes from market structure and fundamentals.",
  "generated_at": "2025-03-19T12:00:00Z"
}
```

Rules:
- contribution percentages must sum to `100`
- `risk_adjustment` must be explicit, even when unused
- the final rationale must explain the combined result, not repeat every agent verbatim

Additional expectations:
- `agent_outputs` should include all required first-phase analysis agents
- missing agent outputs should be treated as an explicit failure condition unless the system intentionally supports degraded operation
- the final recommendation should not hide whether agents disagreed

## Portfolio Decision Contract

The portfolio management agent should produce a simple paper-allocation decision.

```json
{
  "ticker": "AAPL",
  "approved_signal": "BUY",
  "paper_position_size": 100,
  "allocation_note": "Approved with moderate size due to good confidence and acceptable risk.",
  "generated_at": "2025-03-19T12:00:00Z"
}
```

Rules:
- the decision must be derived from an approved recommendation
- position sizing logic must follow the phase-one risk boundaries
- the output must remain simple enough for the dashboard to display clearly

## Simulated Trade Contract

The trade execution agent should record a simulated trade result.

```json
{
  "ticker": "AAPL",
  "side": "BUY",
  "quantity": 100,
  "execution_mode": "paper",
  "status": "executed",
  "executed_at": "2025-03-19T12:01:00Z"
}
```

Rules:
- `execution_mode` must remain `paper` for phase one
- the simulated trade should be tied to a valid portfolio decision
- status values should be limited and documented

## Reporting Summary Contract

The reporting layer should be able to produce a concise summary for the dashboard.

```json
{
  "ticker": "AAPL",
  "final_signal": "BUY",
  "risk_override_applied": false,
  "trade_executed": true,
  "summary": "Buy recommendation executed in paper mode with strongest support from market and fundamentals.",
  "generated_at": "2025-03-19T12:02:00Z"
}
```

Rules:
- the summary must be readable by a non-developer
- it should describe what happened, not just repeat raw fields
- it should be consistent with the final recommendation, risk result, and simulated trade result

## Contract Evolution Rule

If any contract changes:
- the corresponding schema definitions must change
- backend implementations must change
- frontend usage must be updated
- this document must be updated in the same change

Do not allow silent contract drift.

## Out Of Scope Contracts For Phase One

The following contracts are not required for the MVP:
- live order placement
- WebSocket payloads
- broker execution lifecycle
