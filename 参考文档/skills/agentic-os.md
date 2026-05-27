---
name: agentic-os
description: 在 Claude Code 上构建持久化多智能体操作系统。涵盖内核架构、专业智能体、斜杠命令、基于文件的记忆、计划任务自动化以及状态管理，无需外部数据库。
origin: ECC
---

# 智能体操作系统

将 Claude Code 视为持久化运行时/操作系统，而非聊天会话。本技能将生产级智能体架构规范化：内核配置负责将任务路由到专业智能体、持久化基于文件的记忆、计划任务自动化，以及 JSON/markdown 数据层。

## 激活时机

- 在 Claude Code 内构建多智能体工作流
- 设置可在会话重启后继续运行的持久化 Claude Code 自动化
- 创建用于重复任务的"个人操作系统"或"智能体操作系统"
- 用户提到"智能体操作系统"、"个人操作系统"、"多智能体"、"智能体协调器"、"持久化智能体"
- 构建长期项目，其上下文必须在会话之间保持连贯

## 架构概览

智能体操作系统有四个层级，每个层级对应项目根目录下的一个目录。

```
project-root/
├── CLAUDE.md          # 内核：身份、路由规则、智能体注册表
├── agents/            # 专业智能体定义（markdown 提示词）
├── .claude/commands/  # 斜杠命令：用户命令行接口
├── scripts/           # 守护脚本：计划任务或事件驱动任务
└── data/              # 状态：JSON/markdown 文件系统，无外部数据库
```

### 层级职责

| 层级 | 用途 | 持久化方式 |
|---|---|---|
| 内核（`CLAUDE.md`） | 身份、路由、模型策略、智能体注册表 | Git 跟踪 |
| 智能体（`agents/`） | 带作用域工具和记忆的专业智能体身份 | Git 跟踪 |
| 命令（`.claude/commands/`） | 用户斜杠命令（`/daily-sync`、`/outreach`） | Git 跟踪 |
| 脚本（`scripts/`） | 由 cron 或 webhooks 触发的 Python/JS 守护进程 | Git 跟踪 |
| 状态（`data/`） | 只追加日志、项目状态、决策记录 | Git 忽略或跟踪 |

## 内核

`CLAUDE.md` 是内核。它充当 COO/编排器。Claude 在会话开始时读取它，并据此路由工作。

### 内核结构

```markdown
# CLAUDE.md - 智能体操作系统内核

## 身份
你是 [项目名] 的 COO。你将任务路由给专业智能体。
你从不直接写代码。你委托给合适的智能体并综合结果。

## 智能体注册表

| 智能体 | 角色 | 触发条件 |
|---|---|---|
| @dev | 代码、架构、调试 | 用户说"构建"、"修复"、"重构" |
| @writer | 文档、内容、邮件 | 用户说"写"、"起草"、"博客" |
| @researcher | 研究、分析、事实核查 | 用户说"研究"、"分析"、"比较" |
| @ops | DevOps、部署、基础设施 | 用户说"部署"、"CI"、"服务器" |

## 路由规则
1. 解析用户请求中的意图关键词
2. 匹配到智能体注册表的触发条件列
3. 从 `agents/<名称>.md` 加载对应的智能体文件
4. 带着完整上下文交接执行
5. 综合结果并呈现给用户

## 模型策略
- 默认模型：使用仓库或 harness 的默认配置。
- @dev 任务：复杂架构优先使用高推理能力模型。
- @researcher 任务：使用配置了研究能力的模型和已批准搜索工具。
- 成本上限：超过项目配置的消费阈值前发出警告。
```

### 核心原则

内核应**小巧且声明式**。路由逻辑存在于普通 markdown 表格中，而非代码中。这使系统可审查、可编辑，且无需调试。

## 专业智能体

每个智能体是 `agents/` 下的独立 markdown 文件。Claude 在路由任务时加载相应的智能体文件。

### 智能体定义格式

