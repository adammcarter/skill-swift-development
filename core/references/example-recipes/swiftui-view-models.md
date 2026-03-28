# Example Recipe: SwiftUI View Models

Use this when the task explicitly needs a concrete example of the preferred SwiftUI-facing `ViewModel` shape.

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
```

`ProfileView.swift`

```swift
import SwiftUI

struct ProfileView: View {
    @Environment(ViewModel.self) private var viewModel

    var body: some View {
        content
            .task {
                await viewModel.loadIfNeeded()
            }
    }

    @ViewBuilder
    private var content: some View {
        switch viewModel.state {
        case .idle,
             .loading:
            ProgressView()

        case .loaded(let profile):
            ProfileContent(profile: profile)

        case .failed(let message):
            ContentUnavailableView(
                "Couldn’t Load Profile",
                systemImage: "person.crop.circle.badge.exclamationmark",
                description: Text(message)
            )
        }
    }
}
```

`ProfileView+ViewModel.swift`

```swift
import Foundation

extension ProfileView {
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

        func reload() async {
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
```

Why it works:
- keeps the SwiftUI-facing view model nested on the view type
- places the nested `ViewModel` in a separate `View+ViewModel.swift` file
- prefers environment-based injection at the SwiftUI boundary
- keeps dependency wiring out of the view initializer
