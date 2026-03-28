# Refactoring Playbook

Use this reference when improving an existing Swift codebase that is messy, oversized, over-coupled, or drifting away from the preferred architecture.

## Core Bias

- Prefer small, behavior-preserving refactors that move the design toward clearer ownership.
- Refactor in an order that keeps the code working and reviewable.
- Split types aggressively once they stop scanning cleanly, but do not introduce forwarding-only architecture.
- Prefer ending with clearer state, boundaries, and responsibilities rather than just more files.
- Prefer the smallest refactor that fixes the real design problem instead of layering workaround code on top of existing confusion.

## Refactoring Priorities

1. Make behavior visible with tests where needed.
2. Clarify state and ownership.
3. Separate boundary work from domain and presentation logic.
4. Split oversized types by responsibility.
5. Tighten APIs, names, and access control.

## First Questions To Ask

- What is this type responsible for today?
- Which parts are domain logic, boundary integration, presentation orchestration, or formatting?
- Where is mutable state living?
- Which seams are real, and which abstractions are cargo culted?
- What is hardest to test or reason about right now?

## Safe Order Of Operations

- Start by identifying the public behavior that must remain intact.
- Add or strengthen focused tests around risky behavior before changing structure when needed.
- Extract explicit state or value models before extracting more layers.
- Move boundary translation and side effects out of view models and god types early.
- Extract focused services, clients, validators, or models only when they become the clearest home for the logic.
- Tighten names and access control after the responsibilities are clearer.

## Splitting A God Type

When a type has too many jobs:

- identify the core public story
- identify helper clusters with different reasons to change
- extract boundary work first: networking, persistence, analytics, platform APIs
- extract domain state and transformation logic next
- keep presentation orchestration or workflow coordination in the remaining owner type
- stop once the file scans cleanly and the responsibilities are obvious

Do not:
- split every helper into its own file just because it exists
- add a service, repository, and use case all at once if one focused extraction would do
- leave the original type as a forwarding shell with no real responsibility

## Refactoring Toward Lean MVVM

- Keep the view focused on presentation.
- Move view-owned state coordination into a nested `ViewModel` when using SwiftUI.
- Move boundary work into services/clients.
- Keep domain models and value transforms independent where practical.
- If validation or mapping becomes substantial, extract it into a focused type.

## Extracting Seams

- Prefer extracting a concrete type first.
- Introduce a protocol only if a real seam emerges for substitution, testing, previews, or controlled runtime behavior.
- Prefer a closure seam for tiny one-off behavior.
- If an extracted seam only forwards calls, it probably is not the right seam yet.

## State-First Refactors

- Replace boolean soup with explicit enums or focused value models.
- Collapse related mutable fields into a single coherent state model where practical.
- Prefer value transformations over mutation spread across many methods.
- If async work mutates state, make task ownership and state transitions explicit.
- Remove compensating flags, duplicate derived properties, and stale workaround branches once the real source of truth is clarified.

## Boundary Refactors

- Move transport, persistence, and platform details to the edge.
- Map low-level errors into feature-facing errors at the boundary.
- Keep domain and presentation layers free from transport-shaped models where practical.
- Prefer simple structured concurrency over nested task creation while refactoring async code.

## When To Stop

- Stop once the type tells a coherent story in review.
- Stop once responsibilities are obvious and the remaining duplication is cheaper than more abstraction.
- Stop before the refactor turns into architecture theater.

## Warning Signs

- A refactor that mostly creates new names without clarifying ownership.
- New layers that forward calls but own no logic.
- Incremental “fixes” that add more flags, modifiers, branches, or helper types without reducing confusion.
- Tests that become harder to read after the refactor.
- More files, but no clearer boundaries.
- A “cleanup” pass that changes behavior unintentionally because the public story was never pinned down.
