# 05 - Rules Authoring

> Write project-level rules, customize code review and quality standards

## Module Objectives

After completing this module, you will be able to:

- Understand Claude Code Rules format and classification
- Write security rules, code style rules, testing rules
- Create project-specific development standards
- Configure rule priorities and execution strategies

## Rules Classification System

| Category | Purpose | Examples |
|----------|---------|----------|
| **Security rules** | Security checks, vulnerability prevention | No hardcoded passwords, SQL injection prevention |
| **Code style** | Coding standards, formatting | Indentation, naming conventions, comment standards |
| **Testing rules** | Test coverage, test quality | 80% coverage requirement, AAA pattern |
| **Git rules** | Commit standards, branch strategy | Commit message format, PR workflow |
| **Performance rules** | Performance optimization, resource management | N+1 query detection, caching strategy |

## Rules Format

Rules files use Markdown format with YAML frontmatter support:

```markdown
---
name: rule-name
severity: high
trigger: tool-name
---

# Rule Name

## Rule Description

Detailed rule description...

## Check Logic

How to detect violations...

## Fix Suggestions

How to fix issues...
```

### Frontmatter Fields

| Field | Description | Required |
|-------|-------------|----------|
| name | Rule name | Yes |
| severity | Severity (critical/high/medium/low) | Yes |
| trigger | Trigger tool | No |
| enabled | Whether enabled | No |

## Security Rules

### No Hardcoded Credentials

```markdown
---
name: no-hardcoded-credentials
severity: critical
trigger: Bash
---

# No Hardcoded Credentials

## Rule Description

Hardcoding API keys, passwords, tokens and other sensitive information in code is prohibited.

## Check Logic

Use regular expressions to detect common credential patterns:
- `api[_-]?key\s*=\s*["'][A-Za-z0-9]{20,}`
- `password\s*=\s*["'][^"']+`
- `token\s*=\s*["'][A-Za-z0-9]{32,}`

## Fix Suggestions

Use environment variables:
```javascript
const apiKey = process.env.API_KEY;
```
```

### SQL Injection Prevention

```markdown
---
name: no-sql-injection
severity: critical
trigger: Read
---

# SQL Injection Prevention

## Rule Description

String concatenation to build SQL queries is prohibited.

## Check Logic

Detect string-concatenated SQL statements:
- `"SELECT * FROM users WHERE id = " + userId`
- `query("SELECT * FROM " + tableName)`

## Fix Suggestions

Use parameterized queries:
```javascript
db.query("SELECT * FROM users WHERE id = ?", [userId]);
```
```

## Code Style Rules

### Function Length Limit

```markdown
---
name: max-function-length
severity: medium
---

# Function Length Limit

## Rule Description

Functions should remain short, ideally no more than 50 lines.

## Check Logic

Count function lines, warn if exceeding 50 lines.

## Fix Suggestions

Split long functions into multiple smaller functions, each responsible for a single responsibility.
```

### Variable Naming Standards

```markdown
---
name: consistent-naming
severity: low
---

# Variable Naming Standards

## Rule Description

Use consistent naming conventions.

## Check Logic

- Variables and functions use camelCase
- Constants use UPPER_SNAKE_CASE
- Class names use PascalCase

## Fix Suggestions

Reference the project's naming-conventions.md.
```

## Testing Rules

### Coverage Requirements

```markdown
---
name: minimum-test-coverage
severity: high
---

# Minimum Test Coverage

## Rule Description

All new code must have more than 80% test coverage.

## Check Logic

Run test coverage tools to check if 80% is reached.

## Fix Suggestions

Write test cases for uncovered code.
```

### AAA Test Pattern

```markdown
---
name: aaa-test-pattern
severity: medium
---

# AAA Test Pattern

## Rule Description

Tests should follow the Arrange-Act-Assert structure.

## Check Logic

Check if tests contain three distinct phases.

## Fix Suggestions

Rewrite tests with clear separation:
1. Arrange - Prepare test data
2. Act - Execute the operation under test
3. Assert - Verify results
```

## Git Rules

### Commit Message Format

```markdown
---
name: conventional-commits
severity: medium
trigger: Bash
condition: command.includes('git commit')
---

# Commit Message Format

## Rule Description

Follow Conventional Commits specification.

## Check Logic

Verify commit message format:
```
<type>: <description>

[optional body]
```

Types: feat, fix, docs, test, refactor, perf, ci, chore

## Fix Suggestions

Rewrite commit message with correct format.
```

## Rule Priorities

| Level | Description | Behavior |
|-------|-------------|----------|
| critical | Serious security risk | Block immediately |
| high | Important quality issue | Warn and block |
| medium | Suggested improvement | Warn but don't block |
| low | Style suggestion | Hint only |

## Writing Project Rules

### Step 1: Analyze Project Requirements

```markdown
# Project Rules Requirements Analysis

## Must-have Standards
1. Security: No hardcoded credentials
2. Testing: 80% coverage for new code
3. Commit: Follow Conventional Commits

## Should-have Standards
1. Functions no more than 50 lines
2. Files no more than 800 lines
3. No console.log
```

### Step 2: Create Rule Files

Create `.claude/rules/` directory in the project:

```
project/
└── .claude/
    └── rules/
        ├── security.md
        ├── testing.md
        ├── coding-style.md
        └── git.md
```

### Step 3: Configure Rule Priorities

Create `rules.json` configuration for execution order:

```json
{
  "order": [
    "security/no-hardcoded-credentials",
    "security/no-sql-injection",
    "testing/minimum-test-coverage",
    "coding-style/max-function-length"
  ]
}
```

## Reference Materials

Complete Rules documentation is located at: `../../Reference-Docs/rules/Git工作流.md`

## Next Steps

- Learn [exercises](./exercises/练习.md)
- Read [complete Rules documentation](../../Reference-Docs/rules/Git工作流.md)