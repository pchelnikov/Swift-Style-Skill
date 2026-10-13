# Formatting Reference

## Core Rules

- 120 characters per line by default, except as described under Column Limit.
- Applicable project instructions and formatter/linter configurations,
  including `.swiftformat` and `.swiftlint.yml`, override conflicting defaults
  in this reference. If applicable project instructions or tool configurations
  conflict with one another and no explicit precedence resolves the conflict,
  identify it and ask the user how to proceed.
- 4-space indentation, never tabs.
- K&R braces for non-empty blocks.
- No semicolons outside comments or string literals.
- At most one statement per line, except for trivial blocks.
- Do not align tokens horizontally across lines, except for wrapped conditions
  as described under Line Wrapping.

## Column Limit

Do not wrap:
- long URLs in comments when wrapping would break the unit of text
- `import` statements
- generated code

## Braces

Opening braces stay on the same line unless wrapping requires otherwise.
`else` and similar continuations stay on the same line as the preceding `}`.
Empty blocks may be written as `{}`.

For closures, keep the signature on the line with `{` when it fits, then break
after `in`.

## Line Wrapping

Keep short declarations, calls, and collections on one line when they fit.
When wrapping is needed:

- Break before the first parameter, argument, or collection element, then put
  one item per line with one additional indentation level (4 spaces).
- Put the closing `)` or `]` on its own line, at the indentation where the
  declaration or call started.
- Keep effects, the return type, and the opening brace on the closing-parenthesis
  line: `) async throws -> T {`.
- Wrapped `where` clauses and continuation lines use indentation rather than
  visual alignment with opening delimiters, except for conditions described below.

Keep `if` and `guard` conditions on one line when the statement fits within the
effective line limit (120 columns by default). Otherwise, keep the first
condition beside the keyword and align subsequent conditions beneath it.
Place `{` after the last `if` condition and `else {` after the last `guard`
condition.

```swift
if let authenticatedUser = authenticationService.currentAuthenticatedUser,
   authorizationService.hasPermissionToRefreshProfile(for: authenticatedUser) {
    refreshProfile(for: authenticatedUser)
}
```

```swift
guard let authenticatedUser = authenticationService.currentAuthenticatedUser,
      authorizationService.hasPermissionToRefreshProfile(for: authenticatedUser) else {
    return
}
```

Short statements stay on one line:

```swift
guard let user = currentUser, user.isActive else {
    return
}
```

These abbreviated examples illustrate the wrapped layout:

```swift
func fetchProfile(
    userID: UserID,
    cachePolicy: CachePolicy
) async throws -> UserProfile {
    // ...
}

let profile = makeProfile(
    displayName: displayName,
    emailAddress: emailAddress,
    notificationsEnabled: true
)

let supportedRegions = [
    "Sweden",
    "Norway",
    "Denmark",
]
```

## Whitespace

- Use a single space around binary and ternary operators, except range
  operators. Do not put spaces around `..<` or `...`: `0..<count`, `1...10`.
- Use a single space after commas and after `:` in dictionary literals and
  labels, but never before `:`.
- Do not put spaces inside brackets.
- Do not put a space before `(` in function calls.

## Vertical Whitespace

- At most one blank line between declarations.
- Insert one blank line after the opening brace of non-empty `struct`, `class`,
  `actor`, and `extension` declarations, including nested declarations. Do not insert
  a leading blank line in `enum`, `protocol`, or other blocks. No block has
  a trailing blank line.
- Use a single blank line between logical declaration groups.

## Parentheses

Omit unnecessary parentheses around top-level condition expressions and return
expressions.

## Specific Constructs

- Omit `self.` unless the compiler requires it or it is needed to distinguish
  a property from a parameter, as in `self.name = name`.
- Indent code inside `#if`, `#elseif`, and `#else` branches by one additional
  level (4 spaces). Keep the directives, including `#endif`, aligned with
  the surrounding scope.
- Non-doc comments use `//`, not block comments.
- Read-only computed properties omit `get` when possible.
- `switch` cases align with the `switch`.
- Enum cases are one per line when they have associated values or comments.
- Single trailing closure: use trailing closure syntax.
- Multiple closure arguments: keep all closures inside the argument list.
- Multiline array and dictionary literals require trailing commas. Omit trailing
  commas in argument and parameter lists by default.
- Numeric separators are recommended for long literals when they improve
  readability; they are not mandatory in every case.
- Parameterized attributes go on their own line.
- Non-parameterized attributes may stay on the declaration line if they fit
  cleanly without forcing a wrap.
