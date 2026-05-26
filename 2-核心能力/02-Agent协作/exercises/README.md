# Agent协作练习

## 练习目标

学会在 Claude Code 中协调多个 Agent 完成复杂任务。

## 练习内容

### 练习 1：并行任务执行
启动 2 个并行 Agent，分别完成：
- Agent A：分析项目结构
- Agent B：检查代码规范

### 练习 2：链式任务
实现 3 个 Agent 的链式调用：
- Agent 1：收集需求
- Agent 2：生成代码
- Agent 3：审查代码

### 练习 3：结果聚合
将多个 Agent 的输出合并为统一报告。

## 模板

```markdown
# Agent 协作方案

## 任务描述
简述要完成的协作任务。

## 参与 Agent
| Agent | 角色 | 任务 |
|-------|------|------|
| agent-a | 分析 | xxx |
| agent-b | 执行 | xxx |

## 执行流程
1. 启动 Agent A
2. 启动 Agent B
3. 聚合结果

## 预期输出
描述最终输出格式。
```

## 验证
检查各 Agent 输出是否正确聚合。
