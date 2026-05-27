---
name: continuous-agent-loop
description: 带有质量门、评估和恢复控制功能的连续自主 Agent 循环模式。
origin: ECC
---

# 连续 Agent 循环

这是 v1.8+ 的规范循环技能名称。它取代了 `autonomous-loops`，同时在一个版本内保持兼容性。

## 循环选择流程

```text
启动
  |
  +-- 需要严格的 CI/PR 控制？ -- 是 --> continuous-pr
  |
  +-- 需要 RFC 分解？ -- 是 --> rfc-dag
  |
  +-- 需要探索性并行生成？ -- 是 --> infinite
  |
  +-- 默认 --> sequential
```

## 组合模式

推荐的生产环境技术栈：
1. RFC 分解 (`ralphinho-rfc-pipeline`)
2. 质量门 (`plankton-code-quality` + `/quality-gate`)
3. 评估循环 (`eval-harness`)
4. 会话持久化 (`nanoclaw-repl`)

## 失败模式

- 循环空转但无实际进展
- 重复重试但根因不变
- 合并队列停滞
- 成本因无限制升级而失控

## 恢复策略

- 冻结循环
- 运行 `/harness-audit`
- 将范围缩小到失败的单元
- 使用明确的验收标准重放