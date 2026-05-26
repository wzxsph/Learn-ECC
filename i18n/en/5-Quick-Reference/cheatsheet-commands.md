# Commands Cheatsheet

## Slash Commands

| Command | Function | Use Case |
|---------|----------|----------|
| `/tdd` | Test-driven development workflow | New feature development |
| `/plan` | Generate implementation plan | Complex task planning |
| `/code-review` | [Code review](../Reference-Docs/commands/01-核心工作流.md) | Code quality check |
| `/build-fix` | [Build fix](../Reference-Docs/commands/04-构建修复.md) | When build fails |
| `/learn` | Extract patterns from session | Knowledge sedimentation |
| `/skill-create` | Generate Skill from Git | Experience reuse |
| `/test` | [Testing commands](../Reference-Docs/commands/02-测试相关.md) | Testing related |

## Script Commands

| Script | Function |
|--------|----------|
| `node scripts/hooks/run-with-flags.js` | Run hooks with flags |
| `node tests/run-all.js` | Run all tests |
| `node scripts/lib/utils.js` | Utility library |

## Agent Commands

| Agent | Function |
|-------|----------|
| `/planner` | Task planning |
| `/code-reviewer` | Code review |
| `/tdd-guide` | TDD guidance |
| `/security-reviewer` | Security review |
| `/build-error-resolver` | Build fix |

## Global Parameters

| Parameter | Description |
|-----------|-------------|
| `--model` | Specify model |
| `--no-input` | Non-interactive mode |
| `--output` | Output format |

## Hook Environment Variables

| Variable | Description |
|----------|-------------|
| `ECC_HOOK_PROFILE` | Hook configuration |
| `ECC_DISABLED_HOOKS` | Disabled hooks list |

---

[Return to Quick Reference README](./README.md)