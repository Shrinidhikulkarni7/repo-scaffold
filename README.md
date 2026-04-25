# repo-scaffold

A Bob Shell skill for generating production-ready repositories using Copier and community templates.

## Installation

1. Clone this repository to your Bob Shell skills directory:
```bash
git clone <repo-url> ~/.bob/skills/repo-scaffold
```

2. Ensure Copier is installed:
```bash
pipx install copier
```

3. Restart Bob Shell or reload skills

## What This Skill Does

When triggered, this skill:

1. Analyzes the user's project requirements
2. Selects the most appropriate community template (Python, Node, Go, Rust, etc.)
3. Executes Copier to generate the base project structure
4. Enhances the output with architecture documentation and Mermaid diagrams
5. Delivers a complete, runnable repository

## Trigger Conditions

This skill activates when users request:
- "Build me a [language] API"
- "Create a production-ready [framework] service"
- "Set up a [type] tool with proper structure"
- "Make this into a proper repo"
- "Scaffold a [framework] application"

See `repo-scaffold.skill` for the complete trigger description.

## Supported Templates

### Python
- FastAPI/API services (pawamoy/copier-pdm)
- Python libraries (pawamoy/copier-poetry)
- Django applications (cookiecutter-django)
- Data science projects (cookiecutter-data-science)

### Node/TypeScript
- Next.js applications (create-next-app)
- Express APIs (node-express-boilerplate)
- TypeScript libraries (typescript-library-template)

### Go
- Go services (cookiecutter-golang)
- CLI tools (cobra-cli)

### Rust
- Rust CLI applications (rust-cli/cli-template)

See `SKILL.md` for the complete template registry and selection logic.

## Files

- `SKILL.md` - Main skill implementation and logic
- `repo-scaffold.skill` - Skill metadata and trigger conditions
- `README.md` - This file
- `LICENSE` - MIT License

## Requirements

- Bob Shell 1.0.0+
- Copier 9.14.3+
- Git

## Development

To modify this skill:

1. Edit `SKILL.md` for logic changes
2. Edit `repo-scaffold.skill` for trigger condition changes
3. Test with Bob Shell in a clean directory
4. Verify generated projects meet quality standards

## Quality Standards

Generated repositories must include:
- Complete package structure
- README with quick start
- Architecture documentation
- Mermaid diagrams
- CI/CD configuration
- Testing setup
- .env.example
- Proper .gitignore

## License

MIT
