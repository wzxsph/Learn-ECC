# Utility Scripts

This document introduces the main utility scripts in the ECC (Everything Claude Code) project.

## Table of Contents

- [ecc.js](#eccjs) - Main CLI entry point
- [install-apply.js](#install-applyjs) - Installation executor
- [claw.js](#clawjs) - NanoClaw REPL
- [harness-audit.js](#harness-auditjs) - Toolchain audit
- [catalog.js](#catalogjs) - Component catalog viewer
- [build-opencode.js](#build-opencodejs) - OpenCode build

---

## ecc.js

**Path**: `scripts/ecc.js`

ECC selective installation CLI main entry point, unified dispatcher for all subcommands.

### Command List

| Command | Script | Description |
|---------|--------|-------------|
| `install` | install-apply.js | Install ECC content to target |
| `plan` | install-plan.js | Check installation manifest and parse plan |
| `catalog` | catalog.js | Discover installation config files and component IDs |
| `consult` | consult.js | Recommend components via natural language query |
| `list-installed` | list-installed.js | Check installation status of current context |
| `doctor` | doctor.js | Diagnose missing or drifted ECC management files |
| `repair` | repair.js | Restore drifted or missing ECC management files |
| `auto-update` | auto-update.js | Pull latest ECC changes and reinstall |
| `status` | status.js | Query SQLite state store summary |
| `platform-audit` | platform-audit.js | Audit GitHub queue, discussions, roadmap, etc. |
| `security-ioc-scan` | ci/scan-supply-chain-iocs.js | Scan supply chain IOCs for dependency and AI tool persistence |
| `sessions` | sessions-cli.js | List or inspect sessions in SQLite state store |
| `work-items` | work-items.js | Track linked Linear, GitHub work items |
| `session-inspect` | session-inspect.js | Emit canonical session snapshot from dmux or Claude history target |
| `loop-status` | loop-status.js | Check stale loop wakeups and pending tool results in Claude scripts |
| `uninstall` | uninstall.js | Delete ECC management files recorded in install-state |

### Usage Examples

```bash
# Direct language installation
ecc typescript

# Installation with config file
ecc install --profile developer --target claude

# View installation plan
ecc plan --profile core --target cursor

# List available components
ecc catalog profiles
ecc catalog components --family language

# Show component details
ecc catalog show framework:nextjs

# Consult recommendations
ecc consult "security reviews"

# Check installed
ecc list-installed --json

# Diagnose issues
ecc doctor --target cursor
ecc repair --dry-run

# Auto update
ecc auto-update --dry-run

# Status query
ecc status --json
ecc status --exit-code
ecc status --markdown --write status.md

# Platform audit
ecc platform-audit --json --allow-untracked docs/drafts/

# Security scan
ecc security-ioc-scan --home

# Session management
ecc sessions
ecc sessions session-active --json

# Work item tracking
ecc work-items upsert linear-ecc-20 --source linear --source-id ECC-20 --title "Review control-plane contract" --status blocked
ecc work-items sync-github --repo affaan-m/ECC

# Check loop status
ecc loop-status --json

# Uninstall
ecc uninstall --target antigravity --dry-run
```

---

## install-apply.js

**Path**: `scripts/install-apply.js`

ECC installation runtime, keeping traditional language installation entry points intact while moving target-specific change logic into testable Node code.

### Supported Targets

| Target | Description |
|--------|-------------|
| `claude` | Install to ~/.claude/, rules/skills go to rules/ecc and skills/ecc |
| `claude-project` | Install to ./.claude/ (project-level) |
| `cursor` | Install rules, hooks, cursor config to ./.cursor/ |
| `antigravity` | Install rules, workflows, skills, agents to ./.agent/ |
| `codex` | Install shared agents/config to ~/.codex/ |
| `gemini` | Install project-level Gemini config to ./.gemini/ |
| `opencode` | Install shared commands/hooks/config to ~/.opencode/ |
| `codebuddy` | Install commands, agents, skills to ./.codebuddy/ |
| `joycode` | Install commands, agents, skills to ./.joycode/ |
| `qwen` | Install to ~/.qwen/ |
| `zed` | Install to ./.zed/ |

### Installation Modes

1. **Traditional Language Mode**: `install.sh <language>` (e.g., `ecc-install typescript`)
2. **Profile Mode**: `install.sh --profile <name> [--with <component>]... [--without <component>]...`
3. **Module Mode**: `install.sh --modules <id,id,...>`
4. **Skill Mode**: `install.sh --skills <skill-id[,skill-id...]>`
5. **Localization Mode**: `install.sh --target claude|claude-project --locale <locale-code>`

### Options

- `--dry-run` - Show installation plan without copying files
- `--json` - Emit machine-readable JSON format
- `--config <path>` - Load installation intent from file

---

## claw.js

**Path**: `scripts/claw.js`

NanoClaw v2 - Zero-dependency, session-aware REPL for Everything Claude Code, built on `claude -p`.

### Features

- Session history persisted in `~/.claude/claw/`
- Support for dynamic skill loading as context
- Session compression for context length management
- Session branching and export
- Cross-session search

### REPL Commands

| Command | Description |
|---------|-------------|
| `/help` | Show help |
| `/clear` | Clear current session history |
| `/history` | Print full conversation history |
| `/sessions` | List saved sessions |
| `/model [name]` | Show/set model |
| `/load <skill-name>` | Load skill into active context |
| `/branch <session-name>` | Branch current session to new session |
| `/search <query>` | Cross-session search |
| `/compact` | Keep recent rounds, compress old context |
| `/export <md|json|txt> [path]` | Export session |
| `/metrics` | Show session metrics |
| `exit` | Exit REPL |

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `CLAW_SESSION` | `default` | Initial session name |
| `CLAW_MODEL` | `sonnet` | Default model |
| `CLAW_SKILLS` | empty | List of skills to load (comma-separated) |

### Usage Examples

```bash
# Start with default session
node scripts/claw.js

# Start with specified session
CLAW_SESSION=my-session node scripts/claw.js

# Start with skills loaded
CLAW_SKILLS="javascript-patterns,python-testing" node scripts/claw.js
```

---

## harness-audit.js

**Path**: `scripts/harness-audit.js`

Deterministic toolchain audit tool, based on explicit file/rule checks. Automatically detects ECC repository mode vs consumer project mode.

### Audit Categories

| Category | Description |
|----------|-------------|
| Tool Coverage | Tool coverage completeness |
| Context Efficiency | Context efficiency |
| Quality Gates | Quality gates |
| Memory Persistence | Memory persistence |
| Eval Coverage | Evaluation coverage |
| Security Guardrails | Security guardrails |
| Cost Efficiency | Cost efficiency |
| GitHub Integration | GitHub integration |
| Vercel/Netlify/Cloudflare/Fly Integration | Platform integrations |

### Usage Examples

```bash
# Audit current directory
node scripts/harness-audit.js

# Specify scope
node scripts/harness-audit.js --scope repo
node scripts/harness-audit.js --scope hooks
node scripts/harness-audit.js --scope skills

# Specify root directory
node scripts/harness-audit.js --root /path/to/project

# JSON format output
node scripts/harness-audit.js --format json

# Combined usage
node scripts/harness-audit.js repo --format json --root .
```

---

## catalog.js

**Path**: `scripts/catalog.js`

Tool to discover ECC installation components and configuration files.

### Commands

#### profiles

List all installation profiles.

```bash
node scripts/catalog.js profiles
node scripts/catalog.js profiles --json
```

#### components

List installation components, filterable by family and target.

```bash
node scripts/catalog.js components
node scripts/catalog.js components --family language
node scripts/catalog.js components --family framework
node scripts/catalog.js components --target claude
node scripts/catalog.js components --json
```

#### show

Show detailed information for a specific component.

```bash
node scripts/catalog.js show <component-id>
node scripts/catalog.js show framework:nextjs --json
```

### Component Families

- `baseline` - Baseline components
- `language` - Language components (javascript, typescript, python, etc.)
- `framework` - Framework components (nextjs, react, vue, etc.)
- `capability` - Capability components
- `agent` - Agent components
- `skill` - Skill components

---

## build-opencode.js

**Path**: `scripts/build-opencode.js`

Build script for OpenCode plugin TypeScript code.

### Features

1. Clean `dist` directory
2. Compile code in `.opencode` directory using TypeScript compiler
3. Output to `.opencode/dist`

### Prerequisites

Requires development dependencies installed in root directory, ensuring TypeScript compiler is available.

### Usage

```bash
node scripts/build-opencode.js
```

## 目录

- [ecc.js](#eccjs) - 主命令行入口
- [install-apply.js](#install-applyjs) - 安装执行器
- [claw.js](#clawjs) - NanoClaw REPL
- [harness-audit.js](#harness-auditjs) - 工具链审计
- [catalog.js](#catalogjs) - 组件目录查看器
- [build-opencode.js](#build-opencodejs) - OpenCode 构建

---

## ecc.js

**路径**: `scripts/ecc.js`

ECC 选择性安装的 CLI 主入口点,统一调度所有子命令。

### 命令列表

| 命令 | 脚本 | 说明 |
|------|------|------|
| `install` | install-apply.js | 安装 ECC 内容到目标 |
| `plan` | install-plan.js | 检查安装清单和解析计划 |
| `catalog` | catalog.js | 发现安装配置文件和组件 ID |
| `consult` | consult.js | 根据自然语言查询推荐组件 |
| `list-installed` | list-installed.js | 检查当前上下文的安装状态 |
| `doctor` | doctor.js | 诊断缺失或漂移的 ECC 管理文件 |
| `repair` | repair.js | 恢复漂移或缺失的 ECC 管理文件 |
| `auto-update` | auto-update.js | 拉取最新 ECC 更改并重新安装 |
| `status` | status.js | 查询 SQLite 状态存储摘要 |
| `platform-audit` | platform-audit.js | 审计 GitHub 队列、讨论、路线图等 |
| `security-ioc-scan` | ci/scan-supply-chain-iocs.js | 扫描依赖和 AI 工具持久化面的供应链 IOC |
| `sessions` | sessions-cli.js | 列出或检查 SQLite 状态存储中的会话 |
| `work-items` | work-items.js | 跟踪链接的 Linear、GitHub 工作项 |
| `session-inspect` | session-inspect.js | 从 dmux 或 Claude 历史目标发出规范会话快照 |
| `loop-status` | loop-status.js | 检查 Claude 脚本中的过时循环唤醒和待处理工具结果 |
| `uninstall` | uninstall.js | 删除记录在 install-state 中的 ECC 管理文件 |

### 使用示例

```bash
# 直接安装语言
ecc typescript

# 带配置文件的安装
ecc install --profile developer --target claude

# 查看安装计划
ecc plan --profile core --target cursor

# 列出可用组件
ecc catalog profiles
ecc catalog components --family language

# 显示组件详情
ecc catalog show framework:nextjs

# 咨询推荐
ecc consult "security reviews"

# 检查已安装
ecc list-installed --json

# 诊断问题
ecc doctor --target cursor
ecc repair --dry-run

# 自动更新
ecc auto-update --dry-run

# 状态查询
ecc status --json
ecc status --exit-code
ecc status --markdown --write status.md

# 平台审计
ecc platform-audit --json --allow-untracked docs/drafts/

# 安全扫描
ecc security-ioc-scan --home

# 会话管理
ecc sessions
ecc sessions session-active --json

# 工作项跟踪
ecc work-items upsert linear-ecc-20 --source linear --source-id ECC-20 --title "Review control-plane contract" --status blocked
ecc work-items sync-github --repo affaan-m/ECC

# 检查循环状态
ecc loop-status --json

# 卸载
ecc uninstall --target antigravity --dry-run
```

---

## install-apply.js

**路径**: `scripts/install-apply.js`

ECC 安装运行时,保持传统语言安装入口点完整,同时将目标特定的变更逻辑移入可测试的 Node 代码。

### 支持的目标

| 目标 | 说明 |
|------|------|
| `claude` | 安装到 ~/.claude/,规则/技能放在 rules/ecc 和 skills/ecc |
| `claude-project` | 安装到 ./.claude/ (项目级) |
| `cursor` | 安装规则、钩子、光标配置到 ./.cursor/ |
| `antigravity` | 安装规则、工作流、技能、代理到 ./.agent/ |
| `codex` | 安装共享代理/配置到 ~/.codex/ |
| `gemini` | 安装项目级 Gemini 配置到 ./.gemini/ |
| `opencode` | 安装共享命令/钩子/配置到 ~/.opencode/ |
| `codebuddy` | 安装命令、代理、技能到 ./.codebuddy/ |
| `joycode` | 安装命令、代理、技能到 ./.joycode/ |
| `qwen` | 安装到 ~/.qwen/ |
| `zed` | 安装到 ./.zed/ |

### 安装方式

1. **传统语言模式**: `install.sh <language>` (如 `ecc-install typescript`)
2. **Profile 模式**: `install.sh --profile <name> [--with <component>]... [--without <component>]...`
3. **模块模式**: `install.sh --modules <id,id,...>`
4. **技能模式**: `install.sh --skills <skill-id[,skill-id...]>`
5. **本地化模式**: `install.sh --target claude|claude-project --locale <locale-code>`

### 选项

- `--dry-run` - 显示安装计划但不复制文件
- `--json` - 发出机器可读的 JSON 格式
- `--config <path>` - 从文件加载安装意图

---

## claw.js

**路径**: `scripts/claw.js`

NanoClaw v2 - 面向 Everything Claude Code 的零外部依赖、会话感知的 REPL,基于 `claude -p` 构建。

### 功能

- 会话历史持久化存储在 `~/.claude/claw/`
- 支持动态加载技能作为上下文
- 会话压缩以管理上下文长度
- 会话分支和导出
- 跨会话搜索

### REPL 命令

| 命令 | 说明 |
|------|------|
| `/help` | 显示帮助 |
| `/clear` | 清除当前会话历史 |
| `/history` | 打印完整对话历史 |
| `/sessions` | 列出已保存的会话 |
| `/model [name]` | 显示/设置模型 |
| `/load <skill-name>` | 加载技能到活动上下文 |
| `/branch <session-name>` | 将当前会话分支到新会话 |
| `/search <query>` | 跨会话搜索 |
| `/compact` | 保留最近的轮次,压缩旧上下文 |
| `/export <md\|json\|txt> [path]` | 导出会话 |
| `/metrics` | 显示会话指标 |
| `exit` | 退出 REPL |

### 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `CLAW_SESSION` | `default` | 初始会话名称 |
| `CLAW_MODEL` | `sonnet` | 默认模型 |
| `CLAW_SKILLS` | 空 | 加载的技能列表 (逗号分隔) |

### 使用示例

```bash
# 使用默认会话启动
node scripts/claw.js

# 使用指定会话启动
CLAW_SESSION=my-session node scripts/claw.js

# 加载技能启动
CLAW_SKILLS="javascript-patterns,python-testing" node scripts/claw.js
```

---

## harness-audit.js

**路径**: `scripts/harness-audit.js`

确定性工具链审计工具,基于显式文件/规则检查。自动检测 ECC 仓库模式 vs 消费者项目模式。

### 审计类别

| 类别 | 说明 |
|------|------|
| Tool Coverage | 工具覆盖完整性 |
| Context Efficiency | 上下文效率 |
| Quality Gates | 质量门禁 |
| Memory Persistence | 记忆持久化 |
| Eval Coverage | 评估覆盖 |
| Security Guardrails | 安全护栏 |
| Cost Efficiency | 成本效率 |
| GitHub Integration | GitHub 集成 |
| Vercel/Netlify/Cloudflare/Fly Integration | 各平台集成 |

### 使用示例

```bash
# 审计当前目录
node scripts/harness-audit.js

# 指定作用域
node scripts/harness-audit.js --scope repo
node scripts/harness-audit.js --scope hooks
node scripts/harness-audit.js --scope skills

# 指定根目录
node scripts/harness-audit.js --root /path/to/project

# JSON 格式输出
node scripts/harness-audit.js --format json

# 组合使用
node scripts/harness-audit.js repo --format json --root .
```

---

## catalog.js

**路径**: `scripts/catalog.js`

发现 ECC 安装组件和配置文件的工具。

### 命令

#### profiles

列出所有安装配置文件。

```bash
node scripts/catalog.js profiles
node scripts/catalog.js profiles --json
```

#### components

列出安装组件,可按家族和目标筛选。

```bash
node scripts/catalog.js components
node scripts/catalog.js components --family language
node scripts/catalog.js components --family framework
node scripts/catalog.js components --target claude
node scripts/catalog.js components --json
```

#### show

显示特定组件的详细信息。

```bash
node scripts/catalog.js show <component-id>
node scripts/catalog.js show framework:nextjs --json
```

### 组件家族

- `baseline` - 基础组件
- `language` - 语言组件 (javascript, typescript, python, etc.)
- `framework` - 框架组件 (nextjs, react, vue, etc.)
- `capability` - 能力组件
- `agent` - 代理组件
- `skill` - 技能组件

---

## build-opencode.js

**路径**: `scripts/build-opencode.js`

构建 OpenCode 插件的 TypeScript 代码。

### 功能

1. 清理 `dist` 目录
2. 使用 TypeScript 编译器编译 `.opencode` 目录下的代码
3. 输出到 `.opencode/dist`

### 前提条件

需要已安装根目录的开发依赖,确保 TypeScript 编译器可用。

### 使用

```bash
node scripts/build-opencode.js
```
