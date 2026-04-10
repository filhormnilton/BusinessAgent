# BusinessAgent — Business Multi-Agent Orchestrator

REST API + Microsoft Teams Bot powered by a multi-agent LLM orchestrator ("frysda").

## Architecture

```
Microsoft Teams  →  Azure Bot Service  →  POST /api/messages  →  Orchestrator (frysda)
External App     →  POST /api/v1/chat (X-API-Key)             →  Orchestrator (frysda)
                                                                        │
                              ┌─────────────────────────────────────────┤
                              ▼         ▼         ▼         ▼          ▼
                            JIRA      DOCS     SLIDES   ARCHITECT    BPMN
                           AGENT     AGENT     AGENT     AGENT       AGENT
                              ▼         ▼         ▼         ▼          ▼
                           WEB       MIRO      CHARTS   Knowledge
                           AGENT     AGENT     AGENT      Base
```

## Agents

| Agent | Scope |
|-------|-------|
| JIRA | Backlog, User Stories, Issues |
| DOCS | Word, PDF, TXT documents |
| SLIDES | PowerPoint presentations |
| ARCHITECT | Draw.io architecture diagrams |
| PROCESS | Camunda BPMN process models |
| WEB | Research & web extraction |
| MIRO | Brainstorming boards |
| CHARTS | PNG charts & graphs |

## API Endpoints

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/health` | none | Azure liveness/readiness probe |
| `POST` | `/api/messages` | Bot Framework JWT | Microsoft Teams webhook |
| `POST` | `/api/v1/chat` | `X-API-Key` | Send message to orchestrator |
| `DELETE` | `/api/v1/conversations/{id}` | `X-API-Key` | Clear conversation history |
| `POST` | `/api/v1/ingest` | `X-API-Key` | Ingest URL into knowledge base |
| `GET` | `/api/v1/files/charts/{filename}` | `X-API-Key` | Download generated PNG chart |
| `GET` | `/api/v1/files/bpmn/{filename}` | `X-API-Key` | Download generated BPMN file |
| `GET` | `/api/v1/files/diagrams/{filename}` | `X-API-Key` | Download Draw.io diagram |

Interactive docs available at `/docs` after deploy.

## Environment Variables

Copy `.env.example` to `.env` and fill in the values:

```bash
cp .env.example .env
```

Key variables:

| Variable | Description |
|----------|-------------|
| `OPENAI_API_KEY` | OpenAI API key |
| `TEAMS_APP_ID` | Azure Bot registration App ID |
| `TEAMS_APP_PASSWORD` | Azure Bot App Password (client secret) |
| `API_KEY` | Secret for REST API authentication |
| `JIRA_SERVER` | Jira instance URL |
| `JIRA_USER` | Jira user email |
| `JIRA_API_TOKEN` | Jira API token |

## Run Locally

```bash
pip install -r requirements.txt
cp .env.example .env   # fill in credentials
python api_server.py
```

## Run with Docker

```bash
docker compose up --build
```

## Azure Deployment

1. Push to `main` branch on GitHub
2. Configure secrets in GitHub → Settings → Secrets and variables → Actions
3. Run the workflow manually: Actions → Build & Deploy to Azure Container Apps → Run workflow
4. Get the Container App URL from Azure Portal
5. Set in Azure Bot → Configuration → Messaging endpoint: `https://<app-url>/api/messages`
6. Connect to Microsoft Teams channel in Azure Bot → Channels
