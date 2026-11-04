---
name: mw-docs
description: Update project documentation that a change has made outdated. Use after a feature is reviewed, whenever the user says "update the docs", "sync docs", or when a change touches architecture, conventions, modules, APIs, env vars, commands or anything listed in the CLAUDE.md docs map. Part of the Minimalistic Workflow; the workflow depends on docs staying true.
model: sonnet
disable-model-invocation: true
---

# mw-docs

The whole workflow stands on docs. A doc that lies is worse than no doc, because every later step trusts it.

## Steps

1. Read the diff, the ticket and, if present, the "Docs to update" section of the grill summary.
2. Read `CLAUDE.md` and every doc in its map that the change could affect.
3. For each doc, decide: still true, needs an update, or a new doc is needed. Typical triggers: new module or layer, new pattern or deviation from one, new entity, API change, new env var or command, changed convention.
4. List the proposed changes (doc, section, what changes) and ask for confirmation.
5. Edit with minimal, precise changes in the doc's existing style. Do not rewrite sections that are still correct.
6. If you added a new doc, add it to the docs map in `CLAUDE.md`.

If nothing needs updating, say so in one line and stop.

## Metrics

`{"step":"docs","ticket":"NNN","date":"<ISO date>","docs_changed":<count>}`
