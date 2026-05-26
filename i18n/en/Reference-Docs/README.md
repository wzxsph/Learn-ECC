# ECC Documentation System

ECC (Everything Claude Code) is a Claude Code plugin providing 75 commands, 60 Agents, 16 Skills, 7 rules documents, and complete Hook/MCP configuration system.

---

## 1. ECC Documentation Overview

ECC provides production-grade development workflow support for Claude Code, covering multiple AI Agent frameworks (Claude Code, Codex, OpenCode, Cursor, Gemini).

### Technology Stack

| Layer | Technology | Version |
|------|------------|---------|
| Primary Language | JavaScript/Node.js | >=18 |
| Secondary Language | Python | >=3.11 |
| Package Manager | Yarn | 4.9.2 |
| Test Runner | Custom Node.js test runner | - |
| Coverage | c8 | 11.x |
| Linting | ESLint + markdownlint-cli | - |

### Directory Structure

```
ECC/
├── agents/        # 60 professional Agents
├── commands/      # 75 command files
├── skills/        # 16 Skills directories
├── rules/         # 7 rules documents (common + language-specific)
├── hooks/         # 3 Hook documents
├── mcp/           # 6 MCP server configurations
├── scripts/       # 54 utility scripts
├── README.md
```

---

## 2. Quick Navigation

| Category | Documentation | Description |
|----------|---------------|--------------|
| [Commands Overview](./commands/) | [commands/](commands/) | Complete list of 75 commands |
| [Agent Index](./agents/) | [agents/](agents/) | 60 professional Agents |
| [Skills Index](./skills/) | [skills/](skills/) | 16 Skills by domain |
| [Rules Index](./rules/) | [rules/](rules/) | 7 rules documents (common + language-specific) |
| [Hooks Index](./hooks/) | [hooks/](hooks/) | Hook system configuration and development |
| [MCP Index](./mcp/) | [mcp/](mcp/) | MCP server configuration and development |
| [Scripts Index](./scripts/) | [scripts/](scripts/) | 54 utility scripts |

---

## 3. Command Index

Total **75 commands**, grouped by category:

### 3.1 Core Workflows (6)

| Command | File | Purpose |
|---------|------|---------|
| `/plan` | [01-Core-Workflow.md](commands/01-Core-Workflow.md) | Requirements analysis + risk assessment + step-by-step plan |
| `/code-review` | [01-Core-Workflow.md](commands/01-Core-Workflow.md) | Code quality / security / maintainability review |
| `/build-fix` | [01-Core-Workflow.md](commands/01-Core-Workflow.md) | Auto-detect language + incrementally fix build errors |
| `/verify` | [01-Core-Workflow.md](commands/01-Core-Workflow.md) | Complete verification loop: build → lint → test → type check |
| `/quality-gate` | [01-Core-Workflow.md](commands/01-Core-Workflow.md) | Run ECC quality pipeline on demand |
| `/tdd` | [01-Core-Workflow.md](commands/01-Core-Workflow.md) | General TDD workflow |

### 3.2 Testing Related (8)

| Command | File | Purpose |
|---------|------|---------|
| `/go-test` | [02-Testing.md](commands/02-Testing.md) | Go TDD (table-driven, 80%+ coverage) |
| `/kotlin-test` | [02-Testing.md](commands/02-Testing.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-Testing.md](commands/02-Testing.md) | Rust TDD (cargo test + integration tests) |
| `/cpp-test` | [02-Testing.md](commands/02-Testing.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-Testing.md](commands/02-Testing.md) | Flutter TDD |
| `/e2e` | [02-Testing.md](commands/02-Testing.md) | Generate + run Playwright e2e tests |
| `/test-coverage` | [02-Testing.md](commands/02-Testing.md) | Test coverage report + gap analysis |
| `/python-testing` | [02-Testing.md](commands/02-Testing.md) | Python testing best practices |

### 3.3 Language Review (7)

