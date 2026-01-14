<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/Storyboard.ai-0d1b2a?style=for-the-badge&logo=openai&logoColor=white">
  <img alt="Storyboard.ai" src="https://img.shields.io/badge/Storyboard.ai-0d1b2a?style=for-the-badge&logo=openai&logoColor=white">
</picture>

# Storyboard.ai

> **AI-Powered Visual Storytelling Platform** — An infinite canvas where writers craft narratives with intelligent assistance.

[![SvelteKit](https://img.shields.io/badge/SvelteKit-5.x-FF3E00?style=flat-square&logo=svelte&logoColor=white)](https://kit.svelte.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-412991?style=flat-square&logo=openai&logoColor=white)](https://openai.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.29-326CE5?style=flat-square&logo=kubernetes&logoColor=white)](https://kubernetes.io)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

---

## Overview

Storyboard.ai transforms the creative writing process with a visual, card-based approach to narrative development. Writers arrange story beats on an infinite zoomable canvas while an AI co-author suggests plot progressions, generates scene imagery, and engages in creative dialogue.

### Key Capabilities

| Feature | Description |
|---------|-------------|
| **Infinite Canvas** | Pan & zoom through your story with D3-powered navigation |
| **Story Cards** | Drag-and-drop beats that auto-save and connect visually |
| **AI Plot Suggestions** | GPT-powered next-beat recommendations based on story context |
| **AI Image Generation** | DALL-E scene visualization for each story card |
| **Co-Author Chat** | Per-card conversational AI for brainstorming and refinement |
| **Progressive Summarization** | Automatic story condensation as narratives grow |
| **Multi-Board Support** | Organize projects across separate storyboards |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                              NGINX PROXY                                │
│                         (TLS termination, routing)                      │
└────────────────────────────────┬────────────────────────────────────────┘
                                 │
          ┌──────────────────────┴──────────────────────┐
          │                                             │
          ▼                                             ▼
┌─────────────────────┐                    ┌─────────────────────┐
│      FRONTEND       │                    │       BACKEND       │
│     (SvelteKit)     │◄──── REST API ────►│     (Express.js)    │
│                     │                    │                     │
│  • Infinite Canvas  │                    │  • Card CRUD        │
│  • Story Cards      │                    │  • Board Management │
│  • AI Chat Sidebar  │                    │  • Persistence      │
│  • D3 Zoom/Pan      │                    │                     │
└─────────┬───────────┘                    └──────────┬──────────┘
          │                                           │
          │  OpenAI API                               │
          │  ┌─────────────────┐                      │
          └──│  • GPT-4o-mini  │                      │
             │  • DALL-E 3     │                      │
             └─────────────────┘                      │
                                                      ▼
                                          ┌─────────────────────┐
                                          │     POSTGRESQL      │
                                          │    (Story Data)     │
                                          └─────────────────────┘
```

---

## Tech Stack

### Application Layer

| Component | Technology | Purpose |
|-----------|------------|---------|
| Frontend | [SvelteKit 2](https://kit.svelte.dev) + [Svelte 5](https://svelte.dev) | Reactive UI framework with runes |
| Styling | [Tailwind CSS 4](https://tailwindcss.com) + [shadcn/ui](https://ui.shadcn.com) | Utility-first styling system |
| Canvas | [D3.js](https://d3js.org) (zoom, selection) | Infinite pan/zoom interactions |
| Drag & Drop | [interact.js](https://interactjs.io) | Touch-friendly card manipulation |
| State | [Redux Toolkit](https://redux-toolkit.js.org) | Predictable state management |
| Backend | [Express.js](https://expressjs.com) | RESTful API server |
| Runtime | [Bun](https://bun.sh) | Fast JavaScript runtime |
| Database | [PostgreSQL 15](https://www.postgresql.org) | Relational data persistence |
| AI | [OpenAI API](https://platform.openai.com) | GPT text & DALL-E image generation |

### Infrastructure Layer

| Component | Technology | Purpose |
|-----------|------------|---------|
| Containers | [Docker](https://www.docker.com) | Application packaging |
| Orchestration | [Kubernetes](https://kubernetes.io) + [Helm 3](https://helm.sh) | Production deployment |
| Cloud | [AWS](https://aws.amazon.com) (VPC, EC2, EIP) | Infrastructure hosting |
| IaC | [Terraform 1.6+](https://www.terraform.io) | Infrastructure provisioning |
| Proxy | [NGINX](https://nginx.org) | Reverse proxy & TLS |
| SSL | [Let's Encrypt](https://letsencrypt.org) + [Certbot](https://certbot.eff.org) | Automated certificates |
| K8s Distro | [k3s](https://k3s.io) | Lightweight Kubernetes |

---

## Project Structure

```
storyboard.ai/
├── client/                    # SvelteKit frontend application
│   ├── src/
│   │   ├── lib/
│   │   │   ├── actions/       # Svelte actions (draggable)
│   │   │   ├── components/    # UI components (StoryCard, SideBar, etc.)
│   │   │   ├── services/      # Business logic (AI, board, dialog)
│   │   │   ├── store/         # Redux slices & store configuration
│   │   │   └── types/         # TypeScript type definitions
│   │   └── routes/
│   │       ├── +page.svelte   # Main canvas view
│   │       └── api/           # Server-side API routes (OpenAI proxy)
│   ├── package.json
│   └── Dockerfile
│
├── server/                    # Express.js backend API
│   ├── src/
│   │   ├── controllers/       # Request handlers
│   │   ├── models/            # Database models & queries
│   │   ├── routes/            # API route definitions
│   │   ├── services/          # Business logic layer
│   │   └── middleware/        # Express middleware
│   ├── package.json
│   └── Dockerfile
│
├── db/
│   └── initdb/                # PostgreSQL initialization scripts
│       └── 001-init.sql       # Schema definitions
│
├── nginx/                     # NGINX reverse proxy
│   ├── default.conf           # Server configuration
│   └── Dockerfile
│
├── helm-chart/                # Kubernetes Helm chart
│   ├── Chart.yaml             # Chart metadata
│   ├── values.yaml            # Default configuration
│   └── templates/             # K8s manifest templates
│
├── k8s/                       # Raw Kubernetes manifests
│   ├── *-deployment.yaml      # Deployment specs
│   ├── *-service.yaml         # Service definitions
│   └── ingress.yaml           # Ingress configuration
│
├── infra/                     # Terraform infrastructure
│   ├── main.tf                # Root module (VPC, EC2, k3s)
│   ├── variables.tf           # Input variables
│   ├── outputs.tf             # Output values
│   └── modules/
│       └── vpc/               # VPC module
│
└── docker-compose.yml         # Local development orchestration
```

---

## Getting Started

### Prerequisites

| Tool | Version | Installation |
|------|---------|--------------|
| Node.js | ≥ 20.x | [nodejs.org](https://nodejs.org) |
| Bun | ≥ 1.x | `curl -fsSL https://bun.sh/install \| bash` |
| Docker | ≥ 24.x | [docker.com](https://docs.docker.com/get-docker/) |
| Docker Compose | ≥ 2.x | Included with Docker Desktop |

**Optional (for deployment):**
- `kubectl` & `helm` (Kubernetes deployment)
- `terraform` ≥ 1.6 (AWS infrastructure)
- AWS CLI with configured credentials

### Environment Configuration

Create a `.env` file in the project root:

```bash
# Database
POSTGRES_HOST=db
POSTGRES_PORT=5432
POSTGRES_USER=sb_user
POSTGRES_PASSWORD=your_secure_password
POSTGRES_DB=storyboard

# Backend
EXPRESS_PORT=4000

# OpenAI Integration
OPENAI_KEY=sk-your-openai-api-key
OPENAI_ACTIVE=true
```

### Local Development

#### Option 1: Docker Compose (Recommended)

```bash
# Start all services with hot-reload
docker compose up --build

# Access the application
# → Frontend:  http://localhost:80
# → Backend:   http://localhost:4000 (internal)
# → Database:  localhost:5432
```

#### Option 2: Manual Setup

```bash
# Terminal 1: Start PostgreSQL
docker compose up db

# Terminal 2: Start backend
cd server
bun install
bun run dev

# Terminal 3: Start frontend
cd client
bun install
bun run dev

# Access at http://localhost:5173
```

### Verify Installation

```bash
# Health check (backend)
curl http://localhost:4000/health

# Database connectivity
docker compose exec db psql -U sb_user -d storyboard -c "SELECT 1"
```

---

## Deployment

### Kubernetes with Helm

```bash
# 1. Configure your values
cp helm-chart/values.yaml helm-chart/values.local.yaml
# Edit values.local.yaml with your settings

# 2. Deploy to cluster
helm upgrade --install storyboard ./helm-chart \
  --namespace storyboard \
  --create-namespace \
  -f helm-chart/values.local.yaml

# 3. Verify deployment
kubectl get pods -n storyboard
kubectl logs -l app=frontend -n storyboard
```

### AWS Infrastructure with Terraform

```bash
cd infra

# Initialize Terraform
terraform init

# Review the plan
terraform plan \
  -var="aws_region=us-east-1" \
  -var="ssh_key_name=your-key" \
  -var="openai_key=sk-..." \
  -var="db_password=secure-password" \
  -out=tfplan

# Apply infrastructure
terraform apply tfplan
```

**Outputs:**
- `instance_public_ip` — EC2 public IP address
- `elastic_ip` — Static EIP for DNS configuration

### Production Checklist

- [ ] Generate strong database credentials
- [ ] Configure OpenAI API key with appropriate rate limits
- [ ] Set up DNS A record pointing to Elastic IP
- [ ] Verify TLS certificate provisioning (Let's Encrypt)
- [ ] Configure backup strategy for PostgreSQL
- [ ] Set up monitoring and alerting
- [ ] Review security group ingress rules

---

## API Reference

### Story Cards

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/cards/:boardId` | Retrieve all cards for a board |
| `POST` | `/api/cards/:boardId` | Create a new story card |
| `PATCH` | `/api/cards/:boardId/:id` | Update card content/position |
| `DELETE` | `/api/cards/:boardId/:id` | Remove a story card |

### AI Services (Frontend API Routes)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/suggest` | Generate next plot point suggestion |
| `POST` | `/api/sidebar/message` | AI co-author conversation |
| `POST` | `/api/sidebar/image` | Generate scene visualization |

---

## AI Features

### Plot Suggestion Engine

The suggestion system maintains a rolling summary of your story and generates contextually appropriate next beats:

1. **Progressive Summarization** — Every 5 cards, the story is condensed to maintain context
2. **Context Window** — Recent unsummarized cards + summary inform suggestions
3. **Single-Sentence Output** — Focused, actionable plot points

### Co-Author Chat

Each story card has an AI assistant that:
- Reads the card's content as initial context
- Maintains conversation history (last 5 exchanges)
- Provides warm, collaborative creative feedback

### Scene Visualization

DALL-E 3 generates cover images for story cards based on their content, helping writers visualize scenes as they develop.

---

## Development

### Code Quality

```bash
# Frontend
cd client
bun run lint          # ESLint + Prettier check
bun run check         # Svelte type checking
bun run format        # Auto-format code

# Backend
cd server
bun run build         # TypeScript compilation
```

### Database Migrations

Schema changes go in `db/initdb/`:

```sql
-- db/initdb/002-add-feature.sql
ALTER TABLE storycards ADD COLUMN color VARCHAR(7);
```

For existing deployments, run migrations manually or use a migration tool.

---

## Configuration Reference

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `POSTGRES_HOST` | Yes | — | Database hostname |
| `POSTGRES_PORT` | Yes | `5432` | Database port |
| `POSTGRES_USER` | Yes | — | Database username |
| `POSTGRES_PASSWORD` | Yes | — | Database password |
| `POSTGRES_DB` | Yes | — | Database name |
| `EXPRESS_PORT` | No | `4000` | Backend API port |
| `OPENAI_KEY` | Yes | — | OpenAI API key |
| `OPENAI_ACTIVE` | No | `true` | Enable/disable AI features |

### Helm Values

Key configuration options in `helm-chart/values.yaml`:

```yaml
# Scaling
replicaCount: 1
autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 10

# Domain
websiteDomain: "your-domain.com"

# Resources
frontend:
  image: "your-registry/frontend:latest"
backend:
  image: "your-registry/backend:latest"
```

---

## Troubleshooting

### Common Issues

<details>
<summary><strong>OpenAI API returns "not active" message</strong></summary>

Verify your API key is set and `OPENAI_ACTIVE=true`:

```bash
echo $OPENAI_KEY
docker compose exec frontend env | grep OPENAI
```
</details>

<details>
<summary><strong>Database connection refused</strong></summary>

Ensure PostgreSQL is healthy before backend starts:

```bash
docker compose ps
docker compose logs db
```
</details>

<details>
<summary><strong>Cards not persisting</strong></summary>

Check backend connectivity and database schema:

```bash
curl http://localhost:4000/api/cards/default
docker compose exec db psql -U sb_user -d storyboard -c "\dt"
```
</details>

---

## Contributing

We welcome contributions! Please follow these guidelines:

1. **Fork & Branch** — Create a feature branch from `main`
2. **Conventional Commits** — Use semantic commit messages
   ```
   feat(canvas): add multi-select for cards
   fix(api): handle empty board gracefully
   docs: update deployment instructions
   ```
3. **Quality Gates** — Ensure linting and type checks pass
4. **Pull Request** — Describe changes and link related issues

### Development Workflow

```bash
# Create feature branch
git checkout -b feat/your-feature

# Make changes and verify
bun run lint && bun run check

# Commit with conventional message
git commit -m "feat(scope): description"

# Push and create PR
git push origin feat/your-feature
```

---

## Security

### Reporting Vulnerabilities

Please report security issues privately via email rather than public issues.

### Best Practices

- Store secrets in environment variables or secret management systems
- Use read-only database credentials where possible
- Enable TLS for all production traffic
- Regularly rotate API keys and credentials
- Review security group rules for least-privilege access

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- [OpenAI](https://openai.com) for GPT and DALL-E APIs
- [Svelte](https://svelte.dev) team for the reactive framework
- [D3.js](https://d3js.org) for powerful visualization primitives
- [shadcn/ui](https://ui.shadcn.com) for beautiful component foundations

---

<div align="center">

**Built with ❤️ for storytellers everywhere**

[Report Bug](../../issues) · [Request Feature](../../issues) · [Discussions](../../discussions)

</div>
