# Persistence Recipes

Use this reference for storage boundaries, model mapping, migrations, caches, and persistence workflows in Swift app and framework code.

## Core Bias

- Keep persistence at the edge.
- Prefer explicit storage boundaries over letting database or file formats leak through the system.
- Map storage models into cleaner domain or feature-facing models when that improves clarity.
- Keep persistence policy deliberate: caching, freshness, migrations, and write behavior should not be accidental.

## Stores, Repositories, And Simple Persistence Clients

- Prefer the simplest persistence boundary that fits the job.
- Use a focused store or client when the boundary is straightforward.
- Introduce a repository-style layer only when it creates a clearer domain-facing abstraction rather than just renaming storage calls.
- Do not add repository ceremony for a tiny persistence seam that is already clear.

## Model Mapping

- Keep storage entities and persistence-specific models near the persistence boundary.
- Map them into domain or feature-facing models before the data spreads upward when that improves clarity.
- Do not let schema convenience dictate every internal type.
- Prefer explicit mapping over magically shared persistence/domain models unless the shapes are truly the same and likely to stay that way.

## Writes, Reads, And Transactions

- Keep write behavior explicit: create, update, delete, merge, conflict handling, and overwrite rules should be understandable.
- Prefer transaction boundaries that are easy to explain.
- Keep persistence coordination near the boundary rather than mixing it into view models or domain types.
- If multiple writes must succeed together, model that intentionally.

## Migrations

- Treat migrations as a first-class concern once stored data has a real lifespan.
- Keep migration steps explicit and reviewable.
- Prefer compatibility strategies that preserve user data and operational confidence.
- Do not bury migration assumptions in unrelated startup or feature code.

## Caching And Freshness

- Make cache ownership clear.
- Prefer explicit policies for fresh, stale, missing, and invalidated data.
- Keep cache invalidation decisions close to the persistence or data boundary.
- Do not let cache checks scatter through presentation logic.

## Offline And Failure Behavior

- Make offline, missing-data, and stale-data behavior explicit when the product experience depends on it.
- Map persistence failures into feature or domain-facing errors when upper layers should not care about storage mechanics.
- Avoid silent fallback behavior unless the product explicitly wants it.
- Prefer one clear policy for “no data,” “stale data,” and “failed to save” rather than ad hoc special cases.

## Testing Persistence Code

- Test mapping, migrations, cache policy, freshness rules, and conflict behavior where they live.
- Prefer in-memory fakes or lightweight test stores when they preserve confidence without heavy harnesses.
- Do not force higher layers to prove low-level persistence mechanics that belong in boundary tests.

## Warning Signs

- View models reading or writing storage primitives directly.
- Storage entities reused everywhere as if they were domain models.
- Repository layers that mostly forward persistence calls without owning policy.
- Cache and freshness checks duplicated across multiple features.
- Migration logic hidden in startup glue with no clear boundary or tests.
