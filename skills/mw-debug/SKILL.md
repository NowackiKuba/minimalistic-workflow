---
name: mw-debug
description: Fix a bug by reproducing it with a failing test first, then finding the root cause and fixing it. Use whenever the user reports a bug, error, crash, regression, failing behaviour or Sentry issue, or says "this is broken", "we have a bug", "fix this", "it keeps failing". Bug flow of the Minimalistic Workflow; replaces grill/split/plan/build for bugs.
model: opus
---

# mw-debug

A bug is fixed when a test that used to fail now passes and nothing else broke. A fix without a reproduction is a guess.

## Steps

1. **Understand.** Collect what is known: expected vs actual behaviour, error and stack trace, input, environment, when it started. Read `CLAUDE.md` and the docs relevant to the affected area. Ask the user only for what you cannot find yourself.
2. **Reproduce.** Write a test that fails because of the bug. Run it and confirm it fails for the right reason (the same error or wrong result, not a setup problem). If you cannot reproduce it, stop and report what you tried. Do not fix blind.
3. **Root cause.** Form a hypothesis, check it against the code and the failing test. Find _why_ it happens, not just _where_. Check whether the same cause exists elsewhere.
4. **Propose.** Before editing, tell the user in a few lines: root cause, proposed fix, files touched. For a small, obvious fix you may proceed; for anything touching shared code, data or behaviour beyond the bug, wait for approval.
5. **Fix.** Minimal change that removes the cause. Follow architecture and conventions.
6. **Verify.** Run the reproduction test (now green), then the full test suite, typecheck and lint. Same loop rules as `mw-build`: fix causes, never weaken tests, max 3 attempts, then stop and report.

## Report

```markdown
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
