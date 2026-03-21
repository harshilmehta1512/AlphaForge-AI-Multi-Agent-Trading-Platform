# AlphaForge

AlphaForge is an explainable multi-agent trading application for US large-cap equities.

The product goal is to show a complete end-to-end trading workflow, not just a raw `BUY` or `SELL` output. For any tracked stock, the system should:
- collect and use cleaned market, news, sentiment, and fundamental data
- run multiple specialized agents on that data
- combine those agent outputs into one recommendation
- explain which agents influenced the decision and by how much
- apply risk checks before allowing the recommendation to proceed
- create a paper-trade style execution result
- show the full flow inside a dashboard

The first phase is a paper-trading MVP. It is not a live broker-connected autonomous trading system.

## What The App Is

AlphaForge is being built as an end-to-end explainable trading platform with these first-phase components:
- four core analysis agents:
  - market data analysis agent
  - news analysis agent
  - sentiment analysis agent
  - fundamental analysis agent
- supporting product-flow agents and services:
  - orchestration agent
  - risk management agent
  - portfolio management agent
  - trade execution agent in simulation mode
  - reporting agent
- one backend API
- one frontend dashboard

The main differentiator is explainability. The system should not only say what to do. It should show:
- each agent's signal
- each agent's confidence
- each agent's contribution to the final decision
- whether risk controls changed the outcome

## First-Phase Scope

The first phase is intentionally narrow so the team can finish an end-to-end MVP on time.

Current scope:
- asset universe: 20 fixed US large-cap tickers
- timeframe: daily data only
- decision horizon: 3-10 trading days
- output labels: `BUY`, `HOLD`, `SELL`
- execution mode: paper trading / simulation only

Tracked ticker basket:
`AAPL, MSFT, NVDA, GOOGL, AMZN, META, TSLA, AMD, NFLX, JPM, GS, V, MA, XOM, CVX, UNH, JNJ, WMT, COST, KO`

Out of scope for the first phase:
- crypto
- forex
- options
- futures
- international equities
- live broker integration
- production-grade compliance automation
- advanced portfolio optimization
- intraday or high-frequency execution

## End-To-End Product Flow

```text
cleaned datasets
-> analysis agents
-> orchestration agent
-> explainability output
-> risk management
-> portfolio decision
-> paper execution
-> reporting
-> dashboard
```

## Repository Structure

```text
agents/           agent folders and ownership boundaries
backend/          FastAPI app, services, schemas, orchestration
frontend/         dashboard application
infrastructure/   Docker and Terraform files
docs/             project documentation and reading path
scripts/          local helper scripts
shared/           shared schemas, constants, and utilities
```

## Where To Read Next

The repository overview is here. Detailed product and engineering documentation lives in `docs/`.

Start with:
1. `docs/README.md`
2. `docs/product/mvp_scope.md`
3. `docs/product/agents.md`
4. `docs/architecture/system_architecture.md`
5. `docs/architecture/backend_architecture.md`
6. `docs/product/dashboard.md`
7. `docs/decisions/techstack.md`
8. `docs/onboarding/setup.md`
9. `docs/onboarding/team_workflow.md`

## Project Management Rule

The GitHub Kanban board should contain implementation tasks, blockers, bugs, and delivery work only. Product definitions, agent boundaries, dashboard behavior, setup instructions, and workflow rules should live in `docs/`.

## Status

This repository is in MVP build phase. Some folders are scaffolds and some implementation areas are still pending.
