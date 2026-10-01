# Getting Started with Enterprise AI OS

## Prerequisites

- Python 3.10+
- Node.js 18+
- Docker & Docker Compose
- Kubernetes 1.25+
- Git

## Local Development Setup

### 1. Clone Repository

```bash
git clone https://github.com/Dou20250/enterprise-ai-os.git
cd enterprise-ai-os
```

### 2. Initialize Environment

```bash
# Copy environment template
cp .env.example .env

# Configure your settings
vim .env
```

### 3. Start Infrastructure

```bash
# Start local services (PostgreSQL, Redis, Kafka, etc.)
docker-compose -f docker-compose.dev.yml up -d
```

### 4. Install Dependencies

```bash
# Backend
cd services/ai-orchestrator
pip install -r requirements.txt

# Frontend
cd frontend
npm install
```

### 5. Initialize Database

```bash
# Run migrations
alembic upgrade head
```

### 6. Start Services

```bash
# Terminal 1: AI Orchestrator
cd services/ai-orchestrator
uvicorn main:app --reload --port 8001

# Terminal 2: Control Plane
cd services/control-plane
uvicorn main:app --reload --port 8002

# Terminal 3: Frontend
cd frontend
npm run dev
```

### 7. Access Dashboard

Open http://localhost:3000

Default credentials:
- Username: `admin@example.com`
- Password: `admin123`

## Project Structure

```
enterprise-ai-os/
├── services/
│   ├── ai-orchestrator/          # Agent orchestration engine
│   │   ├── agents/
│   │   ├── llm/
│   │   ├── tools/
│   │   └── main.py
│   ├── data-os/                  # Data layer management
│   │   ├── pipelines/
│   │   ├── connectors/
│   │   └── main.py
│   ├── control-plane/            # Monitoring & management
│   │   ├── api/
│   │   ├── metrics/
│   │   └── main.py
│   ├── cloud-provider/           # Multi-cloud abstraction
│   └── security/                 # IAM & policies
├── frontend/                     # Next.js dashboard
│   ├── pages/
│   ├── components/
│   └── package.json
├── infrastructure/               # Kubernetes & Terraform
│   ├── k8s/
│   └── terraform/
├── docs/                         # Documentation
├── examples/                     # Reference implementations
├── docker-compose.dev.yml
└── README.md
```

## First Steps

### 1. Create Your First Agent

```python
from ai_orchestrator.agents import Agent
from ai_orchestrator.llm import LLMGateway

# Initialize LLM Gateway
llm = LLMGateway(provider="openai", model="gpt-4")

# Create Agent
agent = Agent(
    name="DataAnalyst",
    role="Analyze business data and provide insights",
    llm=llm,
    tools=["query_database", "create_visualization"]
)

# Execute
response = agent.execute("Analyze Q3 sales trends")
print(response)
```

### 2. Create Your First Workflow

```yaml
# workflows/invoice_processing.yml
name: Invoice Processing
description: Automated invoice approval workflow

steps:
  - name: Extract Invoice Data
    agent: DocumentAI
    action: extract_invoice_data
    
  - name: Fraud Detection
    agent: SecurityAgent
    action: detect_anomalies
    
  - name: Budget Verification
    agent: FinanceAgent
    action: verify_budget
    
  - name: Approval
    type: human_approval
    required_role: finance_manager
    
  - name: Process Payment
    agent: TreasuryAgent
    action: process_payment
```

### 3. Monitor with Control Plane

Visit http://localhost:3000/control-plane to:
- View active agents
- Monitor workflows
- Check system health
- Review audit logs

## Common Tasks

### Add a New Data Source

```python
# services/data-os/connectors/new_source.py
from data_os.connectors import BaseConnector

class NewSourceConnector(BaseConnector):
    def connect(self):
        # Implementation
        pass
    
    def extract(self):
        # Implementation
        pass
```

### Add a New Tool for Agents

```python
# services/ai-orchestrator/tools/new_tool.py
from ai_orchestrator.tools import BaseTool

class NewTool(BaseTool):
    name = "new_tool"
    description = "Description of what this tool does"
    
    def execute(self, **kwargs):
        # Implementation
        pass
```

### Deploy to Kubernetes

```bash
# Apply configurations
kubectl apply -f infrastructure/k8s/

# Check deployment status
kubectl get pods -n ai-os

# View logs
kubectl logs -f deployment/ai-orchestrator -n ai-os
```

## Testing

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=services/ --cov-report=html

# Run specific test
pytest tests/test_agent.py -v
```

## Troubleshooting

### Port Already in Use

```bash
# Find process using port
lsof -i :8001

# Kill process
kill -9 <PID>
```

### Database Connection Issues

```bash
# Check PostgreSQL
docker-compose ps postgres

# View logs
docker-compose logs postgres
```

### Missing LLM API Keys

Ensure your `.env` file contains:
```
OPENAI_API_KEY=your_key_here
ANTHROPIC_API_KEY=your_key_here
```

## Next Steps

1. Read [Architecture Overview](./ARCHITECTURE.md)
2. Explore [AI Layer Specification](./specs/AI_LAYER.md)
3. Review [Examples](../examples/)
4. Join our [Discord Community](https://discord.gg/example)

## Support

- 📖 [Documentation](../docs/)
- 🐛 [Issue Tracker](https://github.com/Dou20250/enterprise-ai-os/issues)
- 💬 [Discussions](https://github.com/Dou20250/enterprise-ai-os/discussions)
