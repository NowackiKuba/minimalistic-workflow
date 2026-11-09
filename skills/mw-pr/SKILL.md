---
name: mw-pr
description: Write a pull request description from the project's PR template, based on the ticket, plan, review and diff. Use whenever the user says "open a PR", "write the PR description", "pull request", or when commits are ready to open a PR. Runs before mw-wid in the Minimalistic Workflow.
model: haiku
disable-model-invocation: true
---

# mw-pr

A PR description lets the reviewer understand the change before reading the code.

## Steps

1. Find the PR template via the `CLAUDE.md` docs map, or at `.github/pull_request_template.md`. Use it exactly; fill every section. If there is no template, use the default below.
2. Read the ticket, the plan, the review result and `git log <base>..HEAD` plus the diff.
3. Fill the template:
   - What and why, in plain language, short
   - How to test, as concrete steps
   - Risks, migrations, config or env changes, breaking changes
   - Anything the reviewer should look at closely
4. Do not describe what the diff already shows line by line. Do not claim tests or checks that were not run.
5. Append this step's metrics line (see Metrics).
6. Show the description. Only open the PR (e.g. `gh pr create`) if the user asks.

Next step: `mw-wid`.

## Default template

```markdown
## What

## Why

Refs: <ticket>

## How to test

1.

## Risks & notes

## Checklist

- [ ] Tests pass
- [ ] Docs updated
```

## Metrics

`.mw/metrics.jsonl` stays local and is never committed. Before appending, run `git check-ignore -q .mw/metrics.jsonl`. If it exits non-zero, the file is not ignored: tell the user and ask to add `.mw/metrics.jsonl` to `.gitignore`. If they agree, add it, and run `git rm --cached .mw/metrics.jsonl` if the file is already tracked. If they decline, append anyway and ask again next run.

Append one line to `.mw/metrics.jsonl`:
`{"step":"pr","ticket":"NNN","date":"<ISO date>"}`
