# Design docs

## Document headers

1. Start every Markdown design doc and PRD with its title and useful content.
2. Do not add repository paths, `Repo:` lines, dates, `Date:` lines, timestamps, author
   names, or generated metadata at the top unless the user explicitly requests them.
3. When editing an existing document, preserve its existing header unless the user asks to
   remove it, but never add a new metadata header.

## When to write a design doc

Use this branch when a new feature or a massive refactor needs a numbered technical doc. A
design doc records the problem, decision, boundaries, validation plan, and execution
order. It is the agent-facing counterpart of a PRD.

The PRD owns what and why. The design doc owns how and done-when. Link each document to
the other. Cite a PRD decision by ID instead of restating its product rationale. State
only the rule that a work item needs to be read alone.

A design doc is an implementation handoff, not a product summary. A reader must be able to
start, review, or reject each part of the work from the design doc alone.

The design doc is the plan. After the work ships, write the final document under
`docs/features/` for a new feature or `docs/refactors/` for a refactor. That document
records what was implemented. `docs/design/` holds the design docs for work in progress.

1. Decide whether a doc is needed before creating or editing one. If the need for
   documentation or the correct existing doc is unclear, ask the user and wait for an
   answer. For a fix, bug correction, or change to an existing feature, do not create a
   new doc. If a relevant doc already exists, edit it; otherwise leave the docs unchanged.
2. Create new design docs under `docs/design/`. After the work ships, write the final
   document under `docs/features/` for a new feature or `docs/refactors/` for a refactor.
3. Use the next numeric prefix in `docs/design/` for the initiative. Give the PRD in
   `docs/prd/` the same number and base name.
4. Use `NNN_UPPERCASE_SNAKE_CASE.md`. Recheck the number immediately before writing.
5. Cover the technical approach, exact interfaces, data changes, sequencing, failure
   modes, done-when criteria, and rollback. Link to the PRD.

## Shape

Use this order. Drop a section only when the design doc has nothing to put in it.

1. **Title.** One line, sentence case, no metadata header.
2. **Opening.** One short paragraph. State what is wrong or what is new, which branch or
   context the document applies to, and what the document decides.
3. **How to read this design doc.** A numbered path through the document. Use it when the
   design doc has questions, decisions, and more than three work items.
4. **Open questions.** A table with `#`, `Question`, `Default`, and `Blocks`. Each
   question names the work item it blocks, or `None`. The default is the behavior until
   the answer arrives. A question that stays in the document must have a safe default, and
   the affected item must state that behavior. Add a short detail paragraph only when a
   choice needs context that the table cannot hold.
5. **Work items at a glance.** One table for all work. Do not hide work in a later section
   that the table does not show.
6. **Decisions.** A table with `#`, `Decision`, and `Used by`. Cite the PRD decision ID
   when the PRD has one. These are final. Refer to them by ID in the work items.
7. **Scope.** `Goals` and `Non-goals` as bullets. The PRD owns the product scope. Restate
   only the boundary that a work item must not cross.
8. **Work items.** Group them into phases in this order: correctness, money, security,
   features, cleanup. Say that phases run in order and that the order inside a phase is
   free.
9. **Tests, risks and rollback.** The cross-cutting checks and the dangers that span
   items.
10. **Appendix.** Work already done, when the design doc follows a branch that already
    changed code. State what the branch did and did not change.

A single-feature design doc keeps the same fields with one work item and no phases. Do not
add empty scaffolding to look complete.

## Work items

1. One item is one reviewable change, and normally one pull request. If two changes cannot
   ship alone, say so in the item.

2. Give each item a stable ID and repeat it in the heading:
   `### W4. Brand cancellation rules`. The heading is the anchor that the at-a-glance
   table links to. Do not rename an ID after other sections cite it.

3. Use the same four fields in this order:

   - **Problem.** What the code does today, which symbol and file does it, and the effect
     on a user, a Brand, or money. One or two sentences. Use bullets only when there are
     several separate causes.
   - **Change.** The required edit, in execution order. One instruction per bullet. Name
     the function, column, procedure, or file that changes.
   - **Blast radius.** The danger level, what the change touches, the worst realistic
     failure, what must not change, and how to undo it.
   - **Done when.** The checks a reviewer runs. One observable statement per line, in the
     form "X does Y" or "X returns Z".

