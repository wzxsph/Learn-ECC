# Pull Request Workflow Commands

## Overview

Pull Request workflow commands are used for creating, managing, and reviewing GitHub Pull Requests.

## Command List

### /pr

**Purpose**: Create GitHub PR from current branch, with unpushed commits

**Description**: Discover templates, analyze changes, push branch and create PR. Analyze full commit history, use `git diff [base-branch]...HEAD` to see all changes.

**Input**: `$ARGUMENTS` - Optional, can contain base branch name and/or flags (like `--draft`)

**Parsing Rules**:
- Extract any recognized flags (like `--draft`)
- Use remaining non-flag text as base branch name
- Default to `main` if not specified

**Workflow**:

| Phase | Description |
|---|---|
| **Phase 1: VALIDATE** | Check prerequisites (not on base branch, no uncommitted changes, have ahead commits, no existing PR) |
| **Phase 2: DISCOVER** | Search PR template, analyze commits, analyze file changes, check related planning artifacts |
| **Phase 3: PUSH** | Push branch (rebase first if needed) |
| **Phase 4: CREATE** | Create PR using template or default format |
| **Phase 5: VERIFY** | Verify PR created successfully |
| **Phase 6: OUTPUT** | Report PR URL and next steps |

**PR Creation Validation Checks**:

| Check | Condition | Action on Failure |
|---|---|---|
| Not on base branch | Current branch ≠ base | Stop: "Switch to feature branch first" |
| Clean workspace | No uncommitted changes | Warning: "Commit or stash first" |
| Have ahead commits | `git log origin/<base>..HEAD` non-empty | Stop: "No ahead commits" |
| No existing PR | `gh pr list` is empty | Stop: "PR already exists" |

**Template Search Order**:
1. `.github/PULL_REQUEST_TEMPLATE/` directory (if exists)
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

**Default PR Format** (when no template):
```markdown
## Summary
<1-2 sentence description>

## Changes
<bulleted list grouped by area>

## Files Changed
<table of changed files>

## Testing
<how changes were tested, or "Needs testing">

## Related Issues
<link issues or "None">
```

**Edge Case Handling**:
- **No `gh` CLI**: Stop and prompt to install
- **Not authenticated**: Prompt to run `gh auth login`
- **Branch diverged**: Use `git fetch && git rebase` then push
- **Large PR (>20 files)**: Warn and suggest splitting

**Integration with Other Commands**:
- Use `/code-review <number>` to review PR
- Use `gh pr merge <number>` to merge ready PR
- Use `/plan-prd` or `/plan` to create related planning artifacts

---

### /review-pr

**Purpose**: Review GitHub PR or local uncommitted changes

**Description**: Review Pull Request on GitHub or local uncommitted changes. PR review mode adapted from PRPs-agentic-eng.

**Input**: `$ARGUMENTS` - Can be PR number, PR URL, or blank (local review)

---

#### Mode Selection

| Input | Action |
|---|---|
| PR number (like `42`) | Use as PR number |
| URL (like `github.com/.../pull/42`) | Extract PR number |
| Branch name | Find PR via `gh pr list --head <branch>` |
| Blank | Use local review mode |

---

#### Local Review Mode

**Workflow**:
1. **GATHER** - Collect changes: `git diff --name-only HEAD`
2. **REVIEW** - Review each file (security issues, quality issues, best practices)
3. **REPORT** - Generate report with severity levels and fix suggestions

**Review Checklist**:

| Category | Check Items |
|---|---|
| **Security Issues (CRITICAL)** | Hardcoded credentials, SQL injection, XSS, missing input validation, insecure dependencies, path traversal |
| **Code Quality (HIGH)** | Functions >50 lines, files >800 lines, nesting >4 levels, missing error handling, console.log, TODO/FIXME |
| **Best Practices (MEDIUM)** | Change patterns, emoji usage, new code missing tests, accessibility issues |

**Decision**:

| Condition | Decision |
|---|---|
| CRITICAL or HIGH issues exist | **BLOCK** - Block commit |
| MEDIUM/LOW issues only | Pass with comments |
| No issues and validation passes | **APPROVE** |

---

#### PR Review Mode

**Workflow**:
1. **FETCH** - Get PR metadata and diff
2. **CONTEXT** - Build review context (project rules, planning artifacts, changed files)
3. **REVIEW** - Read full files, apply 7-category review checklist
4. **VALIDATE** - Run project-type validation commands
5. **DECIDE** - Form suggestions based on findings
6. **REPORT** - Create review artifact
7. **PUBLISH** - Publish review to GitHub
8. **OUTPUT** - Report to user

**7-Category Review Checklist**:

| Category | Check Items |
|---|---|
| **Correctness** | Logic errors, edge cases, null handling, race conditions |
| **Type Safety** | Type mismatches, unsafe type casts, `any` usage, missing generics |
| **Pattern Compliance** | Matches project conventions (naming, file structure, error handling, imports) |
| **Security** | Injection, auth gaps, secret exposure, SSRF, path traversal, XSS |
| **Performance** | N+1 queries, missing indexes, unbounded loops, memory leaks, large payloads |
| **Completeness** | Missing tests, missing error handling, incomplete migrations, missing documentation |
| **Maintainability** | Dead code, magic numbers, deep nesting, poor naming, missing types |

**Validation Command Detection**:

| Project Type | Detection File | Validation Commands |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

**Decision Levels**:

| Level | Meaning | Action |
|---|---|---|
| CRITICAL | Security vulnerability or data loss risk | Must fix to merge |
| HIGH | Bug or logic error that could cause problems | Should fix before merge |
| MEDIUM | Code quality or missing best practices | Suggested fix |
| LOW | Style issue or small suggestion | Optional |

**Publish Review to GitHub**:
```bash
# APPROVE
gh pr review <NUMBER> --approve --body "<summary>"

# REQUEST_CHANGES
gh pr review <NUMBER> --request-changes --body "<summary with required fixes>"

# COMMENT only (draft PR or informational)
gh pr review <NUMBER> --comment --body "<summary>"
```

---

### /multi-workflow (To be completed)

> (To be completed) Command details pending

**Purpose**: Multi-model collaborative development

**Description**: Coordinate multiple AI models for collaborative development.

---

### /multi-backend (To be completed)

> (To be completed) Command details pending

**Purpose**: Backend multi-model development

**Description**: Multi-model collaboration centered on backend development.

---

### /multi-frontend (To be completed)

> (To be completed) Command details pending

**Purpose**: Frontend multi-model development

**Description**: Multi-model collaboration centered on frontend development.

---

### /multi-execute (To be completed)

> (To be completed) Command details pending

**Purpose**: Multi-model collaborative execution

**Description**: Coordinate multiple models to execute tasks in parallel.

---

## Related Commands

- `/pr` - Create PR
- `/review-pr` - Review PR
- `/multi-workflow` - Multi-model collaboration