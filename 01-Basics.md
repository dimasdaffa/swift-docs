# 01. Swift Basics

Fundamental syntax and concepts that differentiate Swift from other languages like PHP.

---

## 1. Variables & Constants

Swift emphasizes safety. By default, you should use constants (`let`) unless you know the value will change.

| Keyword | Type                  | Description                      |
| ------- | --------------------- | -------------------------------- |
| `let`   | Constant (Immutable)  | Cannot be changed once assigned  |
| `var`   | Variable (Mutable)    | Can be changed                   |

```swift
let pi = 3.14159
var age = 20

// age = 21   // ✅ Allowed
// pi = 3.14  // ❌ Error: Cannot assign to value: 'pi' is a 'let' constant
```

---

## 2. Data Types & Type Inference

Swift is strongly typed. However, it can infer the type based on the value.

```swift
let name = "Dimas"              // Inferred as String
let count = 10                  // Inferred as Int
let explicitDouble: Double = 70.0  // Explicit type annotation
```

---

## 3. Optionals (`?` and `!`)

Swift variables cannot be `null` (`nil`) by default. To allow `nil`, you must declare the variable as **Optional** by adding `?`.

### Unwrapping Optionals

You must "unwrap" an optional to access its value safely.

### A. If-Let (Safe Unwrap)

```swift
var username: String? = nil

if let validName = username {
    print("Hello, \(validName)")
} else {
    print("Username is empty")
}
```

### B. Guard-Let (Early Exit)

Commonly used in functions to exit early if data is missing.

```swift
func login(user: String?) {
    guard let user = user else {
        print("No user found!")
        return
    }
    print("Welcome, \(user)")
}
```

### C. Nil-Coalescing (`??`)

Provide a default value if the optional is `nil`.

```swift
let finalName = username ?? "Guest"  // If username is nil, use "Guest"
```

---

## 4. Control Flow

Switch statements are powerful in Swift and must be **exhaustive** (cover all cases).

```swift
let role = "admin"

switch role {
case "admin":
    print("Full Access")
case "editor":
    print("Edit Access")
default:
    print("Read Only")
}
```