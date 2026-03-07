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

### Deinitializers (`deinit`)

Because classes are managed by ARC, you can use a `deinit` block to run cleanup code exactly when an object's reference count drops to zero, right before it is destroyed.

*Note: `deinit` is only available for Classes, not Structs.*

```swift
class FileHandle {
    var filename: String
    
    init(filename: String) { 
        self.filename = filename
        print("Opened \(filename)") 
    }
    
    // Automatically called when the object is deallocated
    deinit { 
        print("Closed \(filename) and freed resources") 
    }
}

var file: FileHandle? = FileHandle(filename: "document.txt") // "Opened document.txt"
file = nil // "Closed document.txt and freed resources"
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

### Failable and Throwing Initializers

Use these when object creation might fail due to invalid data.

**Failable (`init?`)**: Returns `nil` if setup fails.

```swift
struct Email {
    let value: String
    init?(_ s: String) { 
        if s.contains("@") { value = s } else { return nil } 
    }
}
```

**Throwing (`init throws`)**: Throws a specific error if setup fails, giving more context than `nil`.

```swift
enum InitError: Error { case invalid }

struct Port {
    let number: Int
    init(_ n: Int) throws { 
        guard (1...65535).contains(n) else { throw InitError.invalid }
        number = n 
    }
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