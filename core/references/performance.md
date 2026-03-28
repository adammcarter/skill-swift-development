# Swift Performance Notes

Use this reference for practical performance decisions in app and framework Swift code.

## Core Rule

Optimize where cost is real, but do not turn performance into superstition.

## Practical Rules

- Prefer simple data flow first, then measure hotspots.
- Avoid unnecessary copying in clearly hot paths, but do not prematurely abandon value semantics everywhere.
- Keep allocation-heavy or repeated transformation work visible in profiling-sensitive areas.
- Be deliberate with async fan-out, caching, and expensive formatting or parsing.

## Warning Signs

- Clever low-level optimization hiding basic design problems.
- Premature complexity added “for performance.”
- Repeated heavy work inside presentation or transformation loops without reason.
