# Knowledge-driven development

A way of building software when most of the builders are agents. There is no backlog beside the
system: the documents that describe the system are the backlog. They describe what the system should
be, in the present tense, and every statement carries a mark saying whether it's true yet.

This repository is the canonical statement of the convention. Repositories that adopt it link here
instead of restating it. The reasoning behind it, and what it's like to work this way, is in a
five-part series on icelander.net, starting with
[There is no TODO](https://icelander.net/v1/processes/there-is-no-todo).

Knowledge-driven development is an experiment. It started on 2026-08-28 and isn't battle-tested yet.

## The four marks

| Mark | Means |
| --- | --- |
| ✅ | **Implemented and verified.** The behavior exists, and someone has read the code and seen its tests pass. |
| ⚠️ | **Not implemented.** The sentence describes the intended behavior. Nothing behind it works yet. This is the backlog item. |
| ❌ | **Implemented but broken.** A known defect. |
| ⛔ | **Blocked.** The gap marked just before it can't be closed from here. Always follows a ⚠️ or ❌, never stands alone, and always says what would unblock it. |

The marks live in documentation only. They don't go in source comments, and they never go in a
string a user of the product reads. → [02 § Where the marks go](02-annotation-convention.md#where-the-marks-go)

## The rules that matter most

1. **One description of the system.** Everything the system should be, as far as anyone currently
   knows it, written once, in the document its reader will open. → [01](01-methodology.md)
2. **The mark is the whole commentary.** Write the sentence as the system should be and mark it. Don't
   explain the mark. → [02](02-annotation-convention.md#what-each-mark-obliges-you-to-write)
3. **✅ means someone read the code and saw the tests pass.** Not that a plan said so, an agent
   reported success or a pull request merged. → [02](02-annotation-convention.md#what-each-mark-obliges-you-to-write)
4. **The mark moves in the same commit as the code.** Shipping a capability turns its ⚠️ into ✅ in
   the same pull request. → [02 § Ownership](02-annotation-convention.md#ownership)
5. **Graduation.** A document with no ⚠️ and no ❌ left loses every ✅ in its body and keeps one line
   at the top. → [02 § Graduation](02-annotation-convention.md#graduation)
6. **There is no `TODO`.** Not as a folder, not as a comment. It's a build failure.
   → [02 § There is no `TODO`](02-annotation-convention.md#there-is-no-todo)

## The documents

| # | Document | What it's for |
| --- | --- | --- |
| 01 | [Methodology](01-methodology.md) | What the method is, why it suits agents, where it breaks, and what it replaces |
| 02 | [Annotation convention](02-annotation-convention.md) | The rules for the marks: placement, what each one obliges you to write, ownership, graduation, `TODO` |
| 03 | [Workflow](03-workflow.md) | How a feature goes through it: planning, implementing, defects, the sprint board, review, scheduling |
| 04 | [Document taxonomy](04-document-taxonomy.md) | The kinds of document, which one a fact belongs in, and adding a kind |
| 05 | [Adoption](05-adoption.md) | Bringing a repository under the convention |
| 06 | [Enforcement](06-enforcement.md) | The one gate that earned a build failure, the optional ones, and when to ask an agent instead |
| — | [templates/](templates/) | A starting file per document kind, and the lines to add to a repository's agent instructions |

## Linking to this convention

A document under the convention says so in one line at the top, and never restates the legend:

```markdown
> Status marks: [knowledge-driven development](https://github.com/david-brandt-the-icelander/knowledge-driven-development)
```

A document without that line hasn't adopted the convention. That's a legitimate state, and not a
claim that everything on it works.

This repository is exempt from its own greps. Every mark in it is an example or a definition, not a
claim about a system.
