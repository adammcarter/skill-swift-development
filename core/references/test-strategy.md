# Test Strategy

Use this reference to decide what kind of tests a Swift feature or module actually needs: unit, boundary integration, end-to-end, UI, or snapshot coverage. Use `testing-playbook.md` once the layer is chosen and `test-doubles.md` when picking seams.

## Core Bias

- Prefer a small, deliberate test pyramid rather than trying to prove everything at one layer.
- Put most confidence in focused unit tests close to the logic.
- Add boundary integration tests where transport, persistence, or framework seams carry risk.
- Use UI and snapshot tests selectively for workflows or regressions that lower layers cannot prove clearly.

## Default Shape

- Unit tests should carry most of the coverage burden for domain models, view models, services, mapping, validation, and state transitions.
- Boundary integration tests should cover networking, persistence, serialization, migrations, or framework seams when the seam itself is risky.
- End-to-end or UI tests should be sparse and focused on the highest-value user journeys.
- Snapshot tests belong where visual correctness is the actual behavior under test.

## Unit Tests

- Prefer unit tests for logic that can be expressed without bringing up the full stack.
- Unit tests should cover behavior, state transitions, mapping, validation, and error translation.
- If a feature can be made reliable with unit tests alone, prefer that over heavier layers.

## Boundary Integration Tests

- Add boundary integration tests when the contract between layers is itself risky or easy to break.
- Good candidates include request encoding, decoding, persistence mapping, migrations, cache policy, and framework integrations.
- Keep them narrower than full end-to-end tests and closer to the seam they are protecting.

## End-To-End And UI Tests

- Use UI tests when the behavior depends on real app flow, navigation, multi-screen interactions, system integration, or issues that are only visible in a running UI.
- Keep UI tests focused on the highest-value workflows rather than trying to cover every branch.
- Prefer a few stable end-to-end tests over a huge flaky suite.

## Snapshot Tests

- Use snapshot tests when visual output, hierarchy, or presentation state is the behavior that matters.
- Keep snapshot guidance mostly in the frontend skill, but recognize that a Swift feature may still depend on visual state verification when it owns presentation logic.
- Do not use snapshots to avoid writing cheaper logic-level tests.

## Choosing The Layer

- If a pure logic test can prove it, prefer a unit test.
- If the boundary contract is the risk, add a boundary integration test.
- If the real user flow is the risk, add a UI or end-to-end test.
- If the visual result is the risk, add snapshot coverage in the UI layer.
- If you are reaching for a heavy test because the production structure is muddy, simplify the production structure first when practical.

## Warning Signs

- Most confidence coming from slow end-to-end tests instead of cheaper lower layers.
- UI tests covering logic that could have been unit-tested.
- Snapshot tests replacing state or behavior tests.
- Integration tests that are effectively full-system tests with brittle setup.
- Large suites with unclear purpose at each layer.
