# 04 — Document taxonomy

> Part of [knowledge-driven development](README.md), which defines the marks.

**A fact belongs in exactly one document.** Where two could hold it, the more specific one holds it and
the other links. Duplication is how a corpus starts contradicting itself, and a contradiction between
two marked documents is worse than an unmarked one, because both look authoritative.

**Every document is written from one named perspective**, and the perspective decides what counts as a
fact. The operator doesn't care that a poll is debounced; the maintainer doesn't care which button is
on the left. A document that serves two perspectives serves neither.

## The usual kinds

| Kind | Perspective | Answers | Marks per |
| --- | --- | --- | --- |
| **Feature** | Whoever uses the capability: operator, customer, back office | "What can this system do about refunds, and what can it not?" | Capability |
| **User journey** | One actor, start to finish | "What happens, step by step, when a customer buys something?" | Step |
| **Subsystem or integration** | The engineer on call, and the integrator | "What is this component, what does it talk to, and how does it fail?" | Behavior or failure mode |
| **Contract** | A consumer outside the repository | "What may I call, what comes back, and what mustn't I rely on?" | Operation and field |
| **Architecture** | The maintainer | "How is the system put together, and on what?" | Property the system should have |
| **Operations** | Whoever runs the system | "How do I deploy, restart, diagnose and recover it?" | Procedure |
| **Build and tooling** | Whoever changes the code | "How is it built, tested and released, and how fast?" | Property of the build |

Templates for the first three are in [templates/](templates/).

These aren't a closed list. Requirements, data, security, compliance: any area that needs covering can
get a kind of its own.

## Choosing between them

The test is **who is inconvenienced when it's wrong.**

- An operator presses a button and nothing happens → *feature* or *journey*.
- An integrator's client breaks after a deploy → *contract*.
- An on-call engineer needs to know why the driver retries three times → *subsystem*.
- An agent needs to know which framework version the system is meant to run on → *architecture*.

Two rules that settle most of the remaining arguments:

- **A capability is described once, in the feature document, and referenced from the journeys that
  traverse it.** A journey that re-describes the capability drifts from the feature document within two
  changes.
- **A journey never carries a mark that no step owns.** "Refunds are unreliable" belongs to the feature;
  "step 6, the refund confirmation message, ⚠️ is sent within a minute" belongs to the journey.

## Cross-cutting work

Work that isn't about a capability, such as "upgrade to the next framework version", "the build is
slow" or "the test runner leaks file handles", has no natural place in the capability documents. That's
the reason for having more than one kind.

The framework upgrade becomes a page in the architecture documents, describing the system on the new
version, marked ⚠️ until it's true. The slow build becomes a page describing the build as it should be.
Same marks, same greps, same rules.

## Adding a kind

Adding a kind is cheap. An agent reverse-engineers the current state of that area into a knowledge
document, and a human reviews it. What has to be decided is which kinds are worth having in a given
system.

## Anti-patterns

- **A document per directory.** Directory structure is a build concern, and a reader looking for "how
  do refunds work" won't guess a folder.
- **A hand-written status page listing every mark in the corpus.** That's a second inventory, it will
  drift, and drift in an index discredits the documents it points at. If an overview is wanted,
  generate it from the marks.
- **Marks in the changelog, decision records or change logs.** They're records of the past.
  → [02 § What doesn't get a mark](02-annotation-convention.md#what-doesnt-get-a-mark)
- **A class tree.** Any IDE draws one. The maintainer needs what the IDE can't say: what each part is
  for, whether it works, and what checks that it does.
