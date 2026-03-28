# Swift Anti-Patterns

Use this reference to steer away from code that looks sophisticated but ages badly.

## Architecture

- Ceremony-heavy layering for simple features.
- View models that become dumping grounds.
- Framework or package code forced through app-style MVVM/service layering when a smaller capability type would do.
- Omnipresent protocol mirrors with no real seams.
- Functional-programming cosplay where ordinary feature code is rewritten into dense pipelines for style points.

## Modeling

- Boolean soup instead of explicit state.
- Stringly typed identifiers and domains where stronger modeling would help.
- Transport models leaking through every layer.
- Mutation-heavy workflows where a few explicit value transforms would be clearer.

## Organization

- Giant files and giant types with mixed responsibilities.
- Over-fragmentation where every tiny helper becomes another file without improving understanding.
- Extensions used as camouflage for one oversized type.
- Tiny protocols and helper types introduced only to “look POP” rather than solve a real boundary problem.
- Incremental workaround code that adds flags, modifiers, properties, or helper layers which merely compensate for each other.

## Concurrency

- Fire-and-forget mutations with unclear ownership.
- Callback compatibility layers left in place for new code without reason.
- Hidden task lifetimes and accidental concurrent mutation.

## Public APIs

- Public APIs that mirror internal folder or layering structure instead of stable domain concepts.
- Public protocols exposed only because the implementation wanted internal seams for testing or injection.
- Public async APIs with unclear actor isolation, cancellation, or sendability expectations.
- Error surfaces that force consumers to understand transport, storage, or implementation details.

## Modules

- `Core`, `Common`, or `Utils` modules with no crisp one-sentence responsibility.
- Shared modules that drag app-specific UI or product policy into supposedly reusable code.
- Package extraction done before a stable shared API exists.
- Access control widened mainly to make a messy boundary compile.

## Testing

- Overbuilt mocks and scaffolding.
- Tests coupled to implementation trivia.
- No seam at all where substitution was genuinely needed.
- So much abstraction that the test has to reconstruct the architecture before it can express behavior.
