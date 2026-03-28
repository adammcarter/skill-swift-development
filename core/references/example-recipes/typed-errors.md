# Example Recipe: Typed Errors

Use this when the task explicitly needs a concrete example of mapping boundary failures into feature-facing errors.

`AccountError.swift`

```swift
import Foundation

enum AccountError: Error, Equatable, Sendable {
    case validationFailed(String)
    case notFound
    case networkUnavailable
    case saveFailed
    case unknown
}

extension AccountError: LocalizedError {
    var errorDescription: String? {
        switch self {
        case .validationFailed(let message):
            message

        case .notFound:
            "Account could not be found."

        case .networkUnavailable:
            "You appear to be offline."

        case .saveFailed:
            "Could not save account."

        case .unknown:
            "Something went wrong."
        }
    }
}
```

```swift
import Foundation

struct Account {}

struct AccountForm {
    let isValid: Bool
}

struct AccountClient {
    var fetchAccount: () async throws -> Account
    var updateAccount: (AccountForm) async throws -> Account
}

enum NetworkError: Error {
    case notFound
    case notConnectedToInternet
    case unknown
}
```

`AccountService.swift`

```swift
import Foundation

struct AccountService {
    private let client: AccountClient

    init(client: AccountClient) {
        self.client = client
    }

    func loadAccount() async throws -> Account {
        do {
            return try await client.fetchAccount()
        } catch let error as NetworkError {
            throw map(error)
        } catch {
            throw AccountError.unknown
        }
    }

    func save(_ form: AccountForm) async throws -> Account {
        guard form.isValid else {
            throw AccountError.validationFailed("Please correct the highlighted fields.")
        }

        do {
            return try await client.updateAccount(form)
        } catch let error as NetworkError {
            throw map(error)
        } catch {
            throw AccountError.saveFailed
        }
    }

    private func map(_ error: NetworkError) -> AccountError {
        switch error {
        case .notFound:
            .notFound

        case .notConnectedToInternet:
            .networkUnavailable

        default:
            .unknown
        }
    }
}
```

Why it works:
- maps boundary errors into feature-facing errors
- uses `LocalizedError` for user-facing descriptions
- keeps the boundary concrete until a real substitution seam is needed
- avoids leaking raw transport failures upward
