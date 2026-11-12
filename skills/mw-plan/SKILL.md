---
name: mw-plan
description: Write an implementation plan for a ticket and stop for human approval before any code is written. Use whenever the user wants to plan a ticket, task or change, says "plan this", "make a plan", "how should we do this", or is about to start implementing a ticket from tickets/. Runs before mw-build in the Minimalistic Workflow.
model: opus
disable-model-invocation: true
---

# mw-plan

A plan is the contract `mw-build` implements and `mw-review` checks against. Fixing a plan costs minutes; fixing code costs hours.

## Core principle (think mode)

The user produces the first version of every decision (scope, split, plan, hypothesis, review findings). You critique it, ask questions and execute. Never present your own full solution first. Reveal your own version only after the user's version exists, or when the user explicitly asks for it.

## Mode

Use the mode recorded in the ticket. If none is recorded and the user did not say, propose a mode with a one-sentence reason and let them confirm or override it:

- **think** if the change touches any of the "Sensitive areas" list in `~/minimalistic-workflow/README.md`; or touches paths you have not changed in the last ~3 months (`git log --author="$(git config user.email)" --since=3.months --oneline -- <affected paths>` prints nothing); or involves a design decision that is hard to reverse.
- **ship** otherwise.

Example: `Proposed mode: think — touches migrations and queue retries. OK?` Accept a one-word reply: `ok` keeps the proposal, `think` or `ship` overrides it.

What the modes mean here:

- **think:** the user writes the plan, you review it.
- **ship:** you write the plan, the user approves.

Record the mode in the plan.

## Steps (think mode)

1. Read the ticket (`tickets/NNN-*.md`) or the task the user described.
2. Ask the user to write the plan in the format below: approach, files, data flow, invariants, edge cases, tests. Tell them the plan must be detailed enough that implementation needs no design decisions. They may mark any section `[human]` to implement it themselves. Wait for it.
3. Read `CLAUDE.md`, the relevant docs from its map, and the code the plan touches: callers, tests, similar existing features.
4. Review the user's plan against the code and docs: risks, missing files, existing patterns they should reuse, unhandled cases, and vague parts that would force a design decision during build. Raise each as a question, with `file:line` where it helps. Do not rewrite the plan.
5. When the user has answered, write the plan, in their decisions, to `.mw/plans/NNN-<slug>.md`. Show it and **stop**. Do not edit any source file until the user approves.
6. After approval, offer (do not force) to show how you would have planned it, for comparison.

## Steps (ship mode)

1. Read the ticket (`tickets/NNN-*.md`) or the task the user described.
2. Read `CLAUDE.md`, then the docs from its map that are relevant to this ticket.
3. Read the code you will change and the code around it: callers, tests, similar existing features. Reuse existing patterns.
4. Write the plan to `.mw/plans/NNN-<slug>.md` in the format below.
5. Show the plan and **stop**. Ask for approval. Do not edit any source file until the user approves.

If the ticket is ambiguous or conflicts with the architecture, list the questions in the plan instead of guessing.

## Plan format

`Data flow`, `Invariants` and `Edge cases` are required in think mode and optional in ship mode. A heading or table row marked `[human]` is implemented by the user; `mw-build` skips it.

```markdown
# Plan: NNN <title>

**Mode:** think | ship

## Approach

<3–6 sentences: what and why this way>

## Changes

| File            | Change                      |
| --------------- | --------------------------- |
| path/to/file.ts | new / modify / delete: what |

## Data flow

## Invariants

## Edge cases

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

On approval: `{"step":"plan","ticket":"NNN","date":"<ISO date>","mode":"think|ship","revisions":<rounds of corrections>}`
