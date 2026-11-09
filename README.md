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

## Core principle

The user produces the first version of every decision (scope, split, plan, hypothesis, review findings). The AI critiques it, asks questions and executes. The AI never presents its own full solution first. It may reveal its own version only after the user's version exists, or when the user explicitly asks for it.

## Think mode vs ship mode

`mw-grill`, `mw-plan` and `mw-debug` start by asking "think or ship?" unless you already said. The mode is recorded in the grill summary, ticket, plan and debug report, so later steps follow it.

| | **think** (default for non-trivial work) | **ship** (small fixes, renames, boilerplate) |
|---|---|---|
| `mw-grill` | You write goal, scope, out of scope, edge cases, risks; the AI asks only about gaps | The AI drives the questions |
| `mw-split` | You propose the split; the AI critiques it | The AI proposes the split |
| `mw-plan` | You write the plan; the AI reviews it, then offers its own version for comparison | The AI writes the plan |
| `mw-build` | Builds only what the plan specifies, stops on open design decisions, ends with a comprehension check | Builds the plan |
| `mw-debug` | You give the hypothesis and the fix; the AI hints in steps and critiques | The AI finds the cause and proposes the fix |
| `mw-review` | You list your findings first; output is You found / You missed / False alarms | Blockers / Suggestions |

In both modes you approve before anything is written, and a plan section marked `[human]` is left for you to implement.

## Files the workflow uses

```
CLAUDE.md                 # project map: docs, commands (see templates/CLAUDE.md)
tickets/NNN-slug.md       # local tickets (mw-split)
.mw/grill/<slug>.md       # grill summaries (mw-grill)
.mw/plans/NNN-slug.md     # approved plans (mw-plan)
.mw/metrics.jsonl         # one line per step, for measuring the workflow (gitignored)
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
5. Every step logs one metrics line, so you can tell whether the workflow actually helps. `.mw/metrics.jsonl` stays local: it belongs in `.gitignore`, and the steps that log metrics ask you to add it when it is missing.
