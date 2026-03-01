# 04. Data Structures & Algorithms in Swift

Implementing standard DSA concepts using Swift's Type System.

---

## 1. Standard Collections

- **Array**: Ordered list. `[1, 2, 3]`
- **Dictionary**: Key-Value pairs. `["key": "value"]`
- **Set**: Unordered collection of unique values.

```swift
var dict: [String: String] = ["id": "123", "role": "admin"]
dict["name"] = "Dimas" // Add new key
print(dict)
// Output: ["id": "123", "role": "admin", "name": "Dimas"]
```

---

## 2. Generics `<T>`

Think of Generics as creating a **"fill-in-the-blank" template** for your code — a universal adapter that adjusts to whatever data type you plug into it.

### The Problem: Code Duplication

Without generics, you'd write separate functions for each type:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) { ... }
func swapTwoStrings(_ a: inout String, _ b: inout String) { ... }
func swapTwoDoubles(_ a: inout Double, _ b: inout Double) { ... }
```

### The Solution: Generic Function

Use a placeholder type `T` instead of a specific type:

```swift
func swapTwo<T>(_ a: inout T, _ b: inout T) {
    let tmp = a
    a = b
    b = tmp
}

var x = 5, y = 10
swapTwo(&x, &y)
print(x, y)  // Output: 10 5

var a = "Hello", b = "World"
swapTwo(&a, &b)
print(a, b)  // Output: World Hello
```

- **`<T>`** — Declares a placeholder type called `T`
- **`(_ a: inout T, _ b: inout T)`** — Both parameters must be the same type `T`

> Swift replaces `T` with the actual type (`Int`, `String`, etc.) at compile time.

### Generic Constraints

Sometimes "any type" is too broad. Use constraints to set rules:

```swift
func minValue<T: Comparable>(_ a: T, _ b: T) -> T { 
    return a < b ? a : b 
}

print(minValue(3, 7))      // Output: 3
print(minValue("b", "a"))  // Output: a
```

- **`<T: Comparable>`** — Only accepts types that support comparison operators (`<`, `>`)

Without the constraint, `a < b` would fail because Swift doesn't know if `T` can be compared.

### Summary

| Syntax | Meaning |
| ------ | ------- |
| `<T>` | Accept any type |
| `<T: Comparable>` | Accept any type that can be compared |
| `<T: Equatable>` | Accept any type that can be checked for equality |

---

## 3. Building a Stack (LIFO)

Using a Struct and Generics to create a Stack data structure.

```swift
struct Stack<T> {
    private var elements: [T] = []
    
    // Push: Add to top
    mutating func push(_ element: T) {
        elements.append(element)
    }
    
    // Pop: Remove from top
    mutating func pop() -> T? {
        return elements.popLast()
    }
    
    // Peek: Look at top
    func peek() -> T? {
        return elements.last
    }
}

// Usage
var stack = Stack<String>()
stack.push("Hello")
stack.push("World")
print(stack.pop()) // "World"
```