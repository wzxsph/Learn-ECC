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
| `/plan` | [01-核心工作流.md](commands/01-核心工作流.md) | Requirements analysis + risk assessment + step-by-step plan |
| `/code-review` | [01-核心工作流.md](commands/01-核心工作流.md) | Code quality / security / maintainability review |
| `/build-fix` | [01-核心工作流.md](commands/01-核心工作流.md) | Auto-detect language + incrementally fix build errors |
| `/verify` | [01-核心工作流.md](commands/01-核心工作流.md) | Complete verification loop: build → lint → test → type check |
| `/quality-gate` | [01-核心工作流.md](commands/01-核心工作流.md) | Run ECC quality pipeline on demand |
| `/tdd` | [01-核心工作流.md](commands/01-核心工作流.md) | General TDD workflow |

### 3.2 Testing Related (8)

| Command | File | Purpose |
|---------|------|---------|
| `/go-test` | [02-测试相关.md](commands/02-测试相关.md) | Go TDD (table-driven, 80%+ coverage) |
| `/kotlin-test` | [02-测试相关.md](commands/02-测试相关.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-测试相关.md](commands/02-测试相关.md) | Rust TDD (cargo test + integration tests) |
| `/cpp-test` | [02-测试相关.md](commands/02-测试相关.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-测试相关.md](commands/02-测试相关.md) | Flutter TDD |
| `/e2e` | [02-测试相关.md](commands/02-测试相关.md) | Generate + run Playwright e2e tests |
| `/test-coverage` | [02-测试相关.md](commands/02-测试相关.md) | Test coverage report + gap analysis |
| `/python-testing` | [02-测试相关.md](commands/02-测试相关.md) | Python testing best practices |

### 3.3 Language Review (7)

| Command | File | Purpose |
|---------|------|---------|
| `/python-review` | [03-语言审查.md](commands/03-语言审查.md) | Python PEP 8, type hints, security |
| `/go-review` | [03-语言审查.md](commands/03-语言审查.md) | Go idioms, concurrency, error handling |
| `/kotlin-review` | [03-语言审查.md](commands/03-语言审查.md) | Kotlin null safety, coroutines, architecture |
| `/rust-review` | [03-语言审查.md](commands/03-语言审查.md) | Rust ownership, lifetimes, unsafe |
| `/cpp-review` | [03-语言审查.md](commands/03-语言审查.md) | C++ memory safety, modern idioms |
| `/flutter-review` | [03-语言审查.md](commands/03-语言审查.md) | Flutter/Dart patterns |
| `/fastapi-review` | [03-语言审查.md](commands/03-语言审查.md) | FastAPI architecture, async, Pydantic |

### 3.4 Build & Fix (6)

| Command | File | Purpose |
|---------|------|---------|
| `/go-build` | [04-构建修复.md](commands/04-构建修复.md) | Fix Go build errors + go vet warnings |
| `/kotlin-build` | [04-构建修复.md](commands/04-构建修复.md) | Fix Kotlin/Gradle compiler errors |
| `/rust-build` | [04-构建修复.md](commands/04-构建修复.md) | Fix Rust build + borrow checker issues |
| `/cpp-build` | [04-构建修复.md](commands/04-构建修复.md) | Fix C++ CMake + linker issues |
| `/gradle-build` | [04-构建修复.md](commands/04-构建修复.md) | Fix Android/KMP Gradle errors |
| `/flutter-build` | [04-构建修复.md](commands/04-构建修复.md) | Flutter build fixes |

### 3.5 Planning & Architecture (7)

| Command | File | Purpose |
|---------|------|---------|
| `/plan-prd` | [05-规划与架构.md](commands/05-规划与架构.md) | Interactive PRD generator |
| `/prp-plan` | [05-规划与架构.md](commands/05-规划与架构.md) | Comprehensive feature planning |
| `/prp-prd` | [05-规划与架构.md](commands/05-规划与架构.md) | PRP workflow PRD generator |
| `/prp-implement` | [05-规划与架构.md](commands/05-规划与架构.md) | Execute PRP plan + verification loop |
| `/prp-pr` | [05-规划与架构.md](commands/05-规划与架构.md) | Create PR from PRP workflow |
| `/prp-commit` | [05-规划与架构.md](commands/05-规划与架构.md) | PRP verified commit |
| `/multi-plan` | [05-规划与架构.md](commands/05-规划与架构.md) | Multi-model collaborative planning (Codex + Gemini) |

