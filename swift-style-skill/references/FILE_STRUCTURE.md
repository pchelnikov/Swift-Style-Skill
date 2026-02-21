# File Structure Reference

## File Naming

**Rule: Name the file after its primary type.**

```swift
// ✅ MyType.swift — contains type MyType
// ✅ MyType+MyProtocol.swift — extension adding protocol conformance
// ✅ MyType+Additions.swift — multiple extensions grouped together
// ✅ Math.swift — related top-level functions with no enclosing type
```

All Swift source files end with `.swift`. No exceptions.

When a file contains one type plus small private helpers, name it after the type — the helpers are not primary.

---

## File Comments

**Rule: File-level comments are optional and usually unnecessary.**

| ❌ Avoid | ✅ Prefer |
|---------|----------|
| File comment on a file with a single type | Doc comment on the type itself |
| Repeating what the type name already says | File comment only if multiple unrelated abstractions need explaining |

File comments are appropriate when a file groups several related types whose collective purpose is not obvious from individual type names.

---

## Import Statements

**Rule: Import exactly what you need — no more, no less.**

```swift
// ✅ Correct
import CoreLocation
import UIKit

// ❌ Wrong: relying on UIKit transitively importing Foundation
import UIKit
// ... uses DateFormatter without importing Foundation
```

**Rule: Import whole modules, not individual declarations or submodules.**

```swift
// ✅
import UIKit

// ❌ Avoid — no tooling support, non-idiomatic
import class UIKit.UIViewController
```

**Exception:** Submodule imports are permitted when the submodule exposes API not available at the top level (e.g., `import UIKit.UIGestureRecognizerSubclass`).

**Ordering — three groups, separated by a blank line, each group sorted lexicographically:**

```swift
// Group 1: Module imports not under test
import CoreLocation
import MyThirdPartyModule
import SpriteKit
import UIKit

// Group 2: Individual declaration imports (rare)
import func Darwin.C.isatty

// Group 3: @testable imports (test files only)
@testable import MyModuleUnderTest
```

Import statements are never line-wrapped.

---

## Type, Variable, and Function Declarations

**Rule: One primary top-level type per file.** Exceptions:

- A class and its delegate protocol may coexist.
- A type and small private helper types may coexist (especially when helpers are `fileprivate`).

**Rule: Use a logical ordering for members.** Whatever order you choose, be consistent and be able to explain it. Do not add new methods at the bottom by habit ("chronological" is not a logical order).

**Rule: Use `// MARK:` to label logical sections.**

```swift
class MovieRatingViewController: UITableViewController {

  // MARK: - View controller lifecycle methods

  override func viewDidLoad() { ... }
  override func viewWillAppear(_ animated: Bool) { ... }

  // MARK: - Movie rating manipulation methods

  @objc private func ratingStarWasTapped(_ sender: UIButton?) { ... }
  @objc private func criticReviewWasTapped(_ sender: UIButton?) { ... }
}
```

`// MARK: -` (with a hyphen) inserts a visual divider in Xcode's navigation bar jump menu.

**Rule: Group overloaded declarations together** with no unrelated code in between.

---

## Extensions

Extensions organise functionality into logical units. The grouping must follow a rationale you could explain to a reviewer.

```swift
// ✅ Protocol conformance in its own extension
extension MyType: Equatable {
  static func ==(lhs: MyType, rhs: MyType) -> Bool { ... }
}

// ✅ Related functionality grouped by concern
extension MyType {
  // MARK: - Serialisation
  func encode() -> Data { ... }
  static func decode(from data: Data) -> MyType? { ... }
}
```

---

## Documentation Comments

**Rule: Document all `public` and `open` declarations.**

Use `///` (triple-slash), not `/* */` block comments, for documentation.

**Format:**

```swift
/// Returns the index of `element` in `collection`, or `nil` if not found.
///
/// - Parameters:
///   - element: The element to search for.
///   - collection: The collection to search.
/// - Returns: The index of the element, or `nil` if absent.
/// - Throws: `SearchError.invalidCollection` if the collection is malformed.
func index<E: Equatable>(of element: E, in collection: [E]) throws -> Int? { ... }
```

**Rules:**

| Rule | Detail |
|------|--------|
| Single-sentence summary | First line: complete sentence, ends with period |
| Blank line before tags | Separate summary from parameter/return/throws tags |
| Use Apple markup | `- Parameters:`, `- Returns:`, `- Throws:`, `- Note:`, `- Warning:` |
| Document non-obvious behaviour | Don't document what the name already says |
| No doc comment needed | For `private`/`internal` where intent is clear from context |

**Where to document:**

- All `public` and `open` declarations: always.
- `internal` declarations: when the logic is non-obvious.
- `private` helpers: only when the implementation is complex enough to warrant it.
- Overrides: document only if the override changes behaviour vs the superclass.
