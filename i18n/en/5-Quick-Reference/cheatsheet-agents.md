# Agents Cheatsheet

## Built-in Agents

### Planning
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `planner` | [Implementation planning](../Reference-Docs/agents/6.%20架构类.md) | Decompose tasks, generate steps |
| `architect` | [System design](../Reference-Docs/agents/6.%20架构类.md) | Architecture decisions, technology selection |

### Review
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `code-reviewer` | Code review | Quality gate, pattern checking |
| `security-reviewer` | Security review | Vulnerability detection, OWASP |
| `rust-reviewer` | Rust review | Lifetimes, borrow checking |

### Development
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `tdd-guide` | [TDD guidance](../Reference-Docs/agents/1.%20代码审查类.md) | Test-first, incremental refactoring |
| `build-error-resolver` | [Build fix](../Reference-Docs/agents/2.%20构建修复类.md) | Error locating, fix suggestions |

### Operations
| Agent | Purpose | Characteristics |
|-------|---------|-----------------|
| `e2e-runner` | [E2E testing](../Reference-Docs/agents/1.%20代码审查类.md) | Critical paths, automation |
| `refactor-cleaner` | [Refactoring cleanup](../Reference-Docs/agents/1.%20代码审查类.md) | Dead code removal |

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