# Dependencies And Boundaries

Use this reference for service boundaries, dependency injection, and integration seams.

## Default Bias

- Prefer initializer injection.
- Use light environment or container patterns when they help composition without hiding ownership.
- Use protocols when there is a real seam: substitution, testing, preview behavior, or controlled runtime behavior.

## Services And Clients

- Services/clients should have clear ownership and narrow purpose.
- Keep network, persistence, analytics, or system boundaries separate unless combining them materially simplifies the design.
- Prefer clear client surfaces over giant omnivorous gateways.

## Protocol Use

- Add a protocol when a real seam exists.
- Do not create protocol mirrors for every concrete type by default.
- Keep protocol requirements small and behavior-focused.
- Prefer clean capability names such as `AccountClient` or `PreferencesStore`, with concrete `Live*` and `Mock*` implementations where useful.
- Prefer protocol requirements that model the boundary in domain terms rather than transport details.
- If the seam is internal and single-use, a closure or small concrete dependency can be better than introducing a whole protocol.

## Functional Boundaries

- Keep impure work at the edge: networking, persistence, clocks, random values, platform APIs.
- Keep the middle of the system biased toward deterministic transforms, mapping, and explicit state transitions.
- Prefer injecting boundary dependencies so the core logic can stay mostly value-oriented and easy to test.
- Do not force every workflow into a functional pipeline if straightforward imperative orchestration is clearer.

## Warning Signs

- DI containers required to understand simple construction.
- Protocols used only to satisfy a style rule.
- Boundary abstractions so generic that they erase domain meaning.
- Boundary types that expose transport or storage details everywhere.
