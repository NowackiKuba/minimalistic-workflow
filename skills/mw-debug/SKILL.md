---
name: mw-debug
description: Fix a bug by reproducing it with a failing test first, then finding the root cause and fixing it. Use whenever the user reports a bug, error, crash, regression, failing behaviour or Sentry issue, or says "this is broken", "we have a bug", "fix this", "it keeps failing". Bug flow of the Minimalistic Workflow; replaces grill/split/plan/build for bugs.
model: opus
---

# mw-debug

A bug is fixed when a test that used to fail now passes and nothing else broke. A fix without a reproduction is a guess.

## Core principle (think mode)

The user produces the first version of every decision (scope, split, plan, hypothesis, review findings). You critique it, ask questions and execute. Never present your own full solution first. Reveal your own version only after the user's version exists, or when the user explicitly asks for it.

## Mode

If the user did not say, ask once: "think or ship?"

- **think** (default for non-trivial bugs): the user forms the hypothesis and proposes the fix; you guide, critique and implement.
- **ship** (routine bugs): you investigate and propose; the user approves.

Record the mode in the report.

## Steps

1. **Understand.** Collect what is known: expected vs actual behaviour, error and stack trace, input, environment, when it started. Read `CLAUDE.md` and the docs relevant to the affected area. Ask the user only for what you cannot find yourself.
   - **Think mode:** before investigating, ask for the user's hypothesis. Wait for it.
2. **Reproduce.** Write a test that fails because of the bug. Run it and confirm it fails for the right reason (the same error or wrong result, not a setup problem). If you cannot reproduce it, stop and report what you tried. Do not fix blind.
3. **Root cause.** Form a hypothesis, check it against the code and the failing test. Find _why_ it happens, not just _where_. Check whether the same cause exists elsewhere.
   - **Think mode:** check the user's hypothesis instead of forming your own. If it is wrong or incomplete, give hints in steps, one at a time: where to look, then what to check, then the likely cause. Give the full root cause only when the user asks for it or has found it.
4. **Propose.** Before editing, tell the user in a few lines: root cause, proposed fix, files touched. For a small, obvious fix you may proceed; for anything touching shared code, data or behaviour beyond the bug, wait for approval.
   - **Think mode:** the user proposes the fix. Critique it (does it remove the cause, side effects, same cause elsewhere, missing cases), then implement it once agreed.
5. **Fix.** Minimal change that removes the cause. Follow architecture and conventions.
6. **Verify.** Run the reproduction test (now green), then the full test suite, typecheck and lint. Same loop rules as `mw-build`: fix causes, never weaken tests, max 3 attempts, then stop and report.

## Report

```markdown
**Mode:** think | ship

## Bug

## Root cause

## Fix

## Test

<name of the reproduction test>

## Same cause elsewhere?
```

Next step: `mw-review` in a fresh session.

## Metrics

`{"step":"debug","date":"<ISO date>","reproduced":true|false,"attempts":<verification runs>,"result":"green|stopped"}`
