# Swift Development 🧩

Write Apple-platform Swift that is easier to reason about, test, and evolve.

This skill helps agents shape features, modules, services, view models, APIs, concurrency boundaries, and tests without drifting into unnecessary architecture.

## What it can do

- Untangle messy feature code into clearer types, files, modules, and dependency seams.
- Shape services, view models, package APIs, and shared modules so they are easier to test and evolve.
- Refactor toward modern concurrency, cleaner error handling, and stronger tests without turning a simple problem into a framework.

## Solves problems like

- Code works, but the structure is messy, tightly coupled, or hard to extend.
- A feature needs cleaner boundaries between views, view models, services, packages, or shared modules.
- A refactor needs modern concurrency, better error handling, and focused tests without over-engineering the solution.

## Good when you want to

- Use it to work out where logic should live instead of stuffing everything into one view model, service, or helper.
- Use it to clean up long files, hard-to-read types, unclear state flow, and basic dependency tangles.
- Use it to review module boundaries, concurrency ownership, and test seams before a refactor grows into a bigger problem.

## Example prompts

- `Use $swift-development to explain where this validation logic should live and refactor it into a cleaner structure.`
- `Use $swift-development to refactor this feature into clearer service, state, and dependency boundaries.`
- `Use $swift-development to design a small Swift package API for this shared module and keep the public surface tight.`
- `Use $swift-development to review this async workflow for cancellation, task ownership, and error propagation.`
- `Use $swift-development to clean up this view model and add focused tests without introducing unnecessary protocols.`

## Skill Judge

This skill scored `120/120` using the separate `skill-judge` skill.

<https://github.com/softaworks/agent-toolkit/blob/main/skills/skill-judge/SKILL.md>

## Install

With skills.sh:

```bash
npx skills add adammcarter/skill-swift-development
```

For a local Codex install:

```bash
ln -s ~/repos/skill-swift-development ~/.codex/skills/swift-development
```
