# Swift Style Guide Skill

Ensure your AI coding tool generates and reviews Swift code that follows the
Google Swift Style Guide for naming, formatting, file structure, documentation,
and guide-backed programming practices.

Built on the [Agent Skills open format](https://agentskills.io/home). Every rule traces to the [Google Swift Style Guide](https://google.github.io/swift/), which is itself grounded in Apple's Swift standard library style.

## Who This Is For

- Teams who want consistent Swift style enforced automatically during code generation and review
- Developers generating new Swift code and wanting idiomatic naming and formatting applied by default
- Anyone reviewing or refactoring Swift code for style violations such as force unwraps, poor naming, import issues, and formatting drift
- Projects that need their AI agent to apply a concrete, well-established style standard

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
3. **Use your AI tool** as usual — the skill triggers automatically on any Swift code generation, review, or style audit task.

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
- Format declarations within the 100-character column limit using consistent wrapping rules

### Review Existing Code for Style Violations

- Detect naming violations against the Google Swift conventions
- Audit imports: explicit, minimal, whole-module, lexicographically ordered
- Catch force unwraps, unsafe IUOs, and `fallthrough` misuse
- Route to the relevant reference file instead of loading a large handbook up front

### Enforce Practices That Prevent Bugs

- Require `guard` for early exits instead of deeply nested `if` chains
- Flag force unwrap and force cast in production code
- Apply the guide's real access-level rules instead of stronger local policy
- Catch missing documentation on `public` and `open` API, subject to the guide's exceptions

## What Makes This Skill Different

**Style Guide Grounded:** Every rule traces to the Google Swift Style Guide (https://google.github.io/swift/), itself based on Apple's Swift standard library style. No personal opinions, no folklore.

**Non-Opinionated Beyond Style:** No architecture enforcement (MVVM, TCA, VIPER). No framework-specific rules. Style only; how you structure your app is your decision.

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

The skill uses progressive disclosure to minimise context window usage:

| Load Level | When | ~Tokens |
|------------|------|---------|
| Metadata only | Every conversation | ~130 |
| `SKILL.md` triggered | Swift style task detected | ~500 |
| `SKILL.md` + one reference | Most real tasks | ~1,000–1,200 |
| All files loaded | Full audit worst case | ~2,600 |

## Sources

- [Google Swift Style Guide](https://google.github.io/swift/) — primary source for all rules
- [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) — naming foundation
- [Swift Standard Library](https://github.com/swiftlang/swift/tree/main/stdlib) — style reference the guide is grounded in

## Contributing

Contributions are welcome! When submitting changes:

- Every rule must trace to the Google Swift Style Guide or Apple's API Design Guidelines
- Code examples should be minimal and self-contained
- Keep `SKILL.md` compact; move detail into the most relevant reference file
- Do not add architecture opinions or framework-specific rules

## License

This skill is open-source and available under the MIT License. See [LICENSE](LICENSE) for details.
