# Data Flow

This document describes the intended MVP data flow.

This file exists because the system will fail in confusing ways if contributors do not agree on where data starts, how it is cleaned, how it is validated, and how it reaches each agent.

## Data Sources

The team is currently collecting and cleaning data for four domains:
- market data
- news
- sentiment
- fundamentals

All source data must be normalized before agent logic consumes it.

## Why Cleaned Data Is Mandatory

Agents should not read arbitrary raw external data during normal MVP flow.

Reason:
- raw feeds are inconsistent
- schemas differ across providers
- timestamps may not align
- duplicates and missing fields are common
- explainability becomes weaker if inputs are inconsistent

The analysis agents should operate on cleaned and documented data only.

## MVP Flow

```text
raw data sources
-> collection and cleaning
-> validated cleaned datasets
-> analysis agents
-> orchestrator
-> explainability layer
-> risk gate
-> portfolio decision
-> simulated execution
-> reporting summary
-> API response
-> dashboard
```

## Data Ownership

- market dataset powers the market data analysis agent only
- news dataset powers the news analysis agent only
- sentiment dataset powers the sentiment analysis agent only
- fundamentals dataset powers the fundamental analysis agent only

This separation is intentional. It keeps agent outputs understandable.

## Dataset Expectations By Domain

### Market Data Dataset
Should contain:
- ticker
- date
- open
- high
- low
- close
- adjusted close if used
- volume
- derived features documented by the team

### News Dataset
Should contain:
- article identifier
- ticker mapping
- source
- publication timestamp
- title
- article summary or cleaned text

### Sentiment Dataset
Should contain:
- text identifier
- ticker mapping
- source
- publication timestamp
- text or processed representation
- sentiment label or score if available

### Fundamentals Dataset
Should contain:
- ticker
- reporting period or effective date
- documented financial fields such as revenue, net income, EPS, ratios, and margins

## Validation Expectations

Before data is consumed by agents, the data layer should ensure:
- required fields exist
- timestamps are normalized
- tickers belong to the fixed 20-stock universe
- duplicate records are removed
- data windows are documented

It should also ensure:
- unsupported tickers are rejected
- fields use consistent units where applicable
- date fields are interpretable by the backend
- missing critical values are surfaced rather than silently ignored

## Handoff Into Agents

The handoff from cleaned data into agents should be explicit.

For each agent, the system should know:
- which dataset it reads from
- which fields are required
- which date window it uses
- what output schema it must return

This handoff should be implemented in code through a shared data-access layer, not through ad hoc file reads scattered across agents.

## Final Output Flow

Each agent should return a structured output to the orchestrator. The orchestrator should then create one combined result for the frontend.

After that:
- risk logic modifies or approves the result
- portfolio logic derives a paper allocation
- execution logic records the simulated action
- reporting logic summarizes the complete result

## Data Risks To Avoid

The biggest data risks in the MVP are:
- contributors using different ticker universes
- inconsistent timestamps across datasets
- undocumented local data files
- agents silently tolerating malformed inputs
- one dataset containing mixed domains that should remain separate

If any of those happen, the system becomes harder to debug and less explainable.
