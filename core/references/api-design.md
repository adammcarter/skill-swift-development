# API Design

Use this reference for Swift-facing APIs in app modules, frameworks, and shared code.

## Core Rule

Prefer APIs that read clearly at the call site and expose intent without excess ceremony.

## Practical Rules

- Optimize names for use, not just declaration.
- Prefer small, explicit APIs over giant parameter-heavy entry points.
- Use labels intentionally.
- Return strong types where they improve correctness and understanding.
- Prefer defaults when they reduce friction without hiding important behavior.
- Prefer parameters on one line until there are three or more, or until the parameters become genuinely awkward to read.
- Prefer concrete types first. Introduce a protocol only when a real long-lived substitution seam appears.
- For small feature-local seams, prefer closures or lightweight stubs/fakes over `Live*` and `Mock*` scaffolding.
- If a protocol is justified, keep the name clean and keep test-only doubles in the test target when practical.
- Prefer functions that transform values clearly over APIs that hide work in mutation-heavy side effects.
- Prefer returning derived values when that reads better than mutating an inout-shaped bag of state.
- Keep higher-order helpers small and domain-named; if a closure-heavy API takes effort to decode, simplify it.

## Async And Effects

- Make async and throwing behavior obvious in the signature.
- Keep side effects visible in API shape and naming.
- Avoid APIs that pretend to be synchronous while hiding asynchronous or expensive work.
- Prefer separating pure decision logic from effectful boundary work when doing so makes testing and reasoning cheaper.

## Access Levels

- Keep public API narrow and deliberate.
- Hide internal machinery unless it needs to be shared.

## Warning Signs

- Generic signatures that are harder to understand than the problem.
- APIs with lots of boolean parameters.
- Closure-driven APIs that are more abstract than the actual use case.
- Naming that reflects implementation details rather than domain intent.
- Factory helpers that hide too much construction detail or create magical global behavior.
