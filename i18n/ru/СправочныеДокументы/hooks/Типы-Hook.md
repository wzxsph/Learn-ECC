# HookТип

## Обзор

ECC Система Hooks是事件驱动的自动化机制，在Claude Code工具执行的前后触发，Используется для强制Качество кода、早期发现错误和自动化重复检查。

```
用户请求 → Claude选择工具 → PreToolUse钩子运行 → 工具执行 → PostToolUse钩子运行
```

## 钩子Тип详解

### 1. PreToolUse 钩子

#### ТипОписание
PreToolUse钩子在工具执行**之前**运行，Можно阻止（exit code 2）或Предупреждение（stderr输出但不阻止）。

#### 触发时机
- 在任何工具（Edit、Write、Bash、Read等）执行之前
- 适Используется для所有工具操作的前置检查

#### 参数Описание
```typescript
interface HookInput {
  tool_name: string;          // 工具Название: "Bash", "Edit", "Write", "Read"等
  tool_input: {
    command?: string;         // Bash: 要执行的命令
    file_path?: string;        // Edit/Write/Read: Цель文件路径
    old_string?: string;       // Edit: 被替换的文本
    new_string?: string;       // Edit: 替换文本
    content?: string;          // Write: 文件内容
  };
}
```

#### 退出码
| 退出码 | 含义 | 行为 |
|--------|------|------|
| 0 | 成功 | 继续执行，不输出Предупреждение |
| 2 | 阻止 | 阻止工具执行 |
| 其他非零 | 错误 | 记录日志但不阻止 |

#### 使用Пример

**开发服务器阻止器（阻止在tmux外运行npm run dev）：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "阻止在tmux外运行开发服务器"
}
```

**文档文件Предупреждение（阻止非标准.md文件）：**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Предупреждение非标准文档文件的创建"
}
```

#### Примечание事项
- PreToolUse是**阻塞型**钩子，Обязательно快速执行（<200ms）
- 不要在PreToolUse钩子中进行网络调用
- 阻止操作（exit 2）应谨慎使用，仅Используется для关键Безопасность检查

---

### 2. PostToolUse 钩子

#### ТипОписание
PostToolUse钩子在工具执行**之后**运行，Можно分析输出但无法阻止工具执行。

#### 触发时机
- 在任何工具Завершить之后运行
- Можно访问工具的输出结果

#### 参数Описание
```typescript
interface HookInput {
  tool_name: string;          // 工具Название
  tool_input: {               // 与PreToolUse相同
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // 仅在PostToolUse中可用
    output?: string;          // 命令/工具的输出
  };
}
```

#### 退出码
| 退出码 | 含义 |
|--------|------|
| 0 | 成功，继续执行 |
| 非零 | 错误，记录日志但不阻止工具执行 |

#### 使用Пример

**PR日志记录（gh pr create后记录PR URL）：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "PR创建后记录URL和审查命令"
}
```

**质量门检查（编辑后运行质量检查）：**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "文件编辑后运行质量门检查"
}
```

#### Примечание事项
- PostToolUse钩子**无法阻止**工具执行
- Можно设置为异步（async: true）以避免阻塞
- 异步钩子应在30秒内Завершить

---

### 3. Stop 钩子

#### ТипОписание
Stop钩子在每次Claude响应后运行，Используется для批量处理、最终检查和会话状态持久化。

#### 触发时机
- 在每个用户请求处理Завершить、Claude生成响应后
- 发生在会话的最后一个响应之后

#### 参数Описание
Stop钩子接收与PostToolUse相同的输入，包含完整的工具调用历史。

#### 使用Пример

**控制台日志检查（响应后检查修改文件中的console.log）：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "检查修改文件中是否有console.log"
}
```

**批量格式化和Тип检查：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "批量格式化所有编辑的JS/TS文件并进行Тип检查"
}
```

**会话摘要持久化：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "保存会话状态"
}
```

#### Примечание事项
- Stop钩子在每次响应后运行，适合做批量操作
- Можно使用async模式进行非阻塞操作
- 格式化/Тип检查等适合在Stop时批量执行

---

### 4. SessionStart 钩子

#### ТипОписание
SessionStart钩子在会话生命周期Начать时触发，Используется для加载先前上下文和检测项目状态。

#### 触发时机
- 新会话Начать时
- Claude Code启动或创建新会话时

#### 使用Пример

**加载先前上下文并检测包Управлять器：**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "在会话Начать时加载有界限的先前上下文并检测项目状态",
  "blocking": false
}
```

#### Примечание事项
- 生命周期钩子，Обязательно快速Завершить（<30秒）
- МожноЧерез`ECC_SESSION_START_MAX_CHARS`环境变量控制额外上下文大小
- Можно用`ECC_SESSION_START_CONTEXT=off`完全禁用

---

### 5. SessionEnd 钩子

#### ТипОписание
SessionEnd钩子在会话结束时触发，Используется для持久化会话结束摘要和清理。

#### 触发时机
- 会话结束时
- 当会话 transcript 元数据可用时

#### 使用Пример

**会话结束标记：**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "当transcript元数据可用时持久化会话结束摘要",
  "blocking": false
}
```

#### Примечание事项
- 异步执行为主，不应阻塞会话结束
- 生命周期标记Используется для指标收集和清理

---

### 6. PreCompact 钩子

#### ТипОписание
PreCompact钩子在上下文压缩前触发，Используется для保存状态。

#### 触发时机
- 在上下文压缩/精简之前
- 当上下文窗口接近满时

#### 使用Пример

**上下文压缩前保存状态：**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "在上下文压缩前持久化会话状态",
  "blocking": false
}
```

#### Примечание事项
- 适合保存长Задача的状态
- 非阻塞，不影响压缩流程

---

### 7. PostToolUseFailure 钩子

#### ТипОписание
PostToolUseFailure钩子在工具执行失败后触发。

#### 触发时机
- 任何工具调用失败后

#### 使用Пример

**MCP健康检查（追踪失败的MCP调用）：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "追踪失败的MCP工具调用，标记不健康的服务器并尝试重连"
}
```

#### Примечание事项
- 可Используется для重试逻辑或健康状态追踪
- 不会改变工具失败的状态

---

## 钩子Тип对比表

| Тип | 触发时机 | 能否阻止 | 能否阻塞 | 典型Назначение |
|------|----------|----------|----------|----------|
| PreToolUse | 工具执行前 | 是（exit 2） | 是（同步） | 质量检查、БезопасностьПроверка |
| PostToolUse | 工具执行后 | 否 | 可选（async） | 日志记录、分析 |
| Stop | 每次响应后 | 否 | 可选（async） | 批量格式化、会话摘要 |
| SessionStart | 会话Начать | 否 | 否 | 加载上下文、检测项目 |
| SessionEnd | 会话结束 | 否 | 否 | 持久化摘要、清理 |
| PreCompact | 压缩前 | 否 | 否 | 状态保存 |
| PostToolUseFailure | 工具失败后 | 否 | 否 | 健康检查、重试 |

---

## 环境变量控制

МожноЧерез环境变量控制钩子行为：

```bash
# minimal | standard | strict (默认: standard)
export ECC_HOOK_PROFILE=standard

# 禁用特定钩子ID（逗号分隔）
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# 禁用GateGuard
export ECC_GATEGUARD=off

# 控制SessionStart额外上下文大小（默认: 8000字符）
export ECC_SESSION_START_MAX_CHARS=4000

# 完全禁用SessionStart额外上下文
export ECC_SESSION_START_CONTEXT=off
```
