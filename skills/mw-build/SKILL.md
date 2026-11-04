---
name: mw-build
description: Implement an approved plan from .mw/plans/, following the project's architecture and conventions, and loop on tests, typecheck and lint until they pass. Use whenever the user approves a plan, says "go", "build it", "implement it", "approved, do it", or asks to implement a ticket that has an approved plan. Runs after mw-plan in the Minimalistic Workflow.
model: opus
disable-model-invocation: true
---

# mw-build

The approved plan is the source of truth. Architecture and conventions are the standard the code must meet.

## Before coding

1. Read the approved plan in `.mw/plans/NNN-*.md`. If there is no approved plan, stop and suggest `mw-plan`.
2. Read the ticket, `CLAUDE.md`, and the architecture and conventions docs.
3. Find the test, typecheck and lint commands in `CLAUDE.md`. If they are missing, read `package.json` (or equivalent) and ask the user to confirm them.

## Implement

- Follow the plan's file list. Touch nothing outside it unless required for the plan to work.
- Write the tests listed in the plan along with the code.
- Match existing patterns in the codebase over your own preferences.

## Verification loop

After implementing, run tests, typecheck and lint.

- All green → done.
- Red → read the actual error, fix the cause (never skip, delete or weaken a test, and never add ignore comments to make it pass), run again.
- Maximum 3 fix attempts. After the third failure, stop and report: what fails, what you tried, your best hypothesis.

## When the plan is wrong

If the plan cannot work as written, or you need a change it does not cover, **stop and ask**. Describe the problem and propose an amendment to the plan. Do not silently deviate: an unannounced deviation is exactly what review would otherwise have to catch.

## Report

When green, report: files changed, tests added, any approved deviation, and a summary of the verification output.

Next step: `mw-review` in a fresh session.

## Metrics

`{"step":"build","ticket":"NNN","date":"<ISO date>","attempts":<verification runs>,"deviations":<count>,"result":"green|stopped"}`
