# File Structure Reference

## File Names

- Swift source files end in `.swift`.
- A file is usually named for its primary type: `MyType.swift`.
- Protocol-conformance extensions use `Type+Protocol.swift`.
- Broader grouped extensions can use `Type+Additions.swift`.

## File Comments

Do not add new `Created by` file headers. Preserve existing headers and required
copyright or license notices.

File comments are optional and usually unnecessary for files that contain a
single abstraction. Prefer documenting the type itself. File comments are most
useful when a file groups multiple related abstractions.

## Imports

- Import exactly the modules the file needs.
- Prefer whole-module imports to individual declarations or submodules.
- Individual declaration imports are allowed when importing the full module
  would pollute the global namespace.
- Submodule imports are allowed when the submodule exposes API not available
  from the top-level module.
- Imports are the first non-comment tokens in a file and are not line-wrapped.

Import groups:
1. Module or submodule imports not under test
2. Individual declaration imports
3. `@testable` imports

Sort each group lexicographically and separate groups with one blank line.

## Top-Level Declarations

- Most files contain one top-level type.
- Related helper types may share the file when they are closely related and
  commonly reviewed together.
- A class and its delegate protocol may share a file.
- Place new protocol declarations near the top of the file. Supporting types
  may precede them when that improves readability. Preserve existing declaration
  order unless reorganizing the file is part of the requested change.
- Use a logical member order that you could explain to a reviewer.

## `// MARK:`

Use `// MARK:` comments to label logical sections when they improve navigation
and readability. `// MARK: -` inserts a divider in Xcode's jump menu.

## Overloaded Declarations

Overloads that appear in the same scope stay together with no unrelated code
between them.

## Extensions

Use extensions to organize functionality into logical units. The grouping should
be explainable and consistent.

## Documentation Comments

Use `///` for documentation comments.

Require documentation for new `public` and `open` APIs intended for consumers
of reusable libraries and packages, regardless of build system, including new
public and open members. A declaration does not require documentation solely
because it is marked `public` or `open` inside an application. Do not flag
existing undocumented declarations in change reviews. In an explicitly requested
documentation audit, assess existing APIs within the requested scope using the
same library/package boundary and exceptions, without editing files.

Common exceptions:
- self-explanatory declarations, including enum cases
- overrides or protocol requirement implementations
- test methods with descriptive names
- extension declarations themselves

Keep docs concise:
- first line is a single-sentence summary
- separate summary from tags with a blank `///` line
- use Apple's markup tags such as `- Parameter:`, `- Parameters:`,
  `- Returns:`, and `- Throws:`
