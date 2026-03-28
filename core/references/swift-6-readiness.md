# Swift 6 Readiness

Use this reference when writing or reviewing Swift that should stay healthy under stricter concurrency and modern language expectations.

## Core Bias

- Prefer code that is naturally compatible with stricter concurrency checking rather than patched later with annotations.
- Make isolation, sendability, and ownership explicit.
- Prefer structured concurrency over unstructured concurrency by default.
- Prefer value semantics, immutable state, and narrow mutation scopes.
- Treat compiler warnings in this area as design feedback, not just friction.

## Sendability

- Prefer `Sendable` value types for domain models, state, requests, responses, and configuration.
- Add `Sendable` conformance where it reflects the truth of the type.
- Prefer immutable stored properties where possible; they make sendability and reasoning easier.
- Do not slap `@unchecked Sendable` on a type unless you have carefully verified the invariants and there is no simpler design.

Good defaults:
- `struct` plus immutable fields
- small enums for state and errors
- explicit non-sendable boundaries when the type should stay isolated

Warning signs:
- reference-heavy models passed freely across tasks
- `@unchecked Sendable` used to silence design problems
- mutable shared objects being treated as if sendability were automatic

## Isolation

- Prefer explicit actor or `@MainActor` isolation where the ownership boundary is real and easy to explain.
- Keep UI-facing observation and view model state on `@MainActor`.
- Prefer moving off the main actor at the UI/model seam rather than hopping on and off across the workflow.
- In model and service layers, prefer simple structured concurrency with minimal actor hopping unless the code genuinely requires it.
- Prefer isolated mutation over “hope this is only called on one queue.”
- Use actors sparingly for truly shared mutable concurrent state, not as a blanket async pattern.

## Structured vs Unstructured Concurrency

- Prefer structured concurrency by default.
- Prefer `async let`, task groups, and ordinary `async` functions when the parent scope should own the work.
- Use `Task {}` only when you genuinely need to create unstructured work with a different lifetime or entry point.
- Treat `Task {}` as a deliberate escape hatch, not a standard way to start async code.
- If a workflow can be expressed as simple awaited calls in the current async context, do that instead.

## API Design Under Strict Concurrency

- Make async and isolation boundaries visible in the type system.
- Prefer APIs that do one asynchronous job clearly.
- Avoid escaping mutable state through callbacks or loosely controlled shared references.
- Prefer returning sendable values from services and clients rather than leaking mutable reference graphs.

## Dependencies

- Inject clocks, IDs, date providers, randomness, and boundary clients explicitly when they affect behavior.
- Prefer deterministic seams that are easy to use in tests and safe across concurrency domains.
- Keep shared mutable dependencies scarce and well-isolated.

## Migration Heuristics

- When concurrency warnings appear, first ask whether the type wants to become a value, become isolated, or shrink in responsibility.
- Prefer redesigning the ownership boundary over piling on annotations.
- If a type is both hard to make sendable and hard to test, it likely has too many jobs.
- Favor small local fixes that improve the design, not broad suppression.

## Warning Signs

- `Task {}` used to escape ownership questions.
- frequent actor hopping without a clear isolation reason
- shared mutable singleton-style dependencies
- callback-era patterns kept around in new code without need
- `nonisolated` or `@unchecked Sendable` used mainly to quiet the compiler
- concurrency annotations spread across a design that still has muddy ownership
