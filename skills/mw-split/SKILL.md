---
name: mw-split
description: Split a grilled feature summary into small local tickets in tickets/. Use whenever the user wants to break a feature, grill summary or scope into tickets or tasks, or says "split this into tickets", "break it down", "make tasks from this". Runs after mw-grill in the Minimalistic Workflow.
model: sonnet
---

# mw-split

Turn an agreed scope into tickets that can each be planned, built and reviewed on their own.

## Core principle (think mode)

The user produces the first version of every decision (scope, split, plan, hypothesis, review findings). You critique it, ask questions and execute. Never present your own full solution first. Reveal your own version only after the user's version exists, or when the user explicitly asks for it.

## Input

The grill summary in `.mw/grill/<slug>.md`. If there is none, ask the user to run `mw-grill` first or to paste the agreed scope. Do not invent scope.

Use the mode recorded in the summary. If none is recorded, ask once: "think or ship?" Copy the mode into every ticket.

## Think mode: the user splits first

1. Ask the user to propose the split: ticket titles, order and dependencies. Wait for it.
2. Critique it against the rules in "How to split": tickets that are too large, hidden dependencies, missing tickets, wrong order. Raise each point as a question or a short remark. Do not rewrite the split unless the user asks.
3. When the user confirms their split, write the ticket files.

In ship mode, you propose the split yourself, as described below.

## How to split

- One ticket = one reviewable PR. If a ticket would touch many unrelated areas, split it.
- Prefer vertical slices (a thin working feature end to end) over horizontal layers ("all entities", then "all endpoints"), unless a shared foundation must land first.
- Put foundations (migrations, shared types) in their own early ticket.
- Every ticket must have acceptance criteria that can be checked by a test or a clear manual step.
- Size: S (under half a day), M (about a day), L (more). Split every L.

## Ticket format

Number tickets after the highest existing number in `tickets/`. File: `tickets/NNN-<slug>.md`.

```markdown
# NNN: <title>

**Feature:** <grill slug>
**Mode:** think | ship
**Size:** S | M
**Depends on:** NNN, NNN | none

## Goal

<one or two sentences>

## Context

<only what this ticket needs from the grill summary>

## Acceptance criteria

- [ ] ...

## Out of scope
```

Show the list (number, title, size, dependencies) and ask for confirmation before writing the files.

Next step: `mw-plan` on the first ticket with no open dependencies.

## Metrics

`{"step":"split","slug":"<slug>","date":"<ISO date>","tickets":<count>}`
