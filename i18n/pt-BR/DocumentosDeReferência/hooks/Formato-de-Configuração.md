# Formato-de-Configuração

## hooks.json 结构概述

ECC Sistema-Hooks的配置文件采用JSON格式，主要结构如下：

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "hooks": {
    "PreToolUse": [...],
    "PostToolUse": [...],
    "Stop": [...],
    "SessionStart": [...],
    "SessionEnd": [...],
    "PreCompact": [...],
    "PostToolUseFailure": [...]
  }
}
```

## 顶层结构

| 字段 | 类型 | 说明 |
|------|------|------|
| `$schema` | string | JSON Schema URL，用于验证配置 |
| `hooks` | object | 包含所有钩子类型的对象 |

---

## 钩子数组结构

每个事件类型（PreToolUse、PostToolUse等）包含一个钩子数组，每个钩子对象具有以下结构：

```json
{
  "matcher": "Bash|Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/my-hook.js",
      "async": false,
      "timeout": 30
    }
  ],
  "description": "钩子描述",
  "id": "unique-hook-id"
}
```

### matcher 字段

#### 类型说明
matcher字段用于指定触发钩子的工具类型，支持单个工具或多个工具（用`|`分隔）。

#### 可用值
| matcher值 | 匹配的工具有哪些 |
|-----------|-----------------|
| `*` | 所有工具 |
| `Bash` | Bash工具 |
| `Edit` | Edit工具 |
| `Write` | Write工具 |
| `Read` | Read工具 |
| `MultiEdit` | MultiEdit工具 |
| `Bash\|Edit\|Write` | Bash、Edit或Write |
| `Edit\|Write\|MultiEdit` | Edit、Write或MultiEdit |

#### 使用示例
```json
// 匹配所有工具
"matcher": "*"

// 仅匹配Bash工具
"matcher": "Bash"

// 匹配多个工具
"matcher": "Edit|Write|MultiEdit"
```

---

## hooks 数组结构

hooks数组包含一个或多个钩子命令对象：

### type 字段

| type值 | 说明 |
|--------|------|
| `command` | 执行Node.js命令 |

### command 字段

要执行的命令，通常是Node.js脚本路径。

### async 字段（可选）

```json
"async": true
```

- `true`: 异步执行，不阻塞工具执行
- `false`或省略: 同步执行，阻塞工具执行

### timeout 字段（可选）

```json
"timeout": 30
```

最大执行时间（秒）。建议：
- 同步钩子: <200ms
- 异步钩子: ≤30秒

---

## 完整配置示例

### PreToolUse 钩子示例

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/pre-bash-dispatcher.js"
    }
  ],
  "description": "Bash预检分发器，用于质量、tmux、推送和GateGuard检查",
  "id": "pre:bash:dispatcher"
}
```

### PostToolUse 钩子示例

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js post:quality-gate scripts/hooks/quality-gate.js standard,strict",
      "async": true,
      "timeout": 30
    }
  ],
  "description": "文件编辑后运行质量门检查",
  "id": "post:quality-gate"
}
```

### Stop 钩子示例

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js stop:session-end scripts/hooks/session-end.js minimal,standard,strict",
      "async": true,
      "timeout": 10
    }
  ],
  "description": "每次响应后保存会话状态",
  "id": "stop:session-end"
}
```

### SessionStart 钩子示例

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/session-start-bootstrap.js"
    }
  ],
  "description": "加载先前上下文并检测新会话的包管理器",
  "id": "session:start"
}
```

---

## 生命周期事件Formato-de-Configuração

对于SessionStart、SessionEnd、PreCompact等生命周期事件，使用不同的配置结构：

```json
{
  "description": "内存持久化的生命周期钩子定义",
  "events": [
    {
      "event": "SessionStart",
      "id": "session:start",
      "script": "scripts/hooks/session-start-bootstrap.js",
      "purpose": "在会话开始时加载有界限的先前上下文并检测项目状态",
      "blocking": false
    },
    {
      "event": "PreCompact",
      "id": "pre:compact",
      "script": "scripts/hooks/pre-compact.js",
      "purpose": "在上下文压缩前持久化会话状态",
      "blocking": false
    }
  ]
}
```

### 生命周期事件字段

| 字段 | 说明 |
|------|------|
| `event` | 事件类型（SessionStart、PreCompact、SessionEnd等） |
| `id` | 唯一标识符 |
| `script` | 脚本路径 |
| `purpose` | 用途描述 |
| `blocking` | 是否阻塞（通常为false） |

---

## 禁用钩子

### 通过编辑配置禁用

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "覆盖：允许所有.md文件创建"
      }
    ]
  }
}
```

### 通过环境变量禁用

```bash
# 禁用特定钩子（逗号分隔）
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# 禁用GateGuard
export ECC_GATEGUARD=off
```

---

## 钩子ID命名规范

ECC使用冒号分隔的命名约定：

| 前缀 | 用途 | 示例 |
|------|------|------|
| `pre:` | PreToolUse钩子 | `pre:bash:dispatcher` |
| `post:` | PostToolUse钩子 | `post:quality-gate` |
| `stop:` | Stop钩子 | `stop:session-end` |
| `session:` | 会话生命周期钩子 | `session:start` |

---

## run-with-flags.js 封装器

ECC使用`run-with-flags.js`封装器来运行钩子，支持配置文件的运行时门控：

```
node scripts/hooks/run-with-flags.js <hook-id> <script-path> <profiles>
```

### 参数说明
| 参数 | 说明 |
|------|------|
| `hook-id` | 钩子的唯一标识符 |
| `script-path` | 实际要运行的脚本路径 |
| `profiles` | 逗号分隔的配置文件列表（minimal, standard, strict） |

### 工作原理
1. 读取`ECC_HOOK_PROFILE`环境变量（默认: standard）
2. 检查钩子ID是否在`ECC_DISABLED_HOOKS`中
3. 如果允许，则运行脚本

---

## 跨平台路径处理

ECC的hook命令使用Node.js脚本实现跨平台行为：

```javascript
const path = require('path');
const homedir = require('os').homedir();

// 跨平台路径解析
const configDir = path.join(homedir, '.claude');
const hookScript = path.join(configDir, 'scripts', 'hooks', 'my-hook.js');
```

---

## 配置验证

建议使用JSON Schema验证配置：
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json"
}
```
