---
name: code-quality-review
description:
  Review code for readability and maintainability against the rules below. Apply them as a
  fixed standard, not as a suggestion list. The review covers quality, not correctness.
disable-model-invocation: true
---

# Code Quality Review

Make the requested code easier to maintain and read. Preserve inputs, outputs, side
effects, ordering, performance constraints, and error behavior. Assume a TypeScript stack:
React with Next.js or TanStack Start, Tailwind, shadcn with Base UI, tRPC, Drizzle over
Postgres or SQLite, Zod, and React Hook Form.

## Code placement

Place code at the narrowest scope that serves its consumers. Move it outward only when a
second consumer appears. In a monorepo:

1. Keep apps thin. An app's `src` holds entrypoints and wiring: server bootstrap, route
   registration, provider mounting. Business logic, services, request context, and
   environment access belong in packages.

2. Give each domain one package: `api` for procedures and resource logic, `db` for
   database access, `auth` for sessions and roles, `env` for configuration, `utils` for
   shared helpers, `ui` for shared components. Each package exposes its public surface
   from `src/index.ts`.

3. Group API code by resource. Each resource owns a folder under
   `packages/api/src/routers/`:

   ```
   routers/
       booking/
           index.ts
           booking.service.ts
           booking-emails.service.ts
       question/
           index.ts
           public.ts
           management.ts
   ```

   - `index.ts` composes the folder's subrouters; callers import only that file.
   - A subrouter file is named for the path segment it adds, so `public.ts` produces
     `question.public`.
   - A procedure parses its own input and owns its authorization gate.
   - Put logic shared by the resource's procedures in `<resource>.service.ts`. Split a
     second concern into `<resource>-<concern>.service.ts` and keep each service focused.
   - Logic used by one resource stays in that resource's folder.

4. Put a helper where its reach matches:

   - More than one package: `packages/utils/src/<topic>.ts`.
   - More than one resource in one package: `packages/<pkg>/src/utils/<topic>.ts`.
   - One resource: that resource's folder.

   Name the file for its topic (`time-zone.ts`, `resend.ts`), not for its shape
   (`helpers.ts`, `misc.ts`). When a shared helper exists, replace the app-local copy
   instead of keeping both.

5. Expose package-owned setup through one accessor from the package root, such as
   `getDb()` from `packages/db/src/index.ts` or `getAuth()` from
   `packages/auth/src/index.ts`. Put one-off setup such as seeding in a named module
   (`admin.ts`). Callers import the accessor and never construct the client themselves.

6. Read configuration through the `env` package's typed `./server` entrypoint. Apps and
   packages import `env`; they do not declare their own environment types or read platform
   globals directly. A package that only needs the environment type may take the
   infrastructure package as a dev dependency.

7. Build request context once in the API package's `context.ts`. An app passes only the
   transport input, such as `headers`, and receives the typed context with the database
   client and session.

### Frontend placement

1. In a Next.js or TanStack app, follow the app router with `src/`:

   ```
   /src
       /app
           /page1
               /component1.tsx
               /page.tsx
               /layout.tsx
           /admin-profile
               /admin-sidebar.tsx
               /component2.tsx
               /page.tsx
               /layout.tsx
           /user-profile
               /page.tsx
           page.tsx
           layout.tsx
           app-sidebar.tsx
       /components
           /profile
               /profile-component.tsx
           /ui
           /shared-component.tsx
   ```

2. `page.tsx` holds the page's content and markup, and fetches data that several
   components consume. `layout.tsx` holds the page skeleton such as the sidebar, header,
   nav bar, and footer, with no business logic or data fetching. A layout may redirect
   unauthorized users from the pages under it, such as dashboards and admin pages.

3. Keep large, complex components in their own file beside the page. Move a component to
   `/components` when several pages use it.

4. Keep primitive components in `/components/ui`. Put shared hooks, client state, and
   styles in `/hooks`, `/stores`, and `/styles`.

5. Outside app router projects, `src/index.ts` may be the entrypoint:

   ```
   /src
       /index.ts
   ```

### File names

1. Use kebab-case: `user-profile.tsx`, `admin-sidebar.tsx`, `time-zone.ts`.
2. In a Next.js app, end server and client modules with `.server` and `.client`.
   Components under `/components` and `/components/ui` keep their plain names.

### Vendored UI

