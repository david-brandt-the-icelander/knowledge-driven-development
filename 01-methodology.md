# 01 — The methodology

> Part of [knowledge-driven development](README.md), which defines the marks.

## The claim

Every software team keeps two descriptions of its system. One is the system itself, plus whatever
documentation survived contact with it. The other is the backlog: tickets, epics, stories, a board with
columns. The first describes what is. The second describes what should be, minus what is, as of
whenever someone last updated it.

The two are drawn by different people, for different readers, in different tools, and updated by
different acts. Changing the code is one act and closing the ticket is another, and only one of them
is enforced by anything. So they drift, however disciplined the people updating them are. And a
description kept by the people it's used to judge will flatter them.

Knowledge-driven development removes the second description. What's left is one description, and it's
the spec: everything the system should be in its end state, as far as anyone currently knows it. It's
written in the present tense, as if all of it were already true, and every statement carries a mark
saying whether it is yet.

- A ⚠️ paragraph marks the gap between what the system should do by design and what has been built so
  far, written in the voice of the person who will use it, in the place they'll look for it.
- A ❌ paragraph is a bug report, in the place the bug will be found.
- A ✅ paragraph is a claim someone has verified, and the only kind you may build on.

The description is split by audience, not by tool. The product owner gets the feature documents. The
operator gets the operator's manual. The integrator gets the API contract. The maintainer gets all of
those, plus the architecture and anything else that really should be documented about a system. A
class tree isn't one of them; any IDE will draw one. → [04](04-document-taxonomy.md)

The specification, the backlog, the release notes and the onboarding material are the same set of
facts, filtered for different readers. Knowledge-driven development keeps them in one place.

The name is deliberate. "Documentation" suggests the thing written after the code, read by nobody, and
out of date before the pull request merges. These documents are written before the code, read on every
change, and they're the only description of the system allowed to exist. They're what the system knows
about itself, including what it knows it can't do yet.

## Why it suits agents

**A backlog is a queue for hands you don't have.** It exists to manage work that isn't being done
because nobody is free to do it. An agent can do that work as fast as someone can guide it and pay for
its tokens. With a serious development team of agents, the bottleneck the backlog was built to manage
is gone, and so is the reason for the planning rituals around it. What's needed instead is a
description of what the system is meant to be, and an account of how much of it is true yet.

**Files beat tickets.** An agent's context is loaded from files. The implementing agents work from the
documents the planner wrote, and the repository's agent instructions tell every agent to read the
documents about whatever it's changing. A ⚠️ in the document about the capability being changed gets
read whether anyone remembers it or not. A ticket gets read when somebody remembers it exists, finds it
and points the agent at it, and then the agent gets a description written for a planning meeting,
about code that may have changed since.

**A human-written ticket rarely carries the intent.** It's usually a subject line, or one paragraph if
you're lucky. A ⚠️ sentence is the specification itself, detailed enough for an agent to build from.

**Most of the backlog is of no interest to most readers.** A ticket tracker is a road atlas of the
whole country, kept in another building, where every reader cares about a different page. A ⚠️ in the
document the agent is reading is a sign on the corner where it's standing.

The backlog becomes a grep:

```bash
grep -rn '❌' documentation/    # every known defect
grep -rn '⚠️' documentation/    # everything still to build
```

It can't drift from the documentation, because it is the documentation.

## Where it breaks

A method that only lists its benefits is marketing. These are the known weaknesses. The list is short
partly because the method is young.

- **Marks pile up when the budget runs out.** A ⚠️ or ❌ is meant to go away fast. When there aren't
  the tokens to work through them, they accumulate like any other backlog.
- **A wrong mark is worse than no mark.** A ✅ next to something nobody ran is a lie that the next
  reader builds on. A blank map tells you to be careful; a wrong one tells you to walk off the cliff
  with confidence. The defenses are that ✅ is expensive to write, and that an agent checks the code
  behind every mark that moved as part of review. → [03 § Review](03-workflow.md#review)
- **Marks go stale.** They don't rot on their own, but the code can move and the page not follow. With
  several agents on parallel branches, the author of a change often doesn't know which pages describe
  what they touched, and a stale ⚠️ against a capability that now exists sends the next agent off to
  build it twice. There's no mechanical fix worth having. An agent reviews the pages, on request or on
  a schedule, and nobody trusts an old page until an agent has checked it against the code.
  → [03 § Keeping the documents true](03-workflow.md#keeping-the-documents-true)
- **One kind of document isn't enough.** "Upgrade to the next framework version", "the build is slow"
  and "the test runner leaks file handles" belong to no capability. They get documents of their own
  kind: the architecture, the build, operations. Same marks, same greps. → [04](04-document-taxonomy.md)
- **It doesn't schedule anything.** A document says what's missing, never when it'll be done or who
  owns it. Scheduling, when the budget forces it, happens in a tracker whose task points at the
  document. → [03 § Scheduling](03-workflow.md#scheduling)
- **Few of its rules have gates.** The rule against `TODO` has a build failure. Most others don't, and
  where a rule has no gate, an agent checks it instead, tokens allowing. → [06](06-enforcement.md)

Every pull request that ships a capability carries a documentation hunk. That isn't a cost: it's the
only part of the diff written for the reviewer.

## What it replaces

Each of these was a second description of the system, kept somewhere other than the system and updated
by some act other than changing the code.

| Replaced | By |
| --- | --- |
| **The ticket tracker**, as the place intent lives | ⚠️ and ❌ sentences in the document that owns the capability. A tracker can still exist for the people who schedule work, but nothing in the repository reads it back, and its tasks point at the documents instead of repeating them |
| **`TODO`, `FIXME`, `HACK`, `XXX`** | A ⚠️ or ❌ in the document that owns the code, saying what the comment never did. `TODO` is `TODO` with no type system and no reader in mind |
| **Plans and work orders** | The plan is the documentation of the system as it will be, marked ⚠️. A work order is true for one afternoon; the sentence stays true for as long as the system works that way. → [03](03-workflow.md) |
| **README-driven development** | The same idea applied to every audience at once, with marks, so the documents survive past version 0.1. A README covers the front door and says nothing about failure, operations or what's half built |
| **Spec-driven development** | The spec is the manual, so it keeps being read. A separate spec is read once by the agent that implements it, and six months later says what the feature was meant to be in March |
| **Hand-written release notes** | The marks that moved. Nobody writes them up, and nobody checks the write-up matches the code |
| **Onboarding material** | Reading the documents for the part of the system you're about to touch. They say what doesn't work, so nobody has to learn which parts of the wiki to trust |

## What stays

- **Architecture decision records and the changelog.** They record the past, and the past doesn't
  drift. They never carry marks: a ✅ in a document about the past is a claim about the present in the
  wrong place.
- **Executable specifications** (doctests, Gherkin scenarios). They can't drift, because they're tests,
  and they win wherever they apply. Where a statement can be a test, make it a test and drop the mark.
  Knowledge-driven development is for the rest: things that aren't executable yet, things that are
  broken, things about people rather than code.

Literate programming was never in the running. It needs its own toolchain, and nobody maintains the
prose half.