| Command | File | Purpose |
|---------|------|---------|
| `/python-review` | [03-Language-Review.md](commands/03-Language-Review.md) | Python PEP 8, type hints, security |
| `/go-review` | [03-Language-Review.md](commands/03-Language-Review.md) | Go idioms, concurrency, error handling |
| `/kotlin-review` | [03-Language-Review.md](commands/03-Language-Review.md) | Kotlin null safety, coroutines, architecture |
| `/rust-review` | [03-Language-Review.md](commands/03-Language-Review.md) | Rust ownership, lifetimes, unsafe |
| `/cpp-review` | [03-Language-Review.md](commands/03-Language-Review.md) | C++ memory safety, modern idioms |
| `/flutter-review` | [03-Language-Review.md](commands/03-Language-Review.md) | Flutter/Dart patterns |
| `/fastapi-review` | [03-Language-Review.md](commands/03-Language-Review.md) | FastAPI architecture, async, Pydantic |

### 3.4 Build & Fix (6)

| Command | File | Purpose |
|---------|------|---------|
| `/go-build` | [04-Build-Fix.md](commands/04-Build-Fix.md) | Fix Go build errors + go vet warnings |
| `/kotlin-build` | [04-Build-Fix.md](commands/04-Build-Fix.md) | Fix Kotlin/Gradle compiler errors |
| `/rust-build` | [04-Build-Fix.md](commands/04-Build-Fix.md) | Fix Rust build + borrow checker issues |
| `/cpp-build` | [04-Build-Fix.md](commands/04-Build-Fix.md) | Fix C++ CMake + linker issues |
| `/gradle-build` | [04-Build-Fix.md](commands/04-Build-Fix.md) | Fix Android/KMP Gradle errors |
| `/flutter-build` | [04-Build-Fix.md](commands/04-Build-Fix.md) | Flutter build fixes |

### 3.5 Planning & Architecture (7)

| Command | File | Purpose |
|---------|------|---------|
| `/plan-prd` | [05-Planning-Architecture.md](commands/05-Planning-Architecture.md) | Interactive PRD generator |
| `/prp-plan` | [05-Planning-Architecture.md](commands/05-Planning-Architecture.md) | Comprehensive feature planning |
| `/prp-prd` | [05-Planning-Architecture.md](commands/05-Planning-Architecture.md) | PRP workflow PRD generator |
| `/prp-implement` | [05-Planning-Architecture.md](commands/05-Planning-Architecture.md) | Execute PRP plan + verification loop |
| `/prp-pr` | [05-Planning-Architecture.md](commands/05-Planning-Architecture.md) | Create PR from PRP workflow |
| `/prp-commit` | [05-Planning-Architecture.md](commands/05-Planning-Architecture.md) | PRP verified commit |
| `/multi-plan` | [05-Planning-Architecture.md](commands/05-Planning-Architecture.md) | Multi-model collaborative planning (Codex + Gemini) |

### 3.6 Session Management (5)

| Command | File | Purpose |
|---------|------|---------|
| `/save-session` | [06-Session-Management.md](commands/06-Session-Management.md) | Save session state to ~/.claude/session-data/ |
| `/resume-session` | [06-Session-Management.md](commands/06-Session-Management.md) | Load and restore saved session |
| `/sessions` | [06-Session-Management.md](commands/06-Session-Management.md) | Browse + search + manage session history |
| `/checkpoint` | [06-Session-Management.md](commands/06-Session-Management.md) | Create / verify workflow checkpoint |
| `/aside` | [06-Session-Management.md](commands/06-Session-Management.md) | Answer side questions without losing context |

### 3.7 Learning & Improvement (10)

| Command | File | Purpose |
|---------|------|---------|
| `/learn` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Extract reusable patterns from session |
| `/learn-eval` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Extract patterns + self-evaluate quality |
| `/evolve` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Analyze instinct + suggest evolution structure |
| `/promote` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Promote project instinct to global scope |
| `/instinct-status` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Show all learned instincts |
| `/instinct-export` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Export instinct to file |
| `/instinct-import` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Import instinct from file / URL |
| `/skill-create` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Analyze git history + generate skill file |
| `/skill-health` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Skill composition health dashboard |
| `/rules-distill` | [07-Learning-Improvement.md](commands/07-Learning-Improvement.md) | Scan skills + extract cross-domain principles |

