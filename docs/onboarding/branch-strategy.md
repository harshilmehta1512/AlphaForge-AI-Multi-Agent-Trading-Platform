# Branch Strategy

## Overview

This document describes the branching strategy used in the AlphaForge AI multi-agent trading platform repository.

The goal of this strategy is to ensure:

- organized development
- clear ownership of features
- stable main branch
- easy collaboration between contributors

All developers should follow this strategy when contributing to the project.

---

# Main Branch

The repository uses a single primary branch:


main


The **main branch** represents the stable version of the system.

Rules for the main branch:

- must always remain stable
- should not contain experimental code
- changes should be merged through pull requests
- direct commits to main should be avoided

---

# Feature Branches

All new work should be done in **feature branches** created from the main branch.

Feature branches allow developers to work independently without affecting the main codebase.

Example branch names:


feature/sentiment-agent
feature/news-analysis-agent
feature/technical-analysis-agent
feature/fundamental-analysis-agent
feature/portfolio-management
feature/trade-execution
feature/frontend-dashboard


Branch names should clearly describe the feature being developed.

---

# Creating a Feature Branch

Before starting development, create a new branch from `main`.

Example:

```bash
git checkout main
git pull origin main
git checkout -b feature/<feature-name>
```

Example:

```bash
git checkout -b feature/sentiment-analysis-agent
```

This creates a new branch where development can safely occur.

Development Workflow

The recommended development workflow is:

1. Create a feature branch

2. Implement changes

3. Test locally

4. Commit changes

5. Push branch to GitHub

6. Open a pull request

7. Review changes

8. Merge into main

Committing Changes

Commit messages should clearly describe the change.

Recommended format:

type: short description

Examples:

feat: add sentiment analysis agent
fix: resolve API response error
docs: update architecture documentation
refactor: improve agent orchestration logic

Clear commit messages improve repository history and collaboration.

Pushing Feature Branches

After committing changes, push the branch to GitHub.

Example:

```bash 
git push origin feature/sentiment-analysis-agent
```

This makes the branch visible to the team.

Pull Requests

When development is complete, open a Pull Request (PR).

Pull requests should include:

description of the feature

explanation of code changes

screenshots or logs if applicable

Pull requests allow the team to review code before merging.

Code Review

Before merging a pull request:

another team member should review the code

reviewers verify functionality and structure

suggested improvements may be requested

Once approved, the branch can be merged into main.

Branch Naming Guidelines

Branches should follow this format:

feature/<feature-name>
fix/<bug-name>
docs/<documentation-update>
refactor/<code-improvement>

Examples:

feature/sentiment-agent
fix/api-error-handling
docs/update-architecture
refactor/orchestrator-logic

This naming system makes the repository easier to understand.

Branch Cleanup

After a feature branch has been merged:

it should be deleted from the repository

developers should pull the latest changes from main

Example:

```bash 
git checkout main 
git pull origin main 
```

This ensures everyone stays up to date.

Best Practices

Recommended practices include:

- avoid large pull requests

- commit changes frequently

- write clear commit messages

- pull updates from main regularly

- communicate major changes with the team