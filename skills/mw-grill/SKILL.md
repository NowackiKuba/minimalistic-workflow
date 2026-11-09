---
name: mw-grill
description: Grill a feature idea or requirement until nothing is ambiguous, grounded in the project's docs and code. Use whenever the user wants to discuss, clarify, scope, stress-test or grill a feature, change or idea before building it, or says things like "let's grill this", "let's scope this feature", "let's talk this through before we start". First step of the Minimalistic Workflow feature flow.
model: opus
---

# mw-grill

Turn a rough idea into a clear, agreed scope before any ticket or code exists. Questions are cheap now and expensive after implementation.

## Core principle (think mode)

The user produces the first version of every decision (scope, split, plan, hypothesis, review findings). You critique it, ask questions and execute. Never present your own full solution first. Reveal your own version only after the user's version exists, or when the user explicitly asks for it.

## 0. Mode

If the user did not say, ask once: "think or ship?"

- **think** (default for non-trivial work): the user writes first; the think-mode rules below apply.
- **ship** (routine work: small fixes, renames, boilerplate): you drive the questions and draft the summary; the user approves.

Record the mode in the summary so later steps follow it.

## 1. The user writes first (think mode)

Ask the user to write their own understanding of the feature:

- **Goal:** who is it for, what problem, how will we know it works
- **Scope:** what is in
- **Out of scope**
- **Edge cases**
- **Risks**

Wait for it. Do not suggest content for any of these points before the user's text exists.

In ship mode, skip this step.

## 2. Read first, ask second

Before the first question:

1. Read `CLAUDE.md` and its docs map. Read every doc relevant to the idea (at minimum architecture and conventions if they exist).
2. Search the codebase for related modules, entities, endpoints and existing solutions.
3. Tell the user in 2–4 lines what you found: relevant docs, existing code that already does part of this, and any conflict with the architecture.

Do not ask anything the docs or code already answer. Asking it wastes the user's time and signals you did not read.

## 3. Grill

Ask 1–3 questions at a time, most important first. Cover, as relevant:

- **Goal:** who is this for, what problem, how will we know it works
- **Scope:** what is explicitly out of scope
- **Behaviour:** happy path, edge cases, errors, empty states, limits
- **Data:** new entities or fields, migrations, existing data
- **Architecture fit:** which layer/module, does it follow the documented patterns, any deviation and why
- **Security & permissions:** who can do this, what input is untrusted
- **Non-functional:** performance, concurrency, idempotency, retries
- **Dependencies:** other teams, services, tickets

**Think mode:** use the list above to find gaps, not as a script. Ask only about gaps, contradictions and risks in the user's text, checked against the docs and code. Ask questions; do not suggest answers. If the user is stuck, give a hint (a scenario to consider, a file to look at), not the answer.

When an answer is vague, push back with a concrete scenario ("what happens if two users do X at once?"). When you disagree with a decision, say so once with the reason, then accept the user's call.

Stop when every point above is either answered or consciously deferred.

## 4. Summary

Write the summary to `.mw/grill/<slug>.md` and show it to the user for confirmation. In think mode, the summary records the user's decisions, not yours.

```markdown
# <Feature name>

**Mode:** think | ship

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

`.mw/metrics.jsonl` stays local and is never committed. Before appending, run `git check-ignore -q .mw/metrics.jsonl`. If it exits non-zero, the file is not ignored: tell the user and ask to add `.mw/metrics.jsonl` to `.gitignore`. If they agree, add it, and run `git rm --cached .mw/metrics.jsonl` if the file is already tracked. If they decline, append anyway and ask again next run.

Append one line to `.mw/metrics.jsonl`:
`{"step":"grill","slug":"<slug>","date":"<ISO date>","questions":<number asked>,"rounds":<rounds>}`