### 3.8 Loops & Automation (3)

| Command | File | Purpose |
|---------|------|---------|
| `/loop-start` | [08-Loops-Automation.md](commands/08-Loops-Automation.md) | Start managed autonomous loop mode |
| `/loop-status` | [08-Loops-Automation.md](commands/08-Loops-Automation.md) | Check status of running loop |
| `/santa-loop` | [08-Loops-Automation.md](commands/08-Loops-Automation.md) | Santa-style autonomous loop |

### 3.9 Project Management (6)

| Command | File | Purpose |
|---------|------|---------|
| `/projects` | [09-Project-Management.md](commands/09-Project-Management.md) | List known projects + instinct stats |
| `/harness-audit` | [09-Project-Management.md](commands/09-Project-Management.md) | Audit agent harness configuration |
| `/model-route` | [09-Project-Management.md](commands/09-Project-Management.md) | Route tasks to appropriate model |
| `/pm2` | [09-Project-Management.md](commands/09-Project-Management.md) | PM2 process manager initialization |
| `/setup-pm` | [09-Project-Management.md](commands/09-Project-Management.md) | Configure package manager (npm/pnpm/yarn/bun) |
| `/project-init` | [09-Project-Management.md](commands/09-Project-Management.md) | Project initialization |

### 3.10 PR Workflow (6)

| Command | File | Purpose |
|---------|------|---------|
| `/pr` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Create GitHub PR from current branch |
| `/review-pr` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Review GitHub PR |
| `/multi-workflow` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Multi-model collaborative development |
| `/multi-backend` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Backend multi-model development |
| `/multi-frontend` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Frontend multi-model development |
| `/multi-execute` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Multi-model collaborative execution |

### 3.11 Hookify System (4)

| Command | File | Purpose |
|---------|------|---------|
| `/hookify` | [11-Hookify-System.md](commands/11-Hookify-System.md) | Create hooks to prevent bad behaviors |
| `/hookify-list` | [11-Hookify-System.md](commands/11-Hookify-System.md) | List all configured hookify rules |
| `/hookify-configure` | [11-Hookify-System.md](commands/11-Hookify-System.md) | Interactively enable / disable hookify rules |
| `/hookify-help` | [11-Hookify-System.md](commands/11-Hookify-System.md) | Hookify system help |

### 3.12 Documentation & Research (3)

| Command | File | Purpose |
|---------|------|---------|
| `/update-docs` | [12-Documentation-Research.md](commands/12-Documentation-Research.md) | Update project documentation |
| `/update-codemaps` | [12-Documentation-Research.md](commands/12-Documentation-Research.md) | Regenerate codemaps |
| `/ecc-guide` | [12-Documentation-Research.md](commands/12-Documentation-Research.md) | ECC user guide |

### 3.13 Refactoring & Cleanup (2)

| Command | File | Purpose |
|---------|------|---------|
| `/refactor-clean` | [13-Refactoring-Cleanup.md](commands/13-Refactoring-Cleanup.md) | Delete dead code + merge duplicates |
| `/auto-update` | [13-Refactoring-Cleanup.md](commands/13-Refactoring-Cleanup.md) | Auto-update capabilities |

### 3.14 Other Commands (7)

| Command | File | Purpose |
|---------|------|---------|
| `/jira` | [14-Other-Commands.md](commands/14-Other-Commands.md) | Jira ticket interaction |
| `/gan-build` | [14-Other-Commands.md](commands/14-Other-Commands.md) | GAN build operations |
| `/gan-design` | [14-Other-Commands.md](commands/14-Other-Commands.md) | GAN design operations |
| `/prune` | [14-Other-Commands.md](commands/14-Other-Commands.md) | Delete stale instincts (>30 days) |
| `/security-scan` | [14-Other-Commands.md](commands/14-Other-Commands.md) | Security scan |
| `/feature-dev` | [14-Other-Commands.md](commands/14-Other-Commands.md) | Feature development assistant |
| `/cost-report` | [14-Other-Commands.md](commands/14-Other-Commands.md) | Model cost report |

