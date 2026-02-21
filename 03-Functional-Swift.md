# 03. Functional Swift

Swift supports functional programming paradigms, making code concise and expressive using Closures and Higher Order Functions.

---

## 1. Closures

Self-contained blocks of functionality (similar to Lambda or Anonymous Functions in PHP/JS).

```swift
let sayHello = { (name: String) -> String in
    return "Hello, \(name)!"
}

print(sayHello("Dimas"))
```

---

## 2. Higher Order Functions

Functions that operate on collections (Arrays).

### Map

Transforms every element in an array.

```swift
let numbers = [1, 2, 3, 4]
let squared = numbers.map { $0 * $0 }
// Result: [1, 4, 9, 16]
```

### Filter

Returns elements that match a condition.

```swift
let evenNumbers = numbers.filter { $0 % 2 == 0 }
// Result: [2, 4]
```

### Reduce

Combines all elements into a single value.

```swift
let sum = numbers.reduce(0, +)
// Result: 10 (0 + 1 + 2 + 3 + 4)
```

### CompactMap

Transforms elements and removes `nil` values automatically.

```swift
let strings = ["1", "2", "apple", "4"]
let validNumbers = strings.compactMap { Int($0) }
// Result: [1, 2, 4] (Removes "apple" because Int("apple") is nil)
```