# 02. Object & Protocol Oriented Programming

Swift leans heavily towards Protocol Oriented Programming (POP), preferring Structs over Classes for data models.

---

## 1. Naming Convention

In Swift, it is good practice to follow these naming conventions:

| Element              | Convention       | Example            |
| -------------------- | ---------------- | ------------------ |
| **Types** (Struct, Class, Enum, Protocol) | UpperCamelCase | `User`, `Vehicle` |
| **Properties & Methods** | lowerCamelCase | `name`, `greet()` |

---

## 2. Struct vs Class (The Most Important Concept)

| Feature         | Struct                    | Class                              |
| --------------- | ------------------------- | ---------------------------------- |
| **Type**        | Value Type                | Reference Type                     |
| **Storage**     | Stack (Faster)            | Heap                               |
| **Inheritance** | No                        | Yes                                |
| **Mutability**  | Needs `mutating` keyword  | Always mutable                     |
| **Use Case**    | Data Models, DTOs         | ViewControllers, Database Connections |

### Value Type (Struct)

When you assign a struct to a new variable, it **copies** the data. Changes to the copy do not affect the original.

```swift
struct Resolution {
    var width = 0
    var height = 0
}

let hd = Resolution(width: 1920, height: 1080)

var lowRes = hd          // Creates a COPY
lowRes.height = 480
lowRes.width = 360

print(hd.height, hd.width, separator: ", ")
// Output: 1080, 1920 ✅ Original unchanged!
```

### Reference Type (Class)

When you assign a class to a new variable, both variables **point to the same object**. Changes to one affect the other.

```swift
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}

let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0

let alsoTenEighty = tenEighty    // Points to SAME object
alsoTenEighty.frameRate = 30.0

print(tenEighty.frameRate)
// Output: 30.0 ⚠️ Original changed!
```

> **Key Takeaway:** With structs, you get independent copies. With classes, you share the same instance.

---

## 3. Properties

A property is a variable or constant that resides inside a structure (Class, Struct, or Enum).

### Stored Properties

The most common type — stores a value directly. Can be `var` or `let`, with optional default values.

```swift
struct User {
    var name: String
    var age = 22    // Default value
}
```

### Lazy Stored Properties

Initialized only when first accessed. Use for expensive computations or external dependencies.

```swift
struct LazyStruct {
    lazy var upperCasedName: String = {
        return "davi".uppercased()
    }()
}

var lazyObj = LazyStruct()
print(lazyObj.upperCasedName)
// Output: DAVI
```

> ⚠️ Lazy properties must be `var` — their value isn't known until after initialization.

### Computed Properties

Don't store a value — instead compute it via `get` and optional `set`.

```swift
import Darwin

struct Square {
    var side: Double
    
    var area: Double {
        get { return side * side }
        set { side = sqrt(newValue) }
    }
    
    // Read-only (get is implicit)
    var perimeter: Double { return side * 4 }
}

var square = Square(side: 4.0)
print(square.area)       // Output: 16.0
print(square.perimeter)  // Output: 16.0

square.area = 25
print(square.side)       // Output: 5.0
```

> ⚠️ Computed properties must be `var` — their value depends on computation.

### Property Observers

React to value changes with `willSet` (before) and `didSet` (after).

```swift
struct ObserverStruct {
    var name: String {
        willSet { print("Will change to \(newValue)") }
        didSet { print("Changed from \(oldValue)") }
    }
    
    var age: Int {
        willSet(newAge) { print("Will change to \(newAge)") }
    }
}

var obj = ObserverStruct(name: "Davi", age: 22)
obj.name = "Test"
// Output:
// Will change to Test
// Changed from Davi
```

| Observer   | When Called              | Access To     |
| ---------- | ------------------------ | ------------- |
| `willSet`  | Before value is assigned | `newValue`    |
| `didSet`   | After value is assigned  | `oldValue`    |

---

## 4. Initializers

### Struct (Auto-Synthesized)

Structs have initializers automatically synthesized from their properties. You only need to define initializers if extra computation is required.

```swift
struct User {
    var name: String
    var age: Int
}

let carl = User(name: "Carl", age: 22)
```

### Class (Explicit Required)

Classes must have initializers explicitly defined by the developer.

```swift
class User {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }

    func greet() {
        print("Hello \(name)")
    }
}

let carl = User(name: "Class Carl", age: 22)
```

