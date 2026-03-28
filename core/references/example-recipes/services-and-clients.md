# Example Recipe: Services And Clients

Use this when the task explicitly needs a concrete example of a service/client boundary with a concrete client and lightweight seams.

```swift
import Foundation

struct Profile: Equatable {
    let displayName: String
}
```

`ProfileService.swift`

```swift
import Foundation

struct ProfileService {
    private let client: ProfileClient

    init(client: ProfileClient) {
        self.client = client
    }

    func loadProfile() async throws -> Profile {
        try await client.fetchProfile()
    }

    func updateDisplayName(_ displayName: String) async throws -> Profile {
        try await client.updateDisplayName(displayName)
    }
}
```

`ProfileClient.swift`

```swift
import Foundation

struct ProfileClient {
    var fetchProfile: () async throws -> Profile

    var updateDisplayName: (String) async throws -> Profile
}
```

Why it works:
- keeps the boundary explicit without protocol ceremony
- uses lightweight closure seams for small feature-local substitution
- keeps the API shape consistent across the service, view-model, and testing recipes
- keeps test-only behavior local to the test rather than teaching a production `Mock*` default
