---
name: nanoclaw-repl
description: 操作和扩展 NanoClaw v2，ECC 的零依赖、会话感知的 REPL，基于 claude -p 构建。
origin: ECC
---

# NanoClaw REPL

使用此技能来运行或扩展 `scripts/claw.js`。

## 功能

- 持久化基于 Markdown 的会话
- 使用 `/model` 切换模型
- 使用 `/load` 动态加载技能
- 使用 `/branch` 进行会话分支
- 使用 `/search` 跨会话搜索
- 使用 `/compact` 进行历史压缩
- 使用 `/export` 导出为 md/json/txt
- 使用 `/metrics` 查看会话指标

## 操作指南

1. 保持会话任务聚焦
2. 在高风险更改前进行分支
3. 在重要里程碑后进行压缩
4. 在分享或归档前进行导出

## 扩展规则

- 保持零外部运行时依赖
- 保留 Markdown 即数据库的兼容性
- 保持命令处理器确定性且本地化