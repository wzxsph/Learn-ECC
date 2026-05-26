# 03 - Basic Commands

## Command Types

ECC provides three command types:

### 1. Slash Commands

Used by direct input:

| Command | Function | Example |
|---------|----------|---------|
| `/ecc:plan` | Create implementation plan | `/ecc:plan "Add user authentication"` |
| `/ecc:code-review` | Code review | `/ecc:code-review` |
| `/ecc:build-fix` | Fix build errors | `/ecc:build-fix` |
| `/ecc:learn` | Extract patterns from session | `/ecc:learn` |
| `/ecc:skill-create` | Generate Skill from Git history | `/ecc:skill-create` |
| `/ecc:sessions` | Session history management | `/ecc:sessions` |

Note: Manually installed versions may use commands without prefix (like `/plan`), plugin installation uses `/ecc:` prefix.

### 2. Skills (Primary Workflow Interface)

Skills are ECC's primary workflow calling method:

| Skill | Function |
|-------|----------|
| `tdd-workflow` | Test-driven development |
| `e2e-testing` | End-to-end testing |
| `security-review` | Security review |
| `verification-loop` | Verification loop |
| `search-first` | Research-first workflow |

Calling method:
```
Use [tdd-workflow](../Reference-Docs/skills/Testing.md) skill
```

### 3. Agent Commands

Execute subtasks through Agents:

| Agent | Purpose |
|-------|---------|
| `planner` | Implementation planning |
| `code-reviewer` | Code review |
| `build-error-resolver` | Build error fixes |
| `security-reviewer` | Security review |
| `architect` | Architecture design |

Calling method:
```
@planner
```

## Common Workflows

### Develop New Feature

```
/ecc:plan "Add user authentication feature"
→ planner creates implementation blueprint
Use [tdd-workflow](../Reference-Docs/skills/Testing.md) skill
→ tdd-guide enforces writing tests first
/ecc:code-review
→ code-reviewer checks code
```

### Fix Bug

```
Use [tdd-workflow](../Reference-Docs/skills/Testing.md) skill
→ tdd-guide: Write reproduction test
→ Implement fix, verify test passes
/ecc:code-review
→ code-reviewer: Check for regressions
```

### Prepare for Release

```
/ecc:security-scan
→ security-reviewer: OWASP Top 10 audit
Use [e2e-testing](../Reference-Docs/skills/Testing.md) skill
→ e2e-runner: Critical user flow testing
/ecc:test-coverage
→ Verify 80%+ coverage
```

## MCP Commands

| Command | Function |
|---------|----------|
| `/mcp` | Manage MCP servers |
| `/ecc:status` | Check status |

## Next Steps

- [Your First Task](./04-First-Task.md) - Complete your first task
- [Return to Getting Started README](../README.md)