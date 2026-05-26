# 05 - Rules Yazımı

Nasıl编写定制化的开发规则。

## Genel Bakış

ECC [Rules](../../ReferansBelgeleri/rules/Kod Stili.md) 是一套强制性开发规则，涵盖Kod Stili、Test、Güvenlik、Git 工作流、Ajan Orkestrasyonu和Performans Optimizasyonu等方面。这些规则确保Kod Kalitesi和一致性，Vasıtasıyla自动化检查和审查流程实施。

---

## 1. Rules 格式

### 1.1 基本格式

Rules 使用 Markdown 格式，包含 YAML frontmatter：

```yaml
---
name: rule-name
description: 规则Tanım
category: 分类
---
```

### 1.2 规则层级

| 层级 | Açıklama | 优先级 |
|------|------|--------|
| **全局规则** | `~/.claude/rules/` | 最高 |
| **项目规则** | 项目 `.claude/rules/` | 中 |
| **本地规则** | 当前会话 | 最低 |

### 1.3 优先级

全局规则 > 项目规则 > 本地规则

---

## 2. 规则Tür详解

### 2.1 Kod Stili规则

定义代码组织的核心原则和En İyi Uygulamalar。

#### 核心要求

**不可变性（关键原则）**

```typescript
// 错误Örnek - 修改原对象
function modify(user, name) {
  user.name = name;  // 改变了原对象
  return user;
}

// 正确Örnek - Geri Dön新对象
function update(user, name) {
  return { ...user, name };  // 创建新副本
}
```

**核心Tasarım原则**

| 原则 | Açıklama | 实践 |
|------|------|------|
| **KISS** | Keep It Simple | 首选最简单的可行方案 |
| **DRY** | Don't Repeat Yourself | 避免复制粘贴导致的Gerçekleştir漂移 |
| **YAGNI** | You Aren't Gonna Need It | 在真正Gerekli时重构 |

**命名约定**

| Tür | 约定 | Örnek |
|------|------|------|
| 变量和函数 | camelCase | `calculateTotal` |
| 布尔值 | is/has/should/can 前缀 | `isActive` |
| 接口、Tür、组件 | PascalCase | `UserProfile` |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |

### 2.2 Test Kuralları

确保Kod Kalitesi的强制性Test标准。

#### 最低Test覆盖率：80%

| Test型 | Açıklama |
|----------|------|
| 单元Test | 独立函数、工具类、组件 |
| 集成Test | API 端点、数据库操作 |
| E2E Test | 关键用户流程 |

#### TDD 工作流

```
1. 编写Test (RED)   - 先写一个会失败的Test
2. 运行Test          - DoğrulamaTest失败
3. 编写最小Gerçekleştir (GREEN) - 编写VasıtasıylaTest的最小代码
4. 运行Test          - DoğrulamaTestVasıtasıyla
5. 重构 (IMPROVE)    - 改进代码结构
6. Doğrulama覆盖率        - 确保达到 80%+
```

### 2.3 Güvenlik Kuralları

强制性Güvenlik检查清单。

#### 提交前Zorunlu检查

- [ ] 无Sabit Kodlu Kimlik Bilgileri（API keys、密码、tokens）
- [ ] 所有用户输入Doğrulama
- [ ] SQL Enjeksiyonu防护（参数化查询）
- [ ] XSS 防护（输出转义）
- [ ] CSRF 保护启用
- [ ] 认证/授权Doğrulama
- [ ] 速率限制
- [ ] 错误消息不泄露敏感数据

#### 密钥Yönet

- **绝不** 在源代码中硬编码密钥
- **始终** 使用环境变量或密钥Yönet器
- 启动时Doğrulama必需的密钥
- 轮换任何可能暴露的密钥

### 2.4 Git 工作流规则

Git 提交规范和 PR 工作流。

#### 提交信息格式

```
<type>: <description>

<optional body>
```

**Type Tür**：
- `feat`: 新功能
- `fix`: 错误修复
- `refactor`: 重构
- `docs`: 文档
- `test`: Test
- `chore`: 维护
- `perf`: 性能
- `ci`: CI/CD

#### PR 工作流

