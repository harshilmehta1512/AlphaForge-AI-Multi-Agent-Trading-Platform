# Milestones

## Milestone 1: Scope And Contracts
- freeze MVP scope
- finalize asset universe
- finalize agent specs
- finalize shared contracts

Exit condition:
- all contributors are working from the same documented scope and contract set

## Milestone 2: Data Readiness
- complete collection and cleaning for market, news, sentiment, and fundamentals
- validate schemas and storage format

Exit condition:
- cleaned data is usable by the backend and agents without ad hoc local fixes

## Milestone 3: Core Agent Outputs
- implement the four analysis agents
- return contract-valid outputs for tracked tickers

Exit condition:
- each analysis agent can produce a valid output for supported tickers

## Milestone 4: Recommendation Engine
- implement orchestrator
- implement explainability output
- implement risk gate

Exit condition:
- the system can produce a final explainable recommendation, not just individual agent outputs

## Milestone 5: Product Demo
- connect backend to frontend
- show recommendation plus explanation in the dashboard
- show portfolio decision, simulated execution, and reporting summary
- prepare deterministic demo flow

Exit condition:
- a reviewer can see the full end-to-end story inside the product