4. Put the applicable decisions on the line after the heading: "D10 and D11 apply." Do not
   restate the decision inside the item.

5. Use `####` sub-headings inside a large item when the change has parts that a reviewer
   handles separately: `Money`, `Schema`, `Claim`, `Release`, `API`, `UI`. Keep them noun
   labels, not sentences.

6. State what does not change, next to the change. "Staff rules do not change." "This is
   the current behavior."

7. Include a code block only when the exact text is the contract: a query, a schema, a
   state transition, a payload form, or a snippet whose shape is the decision. Do not
   paste the final implementation. The code must follow the repository's conventions.

8. Write the requirement in `Change`. `Done when` confirms it and does not introduce a new
   rule.

## Prose

Write for two readers at once:

- A person scans the page and reads one item.
- An agent executes that item without the rest of the document.

Both need short sentences, concrete nouns, and no unstated context. Write in ASD-STE100
Simplified Technical English.

### Words

1. Use the plain word: "use", not "utilize"; "start", not "initiate"; "send", not
   "transmit". Keep a technical term only when the repository uses it, and define it the
   first time.

2. Use one word for one thing through the whole document. Take the term from the
   repository glossary and keep its capitalisation: Buyer, Order, Payout, Brand earnings.
   Never alternate between two names for one concept.

3. Name the actor: the Brand, staff, an admin, the server, `placeCodCheckout`. Do not
   write "we", "the team", or "the system" when a role or a function is the actor.

4. Delete filler: "just", "simply", "very", "robust", "seamless", "comprehensive",
   "leverage", "in order to", "it is important to note". Do not use a metaphor, idiom,
   joke, or sports term. A reader whose first language is not English, or an agent, takes
   it literally.

### Sentences

5. Write one fact per sentence. Keep most sentences under 20 words. Split a sentence that
   joins two separate facts with "and". Do not chain dashes, semicolons, and commas.

6. Put the subject first, then the verb, then the object. Write "The Catalog hides the
   product", not "The product is hidden by the Catalog". Avoid "it", "this", and "there
   is" as sentence subjects.

7. Use the present tense for current behavior and the imperative for the required change.
   "The Catalog hides the product." "Return `NOT_FOUND`."

8. Write a requirement as "must" or as a command. Do not use "should", "could", "may",
   "consider", "ideally", "as needed", "appropriate", "similar", or "etc.". A soft or
   open-ended requirement cannot be reviewed or executed.

9. Put the reason in the same sentence as the rule, after a comma or "because". A reader
   who knows the reason can judge an edge case that the rule does not name.

### Layout

10. Start a paragraph with its point. Put the background after the point, or delete it.

11. Use a table for three or more comparable facts, bullets for steps and lists, and prose
    for cause and effect. Do not turn one sentence into a one-line bullet.

12. Keep a fixed field order in every item: `Problem`, `Change`, `Blast radius`,
    `Done when`. Never rename or move a field, so each fact stays in the same place.

13. Use sentence-case headings that start with a noun or a verb. Avoid "About",
    "Regarding", "Note on", and "Introduction". Write a bold run-in label as a label:
    `**Problem.**`, then one space, then the text. Do not bold a whole sentence inside a
    paragraph.

### No ambiguity

14. Make every item readable alone. Do not write "the above", "as mentioned", or "this
    change" across sections. Repeat the item ID or the identifier.

15. Name every input, output, and value. Write "returns `{ changed: boolean }`", not
    "returns a result". Give the number: "7 days", "50 products", "90 percent". Do not
    write "a few" or "several" when the number exists.

16. Put every identifier, table, column, procedure, error code, and value in backticks. Do
    not paraphrase an identifier. Write `order.management.get`, not "the order get call".

17. Show an example when a rule is easy to misread. Use before and after, or input and
    output. One short example replaces a paragraph.

18. Address the reader as "you" only in a question that needs their decision. Everywhere
    else, name the actor.

### Rewrite examples

