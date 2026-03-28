# State And Domain Modeling

Use this reference for domain models, view-model state, and explicit behavior modeling.

## Core Rule

Prefer explicit models over informal conventions spread across optionals, strings, and loosely related flags.

## Domain Models

- Prefer value types where practical.
- Use strong types for meaningful identifiers, units, and domain distinctions when they improve correctness.
- Keep invariants close to the model.
- Avoid turning domain models into transport dumps with UI formatting baked in.

## View-Model State

- Model loading, loaded, empty, error, editing, and selection states explicitly when they matter.
- Prefer enums or tightly related structs over bags of booleans.
- Derived display state should stay derived where practical rather than duplicated and drift-prone.

## Mutation

- Make state transitions legible.
- Avoid mutation patterns that require the reader to infer hidden coupling.
- Prefer small, named mutation points over giant “update everything” methods.

## Warning Signs

- Multiple booleans that can represent contradictory states.
- Raw dictionaries or unstructured payloads leaking through business logic.
- One model type serving unrelated layers without clear justification.
