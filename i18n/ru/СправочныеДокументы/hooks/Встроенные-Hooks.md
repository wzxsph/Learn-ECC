# Встроенные Hooks

## Обзор

ECC插件Предоставлять了丰富的内置HookРеализовать，位于`ECC/scripts/hooks/`Содержание中。

## PreToolUse 内置钩子

### pre:bash:dispatcher

**Тип**: PreToolUse
**matcher**: Bash
**Описание**: Bash预检分发器，整合了质量检查、tmux提醒、git推送提醒和GateGuard检查

**功能**:
- 开发服务器阻止器 - 阻止在tmux外运行`npm run dev`等命令
- Tmux提醒 - 建议对长时间运行的命令（npm test、cargo build、docker）使用tmux
- Git推送提醒 - 提醒在`git push`前审查更改
- Pre-commit质量检查 - 运行质量检查：lint暂存文件、Проверка提交信息格式、检测console.log/debugger/密钥

**退出码**:
- 0: Предупреждение但继续
- 2: 阻止执行

---

### pre:write:doc-file-warning

**Тип**: PreToolUse
**matcher**: Write
**Описание**: Предупреждение创建非标准文档文件

**功能**:
- 允许标准文件: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- 允许Содержание: docs/, skills/
- Предупреждение其他.md/.txt文件
- 跨平台路径处理

**退出码**: 0（仅Предупреждение）

---

### pre:edit-write:suggest-compact

**Тип**: PreToolUse
**matcher**: Edit|Write
**Описание**: 在逻辑间隔（约每50次工具调用）建议手动`/compact`

**功能**:
- 追踪工具调用次数
- 在适当间隔提醒上下文压缩
- 非阻塞，仅Предоставлять建议

**退出码**: 0（建议）

---

### pre:observe:continuous-learning

**Тип**: PreToolUse
**matcher**: *
**Описание**: 记录工具意图以Поддерживать持续学习信号

**功能**:
- 捕获工具使用观察
- Используется для模式提取和分析
- 异步执行，不阻塞

**退出码**: 0

---

### pre:governance-capture

**Тип**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Описание**: 捕获治理事件（密钥、策略违规、审批请求）

**功能**:
- 检测秘密/密钥泄露
- 捕获策略违规
- 记录审批请求

**启用条件**: `ECC_GOVERNANCE_CAPTURE=1`

**退出码**: 0

---

### pre:config-protection

**Тип**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**Описание**: 阻止修改linter/formatter配置文件

**功能**:
- 阻止.eslintrc、.prettierrc等修改
- 引导修复代码而非削弱配置

**退出码**: 0

---

### pre:mcp-health-check

**Тип**: PreToolUse
**matcher**: *
**Описание**: MCP工具执行前检查MCP服务器健康状态

**功能**:
- 检查MCP服务器状态
- 阻止对不健康MCP服务器的调用

**退出码**: 0

---

### pre:edit-write:gateguard-fact-force

**Тип**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: 事实强制GateGuard：阻止每个文件的首次编辑/写入，要求在允许前进行调查

**功能**:
- 阻止首次编辑
- 要求确认：导入、数据模式、用户指令
- 确保有充分的Обоснование再修改

**退出码**: 0

---

## PostToolUse 内置钩子

### post:bash:dispatcher

**Тип**: PostToolUse
**matcher**: Bash
**Описание**: Bash后检分发器，Используется для日志记录、PR和构建通知

**功能**:
- PR日志 - `gh pr create`后记录PR URL和审查命令
- 构建分析 - 构建命令后的后台分析（异步，非阻塞）

**退出码**: 0

---

### post:quality-gate

**Тип**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: 文件编辑后运行快速质量检查

**功能**:
- Качество кода检查
- 异步执行
- 不阻塞编辑操作

**退出码**: 0

---

### post:edit:design-quality-check

**Тип**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: Предупреждение前端编辑偏离为通用Шаблон外观的UI

**功能**:
- 检测UIСпроектировать质量
- 防止过于通用的Шаблон外观

**退出码**: 0

---

### post:edit:accumulator

**Тип**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: 记录编辑的JS/TS文件路径，以便在Stop时批量格式化和Тип检查

**功能**:
- 累积编辑的文件列表
- 供stop:format-typecheck使用

**退出码**: 0

---

### post:edit:console-warn

**Тип**: PostToolUse
**matcher**: Edit
**Описание**: 编辑后Предупреждениеconsole.log语句

**功能**:
- 检测代码中的console.log
- Предупреждение开发者清理调试代码

**退出码**: 0

---

### post:governance-capture

**Тип**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Описание**: 从工具输出捕获治理事件

**功能**:
- 分析工具输出
- 检测潜在问题

**启用条件**: `ECC_GOVERNANCE_CAPTURE=1`

**退出码**: 0

---

### post:session-activity-tracker

**Тип**: PostToolUse
**matcher**: *
**Описание**: 记录每个会话的工具调用和文件活动

**功能**:
- 追踪工具使用统计
- 文件活动记录
- Используется дляECC2指标

**退出码**: 0

---

### post:observe:continuous-learning

**Тип**: PostToolUse
**matcher**: *
**Описание**: 记录工具结果以Поддерживать持续学习信号

**功能**:
- 捕获工具执行结果
- Поддерживать模式分析

**退出码**: 0

---

### post:ecc-metrics-bridge

**Тип**: PostToolUse
**matcher**: *
**Описание**: 维护运行中的会话指标聚合

**功能**:
- 追踪token和成本指标
- 供状态栏和上下文监视器使用

**退出码**: 0

---

### post:ecc-context-monitor

