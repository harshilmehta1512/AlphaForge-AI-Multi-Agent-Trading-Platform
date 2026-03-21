# Coding Standards

Keep standards simple and enforceable.

## General Rules

- prefer clear code over clever code
- keep functions focused
- avoid mixing multiple responsibilities in one module
- document assumptions where the logic is not obvious

## Python

- follow PEP 8
- use type hints where practical
- keep file names in `snake_case`
- return structured data, not ad hoc dictionaries when schemas exist

## Frontend

- use TypeScript
- keep components focused
- keep API response types explicit
- separate presentation from request logic where practical

## Agent Rule

Each agent should stay inside its own domain. If a market-data agent starts reading sentiment inputs directly, explainability and ownership become unclear.

## Backend Rule

- keep route files thin
- place workflow logic in services, not endpoints
- validate inputs and outputs against shared contracts
- avoid hidden side effects

## Documentation Rule

If code behavior changes in a way that affects:
- product scope
- agent behavior
- dashboard behavior
- API contracts
- setup instructions

the relevant docs must be updated in the same change.
