---
name: how
description: Explain how part of this codebase works, critique its architecture, or onboard a new hire to the codebase, an issue, or a PR.
disable-model-invocation: true
---

# How

Answer "how does X work?" questions about this codebase. The reader is a senior engineer new to the area: give them a working **mental model** of the subsystem, not annotated source code.

Modes:

- **Explain** (default).
- **Critique**, when the user asks for problems, issues, or improvements: explain first, then review the architecture.
- **New-hire**, when the user asks for it or says they are new to the company: explain the codebase, an issue, or a PR to an engineer with no company context. Read [`references/new-hire.md`](references/new-hire.md).

## Writing

Write all prose in every mode in ASD-STE100 Simplified Technical English. This includes follow-up answers in the same conversation.

- Write one topic in each sentence. Use a maximum of 20 words in an instruction and 25 words in a description.
- Use the active voice and simple tenses.
- Use one word for one meaning. Use the same word for the same thing each time.
- Use a maximum of three nouns in a noun cluster.
- Use a maximum of six sentences in a paragraph.
- Keep articles ("a", "the").
- Keep code identifiers, file paths, and repository terms in their exact form. Repository terms take precedence over the STE dictionary.
- Name the actors: "`ComposerService` calls `StreamHandler.begin()`", not "the service delegates to the handler".
- Put every link in the Code map (Explain step 3). Everywhere else, name symbols in backticks, with no link and no path.
- Explain why complex parts are complex; keep simple parts short.
- State open questions plainly.

## Explain

### 1. Scope

Name what the question targets: a subsystem, a feature flow, an architectural overview, or a runtime trace. If it is ambiguous, state your interpretation and proceed; the user redirects if needed.

For a question that spans several files or services, split it into 2–4 distinct **angles** (for message virtualization: data model and state; rendering and DOM; scroll and measurement). A narrow question is one angle.

### 2. Explore

Read the code; names are hints, not facts. For each angle:

1. **Entry point**: what triggers the behavior (user action, API call, job).
2. **Flow**: follow the call chain, reading each function and the data passed between steps.
3. **Abstractions**: read the definitions of the central types, interfaces, and services.
4. **Boundaries**: where it connects to other subsystems, and what goes in and out.
5. **Surprises**: historical artifacts and anything a newcomer would misread.

Done when every angle traces from trigger to effect with no hand-waved step. Record each connection you cannot trace as an open question ("I couldn't determine how X connects to Y").

### 3. Explain

Adapt this structure to the question; drop sections that add nothing.

- **Overview**: 1–2 paragraphs on what it is, what it does, and why it exists. Enough for the reader to decide whether to read on.
- **Code map**: a table with one row for each symbol, file, and directory the answer names, in order of first mention. Columns: the name, and a link to its absolute path. The link text is the file name, plus the definition line for a symbol: [stream-handler.ts:42](/abs/path/src/stream-handler.ts:42). Take the line number from the file you read.
- **Key concepts**: the types, services, and abstractions needed to follow the rest, briefly.
- **How it works**: the longest section. The trigger, each step, where data goes, the decision points. Write prose, not pseudocode; name the functions; quote code only when a snippet is essential. Add a diagram when several components interact or data moves through stages: mermaid for sequences, flowcharts, and component graphs; ASCII for simple relationships.
- **Gotchas**: sharp edges, surprising behavior, and history that explains odd code.
- **Open questions**: each connection you could not trace in step 2.

## Critique

1. Run Explain in full.
2. Read [`references/critique.md`](references/critique.md) and review the architecture against it. Re-read the code rather than trusting your explanation, which may frame things charitably.
3. Present the explanation first and the verdict below it, so the explanation stands on its own.
