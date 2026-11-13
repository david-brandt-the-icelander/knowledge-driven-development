# Agent instructions

This repository is the canonical statement of knowledge-driven development: [README.md](README.md), the numbered documents and [templates/](templates/). The convention is superseded and the repository stays as it was written, so do not change what the documents say unless the owner asks. It is exempt from its own greps: every mark in it is an example or a definition.

## Architecture diagrams and knowledge files

Each folder documents itself in a `.dev/` directory, as JSON following the skills in `Meta/Agents/Skills/` and the knowledge catalogue in `Meta/Agents/Knowledge/` (human-assisted software development). Read a folder's `.dev` files before changing it. `populate-dev` decides what a folder needs; the diagrams are:

- `describe-component-diagram`: the folder is a cohesive capability, adapter, boundary or subsystem.
- `describe-class-diagram`: the folder itself declares more than one class.
- `describe-state-diagram`: the folder implements a state machine. One `<Identifier>.state-diagram.json` per machine, named after the class or type it belongs to.
- `describe-deployment-diagram`: the folder holds something independently deployable.

The repository holds documents, with no code, nothing deployed and no state machine, so only the root (`.`) gets a diagram: a component diagram of the documents, `templates/`, `Meta/Agents` and what they point at outside the repository. The root and `templates/` each have a `folder` record; the root's `.dev/` also holds the convention's decisions, references, contracts, limitations, glossary and backlog. `Meta/` is a copy and has no `.dev/`.

What a folder does, offers and exposes goes in its use cases, user journeys, capabilities, features and operations (`describe-use-case`, `describe-user-journey`, `describe-capability`, `describe-feature`, `describe-operation`). What it intends, promises and needs goes in knowledge files, written with `describe-knowledge`. Wanted work, including a defect found in the documents, is a `backlog` item in the folder that will own it, and a question only the owner can answer is a `decisions` record with `status: open`.

Every actor a use case, journey or diagram names has a file in `Actors/` at the repository root, written with `describe-actor` and linked from whatever names it; delete an actor nothing links to. None is named yet, so there is no `Actors/`. Update the `.dev` files and actors in the same commit as what they describe, and delete one that stops applying. Before finishing, run both validators from the repository root, with the `jsonschema` package installed:

```bash
python3 Meta/Agents/Skills/validate_diagrams.py
python3 Meta/Agents/Knowledge/validate_knowledge.py
```

The skills and the catalogue come from <https://github.com/david-brandt-the-icelander/human-assisted-software-development> (`Agents/Skills` and `Agents/Knowledge`). Change them there and copy them here, unchanged, so there is one version.
