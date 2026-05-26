# 循环与自动化命令

## 概述

循环与自动化命令用于启动和管理自主循环工作模式。

## 命令列表

### /loop-start

**用途**: 启动托管自主循环模式，带安全默认设置和明确停止条件

**描述**: 启动带安全默认设置和明确停止条件的托管自主循环模式。

**用法**: `/loop-start [pattern] [--mode safe|fast]`

**参数**:

| 参数 | 值 | 说明 |
|---|---|---|
| `pattern` | `sequential` | 顺序执行任务，一个接一个 |
| | `continuous-pr` | 持续 PR 创建和合并循环 |
| | `rfc-dag` | 基于 RFC 的有向无环图工作流 |
| | `infinite` | 无限循环（需明确停止条件） |
| `--mode` | `safe`（默认） | 严格质量门禁和检查点 |
| | `fast` | 为速度减少门禁 |

**工作流**:
1. 确认仓库状态和分支策略
2. 选择循环模式和相关模型层级策略
3. 为所选模式启用所需 hooks/profile
4. 创建循环计划并在 `.claude/plans/` 下写 runbook
5. 打印启动和监控命令

**必需安全检查**:
- 验证测试在首次循环前通过
- 确保 `ECC_HOOK_PROFILE` 未全局禁用
- 确保循环有明确停止条件

**参数传递**:
- `$ARGUMENTS`: `[pattern]` 和可选的 `[--mode safe|fast]`
- 模式是可选的（默认为 sequential）
- mode 标志是可选的（默认为 safe）

**Melhores-Práticas**:
- 在 infinite 模式之前确保设置明确的停止条件
- fast 模式仅在充分测试的代码上使用
- 监控循环进度，使用 `/loop-status` 检查状态

**常见陷阱**:
- 在未验证测试的情况下启动循环
- 使用 infinite 模式而无明确停止条件
- 在全局禁用 hooks 时尝试使用安全模式

**与其他命令集成**:
- 使用 `/loop-status` 监控运行中循环
- 使用 `/santa-loop` 进行 Santa 风格循环
- 使用 `/checkpoint` 创建关键节点检查点

---

### /loop-status

**用途**: 检查运行中循环的状态

**描述**: 查看当前运行中循环的状态和进度。

---

### /santa-loop

**用途**: Santa 风格自主循环

**描述**: 一种特殊风格的自主循环模式。

---

## 相关命令

- `/loop-start` - 启动循环
- `/loop-status` - 检查状态
- `/santa-loop` - Santa 风格
