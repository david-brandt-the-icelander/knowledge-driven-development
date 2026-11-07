# Agent instructions

> Part of [knowledge-driven development](../README.md), which defines the marks.

Paste the block below into the repository's agent instructions (`AGENTS.md`, `CLAUDE.md`, or whatever
every agent reads), and adjust the path to the documentation. Keep it short: those files are re-read on
every turn.

```markdown
## Knowledge-driven development

This repository's documentation follows knowledge-driven development:
https://github.com/david-brandt-the-icelander/knowledge-driven-development

- Before changing anything, read the documents in `documentation/` that describe it. They are your
  primary guidance, ahead of the prompt, the chat history or any ticket.
- Move the mark in the same commit as the code: what you ship turns its ⚠️ into ✅.
- Write ✅ only after reading the code and seeing its tests pass.
- When a fix has to wait, record the defect as a ❌ in the document that owns the behavior.
- Never write `TODO`, `FIXME`, `HACK` or `XXX`. Write a ⚠️ or ❌ in the owning document instead.
- Write a change log for every prompt that changes the code: what was asked, how you interpreted it,
  and what the change needs (testing, data migration, compatibility).
```

The change-log line is optional, but it's half of what makes review fast.
→ [03 § Review](../03-workflow.md#review)