**Тип**: PostToolUse
**matcher**: *
**Описание**: 在上下文耗尽、高成本、范围 creep 或工具循环时注入代理Предупреждение

**功能**:
- 上下文使用监控
- 成本Предупреждение
- 范围 creep 检测
- 工具循环检测

**退出码**: 0

---

### post:mcp-health-check

**Тип**: PostToolUseFailure
**matcher**: *
**Описание**: 追踪失败的MCP工具调用，标记不健康的服务器并尝试重连

**功能**:
- 失败追踪
- 健康状态标记
- 自动重连尝试

**退出码**: 0

---

## Stop 内置钩子

### stop:format-typecheck

**Тип**: Stop
**matcher**: *
**Описание**: 批量格式化（Biome/Prettier）和Тип检查（tsc）本次响应中编辑的所有JS/TS文件

**功能**:
- Prettier/Biome格式化
- TypeScriptТип检查
- 在Stop时运行一次，而非每次编辑后

**退出码**: 0

**超时**: 300秒

---

### stop:check-console-log

**Тип**: Stop
**matcher**: *
**Описание**: 每次响应后检查修改文件中的console.log

**功能**:
- 扫描修改的文件
- 报告console.log使用情况

**退出码**: 0

---

### stop:session-end

**Тип**: Stop
**matcher**: *
**Описание**: 每次响应后持久化会话状态（Stop携带transcript_path）

**功能**:
- 会话状态保存
- 上下文持久化

**退出码**: 0

---

### stop:evaluate-session

**Тип**: Stop
**matcher**: *
**Описание**: 评估会话以提取可学习的模式

**功能**:
- 模式识别
- 持续学习Поддерживать
- 异步执行

**退出码**: 0

---

### stop:cost-tracker

**Тип**: Stop
**matcher**: *
**Описание**: 追踪每个会话的token和成本指标

**功能**:
- Token使用统计
- 成本估算
- 轻量级运行成本遥测标记

**退出码**: 0

---

### stop:desktop-notify

**Тип**: Stop
**matcher**: *
**Описание**: 在Claude响应时发送macOS/WSL桌面通知，包含Задача摘要

**功能**:
- 桌面通知
- Задача摘要显示

**退出码**: 0

---

## SessionStart 内置钩子

### session:start

**Тип**: SessionStart
**matcher**: *
**Описание**: 加载先前上下文并检测新会话的包Управлять器

**功能**:
- 加载有界限的先前上下文
- 项目状态检测
- 包Управлять器检测（npm/pnpm/yarn/bun）

**环境变量**:
- `ECC_SESSION_START_MAX_CHARS`: 控制额外上下文大小（默认8000字符）
- `ECC_SESSION_START_CONTEXT=off`: 完全禁用额外上下文

**退出码**: 0

---

## PreCompact 内置钩子

### pre:compact

**Тип**: PreCompact
**matcher**: *
**Описание**: 在上下文压缩前保存状态

**功能**:
- 会话状态持久化
- 为压缩准备上下文

**退出码**: 0

---

## SessionEnd 内置钩子

### session:end:marker

**Тип**: SessionEnd
**matcher**: *
**Описание**: 会话结束生命周期标记

**功能**:
- 生命周期事件标记
- 清理日志

**退出码**: 0

---

## 内置钩子速查表

### PreToolUse 钩子

| ID | Matcher | 功能 | 阻塞 |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | 质量/tmux/推送/GateGuard检查 | 可阻塞 |
| pre:write:doc-file-warning | Write | 文档文件Предупреждение | 否 |
| pre:edit-write:suggest-compact | Edit\|Write | 建议压缩 | 否 |
| pre:observe:continuous-learning | * | 持续学习观察 | 否 |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit | 治理事件捕获 | 否 |
| pre:config-protection | Write\|Edit\|MultiEdit | 配置保护 | 否 |
| pre:mcp-health-check | * | MCP健康检查 | 可阻塞 |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | 首次编辑GateGuard | 可阻塞 |

### PostToolUse 钩子

| ID | Matcher | 功能 | 异步 |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR日志/构建通知 | 是 |
| post:quality-gate | Edit\|Write\|MultiEdit | 质量门检查 | 是 |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | Спроектировать质量检查 | 否 |
| post:edit:accumulator | Edit\|Write\|MultiEdit | 编辑累积器 | 否 |
| post:edit:console-warn | Edit | console.logПредупреждение | 否 |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit | 治理事件捕获 | 否 |
| post:session-activity-tracker | * | 会话活动追踪 | 否 |
| post:observe:continuous-learning | * | 持续学习观察 | 是 |
| post:ecc-metrics-bridge | * | 指标桥接 | 否 |
| post:ecc-context-monitor | * | 上下文监控 | 否 |
| post:mcp-health-check | * (PostToolUseFailure) | MCP健康检查 | 否 |

### Stop 钩子

| ID | Matcher | 功能 | 超时 |
|----|---------|------|------|
| stop:format-typecheck | * | 批量格式化和Тип检查 | 300s |
| stop:check-console-log | * | console.log检查 | 30s |
| stop:session-end | * | 会话状态持久化 | 10s |
| stop:evaluate-session | * | 会话评估 | 10s |
| stop:cost-tracker | * | 成本追踪 | 10s |
| stop:desktop-notify | * | 桌面通知 | 10s |

### 生命周期钩子

| ID | Event | 功能 |
|----|-------|------|
| session:start | SessionStart | 加载上下文和检测包Управлять器 |
| pre:compact | PreCompact | 压缩前状态保存 |
| session:end:marker | SessionEnd | 会话结束标记 |
