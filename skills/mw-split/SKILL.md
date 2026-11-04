---
name: mw-split
description: Split a grilled feature summary into small local tickets in tickets/. Use whenever the user wants to break a feature, grill summary or scope into tickets or tasks, or says "split this into tickets", "break it down", "make tasks from this". Runs after mw-grill in the Minimalistic Workflow.
model: sonnet
---

# mw-split

Turn an agreed scope into tickets that can each be planned, built and reviewed on their own.

## Input

The grill summary in `.mw/grill/<slug>.md`. If there is none, ask the user to run `mw-grill` first or to paste the agreed scope. Do not invent scope.

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
