# Naming Reference

## Foundation: Apple's API Style Guidelines

This skill follows [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) as the baseline, with the additions below.

---

## Casing Rules

| Symbol type | Convention | Example |
|-------------|-----------|---------|
| Types (class, struct, enum, protocol, typealias) | UpperCamelCase | `UserProfile`, `NetworkError` |
| Enum cases | UpperCamelCase | `case notFound`, `case unauthorized` |
| Functions, methods | lowerCamelCase | `func fetchUser()` |
| Properties, variables, constants | lowerCamelCase | `var isLoading`, `let baseURL` |
| Parameters | lowerCamelCase | `func greet(name: String)` |
| Local variables | lowerCamelCase | `let elapsed = Date()` |
| Module names | UpperCamelCase | `import MyModule` |

---

## Acronyms and Initialisms

**Rule: Acronyms follow casing rules as a unit.**

- If the identifier is all-acronym, it is all-caps: `URL`, `HTTP`, `JSON`.
- If the acronym appears inside a lowerCamelCase identifier, it is all-lowercase: `urlString`, `httpRequest`, `jsonDecoder`.
- If the acronym appears inside an UpperCamelCase identifier, it is all-caps: `HTTPSConnection`, `JSONParser`.

```swift
// ✅
var url: URL
var urlString: String
class HTTPSConnection { }
func parseJSON(_ data: Data) -> JSONResponse { }

// ❌
var URL: String          // Looks like a type name
var urlString: String    // ✅ already shown
class HttpsConnection { } // Should be HTTPSConnection
func parseJson(_ data: Data) -> JSONResponse { }  // json → JSON
```

---

## Identifiers

**Rule: Do not use non-ASCII characters in identifiers** unless required by the domain (e.g., a mathematical formula).

**Rule: Do not use keywords as identifiers** even though Swift allows backtick-escaping (`\`class\``). Rename the symbol instead.

---

## Naming for Clarity

**Rule: Name for clarity at the call site.**

The goal is that a call site reads clearly as an English phrase:

```swift
// ✅ — reads as "insert element at index"
list.insert(element, at: index)

// ❌ — redundant word
list.insertElement(element, atIndex: index)
```

**Rule: Omit needless words** — especially words that merely restate the type.

```swift
// ✅
users.remove(at: index)
view.backgroundColor = .red

// ❌
users.removeElement(atIndex: index)   // "Element" repeats what the type says
view.setBackgroundColor(.red)         // "set" is often redundant
```

**Rule: Compensate for missing type information with words** that clarify role.

```swift
// ✅ — "anchor" clarifies the role of the point
func fade(from anchor: CGPoint) { }

// ❌ — caller loses meaning
func fade(from point: CGPoint) { }   // fine actually, but:
func fade(from: CGPoint) { }         // "from" alone is unclear at declaration
```

---

## Initializers

**Rule: Initializers do not need a label for their first argument** when the first argument forms a natural phrase.

```swift
// ✅
let colour = Color(red: 1, green: 0, blue: 0)
let parser = XMLParser(data: data)

// ❌ — redundant
let colour = Color(redChannel: 1, greenChannel: 0, blueChannel: 0)
```

Conversion initializers may omit the label entirely:

```swift
// ✅ — type conversion is self-evident
let intValue = Int(someDouble)
let string = String(42)
```

---

## Static and Class Properties

**Rule: Static and class properties do not need the type name as a prefix.**

```swift
// ✅
extension Color {
  static let red = Color(red: 1, green: 0, blue: 0)
}
let c: Color = .red

// ❌
extension Color {
  static let colorRed = Color(red: 1, green: 0, blue: 0)
}
```

---

## Global Constants

**Rule: Global constants follow lowerCamelCase**, not ALL_CAPS or k-prefix.

```swift
// ✅
let defaultTimeout: TimeInterval = 30
let maxRetryCount = 3

// ❌
let DEFAULT_TIMEOUT: TimeInterval = 30
let kMaxRetryCount = 3
```

---

## Delegate Methods

**Rule: Delegate method names begin with the name of the type that is the source of the delegate call.**

```swift
// ✅
protocol SessionDelegate: AnyObject {
  func sessionDidExpire(_ session: Session)
  func session(_ session: Session, didReceiveData data: Data)
  func session(_ session: Session, didFailWithError error: Error)
}

// ❌ — name does not identify the delegating source
protocol SessionDelegate: AnyObject {
  func didExpire()
  func didReceiveData(_ data: Data)
}
```

The first parameter is the delegating object (unlabelled or with a short label), allowing the method to be disambiguated if multiple delegate sources exist.

---

## Naming Conventions Are Not Access Control

**Rule: Never use naming conventions to imply access level.** Use Swift's access control keywords.

```swift
// ✅
private var name: String
internal func helper() { }

// ❌ — underscore does not make it private
var _name: String      // Still internal
func _helper() { }    // Still internal
```

Leading underscores communicate nothing to the compiler and create confusion for readers.

---

## Boolean Naming

**Rule: Boolean properties and variables should read as assertions.**

```swift
// ✅
var isEmpty: Bool
var isLoading: Bool
var hasUnreadMessages: Bool
var canSubmit: Bool

// ❌
var empty: Bool
var loading: Bool
var unreadMessages: Bool
```
