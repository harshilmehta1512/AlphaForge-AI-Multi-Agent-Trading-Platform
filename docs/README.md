# Documentation Index

This folder is the written source of truth for AlphaForge.

The purpose of this folder is to make the project understandable without relying on verbal explanations, private chats, or assumptions. A teammate should be able to read these files and answer the following questions without confusion:
- what the product is
- what the first phase includes and excludes
- which agents and services exist
- how data moves through the system
- how the backend is expected to be structured
- how the dashboard should behave
- which API contracts must be respected
- how the team should work and contribute

## Required Reading Order

Every contributor should read the docs in this order before starting work:

1. `product/mvp_scope.md`
2. `product/agents.md`
3. `product/dashboard.md`
4. `architecture/system_architecture.md`
5. `architecture/backend_architecture.md`
6. `architecture/data_flow.md`
7. `api/agent_contracts.md`
8. `api/backend-api.md`
9. `decisions/techstack.md`
10. `onboarding/setup.md`
11. `onboarding/team_workflow.md`
12. `onboarding/branch-strategy.md`
13. `onboarding/coding-standard.md`
14. `roadmap/milestones.md`

## Folder Guide

### `product/`
Explains what AlphaForge is as a product.

Files in this section define:
- what the first-phase MVP must deliver
- which agents and services are in scope
- what the dashboard should contain

### `architecture/`
Explains how the system should be built.

Files in this section define:
- overall system flow
- backend structure and responsibilities
- how data should move from cleaned inputs into agents and then into user-facing output

### `api/`
Explains the contract layer that ties the system together.

Files in this section define:
- the expected shape of agent outputs
- the expected shape of final recommendation, portfolio, execution, and reporting outputs
- the required backend endpoints for the MVP

### `decisions/`
Explains major choices that should not be repeatedly debated during implementation.

The remaining decision file records:
- the chosen stack
- why it was chosen
- what is intentionally deferred

### `onboarding/`
Explains how a teammate should set up, contribute, and work safely within the codebase.

### `roadmap/`
Explains milestone-level delivery expectations.

This section is for high-level planning, not day-to-day task execution. The GitHub Kanban board remains the execution tool.

## Priority Order If Docs Conflict

If two files appear to disagree, use this order:

1. `product/mvp_scope.md`
2. `product/agents.md`
3. `product/dashboard.md`
4. `architecture/system_architecture.md`
5. `architecture/backend_architecture.md`
6. `architecture/data_flow.md`
7. `api/agent_contracts.md`
8. implementation in code if it was intentionally updated and reviewed
9. `roadmap/milestones.md`

If a conflict is found, the docs should be corrected immediately rather than worked around.

## Current Final Docs Set

The final curated docs set is:

- `README.md`
- `product/mvp_scope.md`
- `product/agents.md`
- `product/dashboard.md`
- `architecture/system_architecture.md`
- `architecture/backend_architecture.md`
- `architecture/data_flow.md`
- `api/agent_contracts.md`
- `api/backend-api.md`
- `decisions/techstack.md`
- `onboarding/setup.md`
- `onboarding/team_workflow.md`
- `onboarding/branch-strategy.md`
- `onboarding/coding-standard.md`
- `roadmap/milestones.md`

## Current Product Summary

AlphaForge is an end-to-end explainable trading MVP for a fixed basket of US large-cap stocks.

The first phase is intentionally narrow:
- daily data only
- 20 fixed tickers only
- 3-10 trading day decision horizon
- paper-trading only

The system is expected to:
- run four analysis agents
- combine them through an orchestrator
- show per-agent contribution to the decision
- pass the recommendation through risk checks
- create a portfolio decision
- simulate trade execution
- show the full result in the dashboard
