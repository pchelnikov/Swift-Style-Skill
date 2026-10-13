# Swift Style Guide Skill

A practical, opinionated Swift style skill for writing, reviewing, and
refactoring modern Swift code. Covers naming, formatting, file organization,
documentation, and coding conventions. Project instructions and formatter/linter
configurations take precedence over its defaults.

Built on the [Agent Skills open format](https://agentskills.io/home).

## Migrating to 3.0.0

Version 3.0.0 establishes independent Swift style conventions. The default line
limit increases from 100 to 120 columns. Documentation is required for new
public/open APIs intended for consumers of reusable libraries and packages,
with documented exceptions. Functions over 50 lines and files over 500 lines
prompt readability assessment, not findings based solely on length. Change
reviews focus on new violations; explicit style audits can assess existing code
and remain read-only. Project instructions and tool configurations override
these defaults.

## Formatting and Precedence

- Use a 120-column limit by default.
- Follow explicit user instructions and applicable project instructions,
  including `AGENTS.md` and `CLAUDE.md`. Project instructions and formatter/linter
  configurations take precedence over this skill's defaults. If applicable
  project instructions or tool configurations conflict with one another and no
  explicit precedence resolves the conflict, identify it and ask the user how
  to proceed.
- Use 4-space indentation, including one additional level for wrapped function
  parameters and call arguments.
- Insert one blank line after the opening brace of non-empty `struct`, `class`,
  `actor`, and `extension` declarations, including nested declarations. Do not insert
  a leading blank line in `enum`, `protocol`, or other blocks. No block has
  a trailing blank line.
- Preserve established project formatting unless a formatting migration is
  requested.

See [the formatting reference](swift-style-skill/references/FORMATTING.md) for
the complete formatting rules.

## Who This Is For

- Teams who want consistent Swift style enforced automatically during code generation and review
- Developers generating new Swift code and wanting idiomatic naming and formatting applied by default
- Anyone reviewing or refactoring Swift code for style violations such as force unwraps, poor naming, import issues, and formatting drift
- Projects that want their AI agent to apply a consistent set of style conventions

## How to Use This Skill

### Option A: Using skills.sh (recommended)

Install this skill with a single command:

```
npx skills add https://github.com/pchelnikov/swift-style-skill --skill swift-style-skill
```

Then use the skill in your AI agent, for example:

> Use the swift style skill and review this file for naming and formatting violations

### Option B: Claude.ai

Upload the `swift-style-skill/` folder as a zip file through **Settings > Features** (requires Pro, Max, Team, or Enterprise plan with code execution enabled).

### Option C: Claude API

Upload via the `/v1/skills` endpoint and reference by `skill_id` in your API calls. See [Using Skills with the Claude API](https://platform.claude.com/docs/en/build-with-claude/skills-guide).

### Option D: Manual Install

1. **Clone** this repository.
2. **Install or symlink** the `swift-style-skill/` folder following your tool's official skills installation docs (see links below).
3. **Use your AI tool** as usual — the skill can then be invoked for Swift code generation, review, or style audit tasks.

#### Where to Save Skills

Follow your tool's official documentation:

- **Codex:** [Where to save skills](https://developers.openai.com/codex/skills/#where-to-save-skills)
- **Claude:** [Using Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#using-skills)
- **Cursor:** [Enabling Skills](https://cursor.com/docs/context/skills#enabling-skills)

**How to verify:** Your agent should use `swift-style-skill/SKILL.md` as a
dispatcher, then open only the relevant reference file for the current task.

## What This Skill Offers

This skill gives your AI coding tool a concrete, rule-based Swift style standard. It can:

### Generate Style-Compliant Swift Code

- Apply correct casing: UpperCamelCase for types, lowerCamelCase for everything else
- Name symbols for clarity at the call site — omitting needless words, following delegate naming conventions
- Structure files with proper import ordering, `// MARK:` sections, and one primary type per file
- Format declarations within the default 120-column limit using consistent wrapping rules

### Review Existing Code for Style Violations

- Keep change reviews focused on new violations without flagging pre-existing
  style violations or requiring unrelated formatting or structural changes
- Assess existing code during explicitly requested style audits, within the
  requested scope and without editing files; preferences excluded from review
  findings remain excluded
- Detect naming violations against the skill’s conventions
- Audit imports: explicit, minimal, whole-module, lexicographically ordered
- Catch force unwraps, unsafe IUOs, and `fallthrough` misuse
- Route to the relevant reference file instead of loading a large handbook up front

### Enforce Practices That Prevent Bugs

- Prefer `guard` for early exits instead of deeply nested `if` chains
- Flag force unwraps and force casts without a clear or documented safety invariant
- Apply the skill’s access-level conventions
- Check documentation for new `public` and `open` APIs intended for consumers
  of reusable libraries and packages, regardless of build system, subject to
  the documented exceptions; assess existing undocumented APIs only during an
  explicit documentation audit within the same library/package scope

## What Makes This Skill Different

**Practical and Opinionated:** Explicit conventions for modern Swift code,
with project instructions and formatter/linter configurations taking precedence over the
skill’s defaults.

**Focused on Swift Style:** Covers language-level conventions. Framework-specific
rules, including SwiftUI view layout and property organization, and application
architecture are outside its scope.

**Dispatcher-Based:** `SKILL.md` is a compact router. It tells the agent which mode it is in, which core rules to apply immediately, and which single reference file to open next.

**Token Efficient:** The top-level skill is intentionally small and pushes detail
into focused reference files so the agent does not load everything by default.

## Skill Structure

```
swift-style-skill/
├── SKILL.md                  # Dispatcher, scope, workflow, core rules, routing
└── references/
    ├── FILE_STRUCTURE.md     # File naming, import ordering, MARK comments, doc comments
    ├── FORMATTING.md         # Column limit, braces, semicolons, line-wrapping, whitespace
    ├── NAMING.md             # Casing rules, acronyms, clarity, delegate naming, access control
    └── PRACTICES.md          # Optionals, force unwrap, access levels, guard, switch, operators
```

## Token Budget

The skill uses progressive disclosure to limit context usage. Load `SKILL.md`
first, then only the references relevant to the task. Actual token usage depends
on the selected references and the host tool's packaging.

## Sources

- [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) — naming foundation
- [Swift Standard Library](https://github.com/swiftlang/swift/tree/main/stdlib) — reference for Swift API conventions

## Contributing

Contributions are welcome! When submitting changes:

- Proposed rules should have a clear readability or consistency benefit and fit the skill’s style scope
- Code examples should be minimal and self-contained
- Keep `SKILL.md` compact; move detail into the most relevant reference file
- Keep contributions focused on Swift style; exclude framework-specific
  conventions and application architecture

## License

This skill is open-source and available under the MIT License. See [LICENSE](LICENSE) for details.
