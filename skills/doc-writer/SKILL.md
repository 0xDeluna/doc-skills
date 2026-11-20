---
name: doc-writer
description:
  Create or revise useful code comments, numbered design docs, and PRDs after requirements
  are clear. Use when a change needs documentation or a user asks for comments, a design
  doc, or a PRD.
---

# Doc writer

Write concise, specific documentation for the next reader.

## Process

1. Inspect the README, applicable `AGENTS.md` or `CLAUDE.md`, target code, nearby tests,
   and existing docs. Done when the audience, purpose, repository rules, and documentation
   location are clear.
2. Choose the requested branch below and draft only what the change needs. State the
   danger and blast radius of every design doc: what the change touches, what fails if it
   is wrong, and how to undo it. Done when every comment or section explains a decision,
   behavior, constraint, or action.
3. Review the result, check the diff, and run available documentation checks. Done when
   the output is accurate, correctly named, free of filler, and limited to the requested
   scope.

## Prose rules

Write all comments, design docs, and PRDs in ASD-STE100 Simplified Technical English. The
repository glossary and conventions take precedence over the standard for terminology and
names.

Write for two readers at once in a design doc: a person who scans the page, and an agent
that executes one section without reading the rest. Write a PRD for a human reader who
needs the problem, the outcome, and the scope without the technical detail.

## Branches

One branch fires per task; load only its guide:

- **Code comments** (adding or revising comments in code):
  [references/code-comments.md](references/code-comments.md)
- **Design docs** (a new feature or a massive refactor needs a numbered design doc under
  `docs/design/`): [references/design-docs.md](references/design-docs.md)
- **PRDs** (the human-facing problem, outcome, and scope for an initiative, under
  `docs/prd/`): [references/prds.md](references/prds.md)
