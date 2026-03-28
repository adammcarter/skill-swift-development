# Example Recipe: Decomposition

Use this when the task explicitly needs a before/after example of decomposing an oversized type.

## Before

```swift
import Foundation

@MainActor
@Observable
final class AccountViewModel {
    private let client: AccountClient
    private let validator: AccountValidator
    private let analytics: AnalyticsClient

    private(set) var state: State = .idle
    private(set) var displayName = ""
    private(set) var emailAddress = ""
    private(set) var validationMessage: String?

    init(
        client: AccountClient,
        validator: AccountValidator,
        analytics: AnalyticsClient
    ) {
        self.client = client
        self.validator = validator
        self.analytics = analytics
    }

    func loadAccount() async {
        state = .loading

        do {
            let response = try await client.fetchAccount()
            displayName = response.displayName
            emailAddress = response.emailAddress
            validationMessage = nil
            state = .loaded
        } catch {
            state = .failed("Could not load account")
        }
    }

    func updateDisplayName(_ value: String) {
        displayName = value
        validationMessage = validator.validate(
            displayName: displayName,
            emailAddress: emailAddress
        )
    }

    func updateEmailAddress(_ value: String) {
        emailAddress = value
        validationMessage = validator.validate(
            displayName: displayName,
            emailAddress: emailAddress
        )
    }

    func save() async {
        if let validationMessage {
            state = .failed(validationMessage)
            return
        }

        state = .saving

        do {
            let request = UpdateAccountRequest(
                displayName: displayName,
                emailAddress: emailAddress
            )
            try await client.updateAccount(request)
            analytics.track("account_saved")
            state = .loaded
        } catch {
            state = .failed("Could not save account")
        }
    }

    enum State: Equatable {
        case idle
        case loading
        case loaded
        case saving
        case failed(String)
    }
}
```

## After

```swift
import Foundation

@MainActor
@Observable
final class AccountViewModel {
    private let service: AccountService

    private(set) var state: State = .idle

    init(service: AccountService) {
        self.service = service
    }

    func loadAccount() async {
        state = .loading

        do {
            let account = try await service.loadAccount()
            state = .loaded(AccountForm(account: account))
        } catch let error as AccountError {
            state = .failed(error.localizedDescription)
        } catch {
            state = .failed("Could not load account")
        }
    }

    func updateDisplayName(_ value: String) {
        guard case .loaded(var form) = state else { return }
        form.displayName = value
        state = .loaded(form)
    }

    func updateEmailAddress(_ value: String) {
        guard case .loaded(var form) = state else { return }
        form.emailAddress = value
        state = .loaded(form)
    }

    func save() async {
        guard case .loaded(let form) = state else { return }

        do {
            let account = try await service.save(form)
            state = .loaded(AccountForm(account: account))
        } catch let error as AccountError {
            state = .failed(error.localizedDescription)
        } catch {
            state = .failed("Could not save account")
        }
    }

    enum State: Equatable {
        case idle
        case loading
        case loaded(AccountForm)
        case failed(String)
    }
}
```

Supporting focused types:
- `AccountService`
- `AccountForm`
- `AccountError`
- `AccountValidator` when the rules are substantial

Why it works:
- reduces mutable field sprawl
- prefers explicit state over many separate properties
- splits work by responsibility
