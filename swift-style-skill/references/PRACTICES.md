# Programming Practices Reference

This file covers selected coding practices within the skill’s style scope.

## Compiler Warnings

Do not introduce new warnings. Address existing warnings only when relevant to
the requested change; do not expand the task into unrelated cleanup.
Deprecation warnings can be a reasonable exception when migration is not yet
practical.

## Initializers

- Use a synthesized memberwise initializer for `struct` when it is suitable.
- Do not call special literal protocol initializers directly.
- Omit `.init` in direct initializer calls unless the receiver is a metatype
  value.

```swift
// Preferred
let x = MyType(value: 1)

// Avoid
let y = MyType.init(value: 1)
```

## Properties

Read-only computed properties omit `get`.

## Type Annotations

Prefer type inference in new code when the initializer makes the type clear.
Use explicit annotations when they clarify intent or specify a different
intended type. Preserve existing annotations, and do not flag redundant
annotations alone in reviews.

## Shorthand Types

Use shorthand spellings when the compiler allows them.

```swift
let names: [String]
let counts: [String: Int]
let user: User?
```

## Optional Types

- Prefer `if let`, `guard let`, optional chaining, and `??`.
- If you only need to test for non-`nil`, compare to `nil` instead of binding
  and discarding the value.

## Errors

Use an `enum` conforming to `Error` when there are multiple possible error
states.

## Force Unwraps and Force Casts

Force unwraps and force casts are strongly discouraged. When one is used
outside tests, it should be extremely clear from context or documented with a
comment explaining the invariant that makes it safe.

Unit tests and test-only code may use them without extra documentation.

## Implicitly Unwrapped Optionals

Avoid IUOs when possible. Common allowed cases:
- `@IBOutlet` properties
- values initialized later by UI lifecycle or external setup
- Objective-C APIs imported with missing nullability information
- test fixtures initialized in `setUp()`

Keep IUOs from spreading through multiple layers of your own abstractions.

## Access Levels

- Omitting an explicit access level is permitted.
- Top-level declarations default to `internal`.
- Nested declarations default to the lesser of `internal` and the enclosing
  declaration's access level.
- Access modifiers may appear on an extension or on its individual members.
  In new files, default to member-level access modifiers. In existing files,
  follow the file's pattern and preserve existing declarations. Do not flag
  either placement style in reviews.
- If a member's access differs from the default, specify it on the member.

## Nesting and Namespacing

- Prefer nesting to express scoped or hierarchical relationships.
- Limit type nesting to two levels below a top-level type by default.
  Project linter configuration controls the effective limit and counting rules.
- Do not flag pre-existing nesting violations in change reviews.
- Nested error types and flag enums are common and encouraged.
- Prefer a caseless enum for new namespaces containing only static constants
  or helper functions when instances are not needed.

```swift
enum DateFormats {
    static let shortDate = "yyyy-MM-dd"
    static let yearMonth = "yyyy-MM"
}
```

## Size Limits

- When a function body exceeds 50 lines, excluding blank lines and comments,
  assess its readability and responsibilities. Length alone is not a review
  finding. Extract helpers when they name meaningful operations or reduce
  complexity, not merely to meet the threshold.
- When a file exceeds 500 lines, assess its organization and responsibilities.
  Length alone is not a review finding. Split files when doing so creates
  cohesive, understandable units, not merely to meet the threshold.
- Use the project's linter counting rules and configured thresholds when
  available.
- Do not make unrelated structural changes; extract cohesive
  responsibilities only when appropriate to the requested change.

## Early Exits and Control Flow

- Prefer `guard` for early exits and precondition checks.
- Prefer `for`-`where` instead of an `if` nested directly in the loop body when
  it is only filtering iteration. This is a preference, not a review finding.
  `forEach` remains acceptable; do not flag its use solely for this preference.
- Avoid `fallthrough`; use clearer patterns unless there is a specific reason.

## Pattern Matching

- Bind only the values you use.
- Use tuple patterns when they improve clarity, not as decoration.

## Literals and Operators

- Numeric separators are recommended when they improve readability.
- Do not define custom operators without strong domain precedent.
- Overload existing operators only when the meaning is unsurprising and matches
  the operator's normal semantics.
