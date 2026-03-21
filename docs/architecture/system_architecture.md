# System Architecture

This document explains the architecture of the first-phase AlphaForge system and how the system should behave from user entry to final output.

## Architecture Goal

The system must support an end-to-end explainable trading workflow while remaining simple enough to deliver within the MVP timeline.

That means:
- modular responsibilities
- clear data ownership
- central orchestration
- explicit explainability
- simulated execution instead of live trading

## High-Level Architecture

```text
Frontend Dashboard
-> Backend API
-> Orchestration Agent
-> Analysis Agents
-> Risk Management Agent
-> Portfolio Management Agent
-> Trade Execution Agent (simulated)
-> Reporting Agent
```

## Why This Architecture Was Chosen

This architecture is intentionally simple for the first phase:
- one frontend
- one backend control layer
- a small fixed set of analysis agents
- a small fixed set of downstream decision and simulation services

This keeps the system understandable and gives the team a realistic chance of shipping a coherent product.

## Main Architectural Principle

Use a central orchestrator rather than letting agents call each other freely.

Reason:
- easier to debug
- easier to explain
- easier to test
- fewer circular dependencies

## Core System Components

## 1. Frontend Dashboard

The dashboard is the user-facing layer. It should allow the user to inspect recommendations, agent reasoning, simulated trades, and summaries.

## 2. Backend API

The backend API is the system entry point for the frontend.

Responsibilities:
- receive UI requests
- validate inputs
- fetch data from internal services
- return recommendation and reporting responses

The backend should act as the control surface of the system, not just a thin proxy.

## 3. Cleaned Data Layer

The system depends on cleaned datasets for:
- market data
- news
- sentiment
- fundamentals

This data must be normalized before agents consume it.

## 4. Analysis Agents

The four analysis agents each own one input domain:
- market data analysis agent
- news analysis agent
- sentiment analysis agent
- fundamental analysis agent

They should not mix responsibilities.

## 5. Orchestration Agent

The orchestrator:
- triggers the required analysis agents
- collects outputs
- computes a combined decision
- computes contribution percentages

## 6. Risk Management Agent

The risk agent reviews the orchestrated recommendation and can weaken or block it if basic risk checks fail.

## 7. Portfolio Management Agent

The portfolio agent converts an approved recommendation into a paper position-sizing or allocation decision.

## 8. Trade Execution Agent

The execution agent records a simulated trade result. It should not connect to a real broker in phase one.

## 9. Reporting Agent

The reporting agent creates summaries and logs so the dashboard can display a complete end-to-end story.

## User Flow From Login

When a user logs in and requests a decision for a ticker, the intended flow is:

1. user enters the dashboard
2. user selects one of the tracked tickers
3. frontend calls the backend recommendation endpoint
4. backend requests the latest cleaned data for that ticker
5. orchestrator runs the four analysis agents
6. orchestrator combines outputs into one preliminary recommendation
7. risk management agent validates or downgrades the recommendation
8. portfolio management agent determines the paper allocation
9. trade execution agent simulates the action
10. reporting agent records the recommendation and simulated result
11. backend returns the final response to the dashboard
12. dashboard shows the final recommendation, per-agent contributions, and simulated trade result

## System Flow Without Login

If the MVP demo does not use full authentication, the product flow should still follow the same internal system path once the user lands on the dashboard.

## Combined Agent Flow Diagram

```text
User
-> Dashboard
-> Backend API
-> Orchestration Agent
   -> Market Data Analysis Agent
   -> News Analysis Agent
   -> Sentiment Analysis Agent
   -> Fundamental Analysis Agent
-> Orchestration Agent combines outputs
-> Risk Management Agent
-> Portfolio Management Agent
-> Trade Execution Agent (simulated)
-> Reporting Agent
-> Backend API response
-> Dashboard
```

## Responsibility Boundaries

### Frontend Should Own
- page layout
- data presentation
- user interactions
- navigation between overview and detail views

### Backend Should Own
- orchestration
- validation
- explainability data assembly
- risk logic
- portfolio decision
- simulated execution
- reporting outputs

### Analysis Agents Should Own
- domain-specific interpretation only

### Shared Contracts Should Own
- data exchange format between components

## Detailed Processing Logic

### Step 1: Input Selection
The user selects a ticker from the fixed 20-stock universe.

### Step 2: Data Retrieval
The backend loads the latest cleaned inputs relevant to that ticker.

### Step 3: Parallel Analysis
The four analysis agents evaluate the ticker in parallel or near-parallel where possible.

### Step 4: Signal Aggregation
The orchestrator combines those outputs using a transparent method.

### Step 5: Explainability
The system records each agent's:
- signal
- confidence
- contribution percentage
- rationale

### Step 6: Risk Filtering
The risk agent may downgrade a recommendation if risk conditions are poor.

### Step 7: Portfolio Action
The portfolio agent decides what the paper position should look like.

### Step 8: Simulated Execution
The trade execution agent records the simulated action.

### Step 9: Reporting
The reporting agent creates a dashboard-friendly summary.

## Architecture Boundaries

The first-phase architecture does not require:
- microservices for every component
- real-time streaming infrastructure
- message queues
- broker connectivity
- production cloud scaling

Those can be added later if the MVP works well.

## Architecture Risks To Avoid

The main architectural risks in this phase are:
- contributors adding extra asset classes without approval
- mixing agent domains and weakening explainability
- putting business logic directly into API routes
- splitting the system into too many services too early
- building advanced infrastructure before the end-to-end flow works

If any of those start happening, the architecture is drifting away from the MVP and should be corrected.
