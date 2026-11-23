# New-hire mode

The reader is an engineer who just joined the company. They know general software engineering. They know nothing about this company, its product, its domain, or its internal names.

## Steps

1. **Read the target.**
   - Codebase or area: do Explain steps 1 and 2 (Scope, Explore).
   - Issue: read it with its comments (`gh issue view <n> --comments`). Then explore the code it is about.
   - PR: read the description, comments, and diff (`gh pr view <n> --comments`, `gh pr diff <n>`). Then explore the code around each change, so you can explain what it was before.
2. **Collect the glossary.** List each term the reader needs that is not general software knowledge: product and domain terms, internal service, team, and tool names, acronyms, central types, and common words this project uses with a special meaning. Done when each such term in your answer has a glossary entry.
3. **Answer** in the structure for the target.

## Structure

Each answer starts with **Overview** (1–2 paragraphs) and **Glossary**. Each glossary entry is the term and a definition of one or two sentences. Then:

- **Codebase or area**: How it works, Gotchas, and Open questions, as in Explain step 3.
- **Issue**: the problem or request, how the affected code works now, where the fix probably goes, and open questions.
- **PR**: why the change is necessary, the background the reader needs, the changes in order of the flow (not file order), and the risks a reviewer must check. Show each key change as a `diff` snippet.

Show snippets as in Explain step 3.

If the user also asks for Critique, put the verdict last.