---

## 4. Agent Index

Total **60 Agents**, grouped by category:

| Agent Category | File | Description |
|----------------|------|-------------|
| [Code Review](agents/01-Code-Review.md) | [1. 代码审查类.md](agents/01-Code-Review.md) | 14 review Agents |
| [Build & Fix](agents/02-Build-Fix.md) | [2. 构建修复类.md](agents/02-Build-Fix.md) | 14 build fix Agents |
| [Planning](agents/03-Planning.md) | [3. 规划类.md](agents/03-Planning.md) | 5 planning Agents |
| [Testing](agents/04-Testing.md) | [4. 测试类.md](agents/04-Testing.md) | 2 testing Agents |
| [Security](agents/05-Security.md) | [5. 安全类.md](agents/05-Security.md) | 3 security Agents |
| [Architecture](agents/06-Architecture.md) | [6. 架构类.md](agents/06-Architecture.md) | 3 architecture Agents |

---

## 5. Skills Index

**16 Skills domains**, categorized by domain:

| Domain | File | Description |
|--------|------|-------------|
| [Best Practices](skills/Best-Practices.md) | [Best-Practices.md](skills/Best-Practices.md) | Coding standards, error handling, autonomous loops |
| [Programming Languages](skills/Programming-Languages.md) | [Programming-Languages.md](skills/Programming-Languages.md) | Python/Go/Rust/Kotlin/C++ etc. |
| [Frameworks](skills/Frameworks.md) | [Frameworks.md](skills/Frameworks.md) | Django/Laravel/NestJS/Spring Boot etc. |
| [Testing](skills/Testing.md) | [Testing.md](skills/Testing.md) | TDD/Unit testing/Integration testing/E2E |
| [Security](skills/Security.md) | [Security.md](skills/Security.md) | Security review, vulnerability scanning |
| [Frontend & Design](skills/Frontend-Design.md) | [Frontend-Design.md](skills/Frontend-Design.md) | Frontend development, design systems |
| [Backend & API](skills/Backend-API.md) | [Backend-API.md](skills/Backend-API.md) | Backend services, API design, databases |
| [Deployment & DevOps](skills/Deployment-DevOps.md) | [Deployment-DevOps.md](skills/Deployment-DevOps.md) | Docker/K8s/deployment strategies |
| [Monitoring & Observability](skills/Monitoring-Observability.md) | [Monitoring-Observability.md](skills/Monitoring-Observability.md) | Observability, network diagnostics |
| [Automation & Scripting](skills/Automation-Scripting.md) | [Automation-Scripting.md](skills/Automation-Scripting.md) | Autonomous loops, continuous learning, agent engineering |
| [Search & Data Retrieval](skills/Search-Data-Retrieval.md) | [Search-Data-Retrieval.md](skills/Search-Data-Retrieval.md) | Exa search, data scraping, MCP |
| [GitHub & Collaboration](skills/GitHub-Collaboration.md) | [GitHub-Collaboration.md](skills/GitHub-Collaboration.md) | GitHub workflows, code review |
| [AI & Machine Learning](skills/AI-Machine-Learning.md) | [AI-Machine-Learning.md](skills/AI-Machine-Learning.md) | Neural networks, PyTorch, MLOps |
| [Cloud Native & Infrastructure](skills/Cloud-Native-Infrastructure.md) | [Cloud-Native-Infrastructure.md](skills/Cloud-Native-Infrastructure.md) | Kubernetes, Docker, Terraform |
| [Specialized Skills](skills/Specialized-Domain-Skills.md) | [Specialized-Domain-Skills.md](skills/Specialized-Domain-Skills.md) | Blockchain, game development, audio/video, IoT |
| [Development Toolchain](skills/Development-Toolchain.md) | [Development-Toolchain.md](skills/Development-Toolchain.md) | Testing frameworks, CI/CD, code quality |
| [Cutting-edge Technology](skills/Cutting-Edge-Technologies.md) | [Cutting-Edge-Technologies.md](skills/Cutting-Edge-Technologies.md) | AI Agent, quantum computing, edge computing |

