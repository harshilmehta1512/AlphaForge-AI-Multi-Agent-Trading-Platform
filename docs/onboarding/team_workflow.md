# Team Workflow

This document defines how the team should work during the first phase.

## Source Of Truth

Use:
- `docs/` for product, architecture, setup, and scope decisions
- GitHub Projects Kanban for implementation tasks and delivery tracking

The Kanban board should not try to repeat all product requirements. It should point to the relevant docs and focus on execution.

## Recommended Team Workstreams

The team can be divided across these streams:
- data collection and cleaning
- backend and API
- analysis agents
- orchestration, explainability, and risk
- portfolio, execution, and reporting flow
- frontend dashboard
- integration, DevOps, and docs support

The exact assignment can change, but every active task should clearly belong to one of these streams.

## Required Rules For Every Task

Every task on the board should include:
- owner
- short objective
- dependency if any
- acceptance criteria
- link to the relevant docs section when needed

Tasks should be small enough that:
- one owner is obvious
- review is feasible
- completion can be verified against acceptance criteria

## Documentation Rule

Before building, contributors must read:
- `docs/product/mvp_scope.md`
- `docs/product/agents.md`
- `docs/product/dashboard.md`
- `docs/architecture/system_architecture.md`
- `docs/architecture/backend_architecture.md`
- `docs/architecture/data_flow.md`
- `docs/api/agent_contracts.md`
- `docs/api/backend-api.md`

This prevents people from making incompatible assumptions.

## Pull Request Rules

Each pull request should:
- reference its Kanban card
- explain what changed
- explain how it was tested
- avoid unrelated edits
- update docs if behavior or scope changed

PRs should also avoid mixing:
- product changes and unrelated refactors
- backend and frontend rewrites in one oversized branch
- multiple unrelated Kanban tasks in one merge

## Coordination Rhythm

Recommended weekly cadence:
- start-of-week planning
- mid-week integration review
- end-of-week demo and blocker review

## Scope Discipline

If a task does not directly support the phase-one end-to-end product, move it to backlog instead of active work.

## Escalation Rule

If a contributor sees a conflict between:
- docs and code
- docs and Kanban tasks
- two different docs

they should raise it immediately instead of making a private assumption and continuing.
