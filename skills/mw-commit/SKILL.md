---
name: mw-commit
description: Create small, logical conventional commits for the current changes, with messages that explain why. Use whenever the user says "commit", "commit this", "make commits", or when changes are ready to commit after review. Part of the Minimalistic Workflow.
model: haiku
disable-model-invocation: true
allowed-tools: Bash(git *), Read, Edit
---

# mw-commit

Commits tell the story of the change to the next person reading `git log` or `git blame`.

## Steps

1. Read the commit rules in the conventions doc. They override the defaults below.
2. Run `git status` and `git diff` (staged and unstaged). Read the ticket or plan for the "why".
3. Group changes into logical commits: each one builds and makes sense alone (e.g. migration, then domain logic with its tests, then API). Do not mix refactors with behaviour changes. Do not split a change from its own tests. Never commit `.mw/metrics.jsonl`.
4. Show the proposed commits (files + message) and wait for confirmation.
5. Stage per commit (`git add <files>`, `git add -p` for split files) and commit.
6. Append this step's metrics line (see Metrics).

Never commit secrets, `.env` files, build output or unrelated changes; point them out instead. Never push and never amend or rewrite commits that may already be pushed unless the user asks.

## Default format (Conventional Commits)

```
<type>(<scope>): <imperative summary, max ~72 chars>

<why this change, what problem it solves, anything non-obvious>

Refs: <ticket>
```

Types: feat, fix, refactor, test, docs, chore, perf, build, ci.

**Example**

```
fix(invoices): prevent duplicate KSeF submission on retry

The BullMQ job retried after a timeout even when KSeF had already
accepted the invoice. Check submission status before resending.

Refs: 014
```

## Metrics

`.mw/metrics.jsonl` stays local and is never committed. Before appending, run `git check-ignore -q .mw/metrics.jsonl`. If it exits non-zero, the file is not ignored: tell the user and ask to add `.mw/metrics.jsonl` to `.gitignore`. If they agree, add it, and run `git rm --cached .mw/metrics.jsonl` if the file is already tracked. If they decline, append anyway and ask again next run.

Append one line to `.mw/metrics.jsonl`:
`{"step":"commit","ticket":"NNN","date":"<ISO date>","commits":<count>}`