---

## 5. OOP Principles

### Abstraction

Hide complex implementation, expose only necessary parts via protocols.

```swift
protocol Shape {
    func calculateArea() -> Double
}

class Rectangle: Shape {
    var width: Double
    var height: Double
    
    init(width: Double, height: Double) {
        self.width = width
        this.height = height
    }
    
    func calculateArea() -> Double {
        return width * height
    }
}

struct Circle: Shape {
    var radius: Double
    
    func calculateArea() -> Double {
        return .pi * radius * radius
    }
}
```

### Encapsulation

Bundle data and methods into a single unit, restrict access using access control.

#### Access Control Levels

Swift's default is `internal` — safe by design.

| Keyword       | Access Level                              |
| ------------- | ----------------------------------------- |
| `private`     | Same scope (type/extension) only          |
| `fileprivate` | Same file only                            |
| `internal`    | Same module (default)                     |
| `public`      | Accessible from other modules             |
| `open`        | Accessible & subclassable from other modules |

```swift
public struct APIClient {
    public init() {}
    public func request() {}
}

struct Repository {  // internal by default
    fileprivate var cache: [String: String] = [:]
    private func reset() { cache.removeAll() }
}
```

#### Types and Members Rule

A member **cannot** be more visible than its enclosing type.

```swift
// ✅ Valid — member is less visible than type
public struct Box {
    internal var value: Int
}

// ❌ Invalid — member more visible than type
internal struct Box {
    public var value: Int  // Error!
}
```

> **Rule:** Members can have lower or equal visibility, never higher than the type.

```swift
class BankAccount {
    private var balance: Double = 0.0
    
    func deposit(amount: Double) { balance += amount }
    
    func withdraw(amount: Double) -> Bool {
        guard amount <= balance else { return false }
        balance -= amount
        return true
    }
    
    func getBalance() -> Double { return balance }
}
```

> `balance` is encapsulated — only accessible through `deposit`, `withdraw`, and `getBalance`.

### Inheritance

Allows a new type to inherit properties and methods from an existing type.

#### Classes (Traditional Inheritance)

```swift
class Vehicle {
    var currentSpeed = 0.0
    func accelerate(by amount: Double) { currentSpeed += amount }
}

class Car: Vehicle {
    var gear = 1
    func changeGear(to newGear: Int) { gear = newGear }
}
```

#### Subclass and Override

Use `override` to override a superclass method.

```swift
class Animal { 
    func speak() { print("...") } 
}

class Dog: Animal { 
    override func speak() { print("Woof") } 
}

let a = Animal()
a.speak()
// Output: ...

let d = Dog()
d.speak()
// Output: Woof
```

#### Call Super

Use `super` to extend a superclass method when overriding.

```swift
class Animal { 
    func speak() { print("...") } 
}

class Dog: Animal {
    override func speak() {
        super.speak()
        print("Woof")
    }
}

let d = Dog()
d.speak()
// Output:
// ...
// Woof
```

#### Structs (Protocol + Extension)

Structs cannot inherit from other structs, but can adopt **Protocols** and use **Extensions** to gain shared behavior.

**Protocols** — Blueprints of methods or properties (similar to Interfaces in PHP).

```swift
protocol Drivable {
    var speed: Int { get }
    func drive()
}

struct Bike: Drivable {
    var speed: Int
    func drive() { print("Biking at \(speed) km/h") }
}
```

#### Extending Built-in Types

```swift
extension Drivable {
    func stop() { print("Stopped") }  // Default implementation for all Drivable types
}

let bike = Bike(speed: 20)
bike.drive()  // Output: Biking at 20 km/h
bike.stop()   // Output: Stopped
```

```swift
extension String {
    func isEmail() -> Bool {
        return self.contains("@")
    }
}

let email = "test@example.com"
print(email.isEmail()) // Output: true
```


**Protocols with Associated Types** — Use `associatedtype` to make a protocol generic over a placeholder type. Conformers bind the placeholder to a concrete type.