```markdown
# @dev - 软件工程师

## 身份
你是一名高级软件工程师。你编写简洁、经过测试的生产级代码。
你偏好简单的解决方案。当需求不明确时，你会提出澄清问题。

## 记忆范围
- 阅读 `data/projects/<当前项目>.md` 获取上下文
- 阅读 `data/decisions/` 了解架构决策
- 将执行日志追加到 `data/logs/<日期>-@dev.md`

## 工具访问
- 项目根目录内的完整文件系统访问
- Git 操作（状态、差异、提交、分支）
- 测试运行器访问
- `.claude/mcp.json` 中配置的 MCP 服务器

## 约束
- 为新功能编写测试
- 不直接提交到 `main`；使用功能分支
- 优先编辑现有文件而非创建新文件
- 函数尽量保持在 50 行以内
```

### 多智能体协作模式

当任务跨越多个智能体时，内核可以顺序或并行运行它们：

```
用户："构建一个着陆页并撰写发布博客文章"

内核路由：
1. @dev - "使用 [需求] 构建着陆页"
2. @writer - "使用着陆页文案为 [产品] 撰写发布博客文章"
3. 内核将两个输出综合为统一回复
```

对于并行执行，可以使用 Claude Code 的后台任务能力或调用 Claude Code 并传入特定智能体上下文的 shell 脚本。

## 命令和日常工作流

斜杠命令是 `.claude/commands/` 下的 markdown 文件。它们定义可复用的工作流。

### 命令结构

```markdown
# /daily-sync

执行早间简报：

1. 阅读 `data/logs/last-sync.md` 获取上下文
2. 检查项目状态：`git status`、待处理 PR、CI 健康状态
3. 审查 `data/inbox/` 中的新任务或需要决策的事项
4. 生成阻塞项、优先级和下一步行动摘要
5. 将简报追加到 `data/logs/daily/<日期>.md`
```

### 标准命令集

| 命令 | 用途 |
|---|---|
| `/daily-sync` | 早间简报：状态、阻塞项、优先级 |
| `/outreach` | 执行外联工作流（邮件、LinkedIn 等） |
| `/research <主题>` | 带引用跟踪的深度研究 |
| `/apply-jobs` | 为目标职位定制简历和求职信 |
| `/analytics` | 从 Stripe、GitHub 或自定义来源拉取指标 |
| `/interview-prep` | 生成闪卡或模拟面试问题 |
| `/decision <主题>` | 记录决策，包含利弊分析和选定方案 |

### 命令激活方式

将命令文件放在 `.claude/commands/<命令名>.md`。Claude Code 自动发现它们。用户通过 `/<命令名>` 调用。

## 持久化记忆

记忆基于文件。不需要向量数据库、Redis 或 PostgreSQL。`data/` 中的 JSON 和 markdown 文件就是数据库。

### 记忆目录结构

```
data/
├── daily-logs/         # 只追加的每日活动日志
├── projects/           # 每个项目的上下文文件
├── decisions/          # 架构和业务决策（ADR 格式）
├── inbox/              # 等待分类的新任务或想法
├── contacts/           # 人、公司、关系备注
└── templates/          # 可复用的提示词和格式
```

### 每日日志格式

```markdown
# 2026-04-22 - 每日日志

## 会话
- 09:00 - 会话 1：重构认证模块（@dev）
- 11:30 - 会话 2：起草投资者更新（@writer）

## 决策
- 从 JWT 切换到会话 cookie（见 `data/decisions/2026-04-22-auth.md`）

## 阻塞项
- 等待供应商提供 API 密钥（2026-04-24 跟进）

## 下一步行动
- [ ] 合并认证重构 PR
- [ ] 发送投资者更新以供审核
```

### 自动反思模式

在每个会话结束时，内核追加一条反思：

```markdown
## 反思 - 会话 3
- 有效的做法：并行智能体执行节省了 20 分钟
- 无效的做法：@researcher 遇到付费墙资源，需要更好的源排序
- 需要改变的：研究笔记中添加 `source-tier` 字段（A/B/C 可信度）
```

这创建了一个反馈循环，无需代码更改即可随时间改进系统。

## 计划自动化

智能体操作系统任务使用外部 cron 运行调度，而非 Claude Code 内置 cron（会话结束即终止）。

### macOS：LaunchAgent

```xml
<!-- ~/Library/LaunchAgents/com.agentic.daily-sync.plist -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" ...>
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.agentic.daily-sync</string>
    <key>ProgramArguments</key>
    <array>
        <string>/claude</string>
        <string>--cwd</string>
        <string>/path/to/project</string>
        <string>--command</string>
        <string>/daily-sync</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>8</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
    <key>StandardOutPath</key>
    <string>/tmp/agentic-daily-sync.log</string>
</dict>
</plist>
```