| Weak                                      | Strong                                                                                             |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------- |
| "The system should handle errors better." | "Return `BAD_REQUEST` when the Order is not COD."                                                  |
| "Utilize the shared helper."              | "Call `normalizeBdPhone` in the field `onBlur` handler."                                           |
| "Deleting a brand is dangerous."          | "Deleting a Brand cascades to `order` and `payout`, so it erases Buyer Orders and Payout history." |
| "Test this well."                         | "A return 8 days after delivery fails and a return 6 days after delivery succeeds."                |

## Danger and blast radius

1. Rate each work item as `Low`, `Medium`, or `High`, in the at-a-glance table and in the
   item:

   - **Low**: one screen or one message. No stored data, money, permission, or shared
     helper changes. A bug is visible and a redeploy fixes it.
   - **Medium**: stored data, a shared code path, or a rule that more than one screen
     depends on. A bug spreads, but existing rows and money stay intact.
   - **High**: money, payouts, permissions, deletion, a migration over existing rows, or a
     security boundary. A bug can lose money or data, and a redeploy does not reverse the
     effect.

2. Answer three questions in the blast radius: what the change touches, what fails in the
   worst realistic case, and how to undo it. Name the concrete surface: the
   `ledger_transaction` table, the Brand portal, a guest Checkout.

3. Find the edge with this prompt: "If this is wrong in production, who notices, what do
   they lose, and how to repair it?" Write the answer, not the prompt.

4. State the invariant that must not move. "Commission is the same with and without a
   Coupon." "Staff rules do not change." A reader must be able to tell an intended change
   from a regression.

5. For a schema change, say what happens to existing rows: they keep their value, they are
   backfilled, or they fail. Put the migration command and the pre-flight check in the
   item. A migration that cannot roll back needs a warning before its command.

6. For money, name every balance, ledger post, and payout that changes. Give the check
   that runs before release and the person who reads it.

7. For permissions, name every role, every new path to data, and every audit event that
   records the actor. State the access that the change removes as well as the access that
   it adds.

8. Collect the dangers that cross items in `Tests, risks and rollback`. Each line names
   the item ID, the danger, and the check or warning before release.

9. Write the rollback as an action: "Deploy the earlier build. The new columns can stay."
   Say what remains in the database after the rollback, and which data a second release
   would read.

10. If you cannot state the blast radius, the item is not ready. Ask, or mark the unknown
    in `Open questions` with the default behavior until the answer arrives.

## Navigation

1. Put the shortest path to the work at the top: questions that block, then the
   at-a-glance table, then one item.

2. Link each at-a-glance row to its item heading. A reader must reach any item in two
   clicks.

3. Keep the at-a-glance columns to: `ID`, `Work item`, `Phase`, `Size`, `Schema change`,
   `Danger`, `Decision`. Define the size scale in one line above the table. Use `No`,
   `Yes`, or `Yes, additive` for schema changes.

4. Do not repeat text. Link to the section or cite the decision ID.

5. Write each heading so a reader who scans only headings knows what the item changes.
   Keep the deepest level for the parts of one item.

## Example

```markdown
## Phase 1: Correctness

### W2. Return window

**Problem.** `return.buyer.create` accepts a return at any time after delivery, also after
the Brand received its Payout.

**Change.**

- Read `order.deliveredAt` under the Order lock.
- Refuse with `BAD_REQUEST` when more than `PAYOUT_HOLD_DAYS` days passed.

**Blast radius.** Medium. Reads and writes `return` and `order` rows for every Buyer
return. A wrong limit either blocks valid returns or lets a refund through after the
Payout. Brand earnings do not change. Rollback is a redeploy; there is no schema change.

**Done when** a return 8 days after delivery fails and a return 6 days after delivery
succeeds.
```

## Checks

Done when all of these hold:

- The design doc links to its PRD, and the PRD links back.
- The design doc has no material ambiguity that could change its scope, design, or
  acceptance criteria.
- Every work item has `Problem`, `Change`, `Blast radius`, and `Done when`.
- A reader can execute one item without reading the rest of the design doc.
- Every requirement is a command or a "must" statement.
- Every `Done when` line is observable by a reviewer.
- Every item has a danger level, and every `High` item names its worst failure and its
  rollback.
- Every open question has a default that the item states.
- The at-a-glance table links to every item, and no item is missing from it.
- No sentence joins two facts with "and", and no filler, metaphor, or idiom remains.
