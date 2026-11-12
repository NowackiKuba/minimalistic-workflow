# <Project name>

<One paragraph: what the project is and who uses it.>

## Docs map

Read the docs relevant to a task before planning it. Mark which ones you read.

| Path | What it covers |
|---|---|
| `docs/ARCHITECTURE.md` | Layers, modules, boundaries, patterns |
| `docs/CONVENTIONS.md` | Naming, code style, testing, commits |
| `.github/pull_request_template.md` | PR description template |
| `<add more>` | `<what it covers>` |

## Commands

Raw commands. The workflow skills wrap them with `mw-run` themselves.

```bash
test:      <e.g. pnpm test>
typecheck: <e.g. pnpm tsc --noEmit>
lint:      <e.g. pnpm lint>
```

## Workflow

This project uses Minimalistic Workflow.
Feature: mw-grill → mw-split → mw-plan → mw-build → mw-review → mw-docs → mw-commit → mw-pr → mw-wid
Bug: mw-debug → mw-review → mw-commit → mw-pr → mw-wid
