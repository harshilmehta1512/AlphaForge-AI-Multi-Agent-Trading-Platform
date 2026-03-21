# Backend Architecture

This document explains how the backend should work in the first phase of AlphaForge.

This file is necessary because multiple teammates will be working on backend, agents, API contracts, orchestration, and dashboard integration at the same time. Without one shared backend design document, different people will make different assumptions about:
- where orchestration logic lives
- how agents are triggered
- what the API should return
- where simulated trade and reporting logic belongs
- how data should move between backend modules

The purpose of this document is to remove those ambiguities.

## Backend Goal

The backend is the control layer of the product.

It must:
- expose clean API endpoints to the frontend
- load validated input data for a ticker
- trigger the analysis agents
- combine the outputs
- apply risk checks
- produce a portfolio decision
- simulate execution
- generate reporting data
- return a frontend-friendly response

The backend should be the place where the entire product flow becomes deterministic and testable.

## Backend Principles

For phase one, the backend should follow these principles:
- keep one clear request flow
- prefer explicit contracts over implicit assumptions
- keep business logic out of route files
- keep agent logic separated by domain
- keep the orchestration path easy to trace
- optimize for correctness and team clarity, not architectural cleverness

## Backend Responsibilities

The backend owns:
- request validation
- data loading and validation
- analysis-agent execution
- orchestration logic
- explainability assembly
- risk checks
- portfolio decision logic
- simulated execution flow
- reporting summary generation
- API response formatting

The backend does not need to own in phase one:
- live broker connectivity
- real-time streaming infrastructure
- distributed queues
- advanced production scheduling

## Suggested Backend Module Responsibilities

The exact file layout can evolve, but responsibilities should be separated roughly like this:

### `backend/api/`
Owns FastAPI routes and request-response handling only.

Should do:
- receive request
- validate path/query/body inputs
- call service or orchestrator layer
- return response

Should not do:
- agent business logic
- heavy data transformation
- direct orchestration logic inside route handlers

### `backend/schemas/`
Owns request and response models.

Should define:
- analysis agent output schema
- final recommendation schema
- portfolio decision schema
- simulated execution schema
- reporting summary schema

### `backend/services/`
Owns reusable business logic and system workflow helpers.

Recommended service areas:
- data access service
- orchestrator service
- risk service
- portfolio service
- execution service
- reporting service

### `backend/core/`
Owns configuration, settings, and logging setup.

### `backend/models/`
Use for internal domain models only if needed. Do not use this as a dumping ground for random dictionaries.

## Suggested Internal Backend Structure

The exact folder layout can evolve, but the structure should remain close to this:

```text
backend/
  api/
    routes/
  core/
  schemas/
  services/
    data_access/
    agents/
    orchestration/
    risk/
    portfolio/
    execution/
    reporting/
  models/
```

The point is not to force a perfect structure immediately. The point is to prevent logic from being scattered without ownership.

## End-To-End Backend Flow

When the frontend asks for a recommendation for a ticker, the backend should follow this flow:

1. validate the ticker belongs to the fixed 20-stock universe
2. load the latest cleaned market, news, sentiment, and fundamental data for that ticker
3. validate that required fields exist for each input domain
4. run the four analysis agents
5. collect and validate the four agent outputs
6. combine them in the orchestrator
7. compute per-agent contribution percentages
8. run the risk management logic
9. generate a portfolio decision for paper trading
10. simulate execution
11. build a reporting summary
12. return the final response to the frontend

This flow should be implemented in a way that is easy to test end to end.

## Recommended Request Flow

```text
Frontend request
-> FastAPI route
-> backend service/orchestrator entrypoint
-> data access layer
-> four analysis agents
-> orchestrator
-> risk service
-> portfolio service
-> execution service
-> reporting service
-> response schema
-> frontend
```

## Key Backend Endpoints

The backend should expose only the endpoints needed for the MVP.

### `GET /api/v1/health`
Purpose:
- verify service is up

### `GET /api/v1/tickers`
Purpose:
- return the fixed supported stock universe

### `GET /api/v1/agents/{agent_name}/{ticker}`
Purpose:
- inspect one agent output for one ticker
- useful for development, debugging, and explainability views

### `GET /api/v1/recommendation/{ticker}`
Purpose:
- return the full end-to-end response for one ticker
- this should be the main dashboard endpoint

### Optional Later Endpoints
These can be added if the dashboard needs them, but they should not delay the MVP:
- `GET /api/v1/portfolio`
- `GET /api/v1/trades`
- `GET /api/v1/reports`

## Recommended Main Backend Response