Keep third-party components vendored in `packages/ui` or `/components/ui`, in their
upstream shape. Mark each local change with a `// Local patch:` comment and the reason.
Keep compiler relaxations for vendored code scoped to that package.

## Coupling

Ask one question of every change: should these pieces change together? The code must
answer correctly. A typical change lands in one place, and no module imports something
more volatile than itself.

1. Show every link between pieces that must change together. Give logic and values one
   home and import it, so a change cannot reach one copy and miss another. Keep one
   feature in one folder, so a change to the feature does not have to find it across
   layers. Put a call order or a convention inside the module that owns it, so a caller
   cannot break it. Keep a module's state and files private, so only the owner changes
   them.

2. Remove every link between pieces that change for different reasons. Let a caller depend
   on an interface, not on internals. Import from what changes less, so stable code never
   depends on volatile code. Add a parameter only when every caller understands it, not
   because one workflow needs it. Keep each module deep: a small interface over a larger
   body.

3. Remove every piece with no reason of its own to change. Delete a wrapper that only
   passes its arguments through, an extension point or config that nothing varies, and
   code that nothing calls. Keep nothing for a future that has not arrived.

4. Replace every special case with the rule it follows. An `if` for one value, a parameter
   for one caller, a swallowed exception, a retry or a sleep, and a disabled test each
   mark a requirement the design missed. When several cases share a cause, fix the cause
   in one change instead of handling each case.

## Dependencies

1. Declare a dependency in each workspace that imports it, and remove one the workspace no
   longer imports. Keep a peer dependency when an adapter requires it.
2. Write `catalog:` for an external package used by more than one workspace, so every
   workspace resolves one version.
3. Keep a package used by one workspace on its own range in that workspace's
   `package.json`.
4. Keep the root `package.json` to shared tooling. Workspace scripts call the root binary
   instead of declaring a second copy.

## Naming conventions

1. Use camelCase for variables and functions. For example `userProfile`, `getUserData()`.
2. Use PascalCase for component names and class names. For example `UserProfile`,
   `UserCard`.
3. Use `is` or `has` prefix for boolean variables. For example `isLoggedIn`,
   `hasPermission`.
4. Use `on` prefix for event handler functions. For example `onClick`, `onSubmit`.
5. Use `get` prefix for functions that return a value or fetch data. For example
   `getUserData()`, `getProfile()`.
6. Every name must reduce the reader's mental work.
7. Match the vocabulary the repository already uses. Use one word for one concept across
   the codebase, not only inside one file.
8. Keep names short. Avoid noun clusters longer than three words and pick the shortest
   word that stays unambiguous. Prefer `user` over `userAccountProfile` and `retry` over
   `retryAttemptCount`.
9. Carry the unit in the name when a bare value would be ambiguous. For example
   `timeoutMs`, not `timeout`.
10. Rename a name only when it is vague or misleading. Keep a name that already states its
    purpose.
11. In API paths, the folder names the resource and each router or procedure names the
    next path segment: `account.applicants`, `account.setRole`, `booking.move`,
    `question.public.list`. Keep role-specific behavior in the resource it changes and
    gate it in the procedure; do not add a parallel `admin.*` namespace.

## Forms

1. Pick one form library per project and use it for every form: `react-hook-form` or
   `@tanstack/react-form`.
2. Keep the form controlled. The library holds each field's value, its validation state,
   and its error state, so the form has one source of truth.
3. Define the shape and the rules of a form in a schema. Use `zod` or `@sinclair/typebox`,
   and let the schema drive the form library through its resolver. The schema is the one
   place that holds the validation rules and their messages.
4. Compile the schema when the validation library can. Zod 4.5 and up compiles a schema
   ahead of time: call `z.compile()` on the final schema, or import `zod/compile` once in
   the entrypoint to compile every schema in the app. A derived schema is not compiled, so
   compile the schema the form parses with. An async refinement or transform falls back to
   the normal parser.
5. Write the error message for the person filling the form. Say what is wrong and what to
   do next, in plain words. Assume the user isn't a developer so the error must be
   understandable for them and help them debug.
