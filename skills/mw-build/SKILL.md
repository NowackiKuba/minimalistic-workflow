---
name: mw-build
description: Implement an approved plan from .mw/plans/, following the project's architecture and conventions, and loop on tests, typecheck and lint until they pass. Use whenever the user approves a plan, says "go", "build it", "implement it", "approved, do it", or asks to implement a ticket that has an approved plan. Runs after mw-plan in the Minimalistic Workflow.
model: opus
---

# mw-build

The approved plan is the source of truth. Architecture and conventions are the standard the code must meet.

## Core principle (think mode)

The user produces the first version of every decision (scope, split, plan, hypothesis, review findings). You critique it, ask questions and execute. Never present your own full solution first. Reveal your own version only after the user's version exists, or when the user explicitly asks for it.

## Before coding

1. Read the approved plan in `.mw/plans/NNN-*.md`. If there is no approved plan, stop and suggest `mw-plan`. Use the mode recorded in the plan; if none is recorded, ask once: "think or ship?"
2. Read the ticket, `CLAUDE.md`, and the architecture and conventions docs.
3. Find the test, typecheck and lint commands in `CLAUDE.md`. If they are missing, read `package.json` (or equivalent) and ask the user to confirm them.

## Implement

- Follow the plan's file list. Touch nothing outside it unless required for the plan to work.
- Write the tests listed in the plan along with the code.
- Match existing patterns in the codebase over your own preferences.
- Skip every part of the plan marked `[human]`. Leave a `TODO [human]: <what, plan section>` comment where that code belongs.
- **Think mode:** implement only what the plan specifies. If the plan leaves a design decision open, stop and ask the user to decide. Never fill the gap yourself.

## Verification loop

After implementing, run tests, typecheck and lint.

- All green → done.
- Red → read the actual error, fix the cause (never skip, delete or weaken a test, and never add ignore comments to make it pass), run again.
- Maximum 3 fix attempts. After the third failure, stop and report: what fails, what you tried, your best hypothesis.
- If a check is red only because a `[human]` part is not written yet, do not work around it. Report which failures belong to it.

## When the plan is wrong

If the plan cannot work as written, or you need a change it does not cover, **stop and ask**. Do not silently deviate: an unannounced deviation is exactly what review would otherwise have to catch.

- **Ship mode:** describe the problem and propose an amendment to the plan.
- **Think mode:** describe the problem and ask the user how to amend the plan. Do not propose the amendment unless they ask.

## Comprehension check (think mode)

After the checks are green, ask the user 2–3 questions that make them trace what the code in this diff does: given this input or state, what happens at `file:line`? Each question must be answerable by reading the diff. Do not give the answers. If an answer is wrong or vague, point to the exact `file:line` and let them try again.

Never ask:
- **Why a decision was made** when the plan or ticket made it. The user made that decision; quizzing them on it tests nothing.
- **About a bug you suspect.** If you think something can break, report it as a finding with `file:line`. Do not hide it in a quiz.
- **About code outside the diff**, or about hypotheticals the code does not handle.

## Report

When green, report: files changed, tests added, any approved deviation, a summary of the verification output, and every `TODO [human]` left for the user (file, plan section, what to write).

Never stage or commit. Leave every change uncommitted in the working tree: `mw-review` reviews uncommitted changes (`git diff HEAD`) by default, and committing is `mw-commit`'s job after review.

Next step: `mw-review` in a fresh session.

## Metrics

`{"step":"build","ticket":"NNN","date":"<ISO date>","attempts":<verification runs>,"deviations":<count>,"result":"green|stopped"}`
