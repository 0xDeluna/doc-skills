# Architectural critique

Judge whether the subsystem is built well for what it does and for how it will evolve. Line-level bugs and style are out of scope.

## Bar

Each finding shows a concrete cost in the current code: harder to test, harder to change, a performance cliff at scale. An intentional tradeoff with clear benefits is not a finding, and neither is a rewrite or extra abstraction without a demonstrated problem. A sound architecture with no findings is a valid verdict.

## Lenses

Apply the ones relevant to this subsystem.

- **Abstraction fit**: does each abstraction model a real concept, with boundaries between things that change independently? Look for speculative indirection, components sharing implementation details, and business logic tangled with framework wiring. Over-abstraction is as costly as under-abstraction; a flat design suits a simple domain.
- **Data model**: does its shape match how the data is accessed? Look for code that keeps reshaping data, and types that claim more structure than the runtime data has.
- **Boundaries**: is validation at entry points, error handling at boundaries, and data crossing them in typed shapes rather than bags of optional fields? Can the subsystem be tested in isolation?
- **Evolution**: if the most plausible next requirement landed tomorrow, does it touch one file or everything? Look for hardcoded assumptions, bolted-on designs, and compatibility paths nothing uses. Judge only changes the codebase's trajectory makes plausible.
- **Complexity vs. value**: does the complexity sit in core logic and invariants, or in boilerplate, indirection, and configuration? Does every component earn its place?
- **Consistency**: are similar problems solved here as elsewhere in the codebase? Unexplained divergence is a maintenance cost.

## Verdict

Group findings by priority:

- **Act on**: an architectural problem worth fixing now, such as a wrong boundary, a broken data model, or coupling that blocks future work.
- **Consider**: a real cost, but the fix's cost/benefit is unclear.
- **Noted**: valid and low priority, such as a tradeoff that may age badly or minor debt.

For each finding:

```
### Short title
**Components**: the parts involved
**Finding**: the issue, naming the components, boundary, or coupling
**Evidence**: code references that show it, such as the dependency chain
**Impact**: what it costs in practice
```
