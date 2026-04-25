# repo-scaffold

> A skill that produces full, production-ready repositories — not just code, but the entire developer experience.

## What it does

When you ask  to build something real — an API, a CLI, a service, a library — this skill kicks in and generates the complete repo: folder structure, architecture docs, Mermaid diagrams, CI/CD workflows, `.env.example`, `CONTRIBUTING.md`, `Dockerfile`, `Makefile`, and a proper README. It knows when to go all-in and when to just write the code.

## Install

Download [`repo-scaffold.skill`](./repo-scaffold.skill) and install it in AI agent via **Settings → Skills → Install from file**.

## What gets generated

When the skill decides a full scaffold is warranted, AI agent produces:

| File/Folder | What it is |
|---|---|
| `README.md` | Honest, structured, no fluff |
| `docs/architecture.md` | Overview, components, data flow, key decisions (ADR-lite) |
| `docs/diagrams/` | Mermaid diagrams — system overview + data flow at minimum |
| `.github/workflows/ci.yml` | install → lint → test → build |
| `.env.example` | Every env var, commented |
| `.gitignore` | Stack-appropriate, complete |
| `CONTRIBUTING.md` | Setup, branching, commit style, PR process |
| `Dockerfile` | Multi-stage, minimal, non-root |
| `Makefile` | Common dev commands wrapped |

Stack layouts are provided for Python, Node/TypeScript, and Go — adapted, not blindly copied.

## When it triggers

The skill includes a judgment gate. It scaffolds fully when the task is a deployable service, reusable library, CLI tool, multi-file application, or anything that will be shared, maintained, or handed off. It skips the scaffold for throwaway scripts, one-off analyses, and anything explicitly scoped as small.

**Phrases that trigger it:**
- "build me an API that..."
- "create a CLI tool for..."
- "make it production-ready"
- "set up a full project"
- "I want to put this on GitHub"

**Phrases that don't:**
- "write a script to..."
- "quick function to..."
- "just give me the code"

## Example prompts

```
Build a FastAPI service that processes webhook events and stores them in Postgres
```
```
Create a CLI tool in Go that syncs files between S3 buckets
```
```
Make a Node.js library for validating Indian GST numbers — proper repo, I want to publish it
```

## What it intentionally skips

- Full test suites (it scaffolds the folder + one sample test — writing all tests is a separate task)
- Full business logic (stubs with TODOs)
- Kubernetes manifests / Terraform (offers to add, doesn't impose)

## License

MIT