6. In a React web or Electron app, render the fields with the shadcn `Field` component, so
   the label, the description, and the error of a field stay accessible and consistent.
   See the shadcn guide for the library the project uses: React Hook Form
   (https://ui.shadcn.com/docs/forms/react-hook-form) or TanStack Form
   (https://ui.shadcn.com/docs/forms/tanstack-form).
7. Keep every error visible to the user. Render a field's error next to the field it
   belongs to. When the error belongs to the form as a whole, report it with a shadcn
   toast. The form submits only when it is valid, so the user learns about an error before
   any submission goes through.
8. Validate the form again on the server with the same schema. The client-side check is a
   convenience, not a guarantee. When the server rejects the input, return the schema's
   errors in the response so the client can render them next to the fields that failed,
   using the messages the schema already defines.

## Typography

1. Use the shadcn typography components for headings, paragraphs, and lists, so every
   screen keeps one type scale and one rhythm.
2. Keep the sans-serif face of the design system as the default. Monospace and serif
   belong to code and quotations, and to the cases where the user asks for them.
3. Treat a font that a comment marks as deliberate as settled. When no comment explains a
   serif or monospace font, ask the user before you remove it.

## Linter

1. Fix what the linter flags. When a rule has to be bypassed for one section, ask the user
   first, and scope the bypass to that section.
2. Keep the lint configuration in one file at the project root.
3. Treat a change that silences the linter without fixing the finding as a bypass.
   Splitting a file, renaming a symbol, or widening a type to dodge a rule counts.

## Source code structure

Read a file as an outline: the dependencies first, the entry point last.

1. Start with the imports.
2. Put the module constants and the types next. Name the constants in UPPERCASE_SNAKE_CASE
   and the types in PascalCase.
3. Put the file-local helpers and utilities next, in camelCase. A helper that another file
   needs moves to a utils file. Keep a helper that only this file uses out of the export
   list.
4. Put the main function or component at the bottom of the file.
5. Give each large component its own file. A small component that only absorbs repeated UI
   code, such as a typography wrapper or a list formatter, stays in the same file. A
   component that another file needs moves to a components folder.

## Comments

A comment carries what the code cannot: the intent, the decision, the constraint, or the
reason.

1. Add one concise comment directly above every function, method, or assigned function
   whose name and signature do not already explain it. State what it does, why it exists,
   and what it returns in simple words. Skip the comment when the name and the signature
   answer those questions; a comment that restates the name is noise.
2. Use JSDoc for a helper, a utility, or an exported or public function. One line fits a
   small helper. A multi-line block adds the parameter and return types, why the function
   exists, and how it fails.
3. Use short comments to separate the large chunks of related work in a long function.
   Group validation, the database query, the mutation, and the response, and name the
   purpose of each group.
4. Keep a comment small, concise, and informative. State one fact, decision, constraint,
   or action. Use a clear subject and an active verb, and write an instruction as a
   command. A good comment gives context the code cannot show. Remove a comment that
   repeats the function name, the next line, or the obvious syntax, uses a vague label
   such as `// Handle edge case`, records temporary history, or describes behavior the
   code does not enforce. When a comment describes behavior the code does not enforce,
   make the code enforce it or remove the comment.
5. Put a comment that explains a key, a value, or an argument at the end of that line. A
   comment on its own line describes the block or the function that follows. In a chained
   definition, keep the comment on the first line of the key.

   ```ts
   export const table = pgTable("table", {
     id: text("id").primaryKey(), // row id
     userId: text("user_id") // owner id
       .notNull()
       .references(() => user.id),
   });
   ```

6. Label every major section of a file with one single-line comment right above it, as in
   `// Types`, `// Global constants`, or `// Request handlers`.
7. Do not use a banner of dashes such as `// ---`, a repeated dash line, a boxed block, or
   a multi-line separator. Keep a function context comment, a chunk comment, and a file
   section comment to one line and at most 80 characters. Move detailed rationale and
   implementation notes into the surrounding documentation or the code structure.
8. In JSX, TSX, HTML, and other markup, use exactly one comment form: a short
   `{/* heading */}` separator between the major regions of the layout, right above the
   region it labels. Keep the logic, the rationale, the accessibility notes, the
   implementation details, and the debugging notes in the surrounding code or in the
   component documentation.

Done when every eligible function has a context comment, every large function has clear
chunk boundaries, every major file section has a single-line label, the comments about
keys, values, and arguments sit at the end of their line, no large or `// ---` separator
remains, and the markup uses only the allowed short separator form.
