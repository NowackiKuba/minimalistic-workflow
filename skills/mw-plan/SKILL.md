---
name: mw-plan
description: Write an implementation plan for a ticket and stop for human approval before any code is written. Use whenever the user wants to plan a ticket, task or change, says "plan this", "make a plan", "how should we do this", or is about to start implementing a ticket from tickets/. Runs before mw-build in the Minimalistic Workflow.
model: opus
disable-model-invocation: true
---

# mw-plan

A plan is the contract `mw-build` implements and `mw-review` checks against. Fixing a plan costs minutes; fixing code costs hours.

## Steps

1. Read the ticket (`tickets/NNN-*.md`) or the task the user described.
2. Read `CLAUDE.md`, then the docs from its map that are relevant to this ticket.
3. Read the code you will change and the code around it: callers, tests, similar existing features. Reuse existing patterns.
4. Write the plan to `.mw/plans/NNN-<slug>.md` in the format below.
5. Show the plan and **stop**. Ask for approval. Do not edit any source file until the user approves.

If the ticket is ambiguous or conflicts with the architecture, list the questions in the plan instead of guessing.

## Plan format

```markdown
# Plan: NNN <title>

## Approach

<3–6 sentences: what and why this way>

## Changes

| File            | Change                      |
| --------------- | --------------------------- |
| path/to/file.ts | new / modify / delete: what |

## Tests

- <test case> — <what it proves>

## Risks

- <risk> — <mitigation>

## Questions

- <only if any>

## Docs read

- <path>
```

## After feedback

Update the plan file with the user's corrections and show only what changed. Count each round of corrections.

## Metrics

On approval: `{"step":"plan","ticket":"NNN","date":"<ISO date>","revisions":<rounds of corrections>}`