### Linux：systemd Timer

```ini
# ~/.config/systemd/user/agentic-daily-sync.service
[Unit]
Description=Agentic OS Daily Sync

[Service]
Type=oneshot
ExecStart=/usr/local/bin/claude --cwd /path/to/project --command /daily-sync
```

```ini
# ~/.config/systemd/user/agentic-daily-sync.timer
[Unit]
Description=Run daily sync every morning

[Timer]
OnCalendar=*-*-* 8:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

### 跨平台：pm2

```bash
# ecosystem.config.js
module.exports = {
  apps: [{
    name: 'agentic-daily-sync',
    script: 'claude',
    args: '--cwd /path/to/project --command /daily-sync',
    cron_restart: '0 8 * * *',
    autorestart: false
  }]
};
```

## 数据层

数据层是你的文件系统。结构化数据使用 JSON，人类可读内容使用 markdown。

### JSON 用于结构化状态

```json
// data/projects/website-v2.json
{
  "name": "Website v2",
  "status": "in-progress",
  "milestone": "beta-launch",
  "agents_involved": ["@dev", "@writer"],
  "files": {
    "spec": "docs/website-v2-spec.md",
    "design": "designs/website-v2.fig"
  },
  "metrics": {
    "commits": 47,
    "last_session": "2026-04-22T11:30:00Z"
  }
}
```

### Markdown 用于叙事内容

任何人类需要阅读的内容使用 markdown：决策、日志、研究笔记、联系人记录。

### 模式演进

不要重命名现有字段。添加新字段并标记旧字段为已废弃：

```json
{
  "name": "Website v2",
  "status": "in-progress",
  "milestone": "beta-launch",
  "_deprecated_priority": "high",
  "priority_v2": { "level": "high", "rationale": "Blocks investor demo" }
}
```

这保持历史数据可读，无需迁移脚本。

## 反模式

### 单体单一智能体

```markdown
# 错误做法 - 一个智能体做所有事
你是一位全栈开发者、作家、研究员和 DevOps 工程师。
```

拆分为专业智能体，由内核处理路由。

### 无状态会话

```markdown
# 错误做法 - 会话之间无记忆
每次 Claude Code 打开时都从头开始。
```

总是在会话开始时读取 `data/`，会话结束时写回。

### 硬编码凭证

```markdown
# 错误做法 - API 密钥在智能体文件或 CLAUDE.md 中
你的 OpenAI API 密钥是 sk-xxxxxxxx
```

使用环境变量或由脚本加载的 `.env` 文件。智能体通过 `process.env.API_KEY` 引用。

### 简单状态使用外部数据库

```markdown
# 错误做法 - 为单人用户的智能体操作系统使用 PostgreSQL
```

在有多并发用户或 GB 级数据之前，使用 JSON/markdown 文件。

### 过度设计的路由

```markdown
# 错误做法 - 路由逻辑在代码而非 markdown 表格中
if (intent.includes('deploy')) { agent = opsAgent; }
```

在 `CLAUDE.md` markdown 表格中保持路由声明式。它是可审查、可编辑、可调试的。

## 最佳实践

- [ ] `CLAUDE.md` 保持在 200 行以内，可放入上下文窗口
- [ ] 每个智能体文件保持在 100 行以内，专注于一个领域
- [ ] `data/` 对敏感日志使用 Git 忽略，对决策和规格说明使用 Git 跟踪
- [ ] 命令使用祈使式命名：`/daily-sync`，而非 `/run-daily-sync`
- [ ] 日志只追加；从不编辑过去的每日日志
- [ ] 每个智能体都有 `记忆范围` 部分定义它读取哪些文件
- [ ] 在每个会话结束时撰写反思
- [ ] 计划任务使用外部 cron（LaunchAgent、systemd、pm2），而非 Claude Code 的会话 cron
- [ ] 成本跟踪：在 `data/logs/<日期>-costs.json` 中记录每个会话的 API 消费
- [ ] 一个项目 = 一个智能体操作系统。不要在不相关项目间共享单个 `CLAUDE.md`