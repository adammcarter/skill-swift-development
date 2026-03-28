# Review Rubric

Use this reference when reviewing Swift logic, architecture, and shared-module design.

## Review Output Shape

- Findings first.
- Lead with correctness, contract, ownership, or boundary issues that could break behavior or force churn.
- For package and framework code, call out public API and access-control issues before stylistic concerns.
- If no issues stand out, say which axes were checked so the review still feels deliberate.

## Immediate Failure Signals

- Public API exposing transport, storage, or temporary implementation shapes.
- Concurrency with unclear task ownership, actor isolation, or mutation safety.
- Architecture that is heavier than the problem or so thin that boundaries are muddy.
- View models, services, or package entry points swallowing multiple unrelated responsibilities.
- Tests proving internal trivia while risky behavior remains under-covered.

## Order Of Evaluation

1. Correctness
2. Architecture sized to context
3. State modeling
4. Boundaries, public API, and access control
5. Concurrency and ownership
6. Testability and tests
7. Maintainability and organization
8. Performance

## Correctness

- Are observable outcomes correct?
- Are error paths explicit enough for the risk of the change?
- Are public-facing contracts and return values clear?

## Architecture Sized To Context

- For app-facing features, is MVVM still lean rather than accidental sprawl?
- For package, framework, or shared-module code, is the type graph smaller than the problem and free of app-specific policy?
- Did the change add abstraction for a real boundary, or just to make the code look architected?

## State Modeling

- Is state explicit and coherent?
- Are identifiers, errors, and domain concepts modeled strongly enough?
- Are transport or persistence details leaking upward?

## Boundaries, Public API, And Access Control

- Are services, clients, and boundaries easy to identify?
- Is the public API smaller and more stable than the internal implementation?
- Are protocols introduced for a real reason, especially when they become public?
- Is access control narrow by default?

## Concurrency And Ownership

- Is task ownership explicit?
- Are cancellation, ordering, and actor isolation handled where relevant?
- Are async APIs and mutable state safe under realistic use?

## Testability And Tests

- Are files and types appropriately focused?
- Are tests covering behavior and seams rather than trivia?
- Are tests deep enough for the risky logic, but still cheap to maintain?

## Maintainability And Organization

- Are files and types appropriately focused?
- Is the module or package boundary making ownership clearer rather than just more fragmented?
- Would this be straightforward to maintain six months from now?

## Performance

- Are there obvious hot paths, copy costs, or needless work in the changed code?
- Did the change introduce extra abstraction or synchronization cost without clear benefit?

## Escalate When

- The boundary or public API smell is systemic rather than local.
- Ownership or concurrency rules are hard to explain in one sentence.
- The test story suggests the production structure is obscuring the real behavior.
