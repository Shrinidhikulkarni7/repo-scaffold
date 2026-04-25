---
name: repo-scaffold
description: |
  Use this skill when a task warrants a production-ready repository — not just code, but the full developer experience around it. Trigger when the user asks to "build", "create", "make", or "set up" something that is clearly a standalone project, tool, service, library, API, CLI, or application — especially when it has multiple moving parts, will be shared or deployed, or when the user says things like "make it proper", "full project", "production-ready", "like a real repo", or "with everything". Also trigger when the user has been iterating on code and you sense the output deserves a real home — architecture docs, diagrams, CI, a README, the works. Do NOT trigger for throwaway scripts, one-off code snippets, analysis tasks, or when the user explicitly wants just the code.
---

# Repo Scaffold Skill

Produce a complete, developer-ready repository — not just source code, but everything a real project needs: architecture diagrams, README, CI/CD config, contribution guide, environment setup, and a coherent folder structure.

---

## Step 0 — Decide if this warrants a full repo

Before scaffolding anything, ask: does this task actually need a repo?

**Yes → full scaffold** when the output is:
- A deployable service, API, or backend
- A library or SDK meant to be reused
- A CLI tool
- A multi-file application
- Something the user will share, maintain, or hand off to others
- Something with infra concerns (DB, auth, queues, deployment)

**No → just write code** when the output is:
- A single script or utility function
- A quick data analysis
- A proof-of-concept/throwaway spike
- Something the user explicitly scoped as small

If in doubt, lean toward the full scaffold — the user asked for it for a reason.

---

## Step 1 — Understand the project

Extract from the conversation (or ask if missing):
- **What it does** — core purpose in one sentence
- **Tech stack** — language, framework, runtime
- **Deployment target** — local only, cloud, Docker, serverless, etc.
- **Audiences** — who will use/maintain this
- **Key integrations** — databases, APIs, external services

Do NOT ask more than 2 clarifying questions. Infer the rest and state your assumptions explicitly.

---

## Step 2 — Plan the repo structure

Choose the right layout for the stack. Below are canonical patterns — adapt, don't copy blindly.

### Generic (language-agnostic) layout
```
project-name/
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── release.yml
├── docs/
│   ├── architecture.md
│   └── diagrams/
│       ├── system-overview.mermaid
│       └── data-flow.mermaid
├── src/                  # or lib/, app/, pkg/ — match language conventions
├── tests/
├── scripts/              # dev scripts (setup, seed, migrate)
├── .env.example
├── .gitignore
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

### Python service
```
project/
├── src/project/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   └── routes/ or handlers/ or models/
├── tests/
├── pyproject.toml        # or setup.cfg, requirements files
├── Dockerfile
├── docker-compose.yml
└── ... (standard above)
```

### Node / TypeScript
```
project/
├── src/
│   ├── index.ts
│   └── ...
├── tests/
├── package.json
├── tsconfig.json
├── .eslintrc.json
└── ... (standard above)
```

### Go service
```
project/
├── cmd/server/main.go
├── internal/
├── pkg/
├── Makefile
├── go.mod
└── ...
```

---

## Step 3 — Generate all the things

Produce **every** file below that applies. Do not skip unless there's a good reason stated explicitly.

### 3a. README.md

Structure:
```markdown
# Project Name
> One-liner tagline

## What it does
[2–4 sentences, plain language]

## Quick start
[Minimal steps to go from zero to running — under 10 lines]

## Architecture
[Short prose + link to docs/architecture.md]

## Configuration
[.env vars table: name | required | default | description]

## Development
[How to install deps, run tests, lint]

## Deployment
[How to build and ship]

## Contributing
[One-liner pointing to CONTRIBUTING.md]

## License
```

Keep it honest. Don't pad it.

### 3b. Architecture doc (`docs/architecture.md`)

Structure:
```markdown
# Architecture

## Overview
[System in 3–5 sentences]

## Components
[Each major component: what it is, what it does, how it talks to others]

## Data flow
[Prose walk-through of the main request/event/data path]

## Key decisions
[ADR-lite: What, Why, Trade-offs — for 2–4 important choices]

