# Minimalistic Workflow

A small set of Claude Code skills for shipping features and fixing bugs end to end. Each skill does one thing. The human approves the plan; the agent implements, verifies and documents.

## Flows

**Feature:** `mw-grill` → `mw-split` → `mw-plan` → `mw-build` → `mw-review` → `mw-docs` → `mw-commit` → `mw-pr`

**Bug:** `mw-debug` → `mw-review` → `mw-commit` → `mw-pr`

| Skill | Does |
|---|---|
| `mw-grill` | Reads docs and code, asks questions until the feature is unambiguous, writes a summary |
| `mw-split` | Turns a grill summary into local tickets in `tickets/` |
| `mw-plan` | Turns a ticket into a plan and stops for approval |
| `mw-build` | Implements the approved plan with a test/typecheck/lint loop |
| `mw-debug` | Reproduces a bug with a failing test, then fixes it |
| `mw-review` | Reviews the diff against the ticket, plan and conventions |
| `mw-docs` | Updates docs the change made stale |
| `mw-commit` | Small, logical conventional commits |
| `mw-pr` | PR description from the project's PR template |

## Files the workflow uses

```
CLAUDE.md                 # project map: docs, commands (see templates/CLAUDE.md)
tickets/NNN-slug.md       # local tickets (mw-split)
.mw/grill/<slug>.md       # grill summaries (mw-grill)
.mw/plans/NNN-slug.md     # approved plans (mw-plan)
.mw/metrics.jsonl         # one line per step, for measuring the workflow
```

## Install

```bash
git clone <repo-url> ~/minimalistic-workflow
cp -r ~/minimalistic-workflow/skills/* ~/.claude/skills/
```

Then add the sections from `templates/CLAUDE.md` to your project's `CLAUDE.md`.

## Principles

1. Fixed context lives in docs and skills; the task prompt carries only what is unique.
2. A human approves the plan before code is written. Fixing a plan is cheaper than fixing code.
3. Nothing is "done" while tests, typecheck or lint are red.
4. Review happens in a fresh session, so the reviewer does not share the author's assumptions.
5. Every step logs one metrics line, so you can tell whether the workflow actually helps.