### 3.6 Session Management (5)

| Command | File | Purpose |
|---------|------|---------|
| `/save-session` | [06-会话管理.md](commands/06-会话管理.md) | Save session state to ~/.claude/session-data/ |
| `/resume-session` | [06-会话管理.md](commands/06-会话管理.md) | Load and restore saved session |
| `/sessions` | [06-会话管理.md](commands/06-会话管理.md) | Browse + search + manage session history |
| `/checkpoint` | [06-会话管理.md](commands/06-会话管理.md) | Create / verify workflow checkpoint |
| `/aside` | [06-会话管理.md](commands/06-会话管理.md) | Answer side questions without losing context |

### 3.7 Learning & Improvement (10)

| Command | File | Purpose |
|---------|------|---------|
| `/learn` | [07-学习与改进.md](commands/07-学习与改进.md) | Extract reusable patterns from session |
| `/learn-eval` | [07-学习与改进.md](commands/07-学习与改进.md) | Extract patterns + self-evaluate quality |
| `/evolve` | [07-学习与改进.md](commands/07-学习与改进.md) | Analyze instinct + suggest evolution structure |
| `/promote` | [07-学习与改进.md](commands/07-学习与改进.md) | Promote project instinct to global scope |
| `/instinct-status` | [07-学习与改进.md](commands/07-学习与改进.md) | Show all learned instincts |
| `/instinct-export` | [07-学习与改进.md](commands/07-学习与改进.md) | Export instinct to file |
| `/instinct-import` | [07-学习与改进.md](commands/07-学习与改进.md) | Import instinct from file / URL |
| `/skill-create` | [07-学习与改进.md](commands/07-学习与改进.md) | Analyze git history + generate skill file |
| `/skill-health` | [07-学习与改进.md](commands/07-学习与改进.md) | Skill composition health dashboard |
| `/rules-distill` | [07-学习与改进.md](commands/07-学习与改进.md) | Scan skills + extract cross-domain principles |

### 3.8 Loops & Automation (3)

| Command | File | Purpose |
|---------|------|---------|
| `/loop-start` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Start managed autonomous loop mode |
| `/loop-status` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Check status of running loop |
| `/santa-loop` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Santa-style autonomous loop |

### 3.9 Project Management (6)

| Command | File | Purpose |
|---------|------|---------|
| `/projects` | [09-项目管理.md](commands/09-项目管理.md) | List known projects + instinct stats |
| `/harness-audit` | [09-项目管理.md](commands/09-项目管理.md) | Audit agent harness configuration |
| `/model-route` | [09-项目管理.md](commands/09-项目管理.md) | Route tasks to appropriate model |
| `/pm2` | [09-项目管理.md](commands/09-项目管理.md) | PM2 process manager initialization |
| `/setup-pm` | [09-项目管理.md](commands/09-项目管理.md) | Configure package manager (npm/pnpm/yarn/bun) |
| `/project-init` | [09-项目管理.md](commands/09-项目管理.md) | Project initialization |

### 3.10 PR Workflow (6)

| Command | File | Purpose |
|---------|------|---------|
| `/pr` | [10-PR工作流.md](commands/10-PR工作流.md) | Create GitHub PR from current branch |
| `/review-pr` | [10-PR工作流.md](commands/10-PR工作流.md) | Review GitHub PR |
| `/multi-workflow` | [10-PR工作流.md](commands/10-PR工作流.md) | Multi-model collaborative development |
| `/multi-backend` | [10-PR工作流.md](commands/10-PR工作流.md) | Backend multi-model development |
| `/multi-frontend` | [10-PR工作流.md](commands/10-PR工作流.md) | Frontend multi-model development |
| `/multi-execute` | [10-PR工作流.md](commands/10-PR工作流.md) | Multi-model collaborative execution |

### 3.11 Hookify System (4)

| Command | File | Purpose |
|---------|------|---------|
| `/hookify` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Create hooks to prevent bad behaviors |
| `/hookify-list` | [11-Hookify系统.md](commands/11-Hookify系统.md) | List all configured hookify rules |
| `/hookify-configure` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Interactively enable / disable hookify rules |
| `/hookify-help` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Hookify system help |

