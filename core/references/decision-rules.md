# Decision Rules

Use this reference when multiple Swift designs are plausible and you need a sharp default.

## Core Bias

- Prefer the simplest design that keeps ownership, mutation, and boundaries obvious.
- Scale the amount of architecture and polish to the project’s size, risk, team needs, and expected lifespan.
- Prefer value types over reference types where possible.
- Prefer concrete types over protocols until a real seam appears.
- Prefer explicit state and straightforward control flow over clever abstraction.
- Add structure only when it reduces real complexity in the code being maintained.

## Pragmatism And Scale

- Small tools, prototypes, and low-risk internal code do not need the same layering or extensibility as large long-lived production systems.
- Higher-risk, longer-lived, or shared code deserves stronger boundaries, more explicit state modeling, and more deliberate testing.
- Infer the appropriate level of rigor from the surrounding codebase and the task rather than applying maximum ceremony every time.
- If the scale or risk is genuinely unclear, ask during planning rather than assuming a small-project or big-production posture.
- Prefer the lightest solution that is still honest about the actual complexity.
- Do not under-engineer code that is clearly central, long-lived, or expensive to get wrong.

## `struct` vs `class` vs `actor`

- Prefer `struct` by default for domain models, state, requests, responses, options, configuration, and other value-like data.
- Prefer `struct` by default for service-like types too when they are immutable wrappers over injected dependencies and reference identity is not part of the design.
- Prefer `class` only when identity, observation, object lifetime, or genuinely shared reference semantics are intrinsic to the problem.
- Prefer `actor` sparingly, only when truly shared mutable state must be protected across concurrency domains and actor isolation materially simplifies correctness.
- Do not use `class` by reflex for every service or model.
- Do not use `actor` just because code is async; most async workflows do not need actor isolation.

Choose `struct` when:
- the type represents data, configuration, requests, responses, or explicit state
- the service is mostly immutable and just orchestrates injected collaborators
- value semantics make tests, comparison, snapshots, and transformations easier

Choose `class` when:
- the type participates in observation or object identity
- ownership and lifecycle are meaningful to the behavior
- shared reference semantics are part of the model rather than an implementation accident

Choose `actor` when:
- multiple tasks can touch the same mutable state
- correctness depends on serialized access
- the isolation boundary is easy to explain at the call site
- the state would otherwise require careful manual synchronization

## Concrete Type vs Protocol vs Closure

- Prefer a concrete type first.
- Introduce a protocol only when a real seam appears.
- Use a closure for a tiny one-off seam when a full protocol would be ceremonial.
- Prefer small capability protocols over broad umbrella protocols.
- Keep protocol names clean and domain-focused.
- Avoid protocol mirrors and abstraction-first design.

Use a protocol when:
- multiple implementations are plausible or already exist
- the seam makes testing simpler and clearer
- the capability deserves a stable boundary

Use a concrete type when:
- abstraction would only mirror the implementation
- the dependency is local to one feature and easy to instantiate directly
- introducing a protocol would make navigation and naming worse
- the concrete type already expresses the design clearly

Use a closure when:
- the dependency is a single behavior
- the call site stays obvious
- a protocol would be ceremonial
- the seam is local and unlikely to grow into a stable capability boundary

## Value Transformation vs Mutation

- Prefer returning transformed values by default in domain and business logic.
- Prefer local mutation mainly at orchestration edges such as view models, tasks, and boundary adapters.
- Keep side effects at boundaries and keep transformation logic as pure as practical.
- Prefer explicit data flow over state being mutated across many steps.
- Avoid mutation-heavy flows that smear business rules across many steps when a small value transformation would be clearer.
- Avoid “functional” cleverness that obscures ordinary business logic.

## Nested Type vs Top-Level Type

- Prefer a nested type when it is tightly scoped to one owner and unlikely to be reused independently.
- Prefer a nested `ViewModel` inside a SwiftUI view for view-owned presentation logic.
- Prefer a top-level type when the type has broader reuse, broad semantic meaning, or enough complexity to deserve standalone navigation.
- Do not nest important domain models so deeply that they become hard to discover.

## Single Type vs Split Types

- Keep a type whole while it still tells one coherent story.
- Split aggressively once a type stops scanning cleanly.
- Split once a type has multiple reasons to change, mixed abstraction levels, or a long tail of helpers.
- Separate presentation orchestration, boundary integration, domain modeling, and validation when the split improves scanability and testability.
- Prefer `View + ViewModel + Services/Clients` as the default lean MVVM shape for app-facing features.

Split sooner when:
- the file no longer scans cleanly in review
- tests are hard to write without reconstructing the whole feature
- private helpers outnumber the public story
- one type owns networking, persistence, mapping, validation, and presentation state together

Keep it together when:
- the extracted type would be tiny and single-use without clarifying the design
- the workflow is simple enough that jumping files would hurt more than help

## Inline Conformance vs Extension

- Prefer inline conformances when the conformance is trivial or the main declaration still reads cleanly.
- Prefer an extension when the conformance has enough implementation to earn its own section.
- Do not scatter a type across many trivial extensions for appearance alone.

## Sync vs `async` vs `AsyncSequence`

- Prefer synchronous APIs for inherently synchronous work.
- Prefer plain `async` for one-shot operations that may suspend.
- Prefer `AsyncSequence` only when the caller genuinely consumes multiple values over time.
- Do not hide async work behind synchronous-looking APIs.
- Do not introduce `AsyncSequence` for simple request/response workflows.
- Do not “reactive-ify” ordinary async work without a real streaming need.

Choose `AsyncSequence` when:
- values arrive incrementally
- cancellation and backpressure semantics matter
- modeling the stream directly makes the API easier to use

Choose `async` when:
- the caller wants a single eventual answer
- streaming would complicate the API without product value

## New Layer vs Keep It Simple

- Add a new layer only when it creates a clearer ownership boundary, improves testability, or removes real complexity from an overloaded type.
- Do not add use cases, coordinators, repositories, managers, or adapters by default just to satisfy an architecture template.
- If the new layer mostly forwards calls, resist it and keep the simpler design.
- If the new layer becomes the cleanest home for business rules, error mapping, or boundary translation, add it.
- For a small feature or short-lived tool, prefer a simpler shape unless the extra structure is already paying for itself.

## Fast Heuristics

- Default to `struct`.
- Escalate to `class` for identity or observation.
- Escalate to `actor` for shared mutable concurrent state.
- Default to concrete types.
- Escalate to a protocol for a real seam.
- Use closures for tiny seams.
- Default to top-level domain types and nested view-owned helper types.
- Default to returning values for pure logic and using mutation at orchestration edges.