The main recommendation endpoint should return one combined object that includes:
- ticker
- final signal
- final confidence
- all analysis agent outputs
- agent contributions
- risk adjustment details
- portfolio decision
- simulated trade result
- reporting summary

This gives the dashboard enough information to render the end-to-end story without many fragmented calls.

## Recommended Recommendation Endpoint Payload Sections

The main recommendation response should be thought of as these logical sections:
- request context
- final recommendation
- per-agent outputs
- explainability and contribution data
- risk result
- portfolio result
- simulated execution result
- reporting summary

This structure makes the payload easier to reason about for both frontend and backend contributors.

## Analysis Agent Execution Model

The four analysis agents should be treated as backend components or service modules for phase one.

They should:
- receive a validated ticker-specific input payload
- return a structured output
- avoid mutating shared global state

They should not:
- call each other directly
- write dashboard-oriented output formats
- reach across domains unnecessarily

## Orchestrator Responsibilities

The orchestrator is the backend coordinator.

It should:
- trigger the four analysis agents
- normalize their outputs
- combine their signals and confidence values
- compute contribution percentages
- produce the preliminary final recommendation

It should not:
- own route handling
- own trade execution logic
- own persistent reporting logic

## Risk Service Responsibilities

The risk service should apply lightweight checks such as:
- low overall confidence
- excessive recent volatility
- unusually strong disagreement between agents

It should be able to:
- allow a recommendation
- downgrade a recommendation
- attach a reason for the change

## Portfolio Service Responsibilities

The portfolio service should:
- decide whether the approved recommendation becomes a paper position
- assign a simple position size
- avoid unrealistic concentration in the simulated portfolio

Phase one does not require a sophisticated optimizer.

## Execution Service Responsibilities

The execution service should:
- accept the paper-trade instruction
- simulate the order
- return a stable execution record
- update whatever in-memory or persisted trade log is used in the MVP

This service must remain clearly marked as simulated execution only.

## Reporting Service Responsibilities

The reporting service should:
- summarize the final recommendation
- summarize risk changes
- summarize the simulated trade result
- return UI-friendly text or structured summaries

This is important because reporting is part of the product story, not an afterthought.

## Data Access Layer

The backend needs one common data access layer so all agents use consistent inputs.

This layer should:
- load cleaned datasets
- validate required fields
- filter by ticker
- expose the required data window for each agent

This avoids duplicate loading logic across different agents.

## State Management For Phase One

Keep state management simple.

For phase one, acceptable approaches include:
- file-backed cleaned datasets
- in-memory state for simple development scenarios
- lightweight database storage if the team implements it cleanly

What matters most is consistency and clarity, not scale.

If a persistent store is introduced, document:
- what is persisted
- where it is stored
- who writes to it
- who reads from it

## Error Handling

The backend should fail clearly.

Examples:
- unsupported ticker should return a clean validation error
- missing cleaned data should return a clear message
- agent failure should be logged with agent name and ticker
- partial failures should not silently produce fake success

If the system supports degraded operation later, that should be explicitly documented. Do not invent fallback behavior invisibly.

## Validation Rules

At minimum, the backend should validate:
- ticker is supported
- required cleaned input exists
- analysis outputs conform to schema
- contribution percentages are coherent
- portfolio and execution payloads are internally consistent

Validation should fail loudly during development rather than producing misleading output.

## Logging Expectations

At minimum, log:
- request start and end
- selected ticker
- agent execution success/failure
- final recommendation
- risk overrides
- simulated trade creation

These logs will make integration debugging much easier.

## Testing Expectations

The backend should be tested at three levels:

### Contract Tests
Validate schema compliance for:
- each analysis agent output
- final recommendation output
- portfolio and execution outputs

### Service Tests
Validate:
- orchestrator behavior
- risk downgrade rules
- portfolio decision logic
- simulated execution behavior

### End-To-End Tests
Validate that one ticker request can move through the complete backend pipeline successfully.

### Recommended End-To-End Test Scenario
At minimum, have one test that proves:
- a valid ticker can be requested
- all four agents run
- a final recommendation is returned
- risk result is attached
- portfolio result is attached
- execution result is attached
- reporting summary is attached

## Practical Backend Delivery Advice

To ship on time, build backend work in this order:
1. shared schemas
2. data access layer
3. individual analysis agents
4. orchestrator
5. risk service
6. portfolio service
7. execution service
8. reporting service
9. final API integration

This order reduces integration pain.

## Backend Non-Goals For Phase One

The backend should not spend time on:
- live trade routing
- queue-based distributed orchestration
- multi-tenant permission systems unless absolutely necessary
- advanced caching layers without a demonstrated performance problem
