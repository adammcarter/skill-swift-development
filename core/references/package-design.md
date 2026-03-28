# Package Design

Use this reference for Swift packages and frameworks that expose shared logic, domain models, infrastructure, or public APIs.

## Core Bias

- Prefer a narrow, intentional public surface.
- Keep implementation details internal by default.
- Design packages around cohesive responsibilities, not convenience dumping grounds.
- Expose stable concepts, not the incidental shape of today’s implementation.

## Public API Defaults

- Start with the smallest public surface that supports real consumers.
- Prefer `internal` or package-scoped visibility by default, then widen deliberately.
- Make public types and members earn their visibility.
- Prefer public entry points that are easy to understand without reading the internals.

## What Should Be Public

- Stable domain models or capabilities that external consumers genuinely need.
- Construction APIs or factories that express the supported way to use the package.
- Error types, configuration values, and protocols only when they are part of the intended contract.
- Extension points only when outside consumers truly need to customize behavior.

## What Should Stay Internal

- Helper types, glue code, mapping details, and temporary refactoring shapes.
- Transport models or persistence details that are not part of the package contract.
- Debug-only conveniences and test-only seams.
- Types widened only because one internal file wanted easier access.

## API Shape

- Prefer small, explicit public APIs over generic “power user” surfaces.
- Prefer value types and strong domain names where they improve clarity and resilience.
- Prefer construction that makes ownership obvious.
- Keep side effects visible in API naming and signatures.
- Avoid leaking internal layering decisions through the public surface.

## Protocols In Public APIs

- Expose a protocol publicly only when it is a real capability boundary or extension point for consumers.
- Do not publish protocols just because the implementation uses protocol-oriented seams internally.
- Prefer concrete public entry points unless outside substitution is part of the product design.
- Keep public protocol requirements small, domain-named, and stable.

## Resilience And Evolution

- Be conservative with what becomes public because public APIs are expensive to change.
- Prefer additive evolution over breaking changes.
- Avoid exposing stored-shape details that may need to change later.
- If an API may evolve quickly, keep it internal until the shape stabilizes.

## Evolution Checks

- Prefer adding new entry points or configuration over changing the meaning of existing ones.
- Be careful with public enums and stored properties that may need non-breaking growth later.
- Prefer domain-named configuration structs over long parameter lists that are hard to evolve.
- Review whether a new public symbol is solving a real consumer problem or only making current internals easier to reach.

## Access Control Heuristics

- Default to the narrowest access that keeps the design honest.
- Widen only for real consumers, not for convenience during implementation.
- If many things need to become public to make the package work, the boundary is probably wrong.
- Keep internal helper seams internal even when public types depend on them.

## Package Cohesion

- A package should have a clear one-sentence purpose.
- Prefer multiple focused packages over one giant package with unrelated concerns.
- Keep app-specific composition and policy out of broadly shared packages.
- If a package contains unrelated UI, networking, parsing, persistence, and test helpers together, the split is probably wrong.

## Warning Signs

- `public` spreading through the codebase as a convenience tool.
- Public APIs mirroring internal folder structure rather than domain intent.
- Consumer code depending on transport, storage, or implementation details.
- A package that cannot evolve without breaking users because too much was exposed.
- “Core” packages that are really just leftovers from everything else.
