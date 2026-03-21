# Branch Strategy

Use a simple branch strategy.

## Main Rules

- `main` must stay stable
- no direct pushes to `main`
- every change goes through a pull request

## Branch Naming

Use one of:
- `feature/<short-name>`
- `fix/<short-name>`
- `docs/<short-name>`
- `chore/<short-name>`

Examples:
- `feature/market-data-agent`
- `feature/orchestrator-explainability`
- `docs/mvp-scope`

## Pull Request Size

Keep PRs small and reviewable. A PR should ideally finish one task card or one narrow slice of a task.

## Recommended Merge Rules

- do not merge directly without review
- prefer squash merge if the branch contains many small iterative commits
- keep `main` deployable or at least stable

## Branch Protection Expectations

The repository should ideally enforce:
- no direct pushes to `main`
- pull request review before merge
- basic checks before merge if CI is configured

## Branch Hygiene

- delete merged branches
- rebase or sync from `main` regularly if your work is long-lived
- avoid letting feature branches drift for too long without integration
