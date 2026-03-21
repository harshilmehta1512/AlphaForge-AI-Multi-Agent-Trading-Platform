# Tech Stack

This document records the tech stack for the first-phase AlphaForge build and the reason for each choice.

## Core Stack

### Backend
- Python
- FastAPI

### Frontend
- React
- TypeScript

### Local Runtime
- Docker
- docker-compose

### Infrastructure Later
- Terraform

## Why This Stack

## Python
Chosen because it fits:
- financial data processing
- data cleaning
- model experimentation
- agent implementation

## FastAPI
Chosen because it provides:
- fast API development
- request validation
- typed contracts
- clean integration with Python services

## React
Chosen because the product needs a dashboard-oriented UI with multiple connected views.

## TypeScript
Chosen because the dashboard will consume structured API contracts and type safety reduces integration errors.

## Docker
Chosen as the default local workflow because the team has multiple contributors working in parallel and needs a reproducible environment.

## Terraform
Chosen for later infrastructure provisioning, not as a first-phase delivery dependency.

## Retrieval And Data Access Direction

For phase one:
- use structured cleaned datasets
- use internal service/data access patterns
- avoid forcing a generic RAG system into the MVP

If a later phase requires search over large document collections such as filings or archived research, retrieval can be added intentionally at that time.

## What We Are Deliberately Not Choosing Right Now

For phase one, the team is intentionally not prioritizing:
- Kubernetes
- complex distributed queues
- mandatory WebSocket infrastructure
- generic RAG setup without a concrete retrieval problem
- live broker connectivity as a foundational dependency

Those choices are deferred because they add complexity faster than they add product value in the current phase.

## Decision Stability

This stack is the default unless there is a documented reason to change it.

If a contributor wants to introduce a major new technology, they should be able to explain:
- what problem it solves
- why the current stack is insufficient
- why it will not delay the MVP
