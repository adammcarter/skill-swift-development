# Testing Playbook

Use this reference after `test-strategy.md` has established the right test layer. Use `test-doubles.md` when you need help choosing collaborators or seams.

## Core Goal

Prefer focused, readable tests that prove behavior without pinning the code to implementation trivia.

## Test Shape

- Prefer modern async tests when code is async.
- Prefer backticked, succinct English sentence test names.
- Keep arrange, act, and assert easy to spot.
- Keep setup local and intention-revealing.
- Do not add `@Suite` or helper layers unless they materially reduce noise.

## Coverage Depth

- Test deeply enough that a refactor can preserve behavior with confidence.
- Go deeper where logic branches, state changes, retries, mapping, validation, or failure handling carry real product risk.
- Stay shallower where code is mostly wiring, trivial pass-through, or a thin wrapper already exercised through a higher-value test.
- Prefer a small number of high-signal tests that cover each meaningful branch over exhaustive combinatorial testing.
- If you feel pressure to test many tiny internals, the production type may want decomposition more than additional test code.

## Assertions

- Assert externally visible state, outputs, or emitted effects.
- Prefer one meaningful behavior per test when practical.
- Use interaction assertions only when the interaction is itself the contract.
- Avoid pinning tests to incidental method order or private helper structure.

## What To Cover

- State transitions.
- Success and failure behavior.
- Boundary interactions where correctness matters.
- Mapping and transformation logic.
- Cancellation, retry, or ordering behavior in async flows when relevant.

## Fixtures And Helpers

- Keep fixtures generic and close to the test.
- Extract builders or helpers only when they make multiple tests easier to scan.
- Prefer small deterministic fixtures over large shared worlds.
- Do not leak real personal or project-specific data into test examples.

## Edge Cases Worth Covering

- Empty or minimal input when it affects logic.
- Boundary values and invalid forms when validation matters.
- Cancellation, duplicate triggers, or stale-task races in async flows when those can happen in production.
- Error mapping cases that deliberately collapse low-level failures into feature-facing errors.

## Warning Signs

- Heavy test harnesses for simple seams.
- Tests coupled to internal method order.
- Huge fixture setup for small behaviors.
- Snapshotting implementation details instead of asserting meaningful state or outputs.
