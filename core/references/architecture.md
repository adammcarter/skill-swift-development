# Swift Architecture

Use this reference for default structure decisions in Apple app and framework Swift code.

## Default Shape

- Prefer lean MVVM for app-facing code: `View + ViewModel + Services/Clients`.
- Keep domain and integration code separated from presentation orchestration.
- Let services and clients own boundary work such as networking, persistence, system integration, or external dependencies.
- Keep view models responsible for presentation-facing state and coordination, not for every concern in the feature.

## What Lean MVVM Means Here

- A view model prepares state for presentation and coordinates calls to services/clients.
- Services/clients own interaction with backends, disk, platform APIs, or domain workflows.
- Domain models stay clean and independent where practical.
- Do not add use-case/interactor/coordinator layers by default unless the problem genuinely needs them.
- For SwiftUI-facing MVVM, prefer a nested `ViewModel` on the view type, usually placed in a separate `View+ViewModel.swift` file.
- For SwiftUI-facing MVVM, prefer `@MainActor` plus modern observation for the view model.
- For SwiftUI-facing code, prefer environment-based injection for the view model and other UI-facing dependencies rather than pushing them through the view initializer.

## Good Defaults

- Keep boundaries explicit in type signatures.
- Inject collaborators through initializers by default below the UI boundary, with light environment or container patterns where they improve composition.
- Prefer smaller focused types over giant “manager” or “controller” classes.
- Keep shared abstractions scarce and justified.
- Prefer protocol-oriented seams and small functional transformations where they improve composability and clarity without adding ceremony.

## Pragmatic Scaling

- Match the architecture to the project rather than forcing every codebase into the same production-grade template.
- Small apps, experiments, utilities, and early-stage features usually want simpler boundaries and less ceremony.
- Shared frameworks, long-lived product code, and high-risk workflows usually justify stronger boundaries, more explicit modeling, and more testing.
- The right answer is not “maximum architecture”; it is enough structure to keep the code honest, understandable, and changeable in context.

## Protocol-Oriented Programming Guidance

- Prefer protocols at boundaries, not everywhere.
- Reach for a protocol when you need substitution, test seams, preview behavior, controlled runtime composition, or to publish a stable capability surface.
- Prefer one focused protocol per capability over broad “god protocols.”
- Keep protocol requirements behavior-oriented and small enough that a mock or alternate implementation stays obvious.
- Prefer concrete types within a feature when there is only one real implementation and no meaningful seam yet.
- If a protocol would only mirror one concrete type with no substitution value, keep the concrete type.

## Functional Style Guidance

- Prefer pure transformations for mapping, validation, formatting, parsing, and state derivation.
- Prefer value-semantic models that can be copied, transformed, and compared cheaply in tests and reviews.
- Keep side effects at boundaries and keep transformation steps explicit in the middle of the workflow.
- Favor small composable helpers when they make a pipeline easier to scan.
- Avoid point-free cleverness, operator-heavy APIs, or pipeline abstractions that make ordinary Swift harder to read.
- Choose the clearest expression for the team; a simple `switch` or local helper is often better than a “functional” trick.

## When To Deviate

- Add a coordinator/router only when navigation complexity is real, recurring, and easier to reason about outside the view model.
- Add more architectural layers only when they reduce actual complexity rather than perform architecture theater.
- Keep framework or SDK integration separate when it would otherwise contaminate clean domain or view-model logic.

## Warning Signs

- View models that know too much about networking, persistence, analytics, and formatting all at once.
- “Service” types that are really bags of unrelated helpers.
- Protocols for nearly every type with no real substitution need.
- “Functional” code that hides simple branching or state updates behind dense chains.
- Files that require a lot of scrolling before their purpose is obvious.
- Top-level SwiftUI view models used everywhere by habit when a nested view-owned model would be clearer.
