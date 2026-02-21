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
```

---

## 2. Generics `<T>`

Generics allow you to write flexible, reusable code that works with any data type.

```swift
func swapValues<T>(_ a: inout T, _ b: inout T) {
    let temp = a
    a = b
    b = temp
}
```

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