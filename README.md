# CoAgents Research Canvas

A research assistant application built with CopilotKit and LangGraph.

## Architecture

```
┌─────────────────┐     ┌─────────────────────────────────┐
│   Frontend      │     │   Backend (FastAPI + LangGraph) │
│   (Next.js)     │────▶│   /copilotkit/agents/*          │
│   Port: 3000    │     │   Port: 8000                    │
└─────────────────┘     └─────────────────────────────────┘
```

## Prerequisites

- Node.js 18+
- Python 3.12
- Poetry (Python package manager)

## Quick Start

### 1. Setup Environment Variables

**Backend (`agent/.env`):**
```bash
cp agent/.env.example agent/.env
# Edit agent/.env and add your API keys
```

**Frontend (`ui/.env.local`):**
```bash
cp ui/.env.local.example ui/.env.local
# Edit ui/.env.local and add your API keys
```

### 2. Install Dependencies

**Backend:**
```bash
cd agent
poetry install
```

**Frontend:**
```bash
cd ui
npm install
```

### 3. Run the Application

**Terminal 1 - Backend:**
```bash
cd agent
poetry run uvicorn research_canvas.demo:app --host 0.0.0.0 --port 8000 --reload
```

**Terminal 2 - Frontend:**
```bash
cd ui
npm run dev
```

### 4. Open the Application

Navigate to http://localhost:3000

## Important Notes

### Why `langgraph dev` doesn't work

The `langgraph dev` command runs the native LangGraph API server which exposes endpoints like:
- `/threads`
- `/assistants`
- `/runs`

However, the CopilotKit frontend expects endpoints at:
- `/copilotkit/agents/research_agent`

The `demo.py` file sets up a FastAPI server with the proper CopilotKit endpoints using `add_langgraph_fastapi_endpoint()`.

### Available Models

Set the `MODEL` environment variable in `agent/.env`:
- `google_genai` - Google Gemini (default)
- `openai` - OpenAI GPT
- `anthropic` - Anthropic Claude

## Project Structure

```
coagents-research-canvas/
├── agent/                    # Python backend
│   ├── research_canvas/
│   │   ├── demo.py          # FastAPI server with CopilotKit endpoints
│   │   ├── langgraph/       # LangGraph agent definition
│   │   │   ├── agent.py     # Main graph definition
│   │   │   ├── chat.py      # Chat node
│   │   │   ├── search.py    # Search node
│   │   │   └── state.py     # Agent state
│   │   └── crewai/          # CrewAI alternative agent
│   ├── pyproject.toml
│   └── .env
├── ui/                       # Next.js frontend
│   ├── src/
│   │   └── app/
│   │       ├── page.tsx
│   │       └── api/
│   │           └── copilotkit/
│   │               └── route.ts  # CopilotKit API route
│   ├── package.json
│   └── .env.local
└── README.md
```

## Troubleshooting

### Error: HTTP 404: Not Found
This means the frontend can't find the backend endpoints. Make sure you're running `demo.py` with uvicorn, not `langgraph dev`.

### Error: Missing API Key
Check your `.env` files have the required API keys set.

### Backend not responding
Ensure port 8000 is not in use by another process:
```bash
lsof -i :8000
```
