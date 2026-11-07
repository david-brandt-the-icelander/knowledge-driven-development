# 06 — Enforcement

> Part of [knowledge-driven development](README.md), which defines the marks.

A convention maintained by attention decays at the rate attention decays. A few rules earn a build
failure. For the rest, there's an agent.

## The deferred-work gate

**Fails the build on a `TODO`, `FIXME`, `HACK` or `XXX` marker opening a comment in any tracked source
file, and on any tracked path under a `TODO/` directory.**

This is the one gate that matters most, and the one rule that was written down and still needed a build
failure. The others protect the quality of a mark; this one protects the fact that a mark is where the
work is recorded at all.

- A marker is the comment opener, optional whitespace, then the word. Prose that discusses the four
  words isn't a marker, and a gate that fires on any appearance of four common English words trains
  people to suppress it.
- Scan comment text, not string literals. A string a user reads isn't a comment.
- The changelog and other append-only history are exempt. They must not be rewritten.
- Put the gate where it can't be skipped: in a test project that runs on every change, whichever part
  of the code the change touched.

## Optional gates

These are cheap and mechanical. None of them is serious if it's missing.

| Gate | Fails when |
| --- | --- |
| **Graduation check** | A document under the convention contains ✅ but no ⚠️ and no ❌. → [02 § Graduation](02-annotation-convention.md#graduation) |
| **Orphan-⛔ check** | A ⛔ isn't immediately preceded by the ⚠️ or ❌ it qualifies. |
| **Legend check** | A document defines a mark instead of linking to the convention: a `Legend:` line, or a table row whose first cell is a bare glyph. → [02 § No page carries the legend](02-annotation-convention.md#no-page-carries-the-legend) |
| **Markdown `TODO` check** | A document under the convention contains a `TODO` marker. |

Each needs to exempt examples, templates and quotations, where a mark is a mention rather than a claim.
That exemption is the fiddly part, and the most likely source of false failures.

## Or an agent

When the convention was first specified, it had seven gates: a coverage cross-check, a test-location
gate, an API contract scan, the vocabulary gate, the deferred-work gate, an orphan-⛔ check and the
graduation check. One of them existed. The rest were described in the present tense and marked ⚠️,
which is to say the enforcement spec was written using the method it enforces.

The missing ones aren't serious. Where a rule has no gate, ask an agent to check the documents
against it, or to do the work the rule was protecting, and it will, tokens allowing.

## What isn't gated

- **Whether a ✅ is true.** No build can check a ✅ on a paragraph about how an operator handles a
  jammed door. An agent can, and does in every review. → [03 § Review](03-workflow.md#review)
- **Whether a ⚠️ is a usable specification.** A judgment, and a review comment.
- **Whether a mark moved in the same commit as the code.** A heuristic would have a false-positive rate
  that teaches people to bypass it. It's the first question a review asks instead.
- **Placement granularity.** A section-level mark hiding nine working paragraphs is a real defect, and
  a machine can't see it. A reviewing agent can.
- **Whether a mark has gone stale.** Line citations survive the edit that invalidates them, so a
  citation check is green on exactly the cases it was meant to catch. An agent reviews the documents
  instead. → [03 § Keeping the documents true](03-workflow.md#keeping-the-documents-true)
