# Coding Standards

## Overview

This document defines the coding standards for the AlphaForge AI multi-agent trading platform.

The goal of these standards is to ensure:

- consistent code style
- readable and maintainable code
- easier collaboration between developers
- predictable project structure

All contributors should follow these guidelines when writing or modifying code.

---

# General Principles

All code in the repository should follow these core principles:

- write clean and readable code
- prefer simple solutions over complex ones
- keep functions small and focused
- avoid unnecessary duplication
- document important logic

Readable code is more valuable than clever code.

---

# Python Coding Standards

The backend is written in Python and should follow **PEP8 guidelines**.

Recommended tools:

- black (code formatting)
- flake8 (linting)

---

## File Naming

Python files should use **snake_case**.

Examples:


sentiment_agent.py
market_data_agent.py
portfolio_manager.py
risk_engine.py


---

## Class Naming

Classes should use **PascalCase**.

Example:


class SentimentAgent:
class MarketDataAgent:
class StrategyAgent:


---

## Function Naming

Functions should use **snake_case**.

Example:


def fetch_market_data():
def generate_trading_signal():
def evaluate_risk():


Function names should clearly describe what the function does.

---

## Variable Naming

Variables should use **descriptive snake_case names**.

Good examples:


market_price
trading_signal
portfolio_value
sentiment_score


Avoid vague names such as:


x
data
value


---

# Agent Structure

Each agent should follow a consistent structure.

Example structure:


backend/agents/sentiment_agent/
agent.py
config.py
utils.py


Recommended responsibilities:

agent.py  
→ main agent logic

config.py  
→ configuration parameters

utils.py  
→ helper functions

This structure keeps agents modular and easy to maintain.

---

# Function Design

Functions should follow these guidelines:

- perform one clear task
- avoid excessive nesting
- return structured data
- include docstrings when needed

Example:

```python
def calculate_sentiment_score(text: str) -> float:
    """
    Calculate sentiment score from input text.
    """
    ...
```
Docstrings help developers understand code quickly.

API Code Guidelines

Backend API endpoints should follow consistent patterns.

Example:

@router.get("/signals")
async def get_signals():
    return {"signals": signals}

API routes should:

validate inputs

return structured JSON responses

handle errors gracefully

Frontend Coding Standards

Frontend code should follow modern React practices.

Guidelines include:

use functional components

keep components modular

separate UI and business logic

maintain consistent naming

Example component naming:

MarketDashboard.tsx
PortfolioView.tsx
SignalPanel.tsx
Code Comments

Comments should explain why something is done, not just what the code does.

Example:

Good comment:

# limit position size to prevent excessive exposure

Avoid unnecessary comments like:

# increment counter
counter += 1
Error Handling

All important operations should include error handling.

Example:

try:
    data = fetch_market_data()
except Exception as e:
    logger.error("Failed to fetch market data", e)

Proper error handling improves system reliability.

Logging

Important operations should be logged.

Examples include:

agent execution

trading signals generated

trade execution

system errors

Logs help with debugging and monitoring.

Code Formatting

Developers should run automatic formatters before committing code.

Recommended tools:

black
flake8

Example:

black .
flake8

This ensures consistent formatting across the project.

Testing

New features should include tests where applicable.

Testing may include:

- unit tests

- integration tests

- manual validation

Testing helps prevent system regressions.

Documentation

If code changes impact system architecture or workflows, update documentation accordingly.

Relevant documentation folders include:

docs/architecture
docs/api
docs/deployment
docs/onboarding

Maintaining documentation is part of the development process.