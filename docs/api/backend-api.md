# Backend API

This document defines the MVP API surface.

## API Goals

The backend API should do only what the MVP needs:
- expose service health
- list the tracked tickers
- return individual agent output for a ticker
- return the final explained recommendation for a ticker
- support the dashboard with one coherent end-to-end response model

## Suggested Base Path

Use `/api/v1`.

## Required Endpoints

### `GET /api/v1/health`
Returns backend status.

Example response:

```json
{
  "status": "ok",
  "service": "alphaforge-backend"
}
```

### `GET /api/v1/tickers`
Returns the fixed MVP ticker universe.

This endpoint exists so the frontend does not hardcode the supported list forever. It also provides one place to validate that backend and frontend agree on the supported universe.

### `GET /api/v1/agents/{agent_name}/{ticker}`
Returns one analysis agent output for one tracked ticker.

Use cases:
- debugging one agent in isolation
- testing contract compliance
- powering an agent breakdown view if needed

### `GET /api/v1/recommendation/{ticker}`
Returns the final orchestrated and explained recommendation for one tracked ticker.

This should be the main dashboard endpoint for the first phase.

Recommended response sections:
- request context
- final recommendation
- analysis agent outputs
- contribution and explainability data
- risk result
- portfolio result
- simulated execution result
- reporting summary

## Suggested Endpoint Ownership

### Health
Owned by backend platform/setup contributors.

### Tickers
Owned by backend/service contributors who maintain the supported universe.

### Per-Agent Output
Owned jointly by backend and analysis-agent contributors.

### Final Recommendation
Owned by the orchestration/backend integration path and should be treated as the most important endpoint in the MVP.

## Non-MVP Endpoints

These should not block the MVP:
- live portfolio endpoints
- broker order endpoints
- backtesting endpoints
- WebSocket support

They can be added later if the codebase reaches that stage.

## API Design Rules

The MVP API should follow these rules:
- keep paths simple
- keep response shapes consistent
- do not expose internal implementation quirks
- fail clearly on invalid tickers or missing data
- avoid adding endpoints that the dashboard does not actually need

## Dashboard Dependency Mapping

The frontend will likely use the API like this:
- overview page -> `GET /api/v1/recommendation/{ticker}`
- recommendation detail page -> `GET /api/v1/recommendation/{ticker}`
- agent breakdown page -> `GET /api/v1/agents/{agent_name}/{ticker}` or the final recommendation payload
- startup ticker selector -> `GET /api/v1/tickers`
- health check/dev verification -> `GET /api/v1/health`
