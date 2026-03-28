# Example Recipe: State Modeling

Use this when the task explicitly needs a concrete example of explicit state modeling instead of boolean soup.

```swift
import Foundation

enum SyncState: Equatable, Sendable {
    case idle
    case syncing
    case synced(lastUpdatedAt: Date)
    case failed(SyncError)

    var isTerminal: Bool {
        switch self {
        case .idle,
             .syncing:
            false

        case .synced,
             .failed:
            true
        }
    }
}

enum SyncError: Error, Equatable, Sendable {
    case networkUnavailable
    case conflict
    case unknown
}
```

Why it works:
- uses explicit states instead of boolean soup
- keeps the model value-semantic
- attaches lightweight derived behavior to the state model
