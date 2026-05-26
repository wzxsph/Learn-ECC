# 01 - [ECC](../เอกสารอ้างอิง/README.md) 简介

## 什么是 [ECC](../เอกสารอ้างอิง/README.md)？

[ECC](../เอกสารอ้างอิง/README.md)（Everything Claude Code）是一个 Claude Code 插件系统，提供生产级的 [Agent](../เอกสารอ้างอิง/agents/1.%20代码审查类.md)、[Skills](../เอกสารอ้างอิง/skills/最佳实践.md)、hooks、rules、[Commands](../เอกสารอ้างอิง/commands/01-Workflowหลัก.md) 和 MCP 配置方案。源自 Anthropic Hackathon 获奖作品，经过 10+ 月的日常使用打磨。

## [ECC](../เอกสารอ้างอิง/README.md) 能做什么？

### 1. [Agent](../เอกสารอ้างอิง/agents/1.%20代码审查类.md) 系统
[ECC](../เอกสารอ้างอิง/README.md) 提供 60+ 专业 [Agent](../เอกสารอ้างอิง/agents/1.%20代码审查类.md)，涵盖多语言审查、构建修复、架构设计等：

| [Agent](../เอกสารอ้างอิง/agents/1.%20代码审查类.md) | 用途 |
|-------|------|
| `planner` | 功能实施规划 |
| `code-reviewer` | 代码质量和安全审查 |
| `security-reviewer` | 安全漏洞分析 |
| `build-error-resolver` | 构建错误修复 |
| `architect` | 系统架构设计 |
| `tdd-guide` | 测试驱动开发指导 |

### 2. [Skills](../เอกสารอ้างอิง/skills/最佳实践.md) 工作流
[Skills](../เอกสารอ้างอิง/skills/最佳实践.md) 是主要的工作流表面，[ECC](../เอกสารอ้างอิง/README.md) 提供 232+ [Skills](../เอกสารอ้างอิง/skills/最佳实践.md)：

- `tdd-workflow` - 测试驱动开发
- `e2e-testing` - 端到端测试
- `security-review` - 安全审查清单
- `continuous-learning-v2` - 基于 instinct 的持续学习
- `eval-harness` - 验证循环评估

### 3. [Commands](../เอกสารอ้างอิง/commands/01-Workflowหลัก.md) 命令
保留传统斜杠命令兼容性（共 75+ 命令）：

| 命令 | 功能 |
|------|------|
| `/plan` | 创建实施计划 |
| `/code-review` | 代码审查 |
| `/build-fix` | 修复构建错误 |
| `/learn` | 从会话提取模式 |
| `/skill-create` | 从 Git 历史生成 Skill |
| `/sessions` | 会话历史管理 |

### 4. Hooks 自动化
灵活的钩子系统（8 种事件类型）：
- `SessionStart` - 会话启动时加载上下文
- `SessionEnd` - 会话结束时保存状态
- `PreToolUse` - 工具执行前验证
- `PostToolUse` - 工具执行后格式化

### 5. Rules 规则体系
全面的开发规范（common + 语言特定）：
- **common/** - 通用原则（安全、编码风格、测试、性能）
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. MCP 服务
支持 14+ MCP 服务器配置：GitHub、Supabase、Vercel、Railway、Context7、Exa、Playwright 等。

## 跨平台支持

[ECC](../เอกสารอ้างอิง/README.md) 支持多种 AI 编程工具：

| 工具 | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 types |
| Cursor | 48 | 共享 | 共享 | 15 types |
| Codex | 共享 | 指令式 | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 types |
| GitHub Copilot | - | 6 prompts | 指令式 | - |

## 核心概念

### [Agent](../เอกสารอ้างอิง/agents/1.%20代码审查类.md)
可复用的子任务执行者，每个 [Agent](../เอกสารอ้างอิง/agents/1.%20代码审查类.md) 有明确职责。通过组合多个 [Agent](../เอกสารอ้างอิง/agents/1.%20代码审查类.md) 构建复杂工作流。

### [Skill](../เอกสารอ้างอิง/skills/最佳实践.md)
定义完成特定任务的工作流程，是 [ECC](../เอกสารอ้างอิง/README.md) 的主要工作流表面。

### Hook
在特定时机自动触发的脚本，支持工作流自动化。

### Rule
Claude Code 必须遵守的约束规则，保证输出质量。


- [安装配置](./02-การติดตั้ง.md) - 开始使用 [ECC](../เอกสารอ้างอิง/README.md)
- [返回入门目录](../README.md)
