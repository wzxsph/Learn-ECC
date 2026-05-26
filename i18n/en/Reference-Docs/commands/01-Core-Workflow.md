# Core Workflow Commands

This document introduces commands for core development workflows in ECC.

---

## /plan

**Purpose**: Restate requirements, assess risks, and create a step-by-step implementation plan. **Waits for user confirmation** before touching any code.

**Usage**:
```
/plan [feature description | PRD file path]
```

**Use Cases**:
- Starting new feature development
- Major architecture changes
- Complex refactoring work
- When requirements are unclear or ambiguous

**Detailed Description**:
This command will:
1. **Restate requirements** - Clearly express what needs to be built
2. **Identify risks** - Reveal potential problems and obstacles
3. **Create step-by-step plan** - Decompose implementation into phases
4. **Wait for confirmation** - Must receive explicit user approval before proceeding

Supports multiple input modes:
| Input | Mode | Behavior |
|-------|------|----------|
| `path/to/name.prd.md` | PRD mode | Read PRD, select next delivery milestone to process |
| Other markdown path | Reference mode | Read file as context and generate inline plan |
| Free-form text | Conversation mode | Generate inline plan |
| Empty input | Clarification mode | Ask what should be planned |

---

## /code-review

**Purpose**: Comprehensive code review for local uncommitted changes or GitHub PRs.

**Usage**:
```
/code-review                 # Local review mode
/code-review [PR number | PR link]  # PR review mode
```

**Use Cases**:
- Comprehensive review before committing code
- PR review to discover potential issues
- Learning best practices in codebase

**Local Review Mode**:
1. Collect changed files (`git diff --name-only HEAD`)
2. Check for security issues (hardcoded credentials, SQL injection, XSS, etc.)
3. Check for code quality issues (functions >50 lines, files >800 lines, etc.)
4. Generate report with severity labels

**PR Review Mode**:
1. Get PR metadata and diff
2. Check relevant project standards and planning documents
3. Read complete changed files
4. Apply 7 review categories (correctness, type safety, pattern compliance, security, performance, completeness, maintainability)
5. Run verification commands (type check, lint, test, build)
6. Post review comments to GitHub

---

## /build-fix

**Purpose**: Detect project build system and incrementally fix build/type errors.

**Usage**:
```
/build-fix
```

**Use Cases**:
- When build fails
- TypeScript/JavaScript type errors
- Compilation errors
- Dependency issues

**Workflow**:
1. **Detect build system** - Select appropriate build command based on file type
2. **Parse errors** - Group by file and dependency order
3. **Fix incrementally** - Fix one error at a time
4. **Verify** - Re-run build after each fix

Supported build systems:

| Indicator | Build Command |
|----------|--------------|
| `package.json` + `build` script | `npm run build` |
| `tsconfig.json` (TypeScript) | `npx tsc --noEmit` |
| `Cargo.toml` | `cargo build` |
| `pom.xml` | `mvn compile` |
| `build.gradle` | `./gradlew compileJava` |
| `go.mod` | `go build ./...` |
| `pyproject.toml` | `python -m compileall` or `mypy` |

---

## /verify

**Purpose**: Verify that code changes work as expected. Used to verify PRs, confirm fixes, or test changes.

**Usage**:
```
/verify [quick]           # Quick verification (recommended)
/verify full             # Full verification
```

**Use Cases**:
- Verifying PR changes
- Confirming fixes are effective
- Manual testing of changes
- Local verification before pushing

**Verification Steps**:
1. Run relevant tests
2. Check type safety
3. Verify build succeeds
4. Check linting

---

## /quality-gate

**Purpose**: Run ECC quality pipeline for file or project scope and report fix steps.

**Usage**:
```
/quality-gate [path|.] [--fix] [--strict]
```

**Use Cases**:
- Run quality checks on demand
- Integrate in CI pipelines
- Ensure code meets project standards

**Pipeline Steps**:
1. Detect target language/tools
2. Run formatter checks
3. Run lint/type checks
4. Generate concise fix list

---

## /tdd

**Purpose**: Test-driven development workflow. Follows red-green-refactor cycle.

**Usage**:
```
/tdd                      # Start TDD workflow
```

**Use Cases**:
- Implementing new features
- Fixing bugs (write failing test first)
- Adding test coverage
- Learning TDD methodology

**TDD Cycle**:
```
RED    → Write a failing test
GREEN  → Write minimum code to make test pass
REFACTOR → Improve code while keeping tests green
REPEAT → Next test case
```

**Best Practices**:
- **Write tests first** - Before any implementation
- **Run tests after every change** - Verify progress
- **Test behavior, not implementation** - Focus on interface not details
- **Include edge cases** - Empty values, null, max values, etc.

---

## Command Integration

```
Unclear requirements → /plan-prd (Create PRD)
     ↓
Requirements clear → /plan (Create implementation plan)
     ↓
/tdd (or /feature-dev) implementation
     ↓
/build-fix fix build errors
     ↓
/code-review code review
     ↓
/quality-gate quality gate
     ↓
/verify verify
     ↓
/pr create PR
```