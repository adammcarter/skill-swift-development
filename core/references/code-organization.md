# Code Organization

Use this reference for file structure, declaration order, extensions, and decomposition.

## Core Rule

Optimize for scanability in review. A file should tell a coherent story without forcing the reader to hunt through a maze.

## Preferred File Structure

For a file with one primary type:

1. `import` statements
2. short type doc/comment only if needed
3. type declaration
4. stored properties ordered by access level: public, internal, private
5. initializers
6. public API
7. internal helpers
8. private helpers
9. nested types
10. protocol conformances/extensions

Additional defaults:

- Prefer inline protocol conformances in the main type declaration when the conformance body is trivial or empty.
- Use extensions for conformances when the conformance has enough implementation to earn the split.

## Extensions And Conformances

- Use extensions to separate true conformances or meaningful logical groupings.
- Do not scatter a small type across many trivial extensions just to simulate “organization.”
- Keep protocol conformances near the end unless a codebase convention requires otherwise.
- Prefer one file per primary type unless a tightly related pair is materially clearer together.

## Decomposition Bias

- Strongly prefer smaller focused files and types when that improves clarity.
- Split a type when it clearly has multiple responsibilities, multiple mental models, or a long helper tail.
- Do not split so aggressively that understanding a simple workflow requires opening six tiny files.

## Comments

- Keep comments minimal.
- Comment intent, tradeoffs, invariants, or surprising behavior.
- Do not narrate obvious code.
- Prefer no comments by default.

## Access Control

- Use the narrowest access level that keeps the design honest.
- Avoid accidental wide visibility.
- Prefer a clean public surface with implementation details kept private.

## Feature Layout

- Group related source files by feature.
- Within a feature, prefer explicit folders such as `Views/`, `Models/`, and `Services/` when that improves navigation.
- Keep tests in the test module or test target, not mixed into the source feature folder.