### 3.12 Documentation & Research (3)

| Command | File | Purpose |
|---------|------|---------|
| `/update-docs` | [12-文档与研究.md](commands/12-文档与研究.md) | Update project documentation |
| `/update-codemaps` | [12-文档与研究.md](commands/12-文档与研究.md) | Regenerate codemaps |
| `/ecc-guide` | [12-文档与研究.md](commands/12-文档与研究.md) | ECC user guide |

### 3.13 Refactoring & Cleanup (2)

| Command | File | Purpose |
|---------|------|---------|
| `/refactor-clean` | [13-重构与清理.md](commands/13-重构与清理.md) | Delete dead code + merge duplicates |
| `/auto-update` | [13-重构与清理.md](commands/13-重构与清理.md) | Auto-update capabilities |

### 3.14 Other Commands (7)

| Command | File | Purpose |
|---------|------|---------|
| `/jira` | [14-其他命令.md](commands/14-其他命令.md) | Jira ticket interaction |
| `/gan-build` | [14-其他命令.md](commands/14-其他命令.md) | GAN build operations |
| `/gan-design` | [14-其他命令.md](commands/14-其他命令.md) | GAN design operations |
| `/prune` | [14-其他命令.md](commands/14-其他命令.md) | Delete stale instincts (>30 days) |
| `/security-scan` | [14-其他命令.md](commands/14-其他命令.md) | Security scan |
| `/feature-dev` | [14-其他命令.md](commands/14-其他命令.md) | Feature development assistant |
| `/cost-report` | [14-其他命令.md](commands/14-其他命令.md) | Model cost report |

---

## 4. Agent Index

Total **60 Agents**, grouped by category:

| Agent Category | File | Description |
|----------------|------|-------------|
| [Code Review](agents/1.%20代码审查类.md) | [1. 代码审查类.md](agents/1.%20代码审查类.md) | 14 review Agents |
| [Build & Fix](agents/2.%20构建修复类.md) | [2. 构建修复类.md](agents/2.%20构建修复类.md) | 14 build fix Agents |
| [Planning](agents/3.%20规划类.md) | [3. 规划类.md](agents/3.%20规划类.md) | 5 planning Agents |
| [Testing](agents/4.%20测试类.md) | [4. 测试类.md](agents/4.%20测试类.md) | 2 testing Agents |
| [Security](agents/5.%20安全类.md) | [5. 安全类.md](agents/5.%20安全类.md) | 3 security Agents |
| [Architecture](agents/6.%20架构类.md) | [6. 架构类.md](agents/6.%20架构类.md) | 3 architecture Agents |

---

## 5. Skills Index

**16 Skills domains**, categorized by domain:

| Domain | File | Description |
|--------|------|-------------|
| [Best Practices](skills/最佳实践.md) | [最佳实践.md](skills/最佳实践.md) | Coding standards, error handling, autonomous loops |
| [Programming Languages](skills/编程语言.md) | [编程语言.md](skills/编程语言.md) | Python/Go/Rust/Kotlin/C++ etc. |
| [Frameworks](skills/框架.md) | [框架.md](skills/框架.md) | Django/Laravel/NestJS/Spring Boot etc. |
| [Testing](skills/测试.md) | [测试.md](skills/测试.md) | TDD/Unit testing/Integration testing/E2E |
| [Security](skills/安全.md) | [安全.md](skills/安全.md) | Security review, vulnerability scanning |
| [Frontend & Design](skills/前端与设计.md) | [前端与设计.md](skills/前端与设计.md) | Frontend development, design systems |
| [Backend & API](skills/后端与API.md) | [后端与API.md](skills/后端与API.md) | Backend services, API design, databases |
| [Deployment & DevOps](skills/部署与DevOps.md) | [部署与DevOps.md](skills/部署与DevOps.md) | Docker/K8s/deployment strategies |
| [Monitoring & Observability](skills/监控与可观测性.md) | [监控与可观测性.md](skills/监控与可观测性.md) | Observability, network diagnostics |
| [Automation & Scripting](skills/自动化与脚本.md) | [自动化与脚本.md](skills/自动化与脚本.md) | Autonomous loops, continuous learning, agent engineering |
| [Search & Data Retrieval](skills/搜索与数据获取.md) | [搜索与数据获取.md](skills/搜索与数据获取.md) | Exa search, data scraping, MCP |
| [GitHub & Collaboration](skills/GitHub与协作.md) | [GitHub与协作.md](skills/GitHub与协作.md) | GitHub workflows, code review |
| [AI & Machine Learning](skills/AI与机器学习.md) | [AI与机器学习.md](skills/AI与机器学习.md) | Neural networks, PyTorch, MLOps |
| [Cloud Native & Infrastructure](skills/云原生与基础设施.md) | [云原生与基础设施.md](skills/云原生与基础设施.md) | Kubernetes, Docker, Terraform |
| [Specialized Skills](skills/特殊领域技能.md) | [特殊领域技能.md](skills/特殊领域技能.md) | Blockchain, game development, audio/video, IoT |
| [Development Toolchain](skills/开发工具链.md) | [开发工具链.md](skills/开发工具链.md) | Testing frameworks, CI/CD, code quality |
| [Cutting-edge Technology](skills/前沿技术.md) | [前沿技术.md](skills/前沿技术.md) | AI Agent, quantum computing, edge computing |

