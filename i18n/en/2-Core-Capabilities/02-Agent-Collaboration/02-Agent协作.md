# 02 Agent Collaboration

> Understand the responsibilities of 6 types of Agents, combine them to complete complex development tasks

## Module Objectives

After completing this module, you will be able to:

- Identify Claude Code's 6 types of Agents and their respective responsibilities
- Select appropriate Agents based on task type
- Combine multiple Agents to complete complex tasks
- Understand Agent collaboration patterns and best practices

## Agent Classification System

Claude Code's Agents are divided into 6 major categories:

| Agent Type | Core Responsibility | Applicable Scenarios |
|------------|---------------------|---------------------|
| **Review** | Code quality, pattern compliance, security vulnerabilities | Code review, tech debt cleanup |
| **Build** | Compilation, error fixing, dependency management | Build failures, compilation errors |
| **Planning** | Task decomposition, architecture design, implementation plans | New features, architecture changes |
| **Testing** | Test writing, coverage analysis, test strategy | TDD, test supplementation |
| **Security** | Security audit, vulnerability scanning, compliance checking | Security-sensitive code, payment modules |
| **Architecture** | System design, pattern selection, technology selection | Large-scale refactoring, architecture evolution |

## 6 Types of Agents Detailed

### 1. Review Agents

**Responsibility**: Code quality review, pattern compliance checking

Available Agents:
- `code-reviewer` - General code review
- `python-reviewer` - Python specialized review
- `go-reviewer` - Go language review
- `rust-reviewer` - Rust security review
- `security-reviewer` - Security vulnerability review

**Applicable scenarios**:
- Quality checks before committing code
- Discovering potential code smells
- Reviewing third-party code

### 2. Build Agents

**Responsibility**: Compilation, error fixing, dependency management

Available Agents:
- `build-error-resolver` - Build error resolution
- `go-build` - Go build
- `rust-build` - Rust build
- `kotlin-build` - Kotlin build
- `flutter-build` - Flutter build

**Applicable scenarios**:
- Error fixing when build fails
- Dependency conflict resolution
- Cross-platform build issues

### 3. Planning Agents

**Responsibility**: Task decomposition, architecture design, implementation plans

Available Agents:
- `planner` - Implementation plan generation
- `architect` - Architecture design
- `tdd-guide` - TDD workflow guidance

**Applicable scenarios**:
- New feature development planning
- Technology solution evaluation
- Complex task decomposition

### 4. Testing Agents

**Responsibility**: Test writing, coverage improvement, test strategy

Available Agents:
- `tdd-guide` - Test-driven development
- `e2e-runner` - End-to-end testing
- `test-coverage` - Coverage analysis

**Applicable scenarios**:
- Writing tests for new features
- Improving test coverage
- E2E test scenario design

### 5. Security Agents

**Responsibility**: Security audit, vulnerability scanning, compliance checking

Available Agents:
- `security-reviewer` - Security vulnerability review
- `security-scan` - Automated security scanning

**Applicable scenarios**:
- Payment and authentication code review
- External data processing checks
- Compliance verification

### 6. Architecture Agents

**Responsibility**: System design, pattern selection, technology selection

Available Agents:
- `architect` - Architecture design
- `refactor-cleaner` - Refactoring guidance
- `api-design-reviewer` - API design review

**Applicable scenarios**:
- Microservices splitting
- Database design
- API architecture evolution

## Agent Collaboration Patterns

### Pattern 1: Serial Collaboration

```
Task → Planner → Implementation → Reviewer → Testing → Complete
```

Applicable: Linear process, dependencies between steps

### Pattern 2: Parallel Collaboration

```
            → Reviewer A →
Task → Decompose → Reviewer B → → Aggregate → Complete
            → Reviewer C →
```

Applicable: Multi-angle review, independent module review

### Pattern 3: Cascade Collaboration

```
Major Task → Architect → Planner → Implementation → All Levels of Reviewer → Merge
```

Applicable: Complex projects, large feature development

## Agent Combination Examples

### Scenario: Develop New Feature

```
1. Use /plan to plan feature
2. Use tdd-guide for TDD development
3. Use code-reviewer to review code
4. Use security-reviewer to check security
5. Use verify to verify functionality
```

### Scenario: Fix Security Vulnerability

```
1. Use security-reviewer to scan vulnerabilities
2. Use planner to create fix plan
3. Use build-fix to implement fix
4. Use security-reviewer to verify again
5. Use verify to confirm fix
```

## Agent Selection Guide

| Task Type | Recommended Agent | Reason |
|-----------|-------------------|--------|
| New feature development | planner + tdd-guide | Need planning and TDD |
| Code review | code-reviewer + security-reviewer | Quality and security dual check |
| Build failure | build-error-resolver | Focus on error fixing |
| Architecture refactoring | architect + refactor-cleaner | Architecture-level guidance |
| Security sensitive | security-reviewer (priority) | Must pass security first |
| Test writing | tdd-guide + test-coverage | Test strategy and coverage |

## Reference Materials

Complete Agent documentation is located at: `../../Reference-Docs/agents/1. Code Review Agents.md`

## Next Steps

- Learn [exercises](./exercises/Getting-Started-Exercises.md)
- Read [complete Agent documentation](../../Reference-Docs/agents/1. Code Review Agents.md)