
# IncidentIQ — AI-Powered Incident Intelligence & Root Cause Analysis

IncidentIQ is an AI-powered incident management and investigation platform designed to help engineering teams detect, investigate, and resolve production incidents faster.

It combines Python backend engineering, event-driven architecture, Retrieval-Augmented Generation (RAG), and Agentic AI to analyze application logs, alerts, historical incidents, service dependencies, and operational documentation.

The platform uses AI agents to investigate incidents, identify potential root causes, gather supporting evidence, and recommend remediation actions with human approval.

---

## Project Overview

Modern distributed applications generate large volumes of logs, alerts, metrics, and traces.

When production incidents occur, engineers often need to manually investigate multiple systems to identify the root cause.

IncidentIQ brings these capabilities into a centralized intelligence platform.

### Key Capabilities

- Centralized incident management
- Application log ingestion and processing
- Alert correlation and deduplication
- Service and environment management
- Knowledge base and document management
- Advanced RAG with hybrid search
- AI-powered incident investigation
- Root cause analysis with supporting evidence
- Agentic workflows using LangGraph
- Human-in-the-loop remediation approval
- Incident reporting and analytics
- Observability and audit logging

---

## Architecture

IncidentIQ follows a modular monolith architecture with independently deployed background workers.

The application uses asynchronous processing and event-driven communication for ingestion and long-running operations.

### High-Level Components

- FastAPI — Backend API
- PostgreSQL — Primary database
- pgvector — Vector similarity search
- RabbitMQ — Message broker
- Celery — Background processing
- Redis — Caching and coordination
- MinIO / Amazon S3 — Document storage
- LangChain — LLM and retrieval integrations
- LangGraph — Stateful AI workflows
- OpenAI / Gemini / Ollama — LLM providers
- OpenTelemetry — Application instrumentation
- Prometheus and Grafana — Monitoring

See [docs/architecture/HLD.md](docs/architecture/HLD.md) for the detailed architecture.

---

## Technology Stack

| Category | Technologies |
|---|---|
| Language | Python |
| Backend | FastAPI |
| ORM | SQLAlchemy |
| Database | PostgreSQL |
| Vector Database | pgvector |
| Cache | Redis |
| Message Broker | RabbitMQ |
| Task Processing | Celery |
| AI Framework | LangChain |
| Agent Orchestration | LangGraph |
| LLM Providers | OpenAI, Gemini, Ollama |
| Embeddings | Configurable embedding providers |
| Storage | MinIO, Amazon S3 |
| Containerization | Docker, Docker Compose |
| Cloud | AWS |
| Observability | OpenTelemetry, Prometheus, Grafana |
| Testing | Pytest |

---

## Core Modules

### 1. Authentication and Authorization

- JWT authentication
- Access and refresh tokens
- Role-based access control
- User and permission management
- Audit logging

### 2. Service Management

- Application and microservice registration
- Environment management
- Service dependencies
- Service ownership and metadata

### 3. Log Intelligence

- REST-based log ingestion
- Batch processing
- Log parsing and normalization
- Severity classification
- Log filtering and search
- Duplicate event handling

### 4. Incident Management

- Incident creation and tracking
- Severity classification
- Alert grouping
- Incident assignment
- Incident timeline
- Comments and status transitions

### 5. Knowledge Base

- PDF, DOCX, TXT and Markdown ingestion
- Document versioning
- Text extraction and cleaning
- Chunking and embedding generation
- Document access control
- Semantic and keyword search

### 6. Advanced RAG

- Vector search using pgvector
- PostgreSQL full-text search
- Hybrid retrieval
- Reciprocal Rank Fusion
- Reranking
- Query rewriting
- Context construction
- Citation generation and validation
- Retrieval and generation evaluation

### 7. AI Investigation Agents

- Incident Investigation Agent
- Root Cause Analysis Workflow
- Remediation Recommendation Workflow
- Tool calling
- Persistent agent state
- Checkpointing
- Human approval
- Execution tracing

### 8. Reporting and Analytics

- Incident frequency and severity
- MTTA and MTTR
- Service health trends
- Root cause categories
- AI usage and cost
- Incident postmortem generation

---

## AI Investigation Workflow

1. Engineer starts an investigation.
2. Incident context and service information are loaded.
3. Related logs and alerts are retrieved.
4. Historical incidents are searched.
5. Relevant runbooks and architecture documents are retrieved using RAG.
6. LangGraph orchestrates the investigation.
7. AI produces possible root causes with supporting evidence.
8. The system recommends diagnostic and remediation steps.
9. Authorized engineers review and approve any side-effecting action.
10. The investigation report and audit history are persisted.

AI-generated root causes are hypotheses that require evidence and engineering validation.

---

## Repository Structure

