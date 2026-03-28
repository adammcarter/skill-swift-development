# Concurrency

Use this reference for async workflows, actor boundaries, task ownership, and cancellation.

## Default Bias

- Prefer structured concurrency and `async`/`await`.
- Prefer async-first APIs over callback wrappers for new code.
- Keep task ownership explicit.
- Prefer simple awaited workflows over unstructured `Task {}` creation.

## Practical Rules

- Make async boundaries visible in the API.
- Keep cancellation behavior intentional where the user experience or resource cost depends on it.
- Avoid detached work unless there is a clear ownership reason.
- Use `Task {}` only when unstructured work is genuinely needed.
- Prefer actors or other isolation tools when mutable shared state would otherwise become fragile.
- Keep async orchestration readable; do not hide every await behind generic wrappers.
- Keep actor hopping minimal unless the code genuinely requires crossing isolation domains.

## In View Models

- Treat loading, retry, success, failure, and cancellation as part of the state model.
- Avoid multiple overlapping tasks mutating the same state without a clear policy.
- If one task supersedes another, encode that behavior deliberately.
- Prefer moving off the main actor at the UI/model seam, then letting model-layer async work stay simple.

## In Services

- Keep transport details, retries, and decoding concerns in services/clients rather than leaking them upward.
- Do not force callers to understand internal concurrency gymnastics unless the behavior is part of the contract.
- Prefer simple structured concurrency in model and service layers with minimal isolation hopping.

## Warning Signs

- Boolean flags scattered around async flows instead of explicit state.
- Hidden background work with unclear lifetime.
- Fire-and-forget tasks that mutate shared state later.
- Wrapping straightforward async code in unnecessary abstraction.
