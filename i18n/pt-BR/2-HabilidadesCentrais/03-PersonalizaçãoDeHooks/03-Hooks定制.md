# 03 - Hooks 定制

学习 ECC [Hook](../../DocumentosDeReferência/hooks/Hook类型.md) 系统，实现工作流自动化。

## 概述

ECC Hooks 系统是事件驱动的自动化机制，在 Claude Code 工具执行的前后触发，用于强制代码质量、早期发现错误和自动化重复检查。

```
用户请求 → Claude选择工具 → PreToolUse钩子运行 → 工具执行 → PostToolUse钩子运行
```

---

## 1. Hook 类型详解

### 1.1 PreToolUse 钩子

在工具执行**之前**运行，可以阻止（exit code 2）或警告（stderr 输出但不阻止）。

#### 触发时机
- 在任何工具（Edit、Write、Bash、Read 等）执行之前
- 适用于所有工具操作的前置检查

#### 退出码
| 退出码 | 含义 | 行为 |
|--------|------|------|
| 0 | 成功 | 继续执行，不输出警告 |
| 2 | 阻止 | 阻止工具执行 |
| 其他非零 | 错误 | 记录日志但不阻止 |

#### 使用示例

**开发服务器阻止器：**
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

**文档文件警告：**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "警告非标准文档文件的创建"
}
```

### 1.2 PostToolUse 钩子

在工具执行**之后**运行，可以分析输出但无法阻止工具执行。

#### 触发时机
- 在任何工具完成之后运行
- 可以访问工具的输出结果

#### 使用示例

**PR 日志记录：**
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

**质量门检查：**
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

### 1.3 Stop 钩子

在每次 Claude 响应后运行，用于批量处理、最终检查和会话状态持久化。

#### 使用示例

**控制台日志检查：**
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

### 1.4 SessionStart / SessionEnd 钩子

生命周期钩子，用于会话开始和结束时的上下文加载和状态持久化。

---

## 2. Hook 类型对比表

| 类型 | 触发时机 | 能否阻止 | 能否阻塞 | 典型用途 |
|------|----------|----------|----------|----------|
| PreToolUse | 工具执行前 | 是（exit 2） | 是（同步） | 质量检查、安全验证 |
| PostToolUse | 工具执行后 | 否 | 可选（async） | 日志记录、分析 |
| Stop | 每次响应后 | 否 | 可选（async） | 批量格式化、会话摘要 |
| SessionStart | 会话开始 | 否 | 否 | 加载上下文、检测项目 |
| SessionEnd | 会话结束 | 否 | 否 | 持久化摘要、清理 |

---

## 3. 自定义 Hook 开发

### 3.1 基础结构

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // 访问工具信息
  const toolName = input.tool_name;        // "Edit", "Bash", "Write"等
  const toolInput = input.tool_input;      // 工具特定参数
  const toolOutput = input.tool_output;    // 仅在PostToolUse可用

  // 警告（非阻塞）：写入stderr
  console.error('[Hook] 警告信息');

  // 阻止（仅PreToolUse）：exit code 2
  // process.exit(2);

  // 始终将原始数据输出到stdout
  console.log(data);
});
```

### 3.2 HookInput 接口

```typescript
interface HookInput {
  tool_name: string;          // 工具名称
  tool_input: {               // 工具输入参数
    command?: string;         // Bash: 命令
    file_path?: string;       // Edit/Write/Read: 文件路径
    old_string?: string;      // Edit: 被替换的文本
    new_string?: string;      // Edit: 替换文本
    content?: string;         // Write: 文件内容
  };
  tool_output?: {             // PostToolUse only
    output?: string;          // 命令/工具输出
  };
}
```

### 3.3 关键规则：exit 0 on 非关键错误

**重要**：所有钩子必须在非关键错误时 exit 0，避免意外阻塞工具执行。

| 退出码 | 含义 | 使用场景 |
|--------|------|----------|
| 0 | 成功 | 继续执行，或非关键警告 |
| 2 | 阻止 | 仅用于 PreToolUse 的关键阻塞 |
| 其他非零 | 错误 | 仅记录日志，**不要使用** |

### 3.4 正确处理错误

```javascript
// 错误示例 - 不要这样做
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // 处理逻辑
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // 错误：会阻塞工具
  }
  console.log(data);
});

// 正确示例 - 这样做
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // 处理逻辑
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // 非关键错误，exit 0继续执行
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## 4. 常见钩子配方

### 4.1 警告 TODO/FIXME 注释

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "警告添加TODO/FIXME注释"
}
```

### 4.2 阻止创建过大文件

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');process.exit(2)}console.log(d)})\""
  }],
  "description": "阻止创建超过800行的文件"
}
```

### 4.3 用 ruff 自动格式化 Python 文件

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "编辑后用ruff自动格式化Python文件"
}
```

### 4.4 要求新源文件附带测试

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p)}}console.log(d)})\""
  }],
  "description": "添加新源文件时提醒创建测试"
}
```

---

## 5. 配置管理

### 5.1 使用 run-with-flags.js

```bash
# 使用 run-with-flags.js 包装器
node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict
```

### 5.2 环境变量控制

```bash
# minimal | standard | strict (默认: standard)
export ECC_HOOK_PROFILE=standard

# 禁用特定钩子ID（逗号分隔）
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# 控制SessionStart额外上下文大小（默认: 8000字符）
export ECC_SESSION_START_MAX_CHARS=4000

# 完全禁用SessionStart额外上下文
export ECC_SESSION_START_CONTEXT=off
```

### 5.3 集成到 hooks.json

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "node scripts/hooks/my-new-hook.js"
          }
        ],
        "description": "我的新钩子描述",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

---

## 6. 异步钩子

对于不应阻塞主流程的钩子（如后台分析）：

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node my-slow-hook.js",
    "async": true,
    "timeout": 30
  }]
}
```

**异步钩子注意事项**：
- 异步钩子运行在后台
- 无法阻塞工具执行
- 应该在 30 秒内完成
- 适用于：日志、分析、遥测

---

## 7. 性能最佳实践

| 钩子类型 | 性能要求 |
|----------|----------|
| PreToolUse | <200ms |
| PostToolUse（同步） | <1秒 |
| 异步钩子 | <30秒 |

### 避免阻塞操作

```javascript
// 不好：同步文件读取
const content = fs.readFileSync('large-file.txt', 'utf8');

// 好：异步或延迟加载
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // 处理
});
```

---

## 8. 调试钩子

### 添加调试输出

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // 处理逻辑
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### 常见问题

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| 工具被意外阻止 | 钩子 exit 非零（除 2 外） | 改用 exit 0 和 stderr 警告 |
| 挂起 | 钩子没有结束 | 确保总是输出到 stdout |
| 数据损坏 | 输出到 stdout 非 JSON | 仅输出原始 data |
| 性能问题 | 钩子太慢 | 优化或改为异步 |

---

## 练习

完成 [练习](./exercises/练习.md) 中的任务。

---

[返回核心能力目录](../README.md)