# Example Recipe: Package APIs

Use this when the task explicitly needs a concrete example of a narrow public package or framework API with internal implementation details kept internal.

`SearchIndex.swift`

```swift
import Foundation

public struct SearchDocument: Sendable, Equatable {
    public let id: UUID
    public let title: String
    public let body: String

    public init(id: UUID, title: String, body: String) {
        self.id = id
        self.title = title
        self.body = body
    }
}

public struct SearchQuery: Sendable, Hashable {
    public let text: String

    public init(_ text: String) {
        self.text = text.trimmingCharacters(in: .whitespacesAndNewlines)
    }
}

public struct SearchMatch: Sendable, Equatable {
    public let id: UUID
    public let title: String
    public let snippet: String
}

public struct SearchIndex {
    private let storage: any SearchIndexStorage

    public init(url: URL) throws {
        self.storage = try SQLiteSearchIndex(url: url)
    }

    public func replaceContents(with documents: [SearchDocument]) async throws {
        try await storage.replaceContents(with: documents)
    }

    public func search(_ query: SearchQuery, limit: Int = 20) async throws -> [SearchMatch] {
        try await storage.search(query, limit: limit)
    }
}
```

`SearchIndexStorage.swift`

```swift
import Foundation

protocol SearchIndexStorage {
    func replaceContents(with documents: [SearchDocument]) async throws
    func search(_ query: SearchQuery, limit: Int) async throws -> [SearchMatch]
}

struct SQLiteSearchIndex: SearchIndexStorage {
    init(url: URL) throws {
        // Open the backing store and validate schema here.
    }

    func replaceContents(with documents: [SearchDocument]) async throws {
        // Persist or rebuild the index.
    }

    func search(_ query: SearchQuery, limit: Int) async throws -> [SearchMatch] {
        []
    }
}
```

Why it works:
- keeps the public surface small, domain-named, and easy for consumers to understand
- hides storage and implementation details behind internal helpers rather than exporting them
- exposes real product capabilities instead of mirroring the internal layer layout
- broadens the skill beyond app-facing MVVM by showing a shared package/framework shape
