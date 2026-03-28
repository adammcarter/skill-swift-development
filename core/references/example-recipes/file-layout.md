# Example Recipe: Single-Type File Layout

Use this when the task explicitly needs a concrete example of the preferred file and member order.

```swift
import Foundation

final class PreferencesService: Sendable {
    let logger: Logger

    let clock: any Clock<Duration>

    private let store: PreferencesStore
    private let decoder: JSONDecoder

    init(
        logger: Logger,
        clock: any Clock<Duration>,
        store: PreferencesStore,
        decoder: JSONDecoder = JSONDecoder()
    ) {
        self.logger = logger
        self.clock = clock
        self.store = store
        self.decoder = decoder
    }

    func loadPreferences(for userID: User.ID) async throws -> UserPreferences {
        let data = try await store.loadPreferences(for: userID)
        return try decodePreferences(from: data)
    }

    func savePreferences(_ preferences: UserPreferences, for userID: User.ID) async throws {
        let data = try JSONEncoder().encode(preferences)
        try await store.savePreferences(data, for: userID)
        logger.log("Saved preferences for \(userID)")
    }

    func resetPreferences(for userID: User.ID) async throws {
        try await store.deletePreferences(for: userID)
    }

    func decodePreferences(from data: Data) throws -> UserPreferences {
        try decoder.decode(UserPreferences.self, from: data)
    }

    private func makeDefaultPreferences() -> UserPreferences {
        UserPreferences()
    }

    enum Error: Swift.Error {
        case invalidData
    }
}
```

Why it works:
- follows the preferred member order
- keeps conformances inline when they are lightweight
- uses no comments unless they add real value
