# Setup Guide

This document explains how a teammate should set up AlphaForge locally.

The goal is that any contributor can run the project without guessing the environment.

## Recommended Local Workflow

Use Docker first.

Reason:
- shared environment for the team
- fewer machine-specific issues
- easier backend/frontend integration
- easier onboarding for new contributors

## Required Tools

- Git
- Python 3.10+
- Node.js 18+
- npm
- Docker

Optional:
- Terraform for later infrastructure work only

## Repository Setup

```bash
git clone <repository-url>
cd AlphaForge-AI-Multi-Agent-Trading-Platform
```

## Preferred Startup Method: Docker

```bash
cd infrastructure/docker
docker-compose up --build
```

Expected services:
- backend on `http://localhost:8000`
- frontend on `http://localhost:5173`

## Manual Backend Setup

Use this if you are actively working on the backend and do not want the full Docker flow during development.

```bash
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload
```

## Manual Frontend Setup

Use this if you are actively working on the dashboard.

```bash
cd frontend
npm install
npm run dev
```

## Expected Environment Variables

Keep real secrets out of the repository.

Suggested placeholders:
- `MARKET_DATA_API_KEY`
- `NEWS_API_KEY`
- `SENTIMENT_DATA_API_KEY`
- `FUNDAMENTAL_DATA_API_KEY`
- `APP_ENV`

If a specific provider is selected later, update this document with the exact required variables.

## Setup Verification

A setup is considered valid when:
- the backend health endpoint responds
- the frontend loads successfully
- the local stack can be started without undocumented manual fixes
- one sample ticker request returns a valid response shape

## Setup Troubleshooting Guidance

If setup fails, contributors should check in this order:

1. Docker is installed and running
2. required ports are free
3. repository path is correct
4. backend dependencies are installed if using manual mode
5. frontend dependencies are installed if using manual mode
6. environment variables or placeholders are present if required

If the issue is not solved, the fix should be documented here once confirmed.

## Setup Rule For Contributors

If a teammate had to perform an undocumented setup step to make the project run, the setup documentation is incomplete and should be updated.
