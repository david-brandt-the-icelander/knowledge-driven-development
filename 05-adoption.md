# 05 — Adoption

> Part of [knowledge-driven development](README.md), which defines the marks.

Bringing an existing repository under the convention. The plan is not "annotate everything". Most of
the value comes from a few small, finishable steps; the rest arrives document by document, carried by a
rule rather than a project.

## 1. Tell the agents

Without this, no agent applies the convention, and every later step is manual. Add the lines in
[templates/agent-instructions.md](templates/agent-instructions.md) to the repository's agent
instructions (`AGENTS.md`, `CLAUDE.md` or whatever the agents read on every turn). They do four things:

- route agents to the documents about whatever they're changing, and make those documents their primary
  guidance;
- make the mark move in the same commit as the code;
- reserve ✅ for someone who has read the code and seen the tests pass;
- ban `TODO`.

Link to this repository rather than copying the convention into the instructions. A second copy drifts.

## 2. Abolish `TODO`

- Move every item in a `TODO/` folder, or any folder of deferred work, into the document that owns the
  capability it's about, as ⚠️ or ❌ sentences. Keep each item's severity, if it had one.
  → [02 § Ownership](02-annotation-convention.md#ownership)
- Move every `TODO`, `FIXME`, `HACK` and `XXX` comment in the source into the document that owns the
  code it sat in, as a ⚠️ or ❌ that says what the comment never did.
- Delete the folder, and add the gate so neither comes back.
  → [06 § The deferred-work gate](06-enforcement.md#the-deferred-work-gate)

This is the whole method in miniature, and it's a day's work for an agent.

## 3. Empty the tracker of detail

Distill whatever the tracker's tasks know into the documents. Tasks too detailed to be anyone's schedule
come out of the tracker. What's left there is the major work items, and each one points at the document
that describes it instead of repeating it. → [03 § Scheduling](03-workflow.md#scheduling)

## 4. Write the documents that are missing

For each area that needs covering, have an agent reverse-engineer its current state into a knowledge
document of the right kind, and have a human review it. → [04](04-document-taxonomy.md)

Start with whatever the next feature touches. The planner will need those documents anyway, and a plan
written into documents that don't exist yet has nowhere to live.

## 5. Convert the rest as it's touched

Existing documents are converted when they're next substantively edited, not before. Most of them
describe the past, are design records, or are fine. A wholesale conversion would be stale before it
finished.

- Never annotate the changelog, decision records or change logs.
- When converting a document that used the glyphs for something else (good and bad examples, yes/no
  table cells, checklist ticks), replace those uses with words. A glyph that isn't a claim breaks the
  greps.
- Convert by meaning, statement by statement. A find-and-replace over glyphs can't tell a mark from a
  mention of one.

## What keeps it from rotting

1. **The mark moves in the same commit**, and review asks about any pull request that changes behavior
   without moving one.
2. **An agent checks the claims** in review, and re-reviews documents on request or on a schedule.
   → [03 § Keeping the documents true](03-workflow.md#keeping-the-documents-true)
3. **Graduation.** Documents that are finished shed their marks, so the marked part of the corpus stays
   small enough to stay true. → [02 § Graduation](02-annotation-convention.md#graduation)
4. **A generated index, never a written one.** If a status overview is wanted, generate it from the
   marks.
