---
name: mw-review
description: Review a diff (current changes by default, or a given branch, commit or range) against its ticket, plan, architecture and conventions, scaling the review to the size and risk of the change by spawning reviewer subagents. Use whenever the user asks for a code review, "review this", "check the diff", "review branch X", "review commit abc123", or after mw-build or mw-debug finishes. Best run in a fresh session. Part of the Minimalistic Workflow.
model: opus
---

# mw-review

Review like a senior colleague who did not write the code. Output only what to change and why.

## Core principle (think mode)

The user produces the first version of every decision (scope, split, plan, hypothesis, review findings). You critique it, ask questions and execute. Never present your own full solution first. Reveal your own version only after the user's version exists, or when the user explicitly asks for it.

## 1. Resolve the target

Use the target from `$ARGUMENTS` or the user's message:

| Input        | Diff                                                      |
| ------------ | --------------------------------------------------------- |
| nothing      | uncommitted changes: `git diff HEAD` (staged + unstaged)  |
| branch name  | `git diff <base>...<branch>` (base = main/master, or ask) |
| commit hash  | `git show <hash>`                                         |
| range `a..b` | `git diff a..b`                                           |

If the diff is empty, say so in one line and stop.

## 2. Gather context

- The diff and `git diff --stat` for the same target.
- The ticket and plan in `tickets/` and `.mw/plans/` matching the change (or the `mw-debug` report), if they exist.
- `CLAUDE.md`, architecture and conventions docs.
- Surrounding code of every changed file, not only the changed lines.

Use the mode recorded in the plan, ticket or debug report. If none is recorded, ask once: "think or ship?"

## 3. The user reviews first (think mode)

Before running any reviewer, ask the user for their own findings on the diff, one per line: `file:line — what — why`. "None" is a valid answer. Wait for it. Do not hint at problems before you have it.

Do not pass the user's findings to the reviewer subagents; they review independently.

## 4. Pick the review size

Count changed lines (added + removed, excluding lockfiles, generated files and snapshots) and files.

**Sensitive** if the diff touches any of: auth/permissions, payments, migrations or schema, public API contracts, security config, infra/deploy, queues/jobs with retries, cryptography, PII handling.

| Size | When                                  | Reviewers                                                                                                             |
| ---- | ------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| S    | ≤ 100 lines, ≤ 3 files, not sensitive | review yourself, no subagents                                                                                         |
| M    | ≤ 400 lines, not sensitive            | 2 subagents: **correctness** (logic, edge cases, tests), **design** (architecture, conventions, plan fidelity)        |
| L    | > 400 lines, or any sensitive change  | 4 subagents: **correctness**, **security**, **design**, **data & tests** (migrations, queries, indexes, test quality) |

State the chosen size in one line before starting.

## 5. Run the reviewers

Give every subagent: the diff, the ticket/plan, the relevant docs, its single focus area, and this instruction:

> Report only real problems in your focus area. For each: `file:line`, what to change, why (one sentence). No praise, no summaries, no style nits a linter would catch. If you find nothing, return "none".

Run subagents in parallel. For size S, apply the same rules yourself across all areas.

## 6. Merge and verify

- Deduplicate overlapping findings.
- Check every finding against the code yourself. Drop anything you cannot confirm.
- Run tests, typecheck and lint. Do not trust earlier reports.
- Classify: **blocker** = bug, security issue, data risk, broken acceptance criterion, plan violation. Everything else is a **suggestion**.
- **Think mode:** check every user finding against the code the same way. Match the user's findings to the verified ones: a verified finding the user reported is **found**, one they did not report is **missed**, and a user finding that is not a real problem is a **false alarm**.

## 7. Output

Nothing beyond this format. No intro, no recap of the change, no closing remarks.

Ship mode:

```
Size: S|M|L · Checks: tests ✓/✗ · typecheck ✓/✗ · lint ✓/✗

Blockers
- path/file.ts:42 — <what to change> — <why>

Suggestions
- path/file.ts:88 — <what to change> — <why>
```

Think mode (list blockers first within each section):

```
Size: S|M|L · Checks: tests ✓/✗ · typecheck ✓/✗ · lint ✓/✗

You found
- [blocker] path/file.ts:42 — <what to change> — <why>

You missed
- [suggestion] path/file.ts:88 — <what to change> — <why>

False alarms
- path/file.ts:10 — <the user's finding> — <why it is not a problem>
```

Omit an empty section. If there are no findings at all, output the first line and `No issues found.`

## Metrics

`{"step":"review","ticket":"NNN","date":"<ISO date>","size":"S|M|L","blockers":<count>,"suggestions":<count>,"user_found":<count>,"user_missed":<count>}`

In ship mode, omit `user_found` and `user_missed`.
