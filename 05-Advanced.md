# 05. Advanced Swift

Deep dive into memory management, concurrency, and error handling.

---

## 1. Memory Management (ARC)

Swift uses Automatic Reference Counting. You don't manage memory manually, but you must avoid **Retain Cycles**.

- **Strong Reference**: Default. Keeps object alive.
- **Weak Reference**: Reference does not keep object alive. Becomes `nil` when the object is deallocated. Used in parent-child relationships (delegates).

```swift
class Person {
    var name: String
    init(name: String) { self.name = name }
    deinit { print("\(name) is being deinitialized") }
}

var reference1: Person? = Person(name: "John")
reference1 = nil // Output: "John is being deinitialized"
```

---

## 2. Error Handling

Swift uses `do-try-catch` blocks.

```swift
enum APIError: Error {
    case networkFail
    case invalidData
}

func fetchData() throws {
    throw APIError.networkFail
}

do {
    try fetchData()
} catch APIError.networkFail {
    print("Network Error!")
} catch {
    print("Unknown error: \(error)")
}
```

---

## 3. Concurrency (Async/Await)

The modern way to handle asynchronous code (replaces Callbacks/Completion Handlers).

```swift
func getUser() async throws -> String {
    // Simulate network delay
    try await Task.sleep(nanoseconds: 1 * 1_000_000_000)
    return "Dimas"
}

Task {
    do {
        let user = try await getUser()
        print(user)
    } catch {
        print("Failed")
    }
}
```