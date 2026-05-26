# 02 - Agent Collaboration

Learn how to design, implement, and manage multi-Agent collaboration systems.

## Overview

ECC provides **66 professional [Agents](../../Reference-Docs/agents/1.%20代码审查类.md)**, divided into 6 major categories. These Agents work together through standardized workflows to complete complex tasks from code review to system design.

---

## 1. Agent Architecture

### 1.1 Single Agent vs Multi-Agent

| Architecture | Use Case | Characteristics |
|--------------|----------|-----------------|
| **Single Agent** | Simple tasks, independent operations | Direct call, fast response |
| **Multi-Agent** | Complex tasks, need collaboration | Task decomposition, result aggregation |

### 1.2 Available Agents List

| Agent | Purpose | Usage Timing |
|-------|--------|-------------|
| **planner** | Implementation planning | Complex features, refactoring |
| **architect** | System design | Architecture decisions |
| **tdd-guide** | Test-driven development | New features, bug fixes |
| **code-reviewer** | Code review | After writing code |
| **security-reviewer** | Security analysis | Before commit |
| **build-error-resolver** | Fix build errors | When build fails |
| **e2e-runner** | End-to-end testing | Critical user flows |
| **refactor-cleaner** | Dead code cleanup | Code maintenance |
| **doc-updater** | Documentation update | When updating docs |
| **rust-reviewer** | Rust code review | Rust projects |
| **harmonyos-app-resolver** | HarmonyOS app development | HarmonyOS/ArkTS projects |

### 1.3 Agent File Format

```yaml
---
name: agent-name
description: Agent purpose
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

---

## 2. Agent Categories Detailed

### 2.1 Code Review (16)

Responsible for code quality, security, and maintainability review.

| Agent | Specialty |
|-------|------------|
| code-reviewer | General code review |
| security-reviewer | Security vulnerability analysis |
| python-reviewer | Python PEP 8, type hints |
| go-reviewer | Go idioms, concurrency |
| kotlin-reviewer | Kotlin null safety, coroutines |
| rust-reviewer | Rust ownership, lifetimes |
| cpp-reviewer | C++ memory safety |
| flutter-reviewer | Flutter/Dart patterns |
| fastapi-reviewer | FastAPI architecture, async |
| typescript-reviewer | TypeScript type safety |
| java-reviewer | Java coding standards |
| csharp-reviewer | C# best practices |
| ruby-reviewer | Ruby idioms |
| php-reviewer | PHP security and performance |
| swift-reviewer | Swift concurrency, memory management |
| go-security-reviewer | Go security review |

### 2.2 Build & Fix (10)

Focus on fixing build errors for various languages.

| Agent | Specialty |
|-------|------------|
| build-error-resolver | General build error fixing |
| go-build | Go build + go vet |
| kotlin-build | Kotlin/Gradle compilation |
| rust-build | Rust borrow checker |
| cpp-build | C++ CMake + linker |
| gradle-build | Android/KMP Gradle |
| flutter-build | Flutter build |
| maven-build | Maven build |
| cmake-build | CMake configuration |
| ninja-build | Ninja build system |

### 2.3 Planning (5)

Responsible for requirements analysis and system planning.

| Agent | Specialty |
|-------|------------|
| planner | Implementation planning |
| architect | System architecture design |
| prp-planner | PRP workflow planning |
| multi-plan | Multi-model collaborative planning |
| product-manager | Product requirements analysis |

### 2.4 Testing (2)

Focus on test-driven development and coverage.

| Agent | Specialty |
|-------|------------|
| tdd-guide | TDD workflow guidance |
| e2e-runner | Playwright E2E testing |

### 2.5 Security (5)

Responsible for security review and vulnerability scanning.

| Agent | Specialty |
|-------|------------|
| security-reviewer | General security review |
| owasp-reviewer | OWASP Top 10 |
| pentest-reviewer | Penetration testing |
| vulnerability-scanner | Vulnerability scanning |
| secrets-detector | Keys and credentials detection |

### 2.6 Architecture (5)

Responsible for system architecture and technical decisions.

| Agent | Specialty |
|-------|------------|
| architect | System architecture design |
| microservices-architect | Microservices architecture |
| frontend-architect | Frontend architecture |
| backend-architect | Backend architecture |
| devops-architect | DevOps architecture |

---

## 3. Collaboration Patterns

### 3.1 Master-Slave Pattern

One master Agent coordinates multiple slave Agents:

```
User request → Master Agent (planner)
           ├── Slave Agent 1 (code-reviewer)
           ├── Slave Agent 2 (tdd-guide)
           └── Slave Agent 3 (build-error-resolver)
```

### 3.2 Peer-to-Peer Pattern

Multiple Agents work in parallel, results aggregated:

```
User request → Agent 1 (security analysis)
        → Agent 2 (performance analysis)
        → Agent 3 (code quality analysis)
           ↓ Result aggregation
        Final report
```

### 3.3 Pipeline Pattern

Agents process sequentially, each Agent's output is the next's input:

```
Input → Agent 1 → Agent 2 → Agent 3 → Output
```

---

## 4. Task Distribution Strategy

### 4.1 Task Decomposition

Decompose complex tasks into independent subtasks:

```markdown
# Complex Task Decomposition Example
Original task: Implement user authentication system

After decomposition:
1. Design database schema → architect
2. Implement API endpoints → backend-developer
3. Write tests → tdd-guide
4. Security review → security-reviewer
```

### 4.2 Parallel Execution

Independent tasks execute in parallel for efficiency:

```markdown
# Good: Parallel execution
Launch 3 Agents in parallel:
1. Agent 1: Authentication module security analysis
2. Agent 2: Cache system performance review
3. Agent 3: Tool type checking

# Bad: Unnecessary sequential execution
First agent 1, then agent 2, then agent 3
```

### 4.3 Load Balancing

Distribute tasks based on Agent load:

| Factor | Description |
|--------|-------------|
| Task complexity | Simple task → lightweight Agent |
| Resource requirements | Heavy computation → dedicated Agent |
| Availability | Avoid overloaded Agents |

---

## 5. Multi-Perspective Analysis

For complex problems, use split role sub-agents:

| Role | Responsibility |
|------|---------------|
| Factual reviewer | Verify information and data accuracy |
| Senior engineer | Evaluate technical feasibility and best practices |
| Security expert | Identify security risks and vulnerabilities |
| Consistency reviewer | Ensure solution is internally consistent |
| Redundancy checker | Identify duplication and waste |

---

## 6. Agent Communication

### 6.1 Direct Invocation

```markdown
Use planner agent to create implementation plan
```

### 6.2 Task Delegation

```markdown
Delegate code review task to code-reviewer agent
```

### 6.3 Result Aggregation

Multiple Agent results need aggregation:

```markdown
Aggregate from:
- security-reviewer: Security issues list
- code-reviewer: Code quality issues list
- tdd-guide: Test coverage report

→ Generate comprehensive report
```

---

## 7. Immediate Use Principles

Automatically invoke without user prompt:

| Scenario | Agent |
|----------|-------|
| Complex feature request | **planner** |
| Code just written/modified | **code-reviewer** |
| Bug fix or new feature | **tdd-guide** |
| Architecture decisions | **architect** |
| Build failure | **build-error-resolver** |
| Security related | **security-reviewer** |

---

## Exercises

Complete tasks in [exercises](./exercises/练习.md).

---

[Return to Core Capabilities README](../README.md)