```text
incidentiq-ai-incident-intelligence/
├── app/
│   ├── api/
│   ├── core/
│   ├── domain/
│   ├── schemas/
│   ├── models/
│   ├── repositories/
│   ├── services/
│   ├── ingestion/
│   ├── rag/
│   ├── agents/
│   ├── llm/
│   ├── integrations/
│   ├── workers/
│   └── infrastructure/
├── alembic/
├── tests/
├── evals/
├── scripts/
├── infrastructure/
├── docs/
├── .github/
├── docker-compose.yml
├── Dockerfile
├── pyproject.toml
├── requirements.txt
├── .env.example
├── Makefile
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.11+
- Docker and Docker Compose
- Git
- An LLM provider API key, or a local Ollama installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/incidentiq-ai-incident-intelligence.git

cd incidentiq-ai-incident-intelligence
```

### 2. Configure environment variables

```bash
cp .env.example .env
```

Configure database, RabbitMQ, Redis, storage and LLM credentials.

Never commit `.env` or real credentials to GitHub.

### 3. Start infrastructure

```bash
docker compose up -d
```

### 4. Install dependencies

```bash
python -m venv .venv

source .venv/bin/activate

pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### 5. Run database migrations

```bash
alembic upgrade head
```

### 6. Start the API

```bash
uvicorn app.main:app --reload
```

### 7. Start background workers

```bash
celery -A app.workers.celery_app worker --loglevel=info
```

### 8. Open API documentation

```text
http://localhost:8000/docs
```

The exact startup commands and environment variables will be finalized with the first working release.

---

## Environment Configuration

An example `.env.example` should include:

```env
APP_ENV=development
APP_NAME=IncidentIQ
DEBUG=false

DATABASE_URL=postgresql+asyncpg://user:password@localhost:5432/incidentiq

REDIS_URL=redis://localhost:6379/0
CELERY_BROKER_URL=amqp://guest:guest@localhost:5672//

OBJECT_STORAGE_PROVIDER=minio
MINIO_ENDPOINT=localhost:9000
MINIO_ACCESS_KEY=
MINIO_SECRET_KEY=
MINIO_BUCKET=incidentiq-documents

LLM_PROVIDER=openai
OPENAI_API_KEY=
GEMINI_API_KEY=
OLLAMA_BASE_URL=http://localhost:11434

JWT_SECRET_KEY=replace-with-a-secure-secret
```

Use development-only credentials locally. Production secrets must be managed through a secure secrets manager.

---

## Testing

Run the test suite:

```bash
pytest
```

Run unit tests:

```bash
pytest tests/unit
```

Run integration tests:

```bash
pytest tests/integration
```

Run security tests:

```bash
pytest tests/security
```

Run RAG and agent evaluations:

```bash
python scripts/run_evaluations.py
```

---

## Evaluation and Quality

The project evaluates multiple dimensions:

### Backend
- API latency
- Throughput
- Error rate
- Ingestion throughput
- Queue processing delay

### RAG
- Recall@K
- Mean Reciprocal Rank
- nDCG
- Citation correctness
- Answer faithfulness

### Agentic AI
- Investigation task success
- Root cause top-k accuracy
- Tool execution success
- Invalid tool-call rate
- Human approval outcomes

Actual evaluation results will be published after running the test datasets.

---

## Deployment

The target cloud deployment uses:

- AWS ECS Fargate
- Amazon RDS PostgreSQL
- Amazon S3
- Amazon MQ for RabbitMQ
- Amazon ElastiCache Redis
- Amazon ECR
- Application Load Balancer
- AWS Secrets Manager
- Amazon CloudWatch

See [docs/deployment/](docs/deployment/) for deployment instructions.

---

## Security

Security is a core part of the design.

- JWT authentication and RBAC
- Document and incident access checks
- Input validation and upload restrictions
- Secret management
- Prompt-injection defenses
- Allowlisted AI tools
- Human approval for sensitive actions
- Audit logging
- Rate limiting and request-size limits

---

## Development Roadmap

- [ ] Foundation and authentication
- [ ] Service and incident management
- [ ] Log ingestion and asynchronous processing
- [ ] Document management and ingestion
- [ ] Basic RAG implementation
- [ ] Advanced hybrid retrieval and reranking
- [ ] RAG evaluation framework
- [ ] LangGraph incident investigation
- [ ] Root cause analysis
- [ ] Human-in-the-loop remediation
- [ ] Observability and production hardening
- [ ] AWS deployment
- [ ] Dashboard and demo release

---

## Engineering Documentation

Detailed project documentation will be maintained under `docs/`.

- HLD and LLD
- Database schema
- API specifications
- Architecture Decision Records
- Security design
- Deployment architecture
- Operational runbooks

---

## License

MIT License — see [LICENSE](LICENSE).

---

## Disclaimer

IncidentIQ is an engineering and learning project.

AI-generated root cause analysis and remediation recommendations are decision-support outputs. They should not be treated as verified diagnoses or executed against production infrastructure without appropriate engineering review and authorization.
```

