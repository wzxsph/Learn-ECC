# 05 - Rules Authoring

Learn how to write customized development rules.

## Overview

ECC [Rules](../../Reference-Docs/rules/代码风格.md) are a set of mandatory development rules covering code style, testing, security, Git workflows, agent orchestration, and performance optimization. These rules ensure code quality and consistency, enforced through automated checks and review processes.

---

## 1. Rules Format

### 1.1 Basic Format

Rules use Markdown format with YAML frontmatter:

```yaml
---
name: rule-name
description: Rule description
category: category
---
```

### 1.2 Rule Levels

| Level | Description | Priority |
|-------|-------------|----------|
| **Global rules** | `~/.claude/rules/` | Highest |
| **Project rules** | Project `.claude/rules/` | Medium |
| **Local rules** | Current session | Lowest |

### 1.3 Priority

Global rules > Project rules > Local rules

---

## 2. Rule Types Detailed

### 2.1 Code Style Rules

Define core principles and best practices for code organization.

#### Core Requirements

**Immutability (Critical Principle)**

```typescript
// Wrong example - Mutating original object
function modify(user, name) {
  user.name = name;  // Mutates original object
  return user;
}

// Correct example - Return new object
function update(user, name) {
  return { ...user, name };  // Creates new copy
}
```

**Core Design Principles**

| Principle | Description | Practice |
|-----------|-------------|----------|
| **KISS** | Keep It Simple | Prefer the simplest viable solution |
| **DRY** | Don't Repeat Yourself | Avoid copy-paste caused implementation drift |
| **YAGNI** | You Aren't Gonna Need It | Refactor when actually needed |

**Naming Conventions**

| Type | Convention | Example |
|------|------------|---------|
| Variables and functions | camelCase | `calculateTotal` |
| Booleans | is/has/should/can prefix | `isActive` |
| Interfaces, types, components | PascalCase | `UserProfile` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |

### 2.2 Testing Rules

Mandatory testing standards that ensure code quality.

#### Minimum Test Coverage: 80%

| Test Type | Description |
|-----------|-------------|
| Unit tests | Independent functions, utilities, components |
| Integration tests | API endpoints, database operations |
| E2E tests | Critical user flows |

#### TDD Workflow

```
1. Write tests (RED)   - Write a failing test first
2. Run tests           - Verify test fails
3. Write minimal implementation (GREEN) - Write minimum code that passes
4. Run tests           - Verify test passes
5. Refactor (IMPROVE)   - Improve code structure
6. Verify coverage      - Ensure 80%+ coverage
```

### 2.3 Security Rules

Mandatory security checklist.

#### Pre-Commit Checks

- [ ] No hardcoded credentials (API keys, passwords, tokens)
- [ ] All user inputs validated
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (output escaping)
- [ ] CSRF protection enabled
- [ ] Authentication/authorization verified
- [ ] Rate limiting
- [ ] Error messages don't leak sensitive data

#### Secret Management

- **Never** hardcode secrets in source code
- **Always** use environment variables or secret manager
- Validate required secrets at startup
- Rotate any secrets that may have been exposed

### 2.4 Git Workflow Rules

Git commit standards and PR workflow.

#### Commit Message Format

```
<type>: <description>

<optional body>
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Refactoring
- `docs`: Documentation
- `test`: Test
- `chore`: Maintenance
- `perf`: Performance
- `ci`: CI/CD

#### PR Workflow

1. Analyze full commit history
2. Use `git diff [base-branch]...HEAD` to see all changes
3. Draft comprehensive PR summary
4. Include test plan and TODOs
5. If new branch, push with `-u` flag

### 2.5 Agent Orchestration Rules

Multi-Agent collaboration patterns.

#### Immediate Use Principles

| Scenario | Agent |
|----------|-------|
| Complex feature request | **planner** |
| Code just written/modified | **code-reviewer** |
| Bug fix or new feature | **tdd-guide** |
| Architecture decisions | **architect** |

#### Parallel Task Execution

```markdown
# Good: Parallel execution
Launch 3 Agents in parallel:
1. Agent 1: Authentication module security analysis
2. Agent 2: Cache system performance review
3. Agent 3: Tool type checking

# Bad: Unnecessary sequential execution
First agent 1, then agent 2, then agent 3
```

### 2.6 Performance Optimization Rules

Model selection and context management.

#### Model Selection Strategy

| Model | Purpose | Cost |
|-------|---------|------|
| **Haiku 4.5** | Lightweight agents, frequent invocation, pair programming | Low |
| **Sonnet 4.6** | Main development work, multi-agent orchestration | Medium |
| **Opus 4.5** | Complex architecture decisions, deep reasoning | High |

#### Context Window Management

Avoid doing in the last 20% of context window:
- Large-scale refactoring
- Feature implementation across multiple files
- Complex interaction debugging

---

## 3. Rule Authoring

### 3.1 Trigger Conditions

```yaml
---
name: no-console-log
description: Block commits containing console.log
trigger:
  - pre-commit
  - pre-push
---
```

### 3.2 Check Logic

```markdown
## Check Logic

1. Scan modified files
2. Look for `console.log`, `console.error`, etc.
3. If found, block commit and show warning
```

### 3.3 Fix Suggestions

```markdown
## Fix Suggestions

- Use structured logging library (like winston, pino)
- Configure log levels (debug, info, warn, error)
- Ensure production environment log levels are appropriate
```

---

## 4. Violation Handling

### 4.1 Severity Levels

| Level | Meaning | Action |
|-------|---------|--------|
| CRITICAL | Security vulnerability or data loss risk | **Block** - Must fix before merge |
| HIGH | Bug or significant quality issue | **Warn** - Should fix before merge |
| MEDIUM | Maintainability concern | **Hint** - Suggest fixing |
| LOW | Style or minor suggestion | **Note** - Optional |

### 4.2 Fix Flow

```
1. Analyze violation reason
2. Determine fix plan
3. Execute fix
4. Verify fix
5. Resubmit for review
```

---

## 5. Code Quality Checklist

Must check before marking work complete:

- [ ] Code is readable and well-named
- [ ] Functions are short (<50 lines)
- [ ] Files are focused (<800 lines)
- [ ] No deep nesting (>4 levels)
- [ ] Proper error handling
- [ ] No hardcoded values (use constants or config)
- [ ] Use immutable patterns (no mutation)
- [ ] Test coverage ≥80%
- [ ] All security checks pass

---

## 6. Creating Project-Specific Rules

### 6.1 Project Rules Location

```
project/
└── .claude/
    └── rules/
        ├── README.md
        └── Custom rule files
```

### 6.2 Inherit Global Rules

Project rules inherit from global rules, can extend on top:

```markdown
# Project-Specific Rules

Inherited from global rules, with additional requirements:

## Additional Requirements

- [ ] Project-specific coding standards
- [ ] Specific testing requirements
- [ ] Specific documentation requirements
```

---

## Exercises

Complete tasks in [exercises](./exercises/练习.md).

---

[Return to Core Capabilities README](../README.md)