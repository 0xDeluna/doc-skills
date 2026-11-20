# PRDs

Create a PRD after a grilling session or complete context has resolved the product
decisions. A PRD is the human layer of an initiative. A reader who is new to the work must
understand the problem, the outcome, and the scope without reading the design doc.

Write it in ASD-STE100 Simplified Technical English, as the parent skill requires. Apply
the document header rules in [design-docs.md](design-docs.md) to the PRD file.

Create PRDs under `docs/prd/`. Give the PRD the same number and base name as its design
doc under `docs/design/`. Link the two. The PRD owns what and why; the design doc owns how
and done-when. Do not repeat a requirement in both.

## Shape

Use this order. Drop a section only when the PRD has nothing to put in it.

1. **Title.** One line, sentence case, no metadata header.
2. **Summary.** One short paragraph. State what is wrong or what is new, who is affected,
   and what the document decides.
3. **The domain in brief.** The few rules that a reader needs to understand the problem:
   the actors, the money, and the main flow. Link to the repository glossary.
4. **Problems.** Group the problems by theme. For each group, state what happens today and
   why it matters. Use the present tense for current behavior. Name a function, table, or
   error code only when a human needs it to understand the problem.
5. **Decisions.** The product decisions that shape the work. Use a table with `#`,
   `Decision`, and `Source` when the decision has an ID. List the rules that stay.
6. **Scope.** `In scope` and `Out of scope` as bullets. A non-goal prevents a plausible
   misreading, so name the tempting change that this PRD refuses.
7. **What success looks like.** Observable outcomes for a Buyer, a Brand, staff, or the
   business. Do not list test steps.
8. **Open questions.** A table with `#`, `Question`, and `Default`. Record each open
   question with a safe default. Do not invent an answer.
9. **Read next.** A link to the design doc and to the glossary.

## Rules

1. Do not include schema changes, function signatures, migration steps, work-item status,
   blast radius, or rollback. Those belong in the design doc.
2. Name the people affected: Buyer, Brand, staff, admin. Use the repository glossary and
   keep its capitalisation.
3. Write one fact per sentence. Keep most sentences under 20 words. Use the plain word.
4. Do not use "should", "could", "may", or "consider". State the decision.
5. Keep the PRD short enough to read in one sitting. If a section needs a code block to be
   clear, it belongs in the design doc.
6. Do not duplicate the design doc. Link to it instead.
7. A PRD is stable. Do not add implementation status. Link to the design doc's status
   section.

## Example

```markdown
## Problems

### A COD Order could not complete

An Order was confirmed only when its Payment status was `paid`. No code set a COD Order to
`paid`. A COD Order therefore stayed `pending` until it was cancelled, and it never
counted as Brand earnings.

**Result:** a COD Order can be confirmed while it is unpaid. Staff mark it paid after
delivery.
```

## Checks

Done when all of these hold:

- A reader who has not seen the design doc can state the problem, the people affected, the
  outcome, and the scope.
- Every product decision has a source or an ID.
- Every open question has a default.
- The PRD links to its design doc and to the glossary.
- No schema, signature, migration step, work-item status, blast radius, or rollback
  appears in the PRD.
- No filler, metaphor, or idiom remains.
