# Persistence And Networking

Use this reference for data boundaries and integration code.

## Core Rule

Keep transport and persistence concerns at the edge. Do not smear them across view models and domain logic.

## Practical Rules

- Keep decoding/encoding, request construction, persistence adapters, and mapping concerns close to the boundary.
- Prefer explicit mapping from transport/storage models into cleaner domain or feature-facing models when it improves clarity.
- Avoid letting backend or database shapes dictate all internal models by default.
- Make caching and freshness policies explicit.

## Error And Retry Behavior

- Keep retry decisions intentional and visible.
- Make offline or stale-data behavior explicit where it matters.

## Warning Signs

- View models decoding JSON.
- Domain logic reaching directly into storage primitives.
- Transport models reused everywhere because it feels convenient in the short term.
