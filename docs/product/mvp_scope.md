# MVP Scope

This document defines exactly what the first phase of AlphaForge is expected to deliver.

The purpose of the MVP is to demonstrate a complete explainable end-to-end trading workflow for a narrow stock universe. This file should answer the most important project-management question in the repository:

`What exactly are we building in phase one, and what are we explicitly not building?`

## MVP Goal

By the end of the first phase, AlphaForge should allow a user to:
- log in or access the dashboard
- choose one tracked stock
- receive a final recommendation
- see which agents contributed to the decision
- see whether risk management changed the result
- see a simulated portfolio and paper-trade outcome
- review the result in dashboard and reporting views

This is the correct standard for the phase-one product. It is not enough to stop at isolated agent outputs.

## Product Definition

AlphaForge in phase one is:
- an explainable multi-agent trading application
- focused on one fixed equity universe
- intended to demonstrate end-to-end product flow
- designed for paper trading and simulation only

AlphaForge in phase one is not:
- a live broker-connected trading engine
- a multi-asset trading platform
- a high-frequency system
- a complete institutional compliance platform
- a finished production deployment system

## Why The MVP Is Narrow

The scope is intentionally tight because the project timeline is short and the team is still building the core system.

The product needs to prove one coherent story well:
- multiple agents analyze a stock
- the system combines those views transparently
- the system shows why it made the decision
- the system applies risk logic
- the system simulates portfolio and trade action
- the system presents the complete result in the dashboard

Anything that weakens that story or expands the asset universe too early increases delivery risk.

## What The Entire First-Phase System Will Do

The first-phase system will:
1. use cleaned market, news, sentiment, and fundamental datasets
2. run four specialized analysis agents
3. combine those outputs through an orchestrator
4. calculate explainability details for the final result
5. pass the recommendation through risk checks
6. convert the approved decision into a simple simulated portfolio action
7. simulate trade execution
8. log and report the result in the dashboard

## In Scope

The MVP includes:
- one backend API
- one frontend dashboard
- four analysis agents
- one orchestration agent
- one risk management agent
- one portfolio management agent
- one simulated trade execution agent
- one reporting agent
- Docker-based local development workflow

In practical product terms, the phase-one MVP must include:
- a working backend endpoint for recommendation generation
- a working frontend path for viewing the recommendation
- visible per-agent outputs and contribution percentages
- a visible risk adjustment explanation when applicable
- a visible simulated trade result
- a visible summary/reporting view

## Explicit End-To-End Success Path

The system should be considered working only if the following complete flow works for at least one tracked ticker:

1. user opens the dashboard
2. user chooses a supported ticker
3. backend loads the required cleaned data
4. all four analysis agents return valid outputs
5. orchestrator creates a combined recommendation
6. risk management reviews the result
7. portfolio logic produces a paper position decision
8. execution logic simulates the trade
9. reporting logic records and summarizes the result
10. frontend displays the full flow clearly

## Asset Universe

All first-phase work must use this fixed basket of US large-cap equities only:
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

## Time Horizon And Data Boundaries

Use:
- daily data only
- recommendation horizon of 3-10 trading days
- paper trading only

Do not build in phase one:
- high-frequency trading
- intraday pipelines
- minute-level or tick-level execution systems

## Data Boundaries

For phase one, all agent work must stay inside these data boundaries:
- only the fixed 20-stock universe
- only cleaned inputs approved by the data pipeline owners
- only documented fields for each agent domain

Do not let agents mix undocumented inputs or ad hoc local datasets. That creates inconsistent behavior and breaks explainability.

## Final Product Output

For a chosen ticker, the system must produce:
- final recommendation: `BUY`, `HOLD`, or `SELL`
- final confidence
- each agent's signal
- each agent's confidence
- each agent's contribution percentage
- combined rationale
- risk adjustment details
- simulated portfolio decision
- simulated trade record
- reporting/logging output for dashboard use

## Main Differentiator

The main differentiator of AlphaForge in phase one is explainability inside an end-to-end trading workflow.

The product must not only say what action to take. It must show:
- which agents supported or opposed the decision
- how strongly each agent influenced the output
- what the risk layer changed
- what simulated execution happened afterward

## Out Of Scope

The following are deferred beyond the first phase:
- live broker integration
- crypto, forex, options, futures, and international markets
- production-grade compliance automation
- advanced portfolio optimization
- full backtesting platform
- generic RAG layer without a concrete retrieval need
- mandatory WebSocket streaming
- production cloud deployment as a release blocker

Out-of-scope means:
- do not create active sprint work for these unless leadership explicitly re-prioritizes
- do not quietly expand the product to include them
- do not let “nice-to-have” features delay the core end-to-end flow

## Success Criteria

The MVP is successful if a reviewer can open the product and understand the full flow without reading code:
- recommendation generation
- explainability
- risk filtering
- portfolio action
- paper execution
- reporting

## Failure Conditions

The MVP should be treated as incomplete if any of the following are true:
- the system can generate recommendations but cannot explain them
- the system can explain recommendations but cannot show simulated downstream action
- the dashboard shows partial information with no clear connection between analysis and execution
- different contributors implement conflicting assumptions about the supported tickers, horizon, or outputs
