# Programming Practices Reference

## Compiler Warnings

**Rule: Resolve all compiler warnings before committing code.**

Warnings signal real issues. Suppress a warning only with a documented reason:

```swift
// ✅ — suppression with justification
@discardableResult
func attemptSave() -> Bool { ... }

// ❌ — silencing a warning without addressing the cause
_ = performOperation()  // valid in some cases, but ask why the result is truly unused
```

---

## Initializers

**Rule: Do not call methods on `self` in `init` before all stored properties are initialised.**

**Rule: Use designated initialisers rather than convenience initialisers wherever possible.**

**Rule: Prefer `init` with explicit argument labels.** Avoid factory methods unless the factory provides meaningful value beyond naming.

---

## Properties

**Rule: Use a computed property (not a method) when:**
- Getting the property has no observable side effects.
- The result is inexpensive to compute or the cost is documented.
- The property expresses a value intrinsic to the type.

**Rule: Use a method (not a computed property) when:**
- The operation can throw.
- The operation is asynchronous.
- The result can be meaningfully different across calls.
- The operation has significant computational cost not obvious to the caller.

---

## Shorthand Type Names

**Rule: Always use shorthand syntax for common types.**

| ❌ Longhand | ✅ Shorthand |
|------------|-------------|
| `Array<String>` | `[String]` |
| `Dictionary<String, Int>` | `[String: Int]` |
| `Optional<User>` | `User?` |
| `Set<UUID>` | `Set<UUID>` (no shorthand — acceptable as-is) |

---

## Optional Types

**Rule: Interrogate optionals using `if let`, `guard let`, or optional chaining. Avoid force unwrapping.**

```swift
// ✅
guard let user = currentUser else { return }
let name = user.name

// ✅ Optional chaining
let city = user.address?.city

// ❌
let user = currentUser!   // Crashes if nil
let city = currentUser!.address!.city
```

**Rule: Use `??` to provide a fallback for optional values.** Avoid force unwrapping as a shortcut for "I know this isn't nil."

**When force unwrap is acceptable:**
- In test code where a `nil` should fail the test loudly.
- In `fatalError` paths where `nil` is a programmer error.
- In `@IBOutlet` and `@IBAction` connections (IUO, see below).

---

## Implicitly Unwrapped Optionals (IUOs)

**Rule: Do not use IUOs unless required by the framework or initialisation constraints.**

```swift
// ✅ — IUO required by UIKit for outlet connections
@IBOutlet weak var titleLabel: UILabel!

// ✅ — set immediately after init, cannot be set in init itself
class ViewController: UIViewController {
  var dataSource: DataSource!  // Acceptable if set in viewDidLoad before any use

// ❌ — lazy init should use Optional or be restructured
var presenter: Presenter!  // Use Optional<Presenter> instead
```

Prefer `Optional` over IUO wherever possible. If you find yourself writing `!`, ask whether the design can be restructured to avoid it.

---

## Error Types

**Rule: Represent errors with `enum` conforming to `Error`.**

```swift
// ✅
enum NetworkError: Error {
  case noConnection
  case timeout(after: TimeInterval)
  case httpError(statusCode: Int)
}
```

**Rule: Use typed throws (`throws(SpecificError)`) for internal API with fixed error sets.** Use untyped `throws` for public API to preserve flexibility for callers.

---

## Force Unwrapping and Force Casts

**Rule: Never force-unwrap or force-cast in production code without a documented, unavoidable reason.**

```swift
// ❌
let number = value as! Int

// ✅
guard let number = value as? Int else {
  // handle the type mismatch
  return
}
```

If a force cast is truly unavoidable, add a comment explaining why it is safe:

```swift
// This cast is safe because the nib guarantees the cell type.
let cell = tableView.dequeueReusableCell(withIdentifier: "Cell") as! MyCell
```

---

## Access Levels

**Rule: Specify access levels explicitly on all declarations.** Do not rely on the `internal` default being implied.

```swift
// ✅
public struct NetworkClient { }
internal class CacheManager { }
private func validateResponse(_ response: URLResponse) { }

// ❌ — intent is ambiguous
struct NetworkClient { }    // Is this deliberately internal?
func validateResponse(_ response: URLResponse) { }
```

**Rule: Apply the most restrictive access level that is correct.** Prefer `private` → `fileprivate` → `internal` → `public` → `open`.

**Rule: Do not use `open` unless you intentionally support subclassing and overriding by external modules.**

---

## Nesting and Namespacing

**Rule: Use nesting to express type hierarchy, not just as a namespace shortcut.**

```swift
// ✅ — nested type has a clear relationship to the outer type
struct Network {
  enum Error: Swift.Error {
    case noConnection
    case timeout
  }
}

// ❌ — nesting used only as a namespace for unrelated types
struct Utilities {
  struct StringHelper { }
  struct DateHelper { }
}
```

Prefer `extension` in the same file for organising related functionality on an existing type.

---

## `guard` for Early Exits

**Rule: Use `guard` to validate preconditions and exit early.** This keeps the "happy path" un-indented.

```swift
// ✅
func processOrder(_ order: Order?) {
  guard let order = order else { return }
  guard order.isValid else {
    logInvalidOrder(order)
    return
  }
  fulfil(order)
}

// ❌ — nested ifs push the happy path to the right
func processOrder(_ order: Order?) {
  if let order = order {
    if order.isValid {
      fulfil(order)
    } else {
      logInvalidOrder(order)
    }
  }
}
```

---

## `for`-`where` Loops

**Rule: Use `for`-`where` instead of `if` inside a loop to filter iteration.**

```swift
// ✅
for user in users where user.isActive {
  notify(user)
}

// ❌
for user in users {
  if user.isActive {
    notify(user)
  }
}
```

---

## `fallthrough` in `switch` Statements

**Rule: Avoid `fallthrough`.** Use comma-separated patterns or explicit code duplication instead.

```swift
// ✅ — comma-joined patterns
switch error {
case .notFound, .gone:
  showNotFoundUI()
default:
  showGenericError()
}

// ❌
switch error {
case .notFound:
  fallthrough
case .gone:
  showNotFoundUI()
}
```

If `fallthrough` is genuinely necessary, add a comment explaining why.

---

## Pattern Matching

**Rule: Use value bindings in patterns to bind only what you need.** Avoid binding variables you do not use.

**Rule: Use tuple patterns judiciously.** Binding a tuple for readability is fine; binding one just to destructure it immediately is noise.

```swift
// ✅
if case .success(let value) = result {
  process(value)
}

// ✅ — tuple pattern for clarity
for (index, element) in list.enumerated() {
  print("\(index): \(element)")
}
```

---

## Numeric and String Literals

**Rule: Use `_` separators in numeric literals with 5 or more digits.**

```swift
// ✅
let population = 8_000_000_000
let threshold: Float = 0.001_5

// ❌
let population = 8000000000
```

**Rule: Do not use magic numbers.** Name constants at the point where their meaning is clear.

---

## Defining New Operators

**Rule: Do not define custom operators** unless there is a strong mathematical or domain precedent (e.g., vector arithmetic). The symbol must be immediately obvious to a reader unfamiliar with the codebase.

When in doubt, use a named function instead.

---

## Overloading Existing Operators

**Rule: Overload existing operators only when the overload is unsurprising and follows the operator's conventional semantics.**

```swift
// ✅ — `+` for concatenation is conventional
func +(lhs: Vector, rhs: Vector) -> Vector { ... }

// ❌ — `+` for unrelated merging surprises the reader
func +(lhs: Config, rhs: Config) -> Config { ... }  // Use merge(_:) instead
```
