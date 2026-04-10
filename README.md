<div align="center">

# 🤖 BusinessAgent
### Business Multi-Agent Orchestrator — *frysda*

[![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115%2B-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![LangChain](https://img.shields.io/badge/LangChain-LangGraph-1C3C3C?style=for-the-badge&logo=chainlink&logoColor=white)](https://langchain.com)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com)
[![Azure](https://img.shields.io/badge/Azure-Container%20Apps-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)](https://azure.microsoft.com)
[![Teams](https://img.shields.io/badge/Microsoft-Teams%20Bot-6264A7?style=for-the-badge&logo=microsoftteams&logoColor=white)](https://teams.microsoft.com)

> **REST API + Microsoft Teams Bot** powered by a supervisor-style multi-agent LLM orchestrator.  
> Automates backlog engineering, architecture diagrams, BPMN processes, documents, presentations, web research, and data visualization — all from a single conversational interface.

</div>

---

## 📐 Architecture Overview

```
╔══════════════════════════════════════════════════════════════════════════╗
║                         ENTRY POINTS                                     ║
║                                                                          ║
║   👥 Microsoft Teams          🌐 External App / Swagger UI              ║
║   (group chat or DM)          (REST client, automation, testing)         ║
║          │                                  │                            ║
║          ▼                                  ▼                            ║
║   Azure Bot Service              X-API-Key header                        ║
║   (JWT validation)                                                       ║
║          │                                  │                            ║
║          ▼                                  ▼                            ║
║   POST /api/messages             POST /api/v1/chat                       ║
╚══════════════════════════════════════════════════════════════════════════╝
                          │                │
                          └────────┬───────┘
                                   ▼
╔══════════════════════════════════════════════════════════════════════════╗
║                    🧠  CHIEF ARCHITECT — frysda                          ║ 
║                    (LangGraph ReAct Supervisor)                          ║
║                                                                          ║
║   ┌─ Knowledge Base Search (BEFORE every response) ──────────────────┐   ║
║   │  Reads prior domain context, decisions, terminology               │  ║
║   └───────────────────────────────────────────────────────────────────┘  ║
║                                                                          ║
║   EXECUTION MODES (auto-detected from intent):                           ║
║   ┌──────────────────────────────────────────────────────────────────┐   ║
║   │  MODE 1 │ User Story Engineering  →  JIRA + DOCS                 │   ║
║   │  MODE 2 │ Heuristic Audit         →  JIRA + MIRO + SLIDES        │   ║
║   │  MODE 3 │ Discovery & Architecture→  WEB + PROCESS + ARCHITECT   │   ║
║   │  MODE 4 │ Mass Change Management  →  JIRA + DOCS + PROCESS       │   ║
║   │  MODE 5 │ Ad-hoc Orchestration    →  Intelligent routing         │   ║
║   │  MODE 6 │ Data Visualization      →  CHARTS + WEB/JIRA           │   ║
║   └──────────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════════╝
          │          │         │        │         │       │       │       │
          ▼          ▼         ▼        ▼         ▼       ▼       ▼       ▼
     ┌────────┐ ┌────────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌────┐ ┌────┐ ┌──────┐
     │  JIRA  │ │  DOCS  │ │SLIDE │ │ARCHI │ │PROCE │ │WEB │ │MIRO│ │CHART │
     │ Agent  │ │ Agent  │ │ Agent│ │ TECT │ │  SS  │ │Agt │ │Agt │ │ Agent│
     └───┬────┘ └───┬────┘ └──┬───┘ └──┬───┘ └──┬───┘ └──┬─┘ └──┬─┘ └──┬───┘
         │          │         │        │         │        │      │      │
         ▼          ▼         ▼        ▼         ▼        ▼      ▼      ▼
     Atlassian   Word/PDF  PowerPoint Draw.io  Camunda  Tavily  Miro   PNG
      Jira API   .docx     .pptx      .drawio  BPMN     Search  Board  Chart
                                                                          │
                                                                          ▼
╔══════════════════════════════════════════════════════════════════════════╗
║                    📚  KNOWLEDGE BASE (Auto-Learn)                       ║
║   After EVERY interaction: extracts & persists domain decisions,         ║
║   user preferences, project terminology → business_output/knowledge_base ║
╚══════════════════════════════════════════════════════════════════════════╝
```

---

## 🤖 Specialist Agents

| # | Agent | Capabilities | Output |
|---|-------|-------------|--------|
| 1 | 🎫 **JIRA** | Create/update issues, User Stories, sprints, backlog engineering | Atlassian Jira |
| 2 | 📄 **DOCS** | Word documents, PDF, structured reports | `.docx` / `.pdf` |
| 3 | 📊 **SLIDES** | PowerPoint presentations, executive decks | `.pptx` |
| 4 | 🏗️ **ARCHITECT** | Architecture diagrams (hexagonal, C4, layered, cloud) | `.drawio` |
| 5 | 🔄 **PROCESS** | BPMN process models, swimlanes, Camunda deployment | `.bpmn` |
| 6 | 🌐 **WEB** | Web research, URL extraction, Tavily search | Markdown text |
| 7 | 🧩 **MIRO** | Brainstorming boards, sticky notes, mind maps | Miro board |
| 8 | 📈 **CHARTS** | Bar, line, pie, scatter charts, data visualization | `.png` |

---

## 🔁 Detailed Flow — Local Testing

```
Developer Machine
─────────────────────────────────────────────────────────────────────────

  Terminal 1: API Server                Terminal 2: Dev UI (optional)
  ┌─────────────────────────────┐       ┌─────────────────────────────┐
  │ python api_server.py        │       │ python dev_ui/server.py     │
  │                             │       │                             │
  │ ✅ Startup events:         │       │ ✅ Mock Teams-like UI       │
  │  · load .env                │       │  · Simulates group chat     │
  │  · init GPT-4o via OpenAI   │       │  · @frysda mention support  │
  │  · create orchestrator      │       │  · Knowledge base viewer    │
  │  · register 8 agents        │       └──────────────┬──────────────┘
  │  · init Bot adapter (Teams) │                      │ HTTP
  │                             │                      ▼
  │ 📡 Listening on :8000       │            http://localhost:8080
  └────────────┬────────────────┘
               │
     ┌─────────▼──────────────────────────┐
     │  http://localhost:8000/docs        │  ← Swagger UI
     │  http://localhost:8000/health      │  ← Health probe
     │  http://localhost:8000/api/v1/chat │  ← Orchestrator entry
     └─────────┬──────────────────────────┘
               │
     ┌─────────▼───────────────────────────────────────────────────── ─┐
     │  POST /api/v1/chat                                              │
     │                                                                 │
     │  Request body:                                                  │
     │  {                                                              │
     │    "conversation_id": "session-01",                             │
     │    "message": "Crie um diagrama de arquitetura hexagonal",      │
     │    "user_name": "Nilton"                                        │
     │  }                                                              │
     │                                                                 │
     │  Headers: X-API-Key: <your-api-key>   (optional locally)        │
     └─────────┬───────────────────────────────────────────────────── ─┘
               │
               ▼
     ┌───────────────────────────────────────────────────────────────── ┐
     │              🧠 ChiefArchitect.invoke()                          │
     │                                                                  │
     │  1. search_knowledge_base("arquitetura hexagonal")               │
     │     → retrieves prior project context (if any)                   │
     │                                                                  │
     │  2. Analyzes intent → MODE 3 (Discovery & Architecture)          │
     │     Presents execution plan to user                              │
     │                                                                  │
     │  3. Delegates:                                                   │
     │     · delegate_to_web     → researches hexagonal architecture    │
     │     · delegate_to_architect → generates .drawio file             │
     │                                                                  │
     │  4. Composes final response                                      │
     │                                                                  │
     │  5. _auto_learn() → extracts & saves domain knowledge            │
     │     → business_output/knowledge_base/<hash>.txt                  │
     └─────────┬──────────────────────────────────────────────────────  ┘
               │
               ▼
     ┌─────────────────────────────────────────────────────────────────┐
     │  Response JSON:                                                 │
     │  {                                                              │
     │    "response": "## Diagrama Gerado\n...",                       │
     │    "conversation_id": "session-01",                             │
     │    "files": ["diagrams/order_management_hexagonal.drawio"]      │
     │  }                                                              │
     └─────────────────────────────────────────────────────────────────┘
```

---

## ☁️ Detailed Flow — Microsoft Teams (Azure Production)

```
Microsoft Teams
─────────────────────────────────────────────────────────────────────────

  👤 User types in Teams group:
     "@frysda crie uma User Story para o módulo de pagamentos"
               │
               ▼
  ┌────────────────────────────────────────────────────────────────────┐
  │                    Microsoft Teams Client                          │
  │  Detects @frysda mention → routes to registered bot                │
  └────────────────────────┬───────────────────────────────────────────┘
                           │  Teams internal routing
                           ▼
  ┌────────────────────────────────────────────────────────────────────┐
  │                    Azure Bot Service                               │
  │                                                                    │
  │  · Validates bot registration (TEAMS_APP_ID)                       │
  │  · Signs request with JWT (RS256)                                  │
  │  · Delivers Activity payload to Messaging Endpoint:                │
  │    POST https://<app>.azurecontainerapps.io/api/messages           │
  └────────────────────────┬───────────────────────────────────────────┘
                           │  HTTPS POST (JWT signed)
                           ▼
  ┌────────────────────────────────────────────────────────────────────┐
  │               Azure Container App — api_server.py                  │
  │                         Port 8000                                  │
  │                                                                    │
  │  POST /api/messages                                                │
  │    ↓                                                               │
  │  BotFrameworkAdapter.process_activity()                            │
  │    · Validates JWT signature against Microsoft JWKS                │
  │    · Deserializes Teams Activity object                            │
  │    ↓                                                               │
  │  BusinessBot.on_message_activity()                                 │
  │    · Extracts: conversation_id, user_text, user_name               │
  │    · Sends typing indicator back to Teams                          │
  │    · Loads conversation history (CosmosDB or in-memory)            │
  │    ↓                                                               │
  │  _orchestrator.invoke(user_text, chat_history=history)             │
  └────────────────────────┬───────────────────────────────────────────┘
                           │
                           ▼
  ┌────────────────────────────────────────────────────────────────────┐
  │              🧠 ChiefArchitect — same as local flow                │
  │                                                                    │
  │  1. search_knowledge_base(user_text)                               │
  │  2. Intent classification → MODE 1 (User Story Engineering)        │
  │  3. Presents execution plan                                        │
  │  4. delegate_to_jira:                                              │
  │     · Principal BA agent drafts complete User Story                │
  │     · Validates acceptance criteria, BDD scenarios                 │
  │     · Creates issue in Atlassian Jira via API                      │
  │  5. _auto_learn() → persists domain knowledge                      │
  │     (Azure File Share: /app/business_output/knowledge_base/)       │
  └────────────────────────┬───────────────────────────────────────────┘
                           │
                           ▼
  ┌────────────────────────────────────────────────────────────────────┐
  │              BusinessBot.on_message_activity()                     │
  │                                                                    │
  │  · Appends HumanMessage + AIMessage to history                     │
  │  · Saves updated history to CosmosDB (or in-memory)                │
  │  · turn_context.send_activity(MessageFactory.text(response))       │
  └────────────────────────┬───────────────────────────────────────────┘
                           │  Bot Framework reply
                           ▼
  ┌────────────────────────────────────────────────────────────────────┐
  │                    Azure Bot Service                               │
  │  Routes response back to originating Teams conversation            │
  └────────────────────────┬───────────────────────────────────────────┘
                           │
                           ▼
  👥 All group members see frysda's response in Teams chat
     with formatted Markdown, tables, and action items
```

---

## 🌐 API Reference

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/health` | — | Azure liveness / readiness probe |
| `POST` | `/api/messages` | Bot Framework JWT | Microsoft Teams webhook — only used by Azure Bot Service |
| `POST` | `/api/v1/chat` | `X-API-Key` | Send a message to the orchestrator |
| `DELETE` | `/api/v1/conversations/{id}` | `X-API-Key` | Clear conversation history for a session |
| `POST` | `/api/v1/ingest` | `X-API-Key` | Ingest a URL into the knowledge base |
| `GET` | `/api/v1/files/charts/{filename}` | `X-API-Key` | Download a generated PNG chart |
| `GET` | `/api/v1/files/bpmn/{filename}` | `X-API-Key` | Download a generated BPMN file |
| `GET` | `/api/v1/files/diagrams/{filename}` | `X-API-Key` | Download a generated Draw.io diagram |

> Interactive docs: **`/docs`** (Swagger UI) · **`/redoc`** (ReDoc)

### Chat Request / Response

```jsonc
// POST /api/v1/chat
{
  "conversation_id": "session-42",       // any string — groups messages into a session
  "message": "Crie um BPMN de aprovação de escopo",
  "user_name": "Nilton"                  // optional — used in Teams group mode
}

// 200 OK
{
  "response": "## Processo Gerado\n\nO BPMN foi criado em...",
  "conversation_id": "session-42"
}
```

---

## 📚 Knowledge Base — Persistent Memory

The orchestrator **automatically learns** from every interaction:

```
User message + Agent response
          │
          ▼
    _auto_learn()
          │
    GPT-4o extracts:
    · Domain decisions made
    · User preferences
    · Project terminology
    · Recurring patterns
          │
          ▼
    add_knowledge_entry()
          │
    business_output/knowledge_base/<hash>.txt
          │
          ▼
    Available on next startup via search_knowledge_base()
```

> **Local**: stored at `./business_output/knowledge_base/`  
> **Azure**: persisted on Azure File Share mounted at `/app/business_output/knowledge_base/`

---

## 🚀 Quick Start — Local

```bash
# 1. Clone & install
git clone https://github.com/filhormnilton/BusinessAgent
cd BusinessAgent
pip install -r requirements.txt

# 2. Configure
cp .env.example .env
# Edit .env — minimum required: OPENAI_API_KEY

# 3a. Start API server + Swagger
python api_server.py
# → http://localhost:8000/docs

# 3b. Start Dev UI (Teams simulator)
python dev_ui/server.py
# → http://localhost:8080
```

### Docker

```bash
docker compose up --build
# → http://localhost:8000
```

---

## ⚙️ Environment Variables

```bash
cp .env.example .env
```

| Variable | Required | Description |
|----------|----------|-------------|
| `OPENAI_API_KEY` | ✅ | OpenAI API key (GPT-4o) |
| `TEAMS_APP_ID` | Teams only | Azure Bot App ID |
| `TEAMS_APP_PASSWORD` | Teams only | Azure Bot client secret |
| `API_KEY` | Recommended | REST API auth key (`X-API-Key` header) |
| `JIRA_SERVER` | Jira features | Jira instance URL |
| `JIRA_USER` | Jira features | Jira user email |
| `JIRA_API_TOKEN` | Jira features | Jira API token |
| `JIRA_PROJECT_KEY` | Jira features | Default project key(s), comma-separated |
| `MIRO_ACCESS_TOKEN` | Miro features | Miro OAuth token |
| `MIRO_BOARD_ID` | Miro features | Target Miro board ID |
| `TAVILY_API_KEY` | Web search | Tavily search API key |
| `CAMUNDA_REST_URL` | BPMN deploy | Camunda engine REST URL |
| `COSMOS_ENDPOINT` | Recommended | CosmosDB for persistent chat history |
| `COSMOS_KEY` | Recommended | CosmosDB primary key |
| `BUSINESS_OUTPUT_DIR` | Optional | Output directory (default: `./business_output`) |

---

## ☁️ Azure Deployment

### Prerequisites

| Resource | Purpose |
|----------|---------|
| Azure Container Registry (ACR) | Docker image storage |
| Azure Container App | Runs the API server |
| Azure Bot Service | Teams channel integration |
| Azure File Share | Persistent `business_output/` |
| Azure CosmosDB *(optional)* | Persistent conversation history |

### GitHub Actions (CI/CD)

The workflow at `.github/workflows/deploy.yml` is triggered **manually** until Azure secrets are configured.

**Required GitHub Secrets:**

```
AZURE_CREDENTIALS          → Service principal JSON
REGISTRY_LOGIN_SERVER      → e.g. businessagentregistry.azurecr.io
REGISTRY_USERNAME          → ACR admin username
REGISTRY_PASSWORD          → ACR admin password
AZURE_RESOURCE_GROUP       → Resource group name
CONTAINER_APP_NAME         → Container App name
```

**Deploy:**
> GitHub → Actions → *Build & Deploy to Azure Container Apps* → **Run workflow**

### Messaging Endpoint (Azure Bot)

After deploy, set in **Azure Bot Resource → Settings → Messaging endpoint**:

```
https://<your-app>.azurecontainerapps.io/api/messages
```

---

## 📁 Project Structure

```
BusinessAgent/
├── api_server.py              ← Production entrypoint (FastAPI + Teams webhook)
├── business_main.py           ← Local CLI for testing (not used in Azure)
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── .env.example
├── .github/
│   └── workflows/
│       └── deploy.yml         ← CI/CD to Azure Container Apps
└── Business/
    ├── config.py              ← Centralized configuration (env vars)
    ├── agents/
    │   ├── base.py
    │   ├── jira_agent.py
    │   ├── docs_agent.py
    │   ├── slides_agent.py
    │   ├── architect_agent.py
    │   ├── process_agent.py
    │   ├── web_agent.py
    │   ├── miro_agent.py
    │   └── charts_agent.py
    ├── mcp/
    │   ├── api_jira.py
    │   ├── api_knowledge_base.py
    │   ├── api_drawio.py
    │   ├── api_powerpoint.py
    │   ├── api_miro.py
    │   ├── api_web.py
    │   ├── api_charts.py
    │   └── api_office_pdf.py
    ├── orchestrator/
    │   └── chief_architect.py ← LangGraph supervisor (frysda persona)
    └── teams_bot/
        ├── bot.py             ← BusinessBot activity handler
        └── history_store.py   ← In-memory or CosmosDB history
```

---

<div align="center">

**frysda** · Chief Business Architect · Powered by GPT-4o + LangGraph

</div>

## Azure Deployment

1. Push to `main` branch on GitHub
2. Configure secrets in GitHub → Settings → Secrets and variables → Actions
3. Run the workflow manually: Actions → Build & Deploy to Azure Container Apps → Run workflow
4. Get the Container App URL from Azure Portal
5. Set in Azure Bot → Configuration → Messaging endpoint: `https://<app-url>/api/messages`
6. Connect to Microsoft Teams channel in Azure Bot → Channels
