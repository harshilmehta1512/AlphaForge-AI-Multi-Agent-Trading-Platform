# Developer Setup Guide

## Overview

This document explains how new developers can set up the AlphaForge AI multi-agent trading platform on their local machines.

The project consists of several main components:

- Backend (Python + FastAPI)
- Frontend (React + TypeScript)
- Infrastructure (Docker + Terraform)

Developers should follow the steps below to prepare their development environment.

---

# Prerequisites

Ensure the following tools are installed before setting up the project.

Required software:

- Git
- Python 3.10+
- Node.js (version 18+)
- npm or yarn
- Docker (recommended)
- Terraform (optional for infrastructure development)

---

# Clone the Repository

First clone the repository from GitHub.

```bash
git clone https://github.com/<org-name>/<repo-name>.git
cd <repo-name>
```

Replace <org-name> and <repo-name> with the actual repository details.

Environment Variables

The project uses environment variables to manage sensitive configuration values.

Create a .env file in the project root.

Example:

.env

Example variables:

MARKET_DATA_API_KEY=
NEWS_API_KEY=
BROKER_API_KEY=
BROKER_SECRET=
JWT_SECRET=

These values should never be committed to the repository.

Backend Setup

Navigate to the backend directory.

```bash
cd backend
```

Create a python virtual environment

```bash
cd python -m venv venv
```

Activate the environment

Mac/Linux:
```bash
source venv/bin/activate
```

Windows:
```bash
venv\Scripts\activate
```

Install dependencies
```bash
pip install -r requirements.txt
```

Run the backend server

```bash
uvicorn main:app --reload
```

The backend server will start at:
http://localhost:8000

API Documentation

FastAPI automatically generates API documentation.

Access it at:

http://localhost:8000/docs

or

http://localhost:8000/redoc
Frontend Setup

Navigate to the frontend directory.

```bash 
cd frontend
```

Install dependencies

```bash
npm install
```

Start the frontend development server

```bash
npm run dev
```

The frontend dashboard will run at:
http://localhost:5173

Running with Docker (Optional)

The system can also be run using Docker containers.

Navigate to:

```bash
cd infrastructure/docker
```

Start the services:

```bash
docker-compose up
```

This will start all required services for local development

Verifying Setup

Once both services are running:

Backend API:

http://localhost:8000

Frontend Dashboard:

``://localhost:5173


If both are accessible, the development environment is correctly configured.

---

# Troubleshooting

Common issues include:

Dependency errors  
→ Run `pip install -r requirements.txt` again.

Port conflicts  
→ Ensure ports **8000** and **5173** are available.

Missing environment variables  
→ Ensure `.env` is properly configured.