---
name: mw-grill
description: Grill a feature idea or requirement until nothing is ambiguous, grounded in the project's docs and code. Use whenever the user wants to discuss, clarify, scope, stress-test or grill a feature, change or idea before building it, or says things like "let's grill this", "let's scope this feature", "let's talk this through before we start". First step of the Minimalistic Workflow feature flow.
model: opus
disable-model-invocation: true
---

# mw-grill

Turn a rough idea into a clear, agreed scope before any ticket or code exists. Questions are cheap now and expensive after implementation.

## 1. Read first, ask second

Before the first question:

1. Read `CLAUDE.md` and its docs map. Read every doc relevant to the idea (at minimum architecture and conventions if they exist).
2. Search the codebase for related modules, entities, endpoints and existing solutions.
3. Tell the user in 2–4 lines what you found: relevant docs, existing code that already does part of this, and any conflict with the architecture.

Do not ask anything the docs or code already answer. Asking it wastes the user's time and signals you did not read.

## 2. Grill

Ask 1–3 questions at a time, most important first. Cover, as relevant:

- **Goal:** who is this for, what problem, how will we know it works
- **Scope:** what is explicitly out of scope
- **Behaviour:** happy path, edge cases, errors, empty states, limits
- **Data:** new entities or fields, migrations, existing data
- **Architecture fit:** which layer/module, does it follow the documented patterns, any deviation and why
- **Security & permissions:** who can do this, what input is untrusted
- **Non-functional:** performance, concurrency, idempotency, retries
- **Dependencies:** other teams, services, tickets

When an answer is vague, push back with a concrete scenario ("what happens if two users do X at once?"). When you disagree with a decision, say so once with the reason, then accept the user's call.

Stop when every point above is either answered or consciously deferred.

## 3. Summary

Write the summary to `.mw/grill/<slug>.md` and show it to the user for confirmation:

```markdown
# <Feature name>

## Goal

## In scope

## Out of scope

## Decisions

- <decision> — <why>

## Edge cases & behaviour

## Data changes

## Architecture notes

## Open questions (deferred)

## Docs to update

## Docs read
```

Next step: `mw-split`.

## Metrics

Append one line to `.mw/metrics.jsonl`:
`{"step":"grill","slug":"<slug>","date":"<ISO date>","questions":<number asked>,"rounds":<rounds>}`