---

## 6. Rules Index

Total **7 rules documents** (common + language-specific):

| Rules | File | Description |
|-------|------|-------------|
| [Git Workflow](rules/Git工作流.md) | [Git工作流.md](rules/Git工作流.md) | Git commit standards and PR workflow |
| [Hooks System](rules/Hooks系统.md) | [Hooks系统.md](rules/Hooks系统.md) | Hook configuration and usage guide |
| [Agent Orchestration](rules/代理编排.md) | [代理编排.md](rules/代理编排.md) | Agent orchestration patterns |
| [Performance Optimization](rules/性能优化.md) | [性能优化.md](rules/性能优化.md) | Performance optimization guide |
| [Code Style](rules/代码风格.md) | [代码风格.md](rules/代码风格.md) | Coding style standards |
| [Testing Rules](rules/测试规则.md) | [测试规则.md](rules/测试规则.md) | Testing requirements (80% coverage) |
| [Security Rules](rules/安全规则.md) | [安全规则.md](rules/安全规则.md) | Security checklist |

---

## 7. Hooks Index

Total **4 documents**:

| Document | File | Description |
|----------|------|-------------|
| [Hook Types](hooks/Hook类型.md) | [Hook类型.md](hooks/Hook类型.md) | PreToolUse, PostToolUse, Stop types |
| [Built-in Hooks](hooks/内置Hooks.md) | [内置Hooks.md](hooks/内置Hooks.md) | Built-in Hook list and usage |
| [Custom Development](hooks/自定义开发.md) | [自定义开发.md](hooks/自定义开发.md) | Custom Hook development guide |
| [Configuration Format](hooks/配置格式.md) | [配置格式.md](hooks/配置格式.md) | hooks.json configuration format |

---

## 8. MCP Index

Total **6 MCP server configurations**:

| Document | File | Description |
|----------|------|-------------|
| [MCP Configuration Format](mcp/MCP配置格式.md) | [MCP配置格式.md](mcp/MCP配置格式.md) | MCP configuration file format |
| [Built-in Servers](mcp/内置服务器.md) | [内置服务器.md](mcp/内置服务器.md) | Built-in MCP servers |
| [Custom Development](mcp/自定义开发.md) | [自定义开发.md](mcp/自定义开发.md) | Custom MCP server development |

---

## 9. Scripts Index

Total **54 utility scripts**:

| Document | File | Description |
|----------|------|-------------|
| [Utility Scripts](scripts/工具脚本.md) | [工具脚本.md](scripts/工具脚本.md) | ecc.js, install-apply.js etc. |
| [Utility Library](scripts/工具函数库.md) | [工具函数库.md](scripts/工具函数库.md) | Shared function library |
| [Test Runner](scripts/测试运行器.md) | [测试运行器.md](scripts/测试运行器.md) | Test runner usage |
| [Build Scripts](scripts/构建脚本.md) | [构建脚本.md](scripts/构建脚本.md) | Build scripts |

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