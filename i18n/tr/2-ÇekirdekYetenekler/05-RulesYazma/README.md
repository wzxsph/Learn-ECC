# 05 Rules Yazımı

> 编写项目级规则，定制Kod İnceleme和质量标准

## 模块Hedef

Bitir本模块后，你将Yapabilir：

- 理解 Claude Code Rules 的格式和分类
- 编写Güvenlik Kuralları、Kod Stili规则、Test Kuralları
- 创建项目特定的开发规范
- 配置规则优先级和执行策略

## Rules 分类体系

| 分类 | Kullanım | Örnek |
|------|------|------|
| **Güvenlik Kuralları** | Güvenlik检查、漏洞防护 | 不允许硬编码密码、SQL Enjeksiyonu防护 |
| **Kod Stili** | 编码规范、格式化 | 缩进、命名约定、注释规范 |
| **Test Kuralları** | Test覆盖率、Test质量 | 80% 覆盖率要求、AAA 模式 |
| **Git 规则** | 提交规范、分支策略 | 提交信息格式、PR 流程 |
| **性能规则** | Performans Optimizasyonu、资源Yönet | N+1 查询检测、缓存策略 |

## Rules 格式

Rules 文件采用 Markdown 格式，Destekle YAML frontmatter：

```markdown
---
name: rule-name
severity: high
trigger: tool-name
---

# 规则Ad

## 规则Tanım

规则详细Açıklama...

## 检查逻辑

如何检测违规...

## 修复建议

如何修复问题...
```

### Frontmatter 字段

| 字段 | Açıklama | 必需 |
|------|------|------|
| name | 规则Ad | 是 |
| severity | 严重程度 (critical/high/medium/low) | 是 |
| trigger | 触发工具 | 否 |
| enabled | 是否启用 | 否 |

## Güvenlik Kuralları

### 禁止Sabit Kodlu Kimlik Bilgileri

```markdown
---
name: no-hardcoded-credentials
severity: critical
trigger: Bash
---

# 禁止Sabit Kodlu Kimlik Bilgileri

## 规则Tanım

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

### SQL Enjeksiyonu防护

```markdown
---
name: no-sql-injection
severity: critical
trigger: Read
---

# SQL Enjeksiyonu防护

## 规则Tanım

禁止使用字符串拼接构建 SQL 查询。

## 检查逻辑

检测字符串拼接的 SQL 语句：
- `"SELECT * FROM users WHERE id = " + userId`
- `query("SELECT * FROM " + tableName)`

## 修复建议

Parametreli Sorgu Kullan：
```javascript
db.query("SELECT * FROM users WHERE id = ?", [userId]);
```
```

## Kod Stili规则

### 函数长度限制

```markdown
---
name: max-function-length
severity: medium
---

# 函数长度限制

## 规则Tanım

函数Olmalı保持简短，理想情况下不超过 50 行。

## 检查逻辑

统计函数的行数，超过 50 行给出Uyarı。

## 修复建议

将长函数拆Bölünmüş多个小函数，每个负责单一Sorumluluk。
```

### 变量命名规范

```markdown
---
name: consistent-naming
severity: low
---

# 变量命名规范

## 规则Tanım

使用一致的命名约定。

## 检查逻辑

- 变量和函数使用 camelCase
- 常量使用 UPPER_SNAKE_CASE
- 类名使用 PascalCase

## 修复建议

参考项目的 naming-conventions.md。
```

## Test Kuralları

### 覆盖率要求

```markdown
---
name: minimum-test-coverage
severity: high
---

# 最低Test覆盖率

## 规则Tanım

所有新代码Zorunlu有 80% 以上的Test覆盖率。

## 检查逻辑

运行Test覆盖率工具，检查是否达到 80%。

## 修复建议

为未覆盖的代码编写Test用例。
```

### AAA Test模式

```markdown
---
name: aaa-test-pattern
severity: medium
---

# AAA Test模式

## 规则Tanım

Test应遵循 Arrange-Act-Assert 结构。

## 检查逻辑

检查Test是否包含三个明确的阶段。

## 修复建议

重写Test，明确分离：
1. Arrange - 准备Test数据
2. Act - 执行被测操作
3. Assert - Doğrulama结果
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

## 规则Tanım

遵循 Conventional Commits 规范。

## 检查逻辑

Doğrulama提交信息格式：
```
<type>: <description>

[optional body]
```

Tür：feat, fix, docs, test, refactor, perf, ci, chore

## 修复建议

使用正确的格式重写提交信息。
```

## 规则优先级

| 级别 | Açıklama | 行为 |
|------|------|------|
| critical | 严重Güvenlik风险 | 立即阻止 |
| high | 重要质量问题 | Uyarı并阻止 |
| medium | 建议改进 | Uyarı但不阻止 |
| low | 风格建议 | 仅İpucu |

## 编写项目规则

### Adımlar 1: 分析项目需求

```markdown
# 项目规则需求分析

## Zorunlu规范
1. Güvenlik：禁止Sabit Kodlu Kimlik Bilgileri
2. Test：新代码 80% 覆盖
3. 提交：符合 Conventional Commits

## 建议规范
1. 函数不超过 50 行
2. 文件不超过 800 行
3. 禁止 console.log
```

### Adımlar 2: 创建规则文件

在项目中创建 `.claude/rules/` Dizin：

```
project/
└── .claude/
    └── rules/
        ├── security.md
        ├── testing.md
        ├── coding-style.md
        └── git.md
```

### Adımlar 3: 配置规则优先级

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

## Destekleyici Materyaller

完整的 Rules 文档位于：`../../ReferansBelgeleri/rules/Git İş Akışı.md`

## Sonraki Adım

- 学习[Alıştırma题目](./exercises/Alıştırma.md)
- 阅读 [Rules 完整文档](../../ReferansBelgeleri/rules/Git İş Akışı.md)