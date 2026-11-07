# 03 — Workflow

> Part of [knowledge-driven development](README.md), which defines the marks.

How a feature goes through knowledge-driven development, from one prompt to a merged pull request. This
is the workflow the method was built for: one person, a planning agent and a fleet of implementing
agents.

## Planning

The whole prompt to the planning agent:

> Plan how to implement this feature, and write it up using knowledge-driven development.

It goes to a top-tier model at high effort, with whatever description of the feature exists. Usually a
paragraph, sometimes a sentence.

The planner doesn't produce a list of steps. It edits the documents that already describe the system,
or creates the ones that should exist, and describes the feature in them, in the present tense, as the
system will be once the feature exists:

- The **feature document** gets a section describing the new capability from the point of view of
  whoever will use it, statement by statement, each one ⚠️.
- The **user journeys** that pass through the feature get new steps, each one ⚠️.
- The **subsystem documents** get the new ways things can fail: what goes wrong, what it looks like from
  the outside, and what an operator does about it. All ⚠️.
- The **API contract** gets the new operations, with their request and response shapes, each one ⚠️.

If something can't be built from where things are (it needs a decision from someone outside, hardware
that isn't available, a third party that hasn't shipped), the planner writes it as ⚠️⛔ and says what
would unblock it. That's how blockers reach the person running the work: as a sentence in the document
that owns them.

The quality bar for those ⚠️ sentences is that an agent can build from them without asking much.
→ [02](02-annotation-convention.md#what-each-mark-obliges-you-to-write)

That set of marked documents is the plan. It doesn't expire, because it's the description of the
system, and implementing it means making its sentences true. When the last ⚠️ in a feature turns into
✅, nothing gets archived.

## Implementing

Every agent that comes after the planner treats the documents as its primary guidance, ahead of the
original prompt, the chat history or any ticket.

That works because of how the documents reach the agents. The implementing agents are handed the
documents the planner wrote, and the repository's agent instructions tell every agent to read the
documents about whatever it's changing. Nobody has to remember to point an agent at them.
→ [templates/agent-instructions.md](templates/agent-instructions.md)

An implementing agent starts from nothing and orients itself from the documents, the way a rover
orients itself from its surroundings. It doesn't need the chat history, the plan so far or the original
paragraph. It reads the documents, finds the ⚠️ marks, and starts building. The ordering, and what can
run in parallel, it works out from the documents too.

The implementing agents can be cheaper than the planner. The planner did the judgment: what the feature
is, where it belongs, how it fails. The implementers translate well-specified sentences into code and
tests, which a smaller model at lower effort does well.

**The one rule they must follow:** the mark moves in the same commit as the code. Shipping a capability
turns its ⚠️ into ✅ in the same pull request.

## Defects

Defects turn up in user testing, a chat message, an email, a tracker task, a phone call, or the
system's own observability ("what went wrong since yesterday?"). Wherever one comes from, the usual
answer is that an agent diagnoses it and fixes it, and the defect is gone before anyone thinks of
writing it down.

When a fix has to wait, an agent updates the knowledge documents instead. That's what a ❌ is for: a
known defect that's still there, in the place the defect lives.

## The backlog and the sprint board

There's no separate backlog. The backlog is every ⚠️ in the repository. The sprint is the ⚠️ marks in
the documents the planner just wrote. Progress is the count of those marks going down. Something
blocked is ⚠️⛔ and says what it's waiting on. A defect waiting for a fix is a ❌.

```bash
grep -rn '⚠️' documentation/Features/my-new-feature.md
```

It updates itself in the same commits as the code.

## Review

A feature pull request built by agents can easily be two thousand lines of code and tests. Nobody can
review that with real attention. So review starts with two short things.

**The delta to the knowledge documents.** Usually twenty or forty lines. It says, in plain sentences,
what the agent claims to have done: these ⚠️ became ✅, this ❌ is a defect left for later, this one is
now ⚠️⛔ and says what it's waiting on. Every mark that moved is a claim, and every claim names the
behavior it's about.

**The change logs.** Every prompt that changes the code base gets a change log file. It records what
was asked for, how the agent interpreted it, and, where it matters, what the change needs: how to test
it, data to migrate, compatibility issues. These are what show whether the agent misunderstood the
request.

Together, those two are the most concise way to see what a pull request thinks it's changing. Sometimes
that's enough on its own to show the agent got it wrong: a data migration written for something a
feature flag would have done.

**Then the claims get checked**, because a ✅ means someone read the code and saw the tests pass, and
agents report success the way a teenager reports that their room is clean. The checking goes to another
agent, one that didn't write the change. It reads the code behind each mark that moved and runs the
tests.

A pull request that changes behavior and moves no mark is the first question the review asks.

Review then means reading a few dozen lines of claims and intent, and handing the checking to an agent,
instead of reading two thousand lines of code and guessing what they were for.

## Scheduling

A document says what's missing. It never says when it'll be done or who owns it, and it isn't supposed
to.

Ideally nothing is scheduled: every ⚠️ gets agent tokens, and the gaps close. In practice the token
budget is finite. When something has to be scheduled, it gets a task in whatever tracker the
organization uses, and the task points at the knowledge document. The task says when. The document says
what, and whether it's true yet.

Only major work items belong in the tracker. Microtasks don't: what they say goes into the knowledge
document, if it's worth keeping at all. Usually it isn't, because an agent can work out the small steps
for itself while implementing. Once a microtask is done, the code is the record of it.

The tracker can still be kept up to date for the project managers and product owners who use it, by
agents, from the documents. Nothing in the repository reads it back. It's a second map, and agents only
ever write to it.

## Keeping the documents true

Marks don't rot on their own, but they go stale when the code moves and the page doesn't, most often
when several agents work on parallel branches and the author of a change doesn't know which pages
describe what they touched.

- **In review**, an agent checks the code behind every mark that moved. → [§ Review](#review)
- **Periodically**, an agent reviews a set of documents against the code, on request or on a schedule.
- **Before trusting an old document**, for example to learn a partially built feature in detail, have
  an agent give it a once-over against the code first. Then read it.

There's no mechanical fix for staleness worth having. Line citations survive the edit that invalidates
them, and a check keyed on what a ⚠️ names fires on every well-written gap, because a good ⚠️ names
things that don't exist yet. Asking an agent works better.
