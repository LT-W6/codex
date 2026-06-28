# Codex

**How we build at LT-W6.**

This is the engineering reference for every repository under the LT-W6 organisation. It covers code structure, implementation methodology, review standards, and the principles that sit behind every decision we make in public.

If you are contributing to any LT-W6 repository, read this first.

---

## Principles

These are not rules imposed from outside. They are the conclusions we keep arriving at.

**Correctness before cleverness.**
Code that is easy to understand is easier to verify. Verification matters more than elegance in financial logic. If a simpler implementation exists, use it.

**Explicit over implicit.**
Every assumption should be visible. In Rust this means using `Result<T, E>` over panics, named structs over tuples, and typed enums over raw strings. A reader should never have to guess what a value represents.

**Errors are first-class.**
Failure modes are part of the design, not an afterthought. Every function that can fail returns a typed error. Error messages are written for the person reading them at 2am under pressure — not for the person writing them at 10am in comfort.

**Small, focused modules.**
Each module does one thing and does it completely. The parser does not validate. The validator does not score. The scorer does not format output. Boundaries are enforced, not suggested.

**The test is the specification.**
If a behaviour is not tested it is not guaranteed. Tests live close to the code they cover. Fixtures are real — drawn from actual market structures, not invented placeholders.

---

## Repository structure

Every LT-W6 repository follows this layout unless there is a documented reason not to:

```
repo-name/
├── src/              # All source code
├── examples/         # Working, runnable examples — not toys
├── docs/             # Specifications, architecture notes, decision records
├── tests/            # Integration tests
│   └── fixtures/     # Real-world test inputs
├── Cargo.toml        # (Rust) or equivalent manifest
├── README.md         # What this repo is and how to use it
└── CHANGELOG.md      # What changed and when
```

No exceptions to the `README.md` requirement. No exceptions to the `examples/` requirement. Examples are the first thing a new contributor reads after the README. They must work, and they must be honest about what the code does.

---

## Code style — Rust

**Formatting:** `rustfmt` with default settings. No custom formatting rules. Consistency over preference.

**Naming:**
- Types and enums: `PascalCase`
- Functions and variables: `snake_case`
- Constants: `SCREAMING_SNAKE_CASE`
- Modules: `snake_case`

**Error handling:**
- Never use `unwrap()` in library code
- Use `?` for propagation
- Define domain-specific error types with `thiserror`
- Error variants should read as sentences: `InvalidOriginCode`, `MarginBelowMinimum`

**Comments:**
- Comment the *why*, not the *what*
- If the code needs a comment to explain what it does, consider renaming first
- Public API items require doc comments (`///`)

**Dependencies:**
- Every new dependency requires a documented reason in the PR
- Prefer the standard library where sufficient
- Audit dependencies before merging — `cargo audit` is not optional

---

## Commit discipline

```
type: short description in present tense

Longer explanation if needed. What changed and why.
Reference any relevant issues or decisions.
```

**Types:**
- `feat` — new capability
- `fix` — corrects a bug
- `refactor` — restructures without changing behaviour
- `docs` — documentation only
- `test` — adds or corrects tests
- `chore` — maintenance, dependency updates, tooling

**Rules:**
- One logical change per commit
- The commit message explains *why*, the diff explains *what*
- No "WIP", "fix", or "asdf" commits on main

---

## Pull request standards

Every PR requires:

- A clear description of what changed and why
- At least one test covering the new behaviour
- Updated documentation if public API changed
- A passing CI run before review is requested

Every PR will receive:

- At least one reviewer who was not involved in writing it
- Feedback on correctness first, style second
- A decision within 72 hours of review request

No PR merges without review. No exceptions for small changes. Small changes are where assumptions hide.

---

## Architecture decision records

Significant decisions — why we chose `pest` over `nom`, why signals are an enum not a float score, why the CLI separates `validate` from `score` — are documented in `docs/decisions/` as short markdown files.

Format:

```
# ADR-001: Title

## Status
Accepted

## Context
What situation forced this decision.

## Decision
What we decided.

## Consequences
What this makes easier. What this makes harder.
```

If you are making a decision that future contributors will have to live with, write the ADR before writing the code.

---

## What does not belong here

Production system architecture. Execution engine internals. Capital allocation logic. Proprietary data formats.

This codex covers LT-W6's public repositories. Loop Trade's internal engineering standards are a separate document.

---

## Versioning

This codex is versioned. Breaking changes to standards are documented in `CHANGELOG.md` with a rationale. Contributors are notified via a pinned issue when a new version is published.

**Current version: 1.0.0**

---

*We build the way we trade — with precision, repeatability, and no tolerance for ambiguity.*
