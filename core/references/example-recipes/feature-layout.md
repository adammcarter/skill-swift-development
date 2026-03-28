# Example Recipe: Feature Layout

Use this when the task explicitly needs a concrete example of feature-oriented folder structure.

```text
Sources/
  Features/
    Account/
      Models/
        AccountForm.swift
        AccountError.swift

      Services/
        AccountService.swift
        AccountClient.swift
        AccountValidator.swift

      Views/
        AccountView.swift
        AccountView+ViewModel.swift

Tests/
  Features/
    Account/
      Support/
        AccountClientStub.swift
      AccountViewModelTests.swift
      AccountServiceTests.swift
```

Why it works:
- groups related code by feature
- separates `Views`, `Models`, and `Services`
- keeps test-only support in the test target instead of teaching production `Mock*` defaults
- keeps tests in the test module rather than mixing them into source folders
