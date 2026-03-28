# Example Implementation Recipes

Use these only when the task explicitly needs a concrete example shape. Load the smallest matching recipe rather than the whole set.

## Quick Picks

- Need a public package or framework surface? Start with `package-apis.md`.
- Need SwiftUI-facing MVVM shape? Start with `swiftui-view-models.md`.
- Need a concrete boundary type for services or clients? Start with `services-and-clients.md`.
- Need a focused `swift-testing` example? Start with `testing.md`.
- Need explicit state modeling or decomposition? Start with `state-modeling.md` or `decomposition.md`.

- `./example-recipes/file-layout.md`
  - preferred file and member ordering
- `./example-recipes/swiftui-view-models.md`
  - nested SwiftUI `ViewModel` shape
  - environment-based injection at the SwiftUI boundary
- `./example-recipes/services-and-clients.md`
  - service/client boundaries
  - concrete client boundaries with local closure seams
- `./example-recipes/package-apis.md`
  - narrow public package/framework API
  - internal helpers and seams staying internal
- `./example-recipes/testing.md`
  - focused `swift-testing` examples
- `./example-recipes/typed-errors.md`
  - typed error mapping from boundary failures
- `./example-recipes/state-modeling.md`
  - explicit state models instead of boolean soup
- `./example-recipes/feature-layout.md`
  - feature folder layout
- `./example-recipes/decomposition.md`
  - before/after decomposition of an oversized type

If none of these match, do not auto-load examples just to "have more context."
