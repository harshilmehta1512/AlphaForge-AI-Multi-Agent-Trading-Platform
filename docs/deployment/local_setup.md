# Local Development Setup

## Overview

This document explains how to set up the AlphaForge AI multi-agent trading platform for local development.

The project consists of two main components:

- Backend (Python + FastAPI)
- Frontend (React + TypeScript)

Developers can run the system either:

- directly using local environments
- using Docker containers

---

# Prerequisites

Before running the project locally, ensure the following tools are installed:

## Required Software

- Git
- Python 3.10 or newer
- Node.js (version 18 or newer)
- npm or yarn
- Docker (optional but recommended)

---

# Clone the Repository

Clone the project repository.

```bash
git clone https://github.com/<your-org>/alphaForge-ai-trading-platform.git
cd alphaForge-ai-trading-platform
Environment Variables

Copy the example environment file.

cp .env.example .env

Edit the .env file and add required credentials.

Example variables:

API_KEY=
MARKET_DATA_API=
NEWS_API_KEY=
BROKER_API_KEY=
BROKER_SECRET=

These environment variables are used by backend agents to connect to external services.

Backend Setup

Navigate to the backend directory.

cd backend

Create a virtual environment.

python -m venv venv

Activate the environment.

Mac/Linux:

source venv/bin/activate

Windows:

venv\Scripts\activate

Install dependencies.

pip install -r requirements.txt

Start the backend server.

uvicorn main:app --reload

The backend server will start at:

http://localhost:8000
API Documentation

FastAPI automatically generates interactive documentation.

You can access it at:

http://localhost:8000/docs

or

http://localhost:8000/redoc
Frontend Setup

Navigate to the frontend directory.

cd frontend

Install dependencies.

npm install

Start the development server.

npm run dev

The frontend application will start at:

http://localhost:5173
Running the Full System

When both services are running:

Backend API → http://localhost:8000

Frontend Dashboard → http://localhost:5173

The frontend communicates with the backend API to retrieve data and trigger agent workflows.

Optional: Running with Docker

Developers can run the entire system using Docker.

Navigate to the infrastructure folder.

cd infrastructure/docker

Start all services.

docker-compose up

This will start:

backend service

frontend service

optional infrastructure services

Stopping Services

To stop Docker services:

docker-compose down
Troubleshooting

Common issues include:

Dependency Errors

Run:

pip install -r requirements.txt

again to ensure all dependencies are installed.

Port Conflicts

Ensure ports are not already in use.

Default ports:

8000 (backend)

5173 (frontend)

Environment Variables Missing

Make sure .env exists and contains required values.