---
name: agent-eval
description: 对编程智能体（Claude Code、Aider、Codex 等）进行自定义任务的头对头比较，包含通过率、成本、时间和一致性指标
origin: ECC
tools: Read, Write, Edit, Bash, Grep, Glob
---

# Agent Eval 技能

一款轻量级 CLI 工具，用于对可复现任务进行编程智能体的头对头比较。每个"哪个编程智能体最好？"的比较都是凭感觉的——这款工具将其系统化。

## 何时激活

- 在你自己的代码库上比较编程智能体（Claude Code、Aider、Codex 等）
- 在采用新工具或模型前测量智能体性能
- 当智能体更新其模型或工具时运行回归检查
- 为团队提供数据驱动的智能体选择决策

## 安装

> **注意：** 在查看源代码后，从其仓库安装 agent-eval。

## 核心概念

### YAML 任务定义

以声明方式定义任务。每个任务指定要做什么、涉及哪些文件，以及如何判断成功：

```yaml
name: add-retry-logic
description: 为 HTTP 客户端添加指数退避重试机制
repo: ./my-project
files:
  - src/http_client.py
prompt: |
  为所有 HTTP 请求添加带指数退避的重试机制。
  最多重试 3 次。初始延迟 1 秒，最大延迟 30 秒。
judge:
  - type: pytest
    command: pytest tests/test_http_client.py -v
  - type: grep
    pattern: "exponential_backoff|retry"
    files: src/http_client.py
commit: "abc1234"  # 固定到特定提交以确保可复现性
```

### Git Worktree 隔离

每次智能体运行都使用独立的 git worktree——无需 Docker。这提供了可复现性隔离，使智能体不会相互干扰或破坏基础仓库。

### 收集的指标

| 指标 | 测量内容 |
|--------|-----------------|
| 通过率 | 智能体产生的代码是否通过评判？ |
| 成本 | 每个任务的 API 花费（当可用时） |
| 时间 | 完成所需的挂钟秒数 |
| 一致性 | 跨重复运行的通过率（例如，3/3 = 100%） |

## 工作流程

### 1. 定义任务

创建 `tasks/` 目录，包含 YAML 文件，每个任务一个：

```bash
mkdir tasks
# 编写任务定义（参见上面的模板）
```

### 2. 运行智能体

针对你的任务执行智能体：

```bash
agent-eval run --task tasks/add-retry-logic.yaml --agent claude-code --agent aider --runs 3
```

每次运行：
1. 从指定提交创建全新的 git worktree
2. 将提示词交给智能体
3. 运行评判标准
4. 记录通过/失败、成本和时间

### 3. 比较结果

生成比较报告：

```bash
agent-eval report --format table
```

```
Task: add-retry-logic (3 runs each)
┌──────────────┬───────────┬────────┬────────┬─────────────┐
│ Agent        │ Pass Rate │ Cost   │ Time   │ Consistency │
├──────────────┼───────────┼────────┼────────┼─────────────┤
│ claude-code  │ 3/3       │ $0.12  │ 45s    │ 100%        │
│ aider        │ 2/3       │ $0.08  │ 38s    │  67%        │
└──────────────┴───────────┴────────┴────────┴─────────────┘
```

## 评判类型

### 基于代码的（确定性）

```yaml
judge:
  - type: pytest
    command: pytest tests/ -v
  - type: command
    command: npm run build
```

### 基于模式匹配的

```yaml
judge:
  - type: grep
    pattern: "class.*Retry"
    files: src/**/*.py
```

### 基于模型的（LLM 作为评判）

```yaml
judge:
  - type: llm
    prompt: |
      此实现是否正确处理了指数退避？
      检查：最大重试次数、递增延迟、抖动。
```

## 最佳实践

- **从 3-5 个任务开始**，代表你的真实工作负载，而不是玩具示例
- **每个智能体至少运行 3 次试验**，以捕捉方差——智能体是非确定性的
- **在任务 YAML 中固定提交**，使结果在数天/数周内可复现
- **每个任务至少包含一个确定性评判**（测试、构建）——LLM 评判会增加噪音
- **同时跟踪成本和通过率**——95% 通过率的智能体如果成本是 10 倍，可能不是正确选择
- **对任务定义进行版本管理**——它们是测试固件，像代码一样对待

## 链接

- 仓库：[github.com/joaquinhuigomez/agent-eval](https://github.com/joaquinhuigomez/agent-eval)