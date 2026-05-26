# 其他命令

## 概述

其他杂项命令，包括 Jira 集成、GAN 操作、安全扫描等功能。

## 命令列表

### /jira

**用途**: 与 Jira 工单交互 - 获取、分析、评论、转换状态、搜索

**描述**: 与 Jira 项目管理工具集成，支持直接工作流中使用 Jira tickets。

**用法**:

| 命令 | 说明 |
|---|---|
| `/jira get <TICKET-KEY>` | 获取并分析 Jira 工单 |
| `/jira comment <TICKET-KEY>` | 添加进度评论 |
| `/jira transition <TICKET-KEY>` | 更改工单状态 |
| `/jira search <JQL>` | 使用 JQL 搜索问题 |

**`/jira get` 工作流**:
1. 从 Jira 获取工单（通过 MCP 或 REST API）
2. 提取所有字段：摘要、描述、验收标准、优先级、标签、关联问题
3. 可选获取评论以获取额外上下文
4. 生成结构化分析

**先决条件** - Jira 凭证配置（二选一）:

**选项 A — MCP Server（推荐）**:
将 `jira` 添加到 `mcpServers` 配置。

**选项 B — 环境变量**:
```bash
export JIRA_URL="https://yourorg.atlassian.net"
export JIRA_EMAIL="your.email@example.com"
export JIRA_API_TOKEN="your-api-token"
```

**与其他命令集成**:
- 分析工单后使用 `/plan` 创建实施计划
- 使用 `tdd-workflow` skill 进行测试驱动开发
- 使用 `/code-review` 审查实现
- 使用 `/jira comment` 或 `/jira transition` 更新工单状态

---

### /gan-build（待完善）

**用途**: GAN 构建操作

**描述**: GAN（Generative Agent Network）构建操作。

---

### /gan-design（待完善）

**用途**: GAN 设计操作

**描述**: GAN 设计相关的操作。

---

### /prune

**用途**: 删除超过 30 天未提升的陈旧 pending instinct

**描述**: 清理超过 30 天的陈旧 instinct，这些 instinct 是自动生成但从未被审查或提升的。

**用法**:
```
/prune                    # 删除超过 30 天的 instinct
/prune --max-age 60      # 自定义天数阈值
/prune --dry-run         # 预览而不删除
```

**实现**:
```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/continuous-learning-v2/scripts/instinct-cli.py" prune
```

或手动安装时（`CLAUDE_PLUGIN_ROOT` 未设置）:
```bash
python3 ~/.claude/skills/continuous-learning-v2/scripts/instinct-cli.py prune
```

**参数**:

| 参数 | 说明 |
|---|---|
| 无 | 删除超过 30 天的 instinct |
| `--max-age <days>` | 自定义时间阈值（天） |
| `--dry-run` | 预览将删除的内容，不实际删除 |

**最佳实践**:
- 使用 `--dry-run` 先预览要删除的内容
- 定期运行 prune 保持 instinct 库存整洁
- 注意：只会删除 pending 状态的 instinct，不会删除已提升的

---

### /security-scan

**用途**: 安全扫描

**描述**: 对代码库进行安全漏洞扫描。

**扫描内容**:
- 硬编码凭证
- SQL 注入风险
- XSS 漏洞
- 依赖漏洞

---

### /feature-dev（待完善）

**用途**: 功能开发助手

**描述**: 提供功能开发的辅助工作流。

---

### /cost-report

**用途**: 模型成本报告

**描述**: 生成 AI 模型使用成本报告。

---

## 相关命令

- `/security-scan` - 安全扫描
- `/jira` - Jira 集成
- `/prune` - 清理 instinct
