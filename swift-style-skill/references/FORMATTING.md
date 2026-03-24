# Formatting Reference

## Core Rules

- 100 characters per line unless a guide exception applies.
- 2-space indentation, never tabs.
- K&R braces for non-empty blocks.
- No semicolons outside comments or string literals.
- At most one statement per line, with the guide's trivial-block exceptions.
- Never align tokens horizontally across lines.

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

Follow the Google wrapping style:
- keep short declarations and expressions on one line
- wrapped comma-delimited lists are either fully horizontal or fully vertical
- wrapped function parameters and call arguments use +2 indentation
- wrapped `where` clauses and continuation lines follow the guide's examples,
  not visual alignment with opening delimiters

## Whitespace

- Use a single space around binary and ternary operators.
- Use a single space after commas and after `:` in dictionary literals and
  labels, but never before `:`.
- Do not put spaces inside brackets.
- Do not put a space before `(` in function calls.

## Vertical Whitespace

- At most one blank line between declarations.
- No blank line at the start or end of a block.
- Use a single blank line between logical declaration groups.

## Parentheses

Omit unnecessary parentheses around top-level condition expressions and return
expressions.

## Specific Constructs

- Non-doc comments use `//`, not block comments.
- Read-only computed properties omit `get` when possible.
- `switch` cases align with the `switch`.
- Enum cases are one per line when they have associated values or comments.
- Single trailing closure: use trailing closure syntax.
- Multiple closure arguments: keep all closures inside the argument list.
- Multi-line array and dictionary literals require trailing commas.
- Numeric separators are recommended for long literals when they improve
  readability; they are not mandatory in every case.
- Parameterized attributes go on their own line.
- Non-parameterized attributes may stay on the declaration line if they fit
  cleanly without forcing a wrap.
