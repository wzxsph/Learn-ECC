# 04 - Skills 开发

学习如何创建可复用的 Skills 工作流。

## 概述

ECC 提供 **234+ [Skills](../../참조문서/skills/最佳实践.md)**，按领域分类，涵盖最佳实践、编程语言、框架、测试、安全、前端与设计、后端与 API、部署与 DevOps 等 16 个领域。

---

## 1. Skill 格式

### 1.1 基本结构

Skills 使用 Markdown 格式，包含清晰的章节结构：

```markdown
# Skill 名称

## When to Use
## How It Works
## Examples
```

### 1.2 YAML Frontmatter

```yaml
---
name: skill-name
description: Skill 用途描述
category: 分类
---
```

### 1.3 章节结构

| 章节 | 说明 |
|------|------|
| **When to Use** | 何时使用此 Skill |
| **How It Works** | 工作原理和步骤 |
| **Examples** | 使用示例 |

---

## 2. Skills 索引

### 2.1 按领域分类

| 领域 | 说明 |
|------|------|
| [最佳实践](../../참조문서/skills/最佳实践.md) | 编码标准、错误处理、自主循环 |
| [编程语言](../../참조문서/skills/编程语言.md) | Python/Go/Rust/Kotlin/C++ 等 |
| [框架](../../참조문서/skills/框架.md) | Django/Laravel/NestJS/Spring Boot 等 |
| [测试](../../참조문서/skills/测试.md) | TDD/单元测试/集成测试/E2E |
| [安全](../../참조문서/skills/安全.md) | 安全审查、漏洞扫描 |
| [前端与设计](../../참조문서/skills/前端与设计.md) | 前端开发、设计系统 |
| [后端与API](../../참조문서/skills/后端与API.md) | 后端服务、API 设计、数据库 |
| [部署与DevOps](../../참조문서/skills/部署与DevOps.md) | Docker/K8s/部署策略 |
| [监控与可观测性](../../참조문서/skills/监控与可观测性.md) | 可观测性、网络诊断 |
| [自动化与脚本](../../참조문서/skills/自动化与脚本.md) | 自主循环、持续学习、代理工程 |
| [搜索与数据获取](../../참조문서/skills/搜索与数据获取.md) | Exa 搜索、数据抓取、MCP |
| [GitHub与协作](../../참조문서/skills/GitHub与协作.md) | GitHub 工作流、代码审查 |
| [AI与机器学习](../../참조문서/skills/AI与机器学习.md) | 神经网络、PyTorch、MLOps |
| [云原生与基础设施](../../참조문서/skills/云原生与基础设施.md) | Kubernetes、Docker、Terraform |
| [特殊领域技能](../../참조문서/skills/特殊领域技能.md) | 区块链、游戏开发、音视频、IoT |
| [开发工具链](../../참조문서/skills/开发工具链.md) | 测试框架、CI/CD、代码质量 |
| [前沿技术](../../참조문서/skills/前沿技术.md) | AI Agent、量子计算、边缘计算 |

### 2.2 热门 Skills

| Skill | 用途 |
|-------|------|
| TDD 工作流 | 测试驱动开发标准流程 |
| 代码审查 | 代码质量检查清单 |
| 安全审查 | OWASP Top 10 检查 |
| API 设计 | RESTful API 设计原则 |
| Docker 部署 | 容器化部署最佳实践 |
| CI/CD 流水线 | 持续集成和部署 |

---

## 3. 创建自定义 Skill

### 3.1 创建步骤

1. **确定用途**：明确 Skill 要解决的问题
2. **编写结构**：按照 When to Use / How It Works / Examples 结构
3. **添加 YAML frontmatter**：提供元数据
4. **保存到 skills/ 目录**：使用小写 + 连字符命名

### 3.2 示例 Skill

```markdown
---
name: my-custom-skill
description: 执行特定任务的工作流
category: 自定义
---

# My Custom Skill

## When to Use

当需要进行特定任务 X 时使用此 Skill。

## How It Works

1. 步骤一：准备数据
2. 步骤二：执行核心操作
3. 步骤三：验证结果
4. 步骤四：清理资源

## Examples

### 示例 1：基本用法

```
执行 My Custom Skill 来完成 X
```

### 示例 2：高级用法

```
使用自定义参数执行 My Custom Skill
```
```

### 3.3 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 文件名 | 小写 + 连字符 | `code-review.md`, `tdd-workflow.md` |
| 目录 | 按领域组织 | `skills/编程语言/` |
| 位置 | 项目 skills/ 或全局 ~/.claude/skills/ | |

---

## 4. 工作流设计

### 4.1 步骤定义

清晰定义每个步骤：

```markdown
## How It Works

1. **需求分析**：理解用户需求
2. **方案设计**：制定实施方案
3. **代码实现**：编写代码
4. **测试验证**：确保质量
5. **文档更新**：记录变更
```

### 4.2 条件分支

处理不同场景：

```markdown
## How It Works

1. 分析输入
2. 根据条件分支：
   - 如果是 A，执行步骤 X
   - 如果是 B，执行步骤 Y
   - 否则，执行步骤 Z
3. 汇总结果
```

### 4.3 错误处理

标准化的错误处理：

```markdown
## How It Works

1. 执行操作
2. 检查结果
   - 成功：继续下一步
   - 失败：记录错误并提供修复建议
3. 返回最终结果
```

---

## 5. 参数化模板

### 5.1 参数定义

```markdown
## How It Works

1. 接收输入参数：
   - `name`: 名称（必填）
   - `type`: 类型（可选，默认值：`standard`）
2. 处理参数
3. 返回结果
```

### 5.2 变量替换

```markdown
## Examples

### 基本用法

使用默认参数：
```
执行 Skill，名称为 {{name}}
```
```

### 自定义参数

指定自定义类型：
```
执行 Skill，名称为 {{name}}，类型为 {{type}}
```
```

---

## 6. 发布和分享

### 6.1 本地存储

| 位置 | 说明 |
|------|------|
| 项目 `skills/` | 项目特定的 Skills |
| `~/.claude/skills/` | 全局可用的 Skills |

### 6.2 导入导出

```bash
# 导出 Skill 到文件
/skill-export my-skill

# 导入 Skill 从文件
/skill-import path/to/skill.md
```

### 6.3 Skill 市场

ECC 支持从市场安装共享的 Skills。

---

## 7. Skill 创建工作流

使用 `/skill-create` 命令分析 git 历史并生成 Skill 文件：

```bash
# 分析 git 历史并生成 Skill
/skill-create

# 指定分析范围
/skill-create --days 30

# 指定输出路径
/skill-create --output ./my-skills/
```

---

## 8. 最佳实践

### 8.1 设计原则

| 原则 | 说明 |
|------|------|
| **单一职责** | 每个 Skill 只做一件事 |
| **可组合** | Skill 之间可以组合使用 |
| **可复用** | 设计通用而非特定 |
| **可测试** | 提供验证步骤 |

### 8.2 文档质量

| 要求 | 说明 |
|------|------|
| 清晰的 When to Use | 让用户知道何时使用 |
| 详细的 How It Works | 步骤清晰可执行 |
| 实用的 Examples | 提供真实用例 |
| 错误处理说明 | 告诉用户如何处理错误 |

### 8.3 维护建议

- 定期更新以反映最新实践
- 根据用户反馈改进
- 保持简洁，避免过度复杂
- 添加版本号便于追踪

---

## 练习

完成 [练习](./exercises/练习.md) 中的任务。

---

[返回核心能力目录](../README.md)