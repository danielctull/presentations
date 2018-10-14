slidenumbers: false


# [fit] **Codify the Pattern**

## [fit] *How Swift lets us eschew convention*

---

# Objective-C Conventions in Swift

**Objective-C Convention** | **Swift**
---------------------------|------------------------------------
Initialization             | `convenience` & `designated`
Mutable / Immutable types  | `var` / `let`
nil, null, NSNull, -1      | Optional
Selectors                  | References to functions -->

---

# [fit] Creating Atomic Types

---

# Initial Rectangle Type Definition

```swift
struct Rectangle {

    let identifier: String

    var x: Double
    var y: Double
    var width: Double
    var height: Double

    var red: Double
    var green: Double
    var blue: Double
    var alpha: Double
}
```

---

# Creating Atomic Types

```swift
struct Frame {
    let x: Double
    let y: Double
    let width: Double
    let height: Double
}

struct Color {
    let red: Double
    let green: Double
    let blue: Double
    let alpha: Double
}
```
---

# Creating Atomic Types

```swift
struct Rectangle {
    let identifier: String
    var frame: Frame
    var color: Color
}
```

* Separation of concerns
* Easier to reason about
* Changes to each property are atomic


---

# [fit] Type Specialization

---

# Type Specialization

```swift
struct Rectangle {
    let identifier: String
    var frame: Frame
    var color: Color
}
```

Identifier is a string so it can be used like a string

```swift
var identifier = rectangle.identifier
let start = identifier.index(after: identifier.startIndex)
let end = identifier.index(before: identifier.endIndex)
identifier.replaceSubrange(start..<end, with: "")
```

---

# Type Specialization

```swift
struct Identifier {

    private let value: String

    init(_ value: String) {
        self.value = value
    }
}
```

---

# Type Specialization

```swift
struct Identifier: Equatable {

    private let value: String

    init(_ value: String) {
        self.value = value
    }
}
```

---

# Type Specialization

```swift
struct Identifier: Equatable {

    private let value: String

    init(_ value: String = NSUUID().uuidString) {
        self.value = value
    }
}
```

---

# Type Specialization

```swift
struct Rectangle {

    let identifier = Identifier()

    var frame: Frame
    var color: Color
}
```

---

# [fit] Making Illegal States<br>Unrepresentable

---

# Making Illegal States Unrepresentable

Developer is confused.

Developer attempts to create color.

```swift
rectangle.color = Color(red: 100, green: 50, blue: 0, alpha: 100)
```

---

# Validate and Throw Fatal Error

```swift
struct Color {

    init(red: Double, green: Double, blue: Double, alpha: Double) {

        guard
            0 <= red, red <= 1,
            0 <= green, green <= 1,
            0 <= blue, blue <= 1,
            0 <= alpha, alpha <= 1
        else {
            fatalError()
        }

        ...
    }
}
```

---

# Validate Using Minimum and Maximum Values

```swift
struct Color {

    init(red: Double, green: Double, blue: Double, alpha: Double) {

        switch red {
			case  ...0 : self.red = 0
			case 1...  : self.red = 1
			default    : self.red = red
		}

        switch green {
			case  ...0 : self.green = 0
			case 1...  : self.green = 1
			default    : self.green = green
		}

        ...
    }
}
```

---

# Validate Using Specialised Type

```swift
struct Percentage {

    fileprivate let value: Double
    init(_ value: Double) {

        switch value {
            case  ...0 : self.value = 0
            case 1...  : self.value = 1
            default    : self.value = value
        }
    }
}
```

---

# Validate Using Specialised Type

```swift
struct Color {
    let red: Percentage
    let green: Percentage
    let blue: Percentage
    let alpha: Percentage
}
```

Each color component now *must* be between zero and one.

Parameters define themselves as percentages.

Unfamiliar developers can look up the Percentage logic.

^
* This is amazing!
* All our values are strongly defined.
* Each color component must be between zero and one.
* Also, as a side effect, the parameters now define themselves:
(A coder coming to this type will either know what a Percentage is or know to delve deeper and not make the assumptions they may have made if it were just a Double.)


---

# Validate Using Specialised Type

```swift
rectangle.color = Color(red: Percentage(1),
                        green: Percentage(0.5),
                        blue: Percentage(0),
                        alpha: Percentage(1))
```

Components are validated before Color is created.

But isn't this a little ugly for Swift?

---

# Expressible By Float/Integer Literal

```swift
extension Percentage: ExpressibleByFloatLiteral {

	public init(floatLiteral value: Double) {
		self.init(value)
	}
}

extension Percentage: ExpressibleByIntegerLiteral {

	public init(integerLiteral value: Int) {
		self.init(Double(value))
	}
}
```

---

# Expressible By Float/Integer Literal

```swift
rectangle.color = Color(red: 1, green: 0.5, blue: 0, alpha: 1)
```

Using integer and float *literal* values

Values from elsewhere still need to call Percentage initialiser

Makes for easier to read tests

---

# [fit] Validating User Input

---

```swift
extension Percentage {

	public init?(_ string: String) {

		guard let double = Double(string) else {
			return nil
		}

		self.init(double)
	}
}
```

---

# [fit]

---



---

# **Daniel Tull**
## *@danielctull*