# Repo Scaffold Skill for Bob Shell

A Bob Shell skill that generates production-ready repositories using **Copier** and community templates.

## Overview

This skill orchestrates repository scaffolding by:

- Selecting appropriate community templates for your project type
- Executing Copier with optimal configuration
- Enhancing output with architecture documentation and diagrams
- Delivering complete, runnable projects in seconds

## Prerequisites

Copier must be installed:

```bash
# Recommended: pipx (isolated)
pipx install copier

# Alternative: uv
uv tool install copier

# Verify
copier --version  # Should show v9.14.3 or later
```

## Example Usage in Bob Shell

### Trigger Phrases

- "Build me a Python API"
- "Create a production-ready Node service"
- "Set up a Go CLI tool with proper structure"
- "Make this into a proper repo with CI/CD"
- "Scaffold a Django application"

### Execution Flow

```bash
# User: "Build me a FastAPI service"

# Bob executes:
copier copy gh:pawamoy/copier-pdm my-api

# Then enhances with:
# - docs/architecture.md
# - docs/diagrams/system-overview.mermaid
# - docs/diagrams/data-flow.mermaid

# Result: Production-ready repo in seconds
```

## Curated Template Registry

The skill uses these community templates:

### Python
- **pawamoy/copier-pdm** — Modern Python API (PDM, Ruff, pytest)
- **pawamoy/copier-poetry** — Python packages (Poetry, publishing)
- **cookiecutter/cookiecutter-django** — Django apps (Docker, PostgreSQL)
- **drivendataorg/cookiecutter-data-science** — Data science projects

### Node/TypeScript
- **create-next-app** — Next.js applications
- **hagopj13/node-express-boilerplate** — Express APIs
- **stegano/typescript-library-template** — TypeScript libraries

### Go
- **lacion/cookiecutter-golang** — Go services
- **spf13/cobra-cli** — CLI tools

### Rust
- **rust-cli/cli-template** — Rust CLI apps

## What Gets Generated

Every scaffolded project includes:

✅ Complete package structure  
✅ README with quick start  
✅ Architecture documentation (added by skill)  
✅ Mermaid diagrams (added by skill)  
✅ CI/CD pipeline (GitHub Actions)  
✅ Docker configuration  
✅ Testing setup  
✅ Linting and type checking  
✅ .env.example  
✅ .gitignore  

## Template Updates

Projects can receive updates when templates evolve:

```bash
# Update project when template evolves
cd my-project
copier update

# Update to specific version
copier update --vcs-ref v2.0.0
```

## Skill Architecture

```
repo-scaffold/
├── SKILL.md              # Main skill logic (template selection, execution)
├── README.md             # This file
├── LICENSE
└── repo-scaffold.skill   # Skill metadata
```

## Quality Standards

Every generated repo:

- [ ] Clone and run in under 5 minutes
- [ ] Clear README with quick start
- [ ] Architecture documentation
- [ ] At least 2 Mermaid diagrams
- [ ] CI/CD on every push
- [ ] Complete .env.example
- [ ] Proper .gitignore
- [ ] No hardcoded secrets
- [ ] Template update instructions

## Contributing

To improve this skill:

1. Add new community templates to `SKILL.md`
2. Improve template selection logic
3. Enhance post-generation customizations
4. Update documentation

## Credits

- **Copier** — https://github.com/copier-org/copier
- **Cookiecutter** — https://github.com/cookiecutter/cookiecutter
- Community template authors

## License

MIT