---

## 6. Rules Index

Total **7 rules documents** (common + language-specific):

| Rules | File | Description |
|-------|------|-------------|
| [Git Workflow](rules/Git-Workflow.md) | [Git-Workflow.md](rules/Git-Workflow.md) | Git commit standards and PR workflow |
| [Hooks System](rules/Hooks-System.md) | [Hooks-System.md](rules/Hooks-System.md) | Hook configuration and usage guide |
| [Agent Orchestration](rules/Agent-Orchestration.md) | [Agent-Orchestration.md](rules/Agent-Orchestration.md) | Agent orchestration patterns |
| [Performance Optimization](rules/Performance-Optimization.md) | [Performance-Optimization.md](rules/Performance-Optimization.md) | Performance optimization guide |
| [Code Style](rules/Coding-Style.md) | [Coding-Style.md](rules/Coding-Style.md) | Coding style standards |
| [Testing Rules](rules/Testing-Rules.md) | [Testing-Rules.md](rules/Testing-Rules.md) | Testing requirements (80% coverage) |
| [Security Rules](rules/Security-Rules.md) | [Security-Rules.md](rules/Security-Rules.md) | Security checklist |

---

## 7. Hooks Index

Total **4 documents**:

| Document | File | Description |
|----------|------|-------------|
| [Hook Types](hooks/Hook-Types.md) | [Hook-Types.md](hooks/Hook-Types.md) | PreToolUse, PostToolUse, Stop types |
| [Built-in Hooks](hooks/Built-in-Hooks.md) | [Built-in-Hooks.md](hooks/Built-in-Hooks.md) | Built-in Hook list and usage |
| [Custom Development](hooks/Custom-Development.md) | [Custom-Development.md](hooks/Custom-Development.md) | Custom Hook development guide |
| [Configuration Format](hooks/Configuration-Format.md) | [Configuration-Format.md](hooks/Configuration-Format.md) | hooks.json configuration format |

---

## 8. MCP Index

Total **6 MCP server configurations**:

| Document | File | Description |
|----------|------|-------------|
| [MCP Configuration Format](mcp/MCPConfiguration-Format.md) | [MCPConfiguration-Format.md](mcp/MCPConfiguration-Format.md) | MCP configuration file format |
| [Built-in Servers](mcp/Built-in-Servers.md) | [Built-in-Servers.md](mcp/Built-in-Servers.md) | Built-in MCP servers |
| [Custom Development](mcp/Custom-Development.md) | [Custom-Development.md](mcp/Custom-Development.md) | Custom MCP server development |

---

## 9. Scripts Index

Total **54 utility scripts**:

| Document | File | Description |
|----------|------|-------------|
| [Utility Scripts](scripts/Tool-Scripts.md) | [Tool-Scripts.md](scripts/Tool-Scripts.md) | ecc.js, install-apply.js etc. |
| [Utility Library](scripts/Utility-Functions.md) | [Utility-Functions.md](scripts/Utility-Functions.md) | Shared function library |
| [Test Runner](scripts/Test-Runner.md) | [Test-Runner.md](scripts/Test-Runner.md) | Test runner usage |
| [Build Scripts](scripts/Build-Scripts.md) | [Build-Scripts.md](scripts/Build-Scripts.md) | Build scripts |

---

## 10. Contribution Guide

### File Naming Conventions

- **Commands, Skills, Agents, Hooks**: Lowercase + hyphen (e.g., `code-review.md`)
- **Scripts**: camelCase or kebab-case (e.g., `session-start.js`)
- **Rules**: Organized by language / topic

### Command File Format

```yaml
---
description: "Short description"
argument-hint: "[Optional argument]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent File Format

```yaml
---
name: agent-name
description: Agent purpose
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill File Format

```markdown
# Skill Name

## When to Use
## How It Works
## Examples
```

### Submission Process

1. Create new file or modify existing file
2. Ensure following naming conventions
3. Run tests: `node tests/run-all.js`
4. Run markdown lint: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. Create PR for review

---

*ECC Documentation System - Production-grade development workflows for Claude Code*