1. 分析完整提交历史
2. 使用 `git diff [base-branch]...HEAD` 查看所有变更
3. 起草综合性 PR 总结
4. 包含Test计划和 TODOs
5. 如是新分支，使用 `-u` 标志推送

### 2.5 Ajan Orkestrasyonu规则

多 Ajans İşbirliği模式。

#### Anında Kullanım İlkesi

| Senaryo | Ajan |
|------|-------|
| Karmaşık özellik isteği | **planner** |
| Kod yazıldı/değiştirildi | **code-reviewer** |
| Hata düzeltme veya yeni özellik | **tdd-guide** |
| Mimari kararlar | **architect** |

#### Paralel Görev Yürütme

```markdown
# 好：Paralel Yürütme
启动 3 个 Ajan 并行：
1. Ajan 1: 认证模块Güvenlik Analizi
2. Ajan 2: 缓存系统性能审查
3. Ajan 3: 工具Tür检查

# Kötü: Gereksiz sıralı yürütme
Önce agent 1, sonra agent 2, sonra agent 3
```

### 2.6 Performans Optimizasyonu规则

模型选择和上下文Yönet。

#### 模型选择策略

| 模型 | Kullanım | 成本 |
|------|------|------|
| **Haiku 4.5** | 轻量 Agent、频繁调用、配对编程 | 低 |
| **Sonnet 4.6** | 主要开发工作、Çoklu Ajan 编排 | 中 |
| **Opus 4.5** | 复杂架构决策、深度推理 | 高 |

#### 上下文窗口Yönet

避免在最后 20% 上下文窗口进行：
- 大规模重构
- 跨多文件的特性Gerçekleştir
- 复杂交互调试

---

## 3. 规则编写

### 3.1 触发条件

```yaml
---
name: no-console-log
description: 阻止提交包含 console.log 的代码
trigger:
  - pre-commit
  - pre-push
---
```

### 3.2 检查逻辑

```markdown
## 检查逻辑

1. 扫描修改的文件
2. 查找 `console.log`、`console.error` 等
3. 如果找到，阻止提交并显示Uyarı
```

### 3.3 修复建议

```markdown
## 修复建议

- 使用结构化日志库（如 winston、pino）
- 配置日志级别（debug、info、warn、error）
- 确保生产环境日志级别合适
```

---

## 4. 违规处理

### 4.1 严重程度分级

| 级别 | 含义 | 操作 |
|------|------|------|
| CRITICAL | Güvenlik Açıkları或数据丢失风险 | **阻止** - 合并前Zorunlu修复 |
| HIGH | Bug 或重大质量问题 | **Uyarı** - 合并前应修复 |
| MEDIUM | 可维护性问题 | **İpucu** - 建议修复 |
| LOW | 样式或小建议 | **备注** - 可选 |

### 4.2 修复流程

```
1. 分析违规原因
2. 确定修复方案
3. 执行修复
4. Doğrulama修复
5. 重新提交审查
```

---

## 5. Kod Kalitesi检查清单

Bitir工作前Zorunlu检查：

- [ ] 代码可读且命名良好
- [ ] 函数短小（<50 行）
- [ ] 文件专注（<800 行）
- [ ] 无深层嵌套（>4 层）
- [ ] 适当的Hata Yönetimi
- [ ] 无硬编码值（使用常量或配置）
- [ ] 使用不可变模式（无 mutation）
- [ ] Test覆盖率 ≥80%
- [ ] 所有Güvenlik检查Vasıtasıyla

---

## 6. 创建项目特定规则

### 6.1 项目规则位置

```
项目/
└── .claude/
    └── rules/
        ├── README.md
        └── 自定义规则文件
```

### 6.2 继承全局规则

项目规则继承全局规则，可在此基础上扩展：

```markdown
# 项目特定规则

继承自全局规则，增加以下要求：

## 额外要求

- [ ] 项目特定的编码规范
- [ ] 特定的Test要求
- [ ] 特定的文档要求
```

---

## Alıştırma

Bitir [Alıştırma](./exercises/Alıştırma.md) 中的Görev。

---

[Geri DönTemel YeteneklerDizin](../README.md)