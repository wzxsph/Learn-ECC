# 04 - Skills Development

Learn how to create reusable Skills workflows.

## Overview

ECC provides **234+ [Skills](../../Reference-Docs/skills/最佳实践.md)**, categorized by domain, covering 16 areas including best practices, programming languages, frameworks, testing, security, frontend & design, backend & API, deployment & DevOps, and more.

---

## 1. Skill Format

### 1.1 Basic Structure

Skills use Markdown format with clear section structure:

```markdown
# Skill Name

## When to Use
## How It Works
## Examples
```

### 1.2 YAML Frontmatter

```yaml
---
name: skill-name
description: Skill purpose description
category: category
---
```

### 1.3 Section Structure

| Section | Description |
|---------|-------------|
| **When to Use** | When to use this Skill |
| **How It Works** | Working principles and steps |
| **Examples** | Usage examples |

---

## 2. Skills Index

### 2.1 Categorized by Domain

| Domain | Description |
|--------|-------------|
| [Best Practices](../../Reference-Docs/skills/最佳实践.md) | Coding standards, error handling, autonomous loops |
| [Programming Languages](../../Reference-Docs/skills/编程语言.md) | Python/Go/Rust/Kotlin/C++ etc. |
| [Frameworks](../../Reference-Docs/skills/框架.md) | Django/Laravel/NestJS/Spring Boot etc. |
| [Testing](../../Reference-Docs/skills/测试.md) | TDD/Unit testing/Integration testing/E2E |
| [Security](../../Reference-Docs/skills/安全.md) | Security review, vulnerability scanning |
| [Frontend & Design](../../Reference-Docs/skills/前端与设计.md) | Frontend development, design systems |
| [Backend & API](../../Reference-Docs/skills/后端与API.md) | Backend services, API design, databases |
| [Deployment & DevOps](../../Reference-Docs/skills/部署与DevOps.md) | Docker/K8s/deployment strategies |
| [Monitoring & Observability](../../Reference-Docs/skills/监控与可观测性.md) | Observability, network diagnostics |
| [Automation & Scripting](../../Reference-Docs/skills/自动化与脚本.md) | Autonomous loops, continuous learning, agent engineering |
| [Search & Data Retrieval](../../Reference-Docs/skills/搜索与数据获取.md) | Exa search, data scraping, MCP |
| [GitHub & Collaboration](../../Reference-Docs/skills/GitHub与协作.md) | GitHub workflows, code review |
| [AI & Machine Learning](../../Reference-Docs/skills/AI与机器学习.md) | Neural networks, PyTorch, MLOps |
| [Cloud Native & Infrastructure](../../Reference-Docs/skills/云原生与基础设施.md) | Kubernetes, Docker, Terraform |
| [Specialized Skills](../../Reference-Docs/skills/特殊领域技能.md) | Blockchain, game development, audio/video, IoT |
| [Development Toolchain](../../Reference-Docs/skills/开发工具链.md) | Testing frameworks, CI/CD, code quality |
| [Cutting-edge Technology](../../Reference-Docs/skills/前沿技术.md) | AI Agent, quantum computing, edge computing |

### 2.2 Popular Skills

| Skill | Purpose |
|-------|---------|
| TDD Workflow | Test-driven development standard workflow |
| Code Review | Code quality checklist |
| Security Review | OWASP Top 10 checklist |
| API Design | RESTful API design principles |
| Docker Deployment | Container deployment best practices |
| CI/CD Pipeline | Continuous integration and deployment |

---

## 3. Creating Custom Skills

### 3.1 Creation Steps

1. **Define purpose**: Clarify what problem the Skill solves
2. **Write structure**: Follow When to Use / How It Works / Examples structure
3. **Add YAML frontmatter**: Provide metadata
4. **Save to skills/ directory**: Use lowercase + hyphen naming

### 3.2 Example Skill

```markdown
---
name: my-custom-skill
description: Workflow for executing specific tasks
category: custom
---

# My Custom Skill

## When to Use

Use this Skill when you need to perform specific task X.

## How It Works

1. Step 1: Prepare data
2. Step 2: Execute core operation
3. Step 3: Verify results
4. Step 4: Cleanup resources

## Examples

### Example 1: Basic Usage

```
Execute My Custom Skill to complete X
```

### Example 2: Advanced Usage

```
Execute My Custom Skill with custom parameters
```
```

### 3.3 Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Filename | Lowercase + hyphen | `code-review.md`, `tdd-workflow.md` |
| Directory | Organized by domain | `skills/programming-languages/` |
| Location | Project skills/ or global ~/.claude/skills/ | |

---

## 4. Workflow Design

### 4.1 Step Definition

Clearly define each step:

```markdown
## How It Works

1. **Requirements Analysis**: Understand user requirements
2. **Solution Design**: Formulate implementation plan
3. **Code Implementation**: Write code
4. **Test Verification**: Ensure quality
5. **Documentation Update**: Record changes
```

### 4.2 Conditional Branches

Handle different scenarios:

```markdown
## How It Works

1. Analyze input
2. Branch by condition:
   - If A, execute step X
   - If B, execute step Y
   - Otherwise, execute step Z
3. Aggregate results
```

### 4.3 Error Handling

Standardized error handling:

```markdown
## How It Works

1. Execute operation
2. Check results
   - Success: Continue to next step
   - Failure: Log error and provide fix suggestions
3. Return final result
```

---

## 5. Parameterized Templates

### 5.1 Parameter Definition

```markdown
## How It Works

1. Receive input parameters:
   - `name`: Name (required)
   - `type`: Type (optional, default: `standard`)
2. Process parameters
3. Return results
```

### 5.2 Variable Substitution

```markdown
## Examples

### Basic Usage

Use default parameters:
```
Execute Skill with name {{name}}
```

### Custom Parameters

Specify custom type:
```
Execute Skill with name {{name}} and type {{type}}
```
```

---

## 6. Publishing and Sharing

### 6.1 Local Storage

| Location | Description |
|----------|-------------|
| Project `skills/` | Project-specific Skills |
| `~/.claude/skills/` | Globally available Skills |

### 6.2 Import and Export

```bash
# Export Skill to file
/skill-export my-skill

# Import Skill from file
/skill-import path/to/skill.md
```

### 6.3 Skill Marketplace

ECC supports installing shared Skills from marketplace.

---

## 7. Skill Creation Workflow

Use `/skill-create` command to analyze git history and generate Skill files:

```bash
# Analyze git history and generate Skill
/skill-create

# Specify analysis scope
/skill-create --days 30

# Specify output path
/skill-create --output ./my-skills/
```

---

## 8. Best Practices

### 8.1 Design Principles

| Principle | Description |
|-----------|-------------|
| **Single Responsibility** | Each Skill does one thing |
| **Composable** | Skills can be combined |
| **Reusable** | Design for general use, not specific |
| **Testable** | Provide verification steps |

### 8.2 Documentation Quality

| Requirements | Description |
|--------------|-------------|
| Clear When to Use | Let users know when to use |
| Detailed How It Works | Steps are clear and executable |
| Practical Examples | Provide real use cases |
| Error handling notes | Tell users how to handle errors |

### 8.3 Maintenance Suggestions

- Update regularly to reflect latest practices
- Improve based on user feedback
- Keep it simple, avoid over-complication
- Add version numbers for tracking

---

## Exercises

Complete tasks in [exercises](./exercises/练习.md).

---

[Return to Core Capabilities README](../README.md)