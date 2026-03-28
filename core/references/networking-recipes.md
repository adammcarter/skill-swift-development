# Networking Recipes

Use this reference for request shaping, decoding, auth boundaries, retries, and transport-to-domain mapping in Swift app and framework code.

## Core Bias

- Keep networking at the edge.
- Prefer small focused clients over giant networking gateways.
- Decode and map near the transport boundary.
- Expose domain or feature-facing models upward rather than raw transport shapes when that improves clarity.

## Clients vs Services

- Clients should own request construction, transport execution, decoding, and low-level transport concerns.
- Services should own feature-facing workflows, domain mapping, validation, retries when product-driven, and error translation when those decisions belong above transport.
- Do not make view models or domain models reason about HTTP, headers, decoding, or status-code trivia.
- If a client is returning transport models everywhere, consider whether the mapping belongs closer to the boundary.

## Request Shaping

- Build requests close to the client boundary.
- Prefer explicit request models when the payload has meaning beyond one call site.
- Keep auth headers, query items, and body encoding out of presentation code.
- Prefer one clear client method per meaningful capability over giant “request builder” APIs.

## Decoding And Mapping

- Decode transport payloads at the client boundary.
- Map transport responses into domain or feature-facing models before the data spreads upward when that improves clarity.
- Keep transport-only fields out of the rest of the system unless they genuinely matter to domain behavior.
- Prefer small explicit mapping steps over clever generic decoding abstractions.

## Auth And Session Boundaries

- Keep token/session concerns in a dedicated auth-aware boundary rather than leaking them through feature code.
- Prefer clients or shared transport infrastructure to handle auth headers and refresh behavior.
- Do not make every feature manually assemble auth concerns.
- If auth refresh or credential loading is complex, isolate it behind a focused dependency.

## Retries And Timeouts

- Keep retries intentional and product-aware.
- Retry only when the operation is safe and the product behavior is clear.
- Prefer explicit retry policy over silent automatic retries hidden deep in the stack.
- Keep timeout behavior visible where it affects product expectations.
- Do not mix retry policy, domain validation, and UI state changes in the same layer.

## Error Translation

- Keep raw transport failures near the network boundary.
- Map status codes, decoding failures, connectivity failures, and auth problems into clearer feature or domain-facing errors when the upper layers should not care about transport trivia.
- Preserve enough context for diagnostics without leaking infrastructure detail into user-facing state.
- Prefer one clear mapping boundary over each layer partially reinterpreting the same failure.

## Pagination, Caching, And Freshness

- Make pagination behavior explicit in the client or service API.
- Keep caching and freshness policy deliberate rather than implicit.
- Prefer modeling “cached vs fresh vs stale” behavior explicitly when the product experience depends on it.
- Do not let ad hoc cache checks spread through unrelated layers.

## Testing Networking Code

- Test request shaping, decoding, error mapping, retry policy, and pagination behavior where they live.
- Prefer stubs or fakes at the transport seam over heavyweight end-to-end harnesses for unit tests.
- Do not force view model tests to prove low-level HTTP behavior that belongs in client or service tests.

## Warning Signs

- View models building URLs, headers, or request bodies.
- Services exposing raw status-code logic to the rest of the app.
- Giant network clients with unrelated endpoints and policies mixed together.
- Silent retry behavior that surprises callers.
- Transport models becoming the de facto domain model because mapping was skipped.