## External dependencies
[List with: name | purpose | docs link]
```

### 3c. Diagrams

Produce **Mermaid** diagrams as `.mermaid` files in `docs/diagrams/`. Always produce at minimum:

1. **System overview** — the major boxes and their relationships
2. **Data / request flow** — sequence or flowchart of the primary happy path

Additional diagrams as needed:
- **ER diagram** if there's a database
- **State machine** if there's a workflow with states
- **Deployment diagram** if there's infra complexity

Example system overview:
```mermaid
graph TB
    Client -->|HTTPS| API[API Gateway]
    API --> Auth[Auth Service]
    API --> App[App Server]
    App --> DB[(Postgres)]
    App --> Cache[(Redis)]
    App --> Queue[Job Queue]
    Queue --> Worker[Background Worker]
```

Example sequence:
```mermaid
sequenceDiagram
    participant U as User
    participant A as API
    participant DB as Database
    U->>A: POST /api/resource
    A->>DB: INSERT record
    DB-->>A: record_id
    A-->>U: 201 Created
```

### 3d. CI/CD (`.github/workflows/ci.yml`)

At minimum: install → lint → test → build. Match to the stack.

Python example:
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: '3.12' }
      - run: pip install -e ".[dev]"
      - run: ruff check .
      - run: pytest --cov
```

### 3e. `.env.example`

Every env var the project uses, with placeholder values and a comment:
```
# Database
DATABASE_URL=postgres://user:pass@localhost:5432/dbname

# Auth
JWT_SECRET=change-me-in-production
JWT_EXPIRY_HOURS=24

# External APIs
OPENAI_API_KEY=sk-...
```

### 3f. `.gitignore`

Full, stack-appropriate gitignore. Use established templates (Python, Node, Go, etc.) and append project-specific additions.

### 3g. `CONTRIBUTING.md`

```markdown
# Contributing

## Setup
[Steps to get a dev environment running]

## Branching
- `main` — production
- Feature branches: `feat/short-description`
- Bug fixes: `fix/short-description`

## Commit style
[Conventional commits or whatever the project uses]

## Pull requests
[What to include, who to tag, review SLA]

## Running tests
[Command(s)]
```

### 3h. `Dockerfile` (if applicable)

Multi-stage, minimal, non-root user:
```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM python:3.12-slim
WORKDIR /app
COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY src/ ./src/
RUN useradd -m appuser && chown -R appuser /app
USER appuser
CMD ["python", "-m", "src.main"]
```

### 3i. `Makefile` (optional but highly recommended)

Wrap common dev commands:
```makefile
.PHONY: install test lint build run clean

install:
	pip install -e ".[dev]"

test:
	pytest

lint:
	ruff check . && mypy src/

build:
	docker build -t project-name .

run:
	uvicorn src.main:app --reload

clean:
	find . -type d -name __pycache__ -exec rm -rf {} +
```

---

## Step 4 — Deliver the output

### If using computer tools (bash / file creation):
1. Create all files under `/home/<your-ai-agent>/<project-name>/`
2. Run a quick sanity check (e.g., `find . -type f | head -30`) to confirm structure
3. Copy to `/mnt/user-data/outputs/<project-name>/`
4. Call `present_files` on the most important files: README, architecture doc, main diagram
5. Give a brief summary of what was generated — one line per major file

### If writing to chat only (no file tools):
- Output each file in a clearly labelled code block
- Use the order: README → architecture → diagrams → CI → supporting files
- End with a tree view of the full structure

---

## Step 5 — What NOT to generate

Skip unless asked or clearly needed:
- **Tests** — scaffold the folder and a sample test; writing all tests is a separate task
- **Full business logic** — stub it with TODOs; the user asked for repo structure, not implementation
- **Kubernetes manifests / Terraform** — offer to add them, don't include by default
- **CHANGELOG.md** — create it empty with the format header only

---

## Quality bar

Every repo scaffold should pass this mental checklist:

- [ ] A new developer can clone and run this in under 5 minutes following the README
- [ ] The architecture doc explains *why*, not just *what*
- [ ] At least 2 Mermaid diagrams exist and are accurate
- [ ] No secrets or hardcoded credentials anywhere
- [ ] CI runs on every push
- [ ] .env.example covers every variable the app uses
- [ ] .gitignore is complete for the stack
- [ ] The folder structure would make a senior engineer nod, not wince
