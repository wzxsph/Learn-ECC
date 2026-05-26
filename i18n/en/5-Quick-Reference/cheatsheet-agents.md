# Agents Cheatsheet

## Built-in Agents

### Planning
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `planner` | [Implementation planning](../Reference-Docs/agents/06-Architecture.md) | Decompose tasks, generate steps |
| `architect` | [System design](../Reference-Docs/agents/06-Architecture.md) | Architecture decisions, technology selection |

### Review
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `code-reviewer` | Code review | Quality gate, pattern checking |
| `security-reviewer` | Security review | Vulnerability detection, OWASP |
| `rust-reviewer` | Rust review | Lifetimes, borrow checking |

### Development
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `tdd-guide` | [TDD guidance](../Reference-Docs/agents/01-Code-Review.md) | Test-first, incremental refactoring |
| `build-error-resolver` | [Build fix](../Reference-Docs/agents/02-Build-Fix.md) | Error locating, fix suggestions |

### Operations
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `e2e-runner` | [E2E testing](../Reference-Docs/agents/01-Code-Review.md) | Critical paths, automation |
| `refactor-cleaner` | [Refactoring cleanup](../Reference-Docs/agents/01-Code-Review.md) | Dead code removal |

## Agent Invocation Methods

```
/[agent-name]                    # Direct invocation
/[agent-name] [parameters]       # With parameters
/[agent-name] --option value    # Option parameters
```

## Custom Agents

Agent file format: `agents/*.md` + YAML frontmatter

```yaml
---
name: my-agent
description: My custom Agent
tools: [Read, Bash, Edit]
model: sonnet
---
```

---

[Return to Quick Reference README](./README.md)