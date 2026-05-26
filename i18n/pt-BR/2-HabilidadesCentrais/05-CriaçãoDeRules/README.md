# 05 Rules 编写

> 编写项目级规则，定制代码审查和质量标准

## 模块目标

完成本模块后，你将能够：

- 理解 Claude Code Rules 的格式和分类
- 编写Regras-de-Segurança、Estilo-de-Código规则、Regras-de-Teste
- 创建项目特定的开发规范
- 配置规则优先级和执行策略

## Rules 分类体系

| 分类 | 用途 | 示例 |
|------|------|------|
| **Regras-de-Segurança** | 安全检查、漏洞防护 | 不允许硬编码密码、SQL 注入防护 |
| **Estilo-de-Código** | 编码规范、格式化 | 缩进、命名约定、注释规范 |
| **Regras-de-Teste** | 测试覆盖率、测试质量 | 80% 覆盖率要求、AAA 模式 |
| **Git 规则** | 提交规范、分支策略 | 提交信息格式、PR 流程 |
| **性能规则** | Otimização-de-Desempenho、资源管理 | N+1 查询检测、缓存策略 |

## Rules 格式

Rules 文件采用 Markdown 格式，支持 YAML frontmatter：

```markdown
---
name: rule-name
severity: high
trigger: tool-name
---

# 规则名称

## 规则描述

规则详细说明...

## 检查逻辑

如何检测违规...

## 修复建议

如何修复问题...
```

### Frontmatter 字段

| 字段 | 说明 | 必需 |
|------|------|------|
| name | 规则名称 | 是 |
| severity | 严重程度 (critical/high/medium/low) | 是 |
| trigger | 触发工具 | 否 |
| enabled | 是否启用 | 否 |

## Regras-de-Segurança

### 禁止硬编码凭证

```markdown
---
name: no-hardcoded-credentials
severity: critical
trigger: Bash
---

# 禁止硬编码凭证

## 规则描述

禁止在代码中硬编码 API keys、密码、tokens 等敏感信息。

## 检查逻辑

使用正则表达式检测常见的凭证模式：
- `api[_-]?key\s*=\s*["'][A-Za-z0-9]{20,}`
- `password\s*=\s*["'][^"']+`
- `token\s*=\s*["'][A-Za-z0-9]{32,}`

## 修复建议

使用环境变量：
```javascript
const apiKey = process.env.API_KEY;
```
```

### SQL 注入防护

```markdown
---
name: no-sql-injection
severity: critical
trigger: Read
---

# SQL 注入防护

## 规则描述

禁止使用字符串拼接构建 SQL 查询。

## 检查逻辑

检测字符串拼接的 SQL 语句：
- `"SELECT * FROM users WHERE id = " + userId`
- `query("SELECT * FROM " + tableName)`

## 修复建议

使用参数化查询：
```javascript
db.query("SELECT * FROM users WHERE id = ?", [userId]);
```
```

## Estilo-de-Código规则

### 函数长度限制

```markdown
---
name: max-function-length
severity: medium
---

# 函数长度限制

## 规则描述

函数应该保持简短，理想情况下不超过 50 行。

## 检查逻辑

统计函数的行数，超过 50 行给出警告。

## 修复建议

将长函数拆分为多个小函数，每个负责单一职责。
```

### 变量命名规范

```markdown
---
name: consistent-naming
severity: low
---

# 变量命名规范

## 规则描述

使用一致的命名约定。

## 检查逻辑

- 变量和函数使用 camelCase
- 常量使用 UPPER_SNAKE_CASE
- 类名使用 PascalCase

## 修复建议

参考项目的 naming-conventions.md。
```

## Regras-de-Teste

### 覆盖率要求

```markdown
---
name: minimum-test-coverage
severity: high
---

# 最低测试覆盖率

## 规则描述

所有新代码必须有 80% 以上的测试覆盖率。

## 检查逻辑

运行测试覆盖率工具，检查是否达到 80%。

## 修复建议

为未覆盖的代码编写测试用例。
```

### AAA 测试模式

```markdown
---
name: aaa-test-pattern
severity: medium
---

# AAA 测试模式

## 规则描述

测试应遵循 Arrange-Act-Assert 结构。

## 检查逻辑

检查测试是否包含三个明确的阶段。

## 修复建议

重写测试，明确分离：
1. Arrange - 准备测试数据
2. Act - 执行被测操作
3. Assert - 验证结果
```

## Git 规则

### 提交信息格式

```markdown
---
name: conventional-commits
severity: medium
trigger: Bash
condition: command.includes('git commit')
---

# 提交信息格式

## 规则描述

遵循 Conventional Commits 规范。

## 检查逻辑

验证提交信息格式：
```
<type>: <description>

[optional body]
```

类型：feat, fix, docs, test, refactor, perf, ci, chore

## 修复建议

使用正确的格式重写提交信息。
```

## 规则优先级

| 级别 | 说明 | 行为 |
|------|------|------|
| critical | 严重安全风险 | 立即阻止 |
| high | 重要质量问题 | 警告并阻止 |
| medium | 建议改进 | 警告但不阻止 |
| low | 风格建议 | 仅提示 |

## 编写项目规则

### 步骤 1: 分析项目需求

```markdown
# 项目规则需求分析

## 必须规范
1. 安全：禁止硬编码凭证
2. 测试：新代码 80% 覆盖
3. 提交：符合 Conventional Commits

## 建议规范
1. 函数不超过 50 行
2. 文件不超过 800 行
3. 禁止 console.log
```

### 步骤 2: 创建规则文件

在项目中创建 `.claude/rules/` 目录：

```
project/
└── .claude/
    └── rules/
        ├── security.md
        ├── testing.md
        ├── coding-style.md
        └── git.md
```

### 步骤 3: 配置规则优先级

创建 `rules.json` 配置执行顺序：

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

## 配套教材

完整的 Rules 文档位于：`../../DocumentosDeReferência/rules/Git-Workflow.md`

## 下一步

- 学习[练习题目](./exercises/Exercícios.md)
- 阅读 [Rules 完整文档](../../DocumentosDeReferência/rules/Git-Workflow.md)