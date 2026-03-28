# Test Doubles

Use this reference when choosing between real collaborators, fakes, stubs, mocks, spies, or closure-based seams in Swift tests after the test layer is already clear.

## Core Bias

- Prefer the simplest test double that preserves confidence.
- Prefer real collaborators when they are fast, deterministic, and easy to construct.
- Prefer lightweight fakes and stubs over expectation-heavy mocks by default.
- Use interaction testing only when the interaction is itself the behavior that matters.
- If a test needs a lot of infrastructure to express a simple behavior, simplify the seam or decompose the production type.

## Default Order

1. Real collaborator
2. Simple fake
3. Stub
4. Closure-based seam
5. Mock or spy

Move down this list only when the simpler option stops giving enough confidence.

## Real Collaborators

- Prefer a real collaborator when the dependency is pure, cheap, deterministic, and local.
- Real collaborators are often best for parsers, formatters, small validators, mappers, and value-semantic helpers.
- Do not replace a simple deterministic dependency with a mock just because “everything must be mocked.”

Use a real collaborator when:
- setup is trivial
- runtime cost is negligible
- deterministic behavior is easy to preserve
- the real implementation is clearer than the fake

## Fakes

- Prefer an in-memory fake when the production collaborator is a boundary like persistence, networking, caching, or a repository-like store.
- A fake should behave like a small honest implementation, not like a bag of test-only switches.
- Keep fakes simple enough that they are easier to trust than a pile of mock expectations.

Use a fake when:
- tests benefit from realistic stateful behavior
- the seam is used across many tests
- a fake makes scenarios easier to read than configuring many closures

Warning signs:
- the fake is becoming a second production system
- the fake has more knobs than the real API

## Stubs

- Prefer a stub when the dependency only needs to return canned values or throw canned errors.
- Stubs are a good default for view model tests and many service tests.
- Keep stub configuration close to the test so the behavior is obvious.

Use a stub when:
- the collaborator only needs to answer a request
- the test cares about state or output more than interaction history
- a full fake would add noise

## Closure-Based Seams

- Prefer closures for tiny single-operation seams in tests when a full fake or protocol type would be overkill.
- Closure-based seams work especially well for one-off success/failure behavior in a feature-local test.
- Keep the closure surface small and obvious; if it starts growing, move to a named type.

Use a closure when:
- one operation is all the test needs
- behavior differs per test in a very local way
- creating a dedicated type would make the test harder to scan

## Mocks And Spies

- Use mocks or spies when the interaction itself is part of the contract.
- Examples include verifying analytics emission, ensuring a callback is triggered once, or proving a retry/cancellation policy.
- Keep interaction assertions focused on meaningful behavior, not incidental call order trivia.
- Prefer a lightweight spy that records calls over a heavyweight framework-style mock.

Use a mock or spy when:
- the number of calls matters
- the sequence of calls matters to correctness
- the payload of an emitted effect is part of the behavior

Avoid mocks when:
- the test really cares about returned state
- interaction assertions are only standing in for easier state assertions
- the mock requires more setup than the feature logic under test

## By Type

- View models: prefer stubs, closure seams, and lightweight spies for emitted effects.
- Services: prefer stubs for boundary responses, fakes for reusable stateful storage seams, and spies only for meaningful side effects.
- Domain models: prefer real collaborators or direct value testing.
- Thin adapters: use a stub or tiny spy only when you need to prove the seam is wired correctly.

## Interaction Testing

- Interaction testing is justified when behavior is defined partly by what gets called, with what payload, or how often.
- Interaction testing is overkill when a resulting state change or output already proves the behavior.
- Prefer asserting one meaningful interaction over reproducing the whole internal execution trace.

## Warning Signs

- Tests that read like mock configuration scripts.
- Multiple expectations asserting internal call order without product value.
- Fakes with sprawling configuration APIs.
- Every dependency mocked by default, including tiny deterministic helpers.
- A test double design that is harder to understand than the production seam.
