---
name: swift-development
description: "Use this skill when the task is Swift engineering for Apple apps, frameworks, packages, or shared modules: architecture, domain logic, package and module design, public APIs, view models, services, concurrency, testing, refactoring, boundaries, and clean code structure."
---

# Swift Development

This is the canonical Swift development skill package for Codex/OpenAI-style agents.

Always load:

- `./core/prompt.md`

Then load only the references that match the task:

If the task involves architecture tradeoffs, feature shape, module layering, view-model/service boundaries, or dependency flow:

- `./core/references/decision-rules.md`
- `./core/references/architecture.md`

If the task is about file shape, decomposition, refactoring order, or cleanup of existing code:

- `./core/references/code-organization.md`
- `./core/references/refactoring-playbook.md`
- `./core/references/anti-patterns.md`

If async work, task ownership, cancellation, or ordering behavior matters:

- `./core/references/concurrency.md`

If the task changes state models, value semantics, validation, or error surfaces:

- `./core/references/state-and-domain-modeling.md`
- `./core/references/error-handling.md`

If the task changes APIs, dependency seams, public contracts, or module boundaries:

- `./core/references/api-design.md`
- `./core/references/dependencies-and-boundaries.md`
- `./core/references/module-boundaries.md`
- `./core/references/package-design.md`

If the task adds, updates, or reviews tests:

- `./core/references/test-strategy.md`

If you are writing or reviewing test code after the layer is clear:

- `./core/references/testing-playbook.md`
- `./core/references/test-doubles.md`

If the task is about package or module shape, shared library structure, or public framework surfaces:

- `./core/references/package-design.md`
- `./core/references/concurrency.md`

If the task is specifically about networking:

- `./core/references/networking-recipes.md`

If the task is specifically about persistence:

- `./core/references/persistence-recipes.md`

If the task is about Swift 6 migration or newer concurrency rules:

- `./core/references/swift-6-readiness.md`

If logging, telemetry, or observability matter:

- `./core/references/observability-and-logging.md`

If performance is the problem:

- `./core/references/performance.md`

If you are reviewing code rather than implementing it:

- `./core/references/review-rubric.md`

If the user explicitly asks for concrete code examples:

- `./core/references/example-implementations.md`

If you are blocked or need authoritative follow-up sources for a tricky Swift issue:

- `./core/references/trusted-resources.md`

Do not auto-load architecture docs, examples, package/networking/persistence/migration docs, performance notes, review rubrics, or trusted resources unless the task clearly needs them.

Additional OpenAI/Codex-specific behavior:

- Produce code, not just direction, unless the user explicitly asks for strategy only.
- Prefer architecture sized to the surface: lean MVVM for app-facing UI, narrow package and module APIs for shared code, modern Swift concurrency, and strongly readable code structure.
- Do not transplant SwiftUI-specific MVVM rules into package, framework, or shared-module code.
- For package, framework, and shared-module work, treat API evolution, access control, and sendability as first-class design constraints.
- Prefer smaller focused files and types when clarity improves.
- Use protocols deliberately for real seams such as DI, testing, previews, or controlled behavior.
- When reviewing code, present findings first and prioritize correctness, clarity, testability, and maintainability before style comments.
- Use file and line references when the environment supports them.
- Keep close-out summaries brief and verification-oriented so the developer can quickly check the change themselves.
