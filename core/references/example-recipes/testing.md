# Example Recipe: Focused Testing

Use this when the task explicitly needs a concrete example of focused `swift-testing` coverage around a view model or service.

```swift
import Foundation

struct Profile: Equatable {
    let displayName: String
}

struct ProfileClient {
    var fetchProfile: () async throws -> Profile
    var updateDisplayName: (String) async throws -> Profile
}

enum ProfileError: Error, Equatable, LocalizedError {
    case notFound

    var errorDescription: String? {
        switch self {
        case .notFound:
            "Profile could not be found."
        }
    }
}

struct ProfileView {
    @MainActor
    @Observable
    final class ViewModel {
        private let client: ProfileClient

        private(set) var state: State = .idle

        init(client: ProfileClient) {
            self.client = client
        }

        func loadIfNeeded() async {
            guard case .idle = state else { return }
            await load()
        }

        private func load() async {
            state = .loading

            do {
                let profile = try await client.fetchProfile()
                state = .loaded(profile)
            } catch let error as ProfileError {
                state = .failed(error.localizedDescription)
            } catch {
                state = .failed("Something went wrong.")
            }
        }

        enum State: Equatable {
            case idle
            case loading
            case loaded(Profile)
            case failed(String)
        }
    }
}

struct ProfileService {
    private let client: ProfileClient

    init(client: ProfileClient) {
        self.client = client
    }

    func loadProfile() async throws -> Profile {
        try await client.fetchProfile()
    }
}
```

`ProfileViewModelTests.swift`

```swift
import Foundation
import Testing

struct ProfileViewModelTests {
    @Test
    @MainActor
    func `loadIfNeeded sets loaded state when client returns profile`() async {
        let profile = Profile(displayName: "Taylor")
        let client = ProfileClient(
            fetchProfile: { profile },
            updateDisplayName: { _ in profile }
        )
        let viewModel = ProfileView.ViewModel(client: client)

        await viewModel.loadIfNeeded()

        #expect(viewModel.state == .loaded(profile))
    }

    @Test
    @MainActor
    func `loadIfNeeded sets failed state when client throws profile error`() async {
        let client = ProfileClient(
            fetchProfile: { throw ProfileError.notFound },
            updateDisplayName: { _ in fatalError("Unexpected call") }
        )
        let viewModel = ProfileView.ViewModel(client: client)

        await viewModel.loadIfNeeded()

        #expect(viewModel.state == .failed(ProfileError.notFound.localizedDescription))
    }

    @Test
    func `loadProfile throws when client throws`() async {
        let client = ProfileClient(
            fetchProfile: { throw ProfileError.notFound },
            updateDisplayName: { _ in fatalError("Unexpected call") }
        )
        let service = ProfileService(client: client)

        await #expect(throws: ProfileError.notFound) {
            try await service.loadProfile()
        }
    }
}
```

Why it works:
- uses modern `swift-testing`
- keeps names as short English sentences in backticks
- focuses on behavior and state
- uses a concrete boundary type with local closure seams instead of defaulting to protocol-plus-mock scaffolding
- keeps the example self-contained and aligned with the service and view-model recipes
