---
name: setup-pre-commit
description: Set up pre-commit hooks for formatting, linting, type checking, and tests in the current repo. Detects the project's language and toolchain to choose the right hook framework. Use when user wants to add pre-commit hooks, set up commit-time checks, or add formatting/linting/testing on commit.
---

# Setup Pre-Commit Hooks

## What This Sets Up

- A **pre-commit hook** that runs formatting, linting, type checking, and/or tests before each commit
- Uses the appropriate hook framework for the project's language/toolchain

## Steps

### 1. Detect the project's language and toolchain

Explore the repo to determine:

- **Primary language(s)** — look at file extensions, build files, config files
- **Package manager** — cargo, go, pip/poetry/uv, npm/pnpm/yarn/bun, mix, gem, etc.
- **Existing formatter/linter** — check for config files (.prettierrc, rustfmt.toml, .golangci.yml, pyproject.toml, .rubocop.yml, etc.)
- **Existing test runner** — check for test scripts, test directories, test config
- **Existing type checker** — mypy, pyright, tsc, etc.

### 2. Choose hook framework

Pick the appropriate approach based on what the project already uses:

- **If the project already has a hook framework** (husky, pre-commit, lefthook, etc.) — use it
- **Otherwise, prefer the simplest option for the ecosystem:**
  - Python/polyglot: [pre-commit](https://pre-commit.com/) framework
  - JS/TS: Husky + lint-staged
  - Rust: cargo-husky or a simple git hook script
  - Go: a simple git hook script or lefthook
  - Other: a direct `.git/hooks/pre-commit` script

### 3. Install and configure

Install the chosen framework and configure it to run:

1. **Formatter** on staged files (fast, staged-only)
2. **Linter** if available (fast, staged-only where possible)
3. **Type checker** if applicable (full project check)
4. **Tests** if a test script exists (full suite or relevant subset)

Order matters — run fast checks first so failures are caught early.

### 4. Verify

- [ ] Hook file exists and is executable
- [ ] Run the hook manually to verify it works
- [ ] Confirm each check runs in the right order

### 5. Commit

Stage all changed/created files and commit. This commit will itself run through the new hooks — a good smoke test.

## Notes

- Always check what's already in place before installing anything new
- Prefer running formatters on staged files only (faster feedback)
- Type checkers and test suites typically need the full project, not just staged files
- If the project has no formatter, linter, or tests, tell the user what's missing rather than installing defaults
