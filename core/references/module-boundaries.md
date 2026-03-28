# Module Boundaries

Use this reference when deciding what should stay inside a feature or target, what should move into a shared module, and how to keep Swift modules cohesive.

## Core Bias

- Prefer cohesive feature-local code first.
- Extract a module only when it creates a clearer ownership boundary or enables genuinely reusable code.
- Prefer duplication over premature shared abstractions when the shared shape is still unstable.
- Keep modules aligned to meaningful responsibilities, not vague “utility” buckets.

## What Should Stay Local

- Feature-specific view models, views, feature state, and feature workflows.
- Types that are tightly coupled to one product surface or one bounded domain area.
- Small helpers whose only consumer is the current feature.
- Code that is still evolving quickly and has not yet revealed a stable shared shape.

## What Should Move To A Shared Module

- Stable domain models or workflows used by multiple features.
- Boundary clients or services that represent a shared system capability.
- Reusable parsing, formatting, persistence, or networking infrastructure with clear ownership.
- Shared testing utilities only when they remove real duplication without obscuring test intent.

Move code only when:
- multiple consumers already exist or are clearly imminent
- the shared API has a stable domain meaning
- the extracted module will be easier to own than duplicated local variants

## App Target vs Framework Or Package

- Keep app-specific composition, feature wiring, and UI-owned orchestration in the app target.
- Prefer a framework or Swift package for stable shared logic that should be reusable across features, apps, or layers.
- Do not move code into a package just to look modular.
- If a package mostly exists to hide messy boundaries, fix the boundaries first.

## Cohesion Rules

- A module should have a clear reason to exist that can be described in one sentence.
- Prefer modules with strong internal relatedness over giant shared buckets.
- Keep public API narrow and intentional.
- Prefer internal implementation details staying internal rather than leaking helpers across the boundary.

## Dependency Direction

- Prefer dependencies pointing inward toward stable domain or infrastructure modules, not sideways between peer features.
- Avoid circular feature dependencies.
- Prefer a feature depending on shared capabilities, not on another feature’s internal types.
- If two features need the same concept, extract the concept rather than letting one feature become the other’s library.

## Shared Code Heuristics

- Extract after the second or third real usage when the shared shape is becoming clear.
- Keep feature-specific policy out of shared infrastructure modules.
- Prefer small shared modules with crisp ownership over mega-packages containing unrelated helpers.
- Be suspicious of names like `Common`, `Utils`, `Shared`, or `Core` unless the responsibility is still concrete and explainable.

## Testing Implications

- Shared modules should be easy to test in isolation.
- Do not force tests in one feature target to reach through another feature’s internals.
- Prefer test helpers that stay close to the module they support unless they are truly cross-cutting.

## Warning Signs

- A shared module accumulating unrelated convenience helpers.
- Feature modules depending directly on each other’s internals.
- Extracted modules that mostly re-export or forward APIs.
- “Reusable” code that only has one real consumer.
- Public APIs widened mainly to make the module boundary work around a design smell.
