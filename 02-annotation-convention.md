# 02 — Annotation convention

> Part of [knowledge-driven development](README.md), which defines the marks. This document is the
> canonical statement of how they're used. Nothing else restates it; everything else links here.

## Where the marks go

**In documentation only.** Feature documents, journeys, subsystem and architecture documents, API
contracts, operator manuals: any Markdown a reader opens to find out what the system does.

- **Not in source code.** No marks in doc comments, inline comments or annotations. The agents find the
  documents because the planner's documents are what they work from, and because the repository's agent
  instructions tell them to read the documents about whatever they change. → [03](03-workflow.md)
- **Never in a string a user reads.** A status icon on an admin page, a severity badge on a log line or
  a tick in a report is product, chosen by whoever designed it. The convention has no standing there.

## No page carries the legend

**A page links to the convention; it never restates it.** One line, at the top:

```markdown
> Status marks: [knowledge-driven development](https://github.com/david-brandt-the-icelander/knowledge-driven-development)
```

The reason is the grep:

```bash
grep -rn '❌' documentation/    # every known defect
grep -rn '⚠️' documentation/    # everything still to build
```

Those only work if every occurrence is a claim about the system. A legend is a mention: four glyphs per
page that aren't claims. A corpus where each page explains the marks is one where both commands return
mostly noise, and readers stop running them. The definition lives in one place, so that everywhere else
the glyph means what it says.

A mark inside an example, a template or a quotation is a mention too. Keep those out of the documents
the greps cover.

## Placement and granularity

**The mark goes on the smallest statement that is wholly true or wholly false.** A section heading
carrying one ⚠️ for eleven paragraphs, nine of which work, is worse than no mark at all, because it
makes the two broken paragraphs unfindable.

- A bullet that describes one capability takes the mark at the start of the bullet.
- A table row that describes one capability takes the mark in its own column, headed `Status`.
- A paragraph takes the mark at the start of the sentence that's untrue, not at the start of the
  paragraph.
- A whole section may take a mark on its heading only when every statement under it shares that state.
  `### Refund reversal ⚠️` is correct when nothing about refund reversal exists.
- Never mark a heading and its contents both. The reader can't tell whether the inner marks refine the
  outer one or contradict it.

## What each mark obliges you to write

**The mark is the whole commentary.** Write the sentence as the system should be, in the present tense,
in the detail someone would need to use it. Mark it. Then stop. Don't explain the mark: no "what it
does instead", no "the customer currently sees", no account of how you found out. The glyph already
says *this sentence isn't true yet*. A paragraph restating that in prose turns a specification back
into a bug report, which is the thing the page replaced.

The instinct runs the other way. A page that just cost somebody a day of debugging wants to say so. It
mustn't. The incident goes in the changelog entry that introduced the mark, and the diagnosis goes in
the pull request that fixes it. What the page owes its reader is the sentence they can build against,
and one glyph telling them whether to trust it.

**⚠️, not implemented.** The sentence describes behavior that doesn't exist yet. A ⚠️ that says "not
implemented" is worthless. A ⚠️ that describes the intended behavior is a specification a cheaper agent
can implement from. Its quality bar is that an agent can build from it without asking much. It doesn't
need to be perfect: the agent is fast, and what it builds can be reviewed and redirected quickly.

**❌, implemented but broken.** The sentence describes behavior that exists and doesn't hold. It states
the correct behavior, not the wrong one.

**⛔, blocked.** Written straight after the ⚠️ or ❌ it qualifies, never on its own: `⚠️⛔`. It says
the gap can't be closed from here: the decision isn't ours, the hardware isn't here, the third party
hasn't shipped, another initiative owns the file. It always says what would unblock it.

⛔ is the only mark that can become false without anybody touching the code. Re-read the blockers when
work in that area is picked up. An unblocked ⛔ is a stale claim, like a stale ✅.

**✅, implemented and verified.** May only be written by someone who has read the code and seen the
tests pass. A plan saying so doesn't count, and neither does an agent reporting success or a pull
request merging. It's the strictest mark in the convention, because ✅ is the one readers build on.
In review, an agent that didn't write the change checks the code behind every ✅ that moved.
→ [03 § Review](03-workflow.md#review)

## Ownership

**The mark moves in the same commit as the code that changes its truth.** Shipping a capability turns
its ⚠️ into ✅ in the same pull request.

A pull request that changes behavior and moves no mark has either changed nothing a reader can see, or
left the documentation lying. Either way, it's the first question the review asks.

**Defects.** Defects rarely come out of the commits. They turn up in user testing, a chat message, an
email, a tracker task, a phone call, or the system's own observability. The usual answer is that an
agent diagnoses and fixes it straight away, and nothing needs writing down. When a fix has to wait, the
defect goes into the document that owns the behavior as a ❌. A ❌ is a known defect that's still there.

**Severity travels with the finding.** When a graded finding (a review result, an incident, an old
ticket with a priority) becomes a marked statement, its grade comes with it, written immediately after
the mark, or the triage is lost even though the content isn't.

## Graduation

**When a document contains no ⚠️ and no ❌, every ✅ in its body is deleted.** The status link at the
top is replaced by one line, carrying the page's only green check:

```markdown
> ✅ This component is fully implemented, tested and free of known defects.
```

A page covered in green ticks has stopped carrying information. The ticks only ever existed to make the
gaps legible, so when the gaps go, the ticks go with them. Graduation is what keeps the marked part of
the corpus small enough to stay true. A convention that only ever adds marks becomes wallpaper.

- **❌ blocks graduation too.** "Free of known defects" over a page with a live defect on it is false.
- **⛔ never blocks on its own**, because it never appears on its own. The ⚠️ or ❌ it qualifies is what
  graduation waits on.
- **Reintroduction is normal and cheap.** The first new ⚠️ or ❌ restores the status link and marks only
  the statements that need it. A graduated document isn't re-ticked wholesale; a ✅ comes back only
  where it neighbors a gap and the contrast is informative.

## There is no `TODO`

**`TODO`, `FIXME`, `HACK` and `XXX` are marks that failed to say anything.** They record that somebody
was unhappy at that line, and nothing else: not whether the code works, whether it's broken, what it
should do instead, or what would fix it.

So the four marks replace them entirely:

- **No `TODO/` directory**, or any other folder of deferred work. It's a second inventory, and it
  drifts from the code the moment it exists. Each item goes into the document that owns the capability
  it's about.
- **No `TODO`, `FIXME`, `HACK` or `XXX` opening a comment** in any source file. The comment's content
  moves into the document that owns the code, as a ⚠️ or ❌ that says what the comment never did.
- **Both are build failures.** → [06 § The deferred-work gate](06-enforcement.md#the-deferred-work-gate)

Before:

```csharp
// TODO: handle timeouts
var result = await accounting.SendAsync(batch);
```

After, in the export's document:

> ❌ When the accounting system doesn't answer within 30 seconds, the export abandons the attempt and
> schedules a retry.

## What doesn't get a mark

- **Facts that aren't capabilities.** "All timestamps are UTC" is a property of the system, not a
  feature with a build state. Marking it invites a ✅ that means nothing.
- **Goals, principles, rules and instructions.** An agent-instructions directive is obeyed or not; it
  isn't implemented or not.
- **Non-goals.** What a capability deliberately doesn't do isn't a gap, and marking it invites a ⚠️
  that will never close.
- **Historical records.** Changelog entries, decision records and change logs describe a past state and
  are immutable. A ✅ there would be a claim about the present in a document about the past.
- **Anything a test can assert instead.** If a statement can be checked mechanically, write the test
  and let the test carry the truth.