```swift
protocol Container {
    associatedtype Item
    var items: [Item] { get set }
    mutating func add(_ item: Item)
    func getAll() -> [Item]
}

struct StringContainer: Container {
    typealias Item = String  // Bind placeholder to String
    var items: [String] = []
    
    mutating func add(_ item: String) {
        items.append(item)
    }
    
    func getAll() -> [String] {
        return items
    }
}

struct IntContainer: Container {
    typealias Item = Int  // Bind placeholder to Int
    var items: [Int] = []
    
    mutating func add(_ item: Int) {
        items.append(item)
    }
    
    func getAll() -> [Int] {
        return items
    }
}

var strings = StringContainer()
strings.add("Hello")
strings.add("World")
print(strings.getAll())  // Output: ["Hello", "World"]

var numbers = IntContainer()
numbers.add(1)
numbers.add(2)
print(numbers.getAll())  // Output: [1, 2]
```

> **Key Point:** `associatedtype` allows one protocol to work with different types. Each conformer decides what `Item` actually is.

**Extensions** — Add functionality to existing types without modifying the original code.

#### The Problem It Solves

Normally, when you create a `struct` or `class`, you put everything inside one giant block of code — variables, formatting functions, database functions, click handlers — all in one place.

As your app grows, that single struct becomes hundreds of lines long. It becomes a nightmare to read and maintain.

#### The Solution: Extensions as "Departments"

In Swift, `extension` isn't just for adding new features to built-in types like `String` or `Int`. You can also use it on **your own types** to organize your code into logical categories, or "responsibilities."

Think of your main `struct` as a company, and the `extensions` as its different departments.

**The Core Identity (The Main Struct)** — The bare minimum data that defines an Article.

```swift
struct Article { 
    let title: String
    let body: String 
}
```

**Department 1: Formatting (Presentation)** — Handles how the article looks on screen.

```swift
extension Article { 
    // This extension only handles how the article looks
    var preview: String { 
        return String(body.prefix(40)) + "..." 
    }
}
```

**Department 2: Networking (Data Fetching)** — Handles getting data from the API.

```swift
extension Article { 
    // This extension only handles getting data from the API
    static func fetchAll() -> [Article] { 
        return [] // Imagine this calls your Vapor backend
    }
}
```

**Usage:**

```swift
let article = Article(title: "Swift Guide", body: "This is a very long article body that needs to be truncated...")
print(article.preview)
// Output: This is a very long article body that n...

let articles = Article.fetchAll()
print(articles)
// Output: []
```

> **Benefits:**
> - **Readability** — Collapse/skip sections you don't need
> - **Separation of Concerns** — Each extension has one responsibility
> - **File Splitting** — Move extensions to separate files (`Article+Networking.swift`)

> **Key Point:** Protocols + Extensions = "Inheritance" for Structs (Protocol Oriented Programming).

### Polymorphism

Treat objects as instances of their parent class or protocol, enabling flexible code.

#### Basic Polymorphism (Classes)

Use inheritance to treat related types uniformly.

```swift
class Animal { 
    func speak() { print("...") } 
}

class Dog: Animal { 
    override func speak() { print("Woof") } 
}

class Cat: Animal { 
    override func speak() { print("Meow") } 
}

let animals: [Animal] = [Dog(), Cat()]
animals.forEach { $0.speak() }
// Output:
// Woof
// Meow
```

#### Protocol Polymorphism (Structs)

Use a protocol to treat unrelated types uniformly.

```swift
protocol Speaker { 
    func speak() 
}

struct StructDog: Speaker { 
    func speak() { print("Woof") } 
}

struct StructCat: Speaker { 
    func speak() { print("Meow") } 
}

let speakers: [Speaker] = [StructDog(), StructCat()]
speakers.forEach { $0.speak() }
// Output:
// Woof
// Meow
```

---

## 6. Conclusion: OOP in Classes vs Structs

| Principle       | Class | Struct |
| --------------- | ----- | ------ |
| **Abstraction** | ✅ Full (with inheritance) | ✅ Via Protocols |
| **Encapsulation** | ✅ Full | ✅ Full |
| **Polymorphism** | ✅ Full | ⚠️ Limited (via Protocols) |
| **Inheritance** | ✅ Full | ❌ No (use Protocols + Extensions) |

### Initializer Inheritance

Classes require all stored properties (including inherited ones) to have initial values. Swift provides two types of class initializers:

- **Designated Initializers** — Primary initializers that fully initialize all properties
- **Convenience Initializers** — Secondary initializers that call designated initializers

> Structs get auto-synthesized initializers. Classes need explicit initializers due to inheritance complexity.