---
name: mw-wid
description: Write a short, human-readable "what I did" note about the finished task into the user's Obsidian vault, in a folder per project. Use whenever the user says "mw-wid", "what I did", "write it up in Obsidian", "log this task", or after mw-pr when the task is done. Last step of the Minimalistic Workflow.
model: sonnet
disable-model-invocation: true
allowed-tools: Bash(git *), Bash(gh pr view *), Read, Write, Edit
---

# mw-wid

A note your future self can read in a minute: what the task was, what changed, and what was learned along the way.

## Steps

1. Find the vault. Read `~/.claude/mw-wid.json` (`{"vault": "<absolute path>"}`). If the file is missing, or the path does not exist or has no `.obsidian/` folder, ask the user for the absolute path to their Obsidian vault. Check that the path exists and contains `.obsidian/`, then save it to `~/.claude/mw-wid.json`.
2. Gather what happened, reading only what exists:
   - the grill summary in `.mw/grill/`, the ticket in `tickets/`, the plan in `.mw/plans/`, or the `mw-debug` report
   - the review result, if it is in the conversation
   - `git log <base>..HEAD` and the diff stat
   - the PR, via `gh pr view --json url,title,state` (skip this if there is no PR)
   - lines in `.mw/metrics.jsonl` that match this ticket or slug
3. Project folder: `<vault>/<repo-name>/`, where `<repo-name>` is the basename of `git rev-parse --show-toplevel`. Create it if it does not exist.
4. Note path: `<vault>/<repo-name>/<YYYY-MM-DD>-<NNN>-<slug>.md` (for a bug with no ticket, use `<YYYY-MM-DD>-<slug>.md`). If the note already exists, ask whether to overwrite it or update it.
5. Write the note in English using the format below. Keep it short: plain sentences, no code dumps, no line-by-line diff. Do not invent anything the sources don't show; if a section has nothing, write "None".
6. Show the path and the note.

## Note format

```markdown
---
date: <YYYY-MM-DD>
project: <repo-name>
ticket: "<NNN or none>"
type: <feature | bug>
pr: <url or none>
tags: [mw, <repo-name>, <feature | bug>]
---

# <Ticket title, in plain words>

## What & why
<2-3 sentences: the task and the problem it solved.>

## What changed
- <key change, in plain words>
- Commits: <count>, PR: [<title>](<url>)

## Decisions & gotchas
- <a choice made in grill/plan/debug, and why>
- <a surprise during build, debug or review>

## Metrics
<Numbers from metrics.jsonl, e.g. grill questions and rounds, commit count. "None" if there are none.>
```
