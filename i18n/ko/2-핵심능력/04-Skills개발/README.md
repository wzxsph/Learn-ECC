# 04 Skills 开发

> 学会创建和使用 Skills，沉淀工作流模式为可复用技能

## 模块目标

完成本模块后，你将能够：

- 理解 Skill 的标准格式和结构
- 使用 `/learn` 从会话中提取模式
- 使用 `/skill-create` 从 git 历史创建 Skill
- 创建和管理自定义 Skills

## Skill 格式

每个 Skill 都是一个 Markdown 文件，包含标准化的部分：

```markdown
# Skill 名称

## When to Use
描述何时使用这个 Skill

## How It Works
详细说明 Skill 的工作原理

## Examples
使用示例

## Related Skills
相关 Skills
```

### 必需部分

| 部分 | 说明 | 必需 |
|------|------|------|
| When to Use | 使用场景描述 | 是 |
| How It Works | 工作原理说明 | 是 |
| Examples | 使用示例 | 是 |

### 可选部分

| 部分 | 说明 |
|------|------|
| Related Skills | 相关 Skills |
| Prerequisites | 前置条件 |
| Tips | 使用技巧 |

## Skill 类型

### 1. 工作流类 Skill

描述完整的工作流程：

```markdown
# TDD Workflow

## When to Use
开发新功能或修复 bug 时使用 TDD 方法

## How It Works
1. 编写测试（红色）
2. 实现功能（绿色）
3. 重构优化（蓝色）

## Examples
/step 1: 编写失败的测试
/step 2: 实现最小代码
/step 3: 运行测试验证
/step 4: 重构
```

### 2. 模式类 Skill

沉淀代码模式：

```markdown
# API Response Pattern

## When to Use
设计 API 返回格式时使用

## How It Works
使用统一响应格式：
- success: 成功状态
- data: 响应数据
- error: 错误信息（可选）

## Examples
{ "success": true, "data": {...}, "error": null }
```

### 3. 最佳实践类 Skill

记录最佳实践：

```markdown
# Git Commit Best Practices

## When to Use
提交代码前检查提交信息

## How It Works
遵循 Conventional Commits 格式：
- feat: 新功能
- fix: 修复
- docs: 文档
- test: 测试
- refactor: 重构

## Examples
feat: add user authentication
fix: resolve login bug
docs: update README
```

## 创建 Skill 的方式

### 方式 1: 使用 `/learn`

从成功的会话中提取模式：

```
/learn "提取这个会话中的工作流模式"
```

### 方式 2: 使用 `/skill-create`

从 git 历史创建 Skill：

```
/skill-create "修复构建错误的工作流"
```

### 方式 3: 手动创建

直接编写 Skill 文件：

```markdown
# My Custom Skill

## When to Use
...

## How It Works
...
```

## Skill 存储位置

| 类型 | 位置 | 说明 |
|------|------|------|
| 官方 Skills | `~/.claude/skills/` | Claude Code 内置 |
| 自定义 Skills | 项目 `skills/` | 项目专用 |
| 规则 Skills | `~/.claude/rules/` | 全局规则 |

## Skill 管理命令

| 命令 | 功能 |
|------|------|
| `/skill-scout` | 搜索可用 Skills |
| `/skill-stocktake` | 列出所有 Skills |
| `/skill-create` | 创建新 Skill |
| `/skill-health` | 检查 Skill 健康状态 |

## 使用 Skill

### 直接使用

```
/skill-name
```

### 在任务中使用

在复杂任务中引用 Skill：

```
我需要开发一个支付模块，使用 TDD 工作流
```

## Skill 最佳实践

1. **单一职责**: 每个 Skill 只做一件事
2. **清晰命名**: 使用描述性的名称
3. **完整文档**: 包含所有必需部分
4. **实际验证**: 基于真实工作流
5. **持续迭代**: 根据使用反馈改进

## 配套教材

完整的 Skills 文档位于：`../../참조문서/skills/最佳实践.md`

## 下一步

- 学习[练习题目](./exercises/练习.md)
- 阅读 [Skills 完整文档](../../참조문서/skills/最佳实践.md)