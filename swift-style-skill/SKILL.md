---
name: swift-style-skill
description: >
  A practical, opinionated Swift style skill for writing, reviewing, and
  refactoring modern Swift code. Covers naming, formatting, file organization,
  documentation, and coding conventions. Project instructions and formatter/linter
  configurations take precedence over its defaults.
license: MIT
metadata:
  author: Michael Pchelnikov
  version: "3.0.2"
---

# Swift Style Guide Skill

Use this skill to apply practical, opinionated conventions when writing,
reviewing, and refactoring modern Swift code. Project instructions and formatter/linter
configurations take precedence over this skill’s defaults.

## Scope

- In scope: Swift naming, imports, file structure, formatting, documentation,
  and selected language-level coding practices.
- Out of scope: framework-specific conventions (including SwiftUI view layout
  and property organization), architecture, dependency choices, concurrency
  strategy, or unrelated project policy.

## Workflow

1. Identify the task mode (`generate`, `review`, `audit`, `refactor`, or `answer`)
   and requested scope.
2. Inspect applicable project instructions and formatter/linter configurations.
3. Apply project rules first, then skill defaults where applicable. Ask the user
   about conflicts only when no explicit precedence resolves them.
4. Read the references needed for the task.
5. Quote or cite only the rule relevant to the code under discussion.

Mode outputs:
- `generate` and `refactor`: apply the rules directly with minimal explanation.
- `review`: report only concrete violations or risks introduced by the change.
- `audit`: assess existing code within the explicitly requested style-audit
  scope and report findings without editing files.
- `answer`: explain briefly and cite only the relevant rule.

## Agent Behavior

- Do not restate the full style guide in responses.
- Apply style defaults to new code and files the requested change fully owns.
  Do not make unrelated formatting or structural changes.
- In change reviews, do not flag pre-existing style violations. When explicitly
  asked to perform a style audit, assess existing code within the requested
  scope. Do not edit files during an audit. Preferences explicitly excluded
  from review findings remain excluded during audits.
- When generating or refactoring, prefer the smallest change that reaches style
  compliance.
- Style changes must preserve behavior and API compatibility. Do not change
  type design, access levels, initialization behavior, or error handling solely
  to satisfy a style preference.
- Follow explicit user instructions and applicable project instructions,
  including `AGENTS.md` and `CLAUDE.md`. Project instructions and formatter/linter
  configurations take precedence over this skill's defaults. If applicable
  project instructions or tool configurations conflict with one another and no
  explicit precedence resolves the conflict, identify it and ask the user how
  to proceed.
- Preserve established project formatting unless a formatting migration is
  requested.

## Core Rules

- Types and modules: `UpperCamelCase`.
- Functions, methods, properties, parameters, locals, globals, and enum cases:
  `lowerCamelCase`.
- Follow Apple's API Design Guidelines for names and call-site clarity.
- 120-column default limit, 4-space indentation, no tabs, no semicolons, K&R braces.
- Insert one blank line after the opening brace of non-empty `struct`, `class`,
  `actor`, and `extension` declarations, including nested declarations. Exception:
  omit the leading blank line in structs and classes containing only stored
  properties, without property observers. Computed properties, observers,
  initializers, methods, or any other declarations retain the leading blank line.
  Actors and extensions retain it even when they contain only properties.
  Do not insert a leading blank line in `enum`, `protocol`, or other blocks.
  No block has a trailing blank line.
- Prefer whole-module imports; keep imports minimal, grouped, and sorted.
- One logical primary type per file; use `// MARK: -` for meaningful sections.
- Prefer shorthand types: `[T]`, `[K: V]`, `T?` when the compiler allows them.
- Avoid force unwraps, force casts, and IUOs except as described in
  `references/PRACTICES.md`.
- Prefer `guard` for early exits and avoid unnecessary `fallthrough`.
- Document new public/open APIs for consumers of reusable libraries and packages;
  see `references/FILE_STRUCTURE.md` for scope, exceptions, and audit behavior.

## Routing

- Naming questions or violations: `references/NAMING.md`
- Imports, file layout, docs: `references/FILE_STRUCTURE.md`
- Wrapping, whitespace, braces, attributes: `references/FORMATTING.md`
- Optionals, access levels, nesting, size limits, guard, operators: `references/PRACTICES.md`
