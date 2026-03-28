# Error Handling

Use this reference for recoverable failures, typed domain errors, and readable failure paths.

## Core Rule

Make failure explicit and useful. Errors should help the caller decide what to do next.

## Practical Rules

- Use typed errors where they improve handling and clarity.
- Preserve useful context without turning every call site into a ceremony pit.
- Distinguish domain failures from transport or persistence failures when the caller needs that distinction.
- Prefer one clear failure path over scattered special cases.

## In View Models

- Translate low-level errors into user-relevant state deliberately.
- Do not leak transport trivia directly into presentation state unless the product truly wants it.
- Prefer feature-facing typed errors and message surfaces via standard error protocols such as `LocalizedError` rather than ad hoc custom message properties by default.

## In Services

- Keep mapping and wrapping decisions consistent.
- Avoid swallowing errors and returning success-shaped defaults unless that behavior is explicitly desired.
- Map transport or persistence errors at the boundary into feature or domain-facing error types when that makes upper layers cleaner.

## Warning Signs

- `catch {}` that discards real meaning.
- Raw stringly typed errors as the main design.
- Failure encoded as `nil` when the distinction matters.
