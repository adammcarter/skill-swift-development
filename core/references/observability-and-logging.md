# Observability And Logging

Use this reference for logging, diagnostics, metrics, and operational visibility in app and framework Swift code.

## Core Bias

- Prefer purposeful observability over noisy logging.
- Keep diagnostics for engineers separate from user-facing error handling.
- Log at meaningful boundaries and state changes, not every line of control flow.
- Prefer privacy-safe structured information over raw payload dumping.

## Logging Boundaries

- Log boundary events where they help explain behavior: requests starting, requests failing, persistence failures, retries, migrations, and important state transitions.
- Prefer logging near service, client, persistence, and integration boundaries rather than scattering logs through every helper.
- Keep view models and UI-facing types light on logging unless the state transition itself is operationally important.
- If a log exists only to understand development-time control flow, it may belong in debugging, not in the shipped design.

## User Errors vs Diagnostics

- User-facing errors should explain what the user can understand or do next.
- Logs should capture operational context useful to developers and support.
- Do not stuff low-level diagnostics into user-visible error text.
- Do not rely on logs as the only way to understand expected product failures; important failures should still be modeled explicitly.

## Structured Logging

- Prefer structured logging APIs and stable event names where available.
- Log identifiers, states, counts, durations, and high-signal metadata rather than giant string blobs.
- Keep event shapes consistent so diagnostics are easier to search and compare.
- Prefer domain language in logs over transport trivia when possible.

## Privacy And Safety

- Do not log secrets, tokens, auth headers, raw personal data, or entire payloads by default.
- Be deliberate with identifiers and user data; log the minimum needed to diagnose the issue.
- Prefer redaction or omission over “just this once” verbose payload logging.
- Treat analytics and logging as separate concerns even if they use similar data.

## Metrics And Signposts

- Prefer counters, timings, and signposts when you need to measure behavior over time.
- Use metrics for repeated operational questions like latency, failure rate, cache hit rate, retry rate, or migration success.
- Use signposts or equivalent tooling for performance-sensitive workflows and expensive paths.
- Do not replace every log with a metric; use the tool that best answers the question.

## Error Reporting

- Report errors at the boundary where enough context exists to make the report useful.
- Prefer one well-contextualized report over the same error being reported repeatedly by each layer.
- Keep low-level failures mapped into feature-facing errors while retaining enough diagnostic context for logs or reporting tools.
- Avoid double-reporting the same failure unless the product and operational needs are genuinely different.

## Debugging vs Design

- Remove temporary trace logging once the design is understood.
- Do not leave permanent log spam in hot paths or common success flows.
- If code needs many logs to be understandable, clarify the structure instead.

## Warning Signs

- Logs spread through every helper method.
- Success-path spam with little diagnostic value.
- Raw payload dumps used as a debugging strategy.
- User-facing copy polluted by transport or infrastructure details.
- Multiple layers logging or reporting the same failure independently.
