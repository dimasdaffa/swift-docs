# 02. Object & Protocol Oriented Programming

Swift leans heavily towards Protocol Oriented Programming (POP), preferring Structs over Classes for data models.

---

## 1. Struct vs Class (The Most Important Concept)

| Feature         | Struct                    | Class                              |
| --------------- | ------------------------- | ---------------------------------- |
| **Type**        | Value Type                | Reference Type                     |
| **Storage**     | Stack (Faster)            | Heap                               |
| **Inheritance** | No                        | Yes                                |
| **Mutability**  | Needs `mutating` keyword  | Always mutable                     |
| **Use Case**    | Data Models, DTOs         | ViewControllers, Database Connections |

### Value Type (Struct) Behavior

When you assign a struct to a new variable, it **copies** the data.

```swift
struct User {
    var name: String
}

var u1 = User(name: "A")
var u2 = u1
u2.name = "B"

print(u1.name) // "A" (Original remains untouched)
print(u2.name) // "B"
```

### Reference Type (Class) Behavior

When you assign a class, it references the same memory address.

```swift
class Manager {
    var name: String
    init(name: String) { self.name = name }
}

var m1 = Manager(name: "A")
var m2 = m1
m2.name = "B"

print(m1.name) // "B" (Changed!)
```

---

## 2. Protocols

Blueprints of methods or properties. Similar to Interfaces in PHP but more powerful.

```swift
protocol Vehicle {
    var speed: Int { get }
    func drive()
}

struct Car: Vehicle {
    var speed: Int
    func drive() {
        print("Vroom! Driving at \(speed) km/h")
    }
}
```

---

## 3. Extensions

Add functionality to existing types without modifying the original code.

```swift
extension String {
    func isEmail() -> Bool {
        return self.contains("@")
    }
}

let email = "test@example.com"
print(email.isEmail()) // true
```