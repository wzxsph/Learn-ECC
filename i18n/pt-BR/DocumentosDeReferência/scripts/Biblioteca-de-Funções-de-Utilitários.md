# Biblioteca-de-Funções-de-Utilitários

本文档介绍 `scripts/lib/` 目录下的核心Biblioteca-de-Funções-de-Utilitários。

---

## utils.js

**路径**: `scripts/lib/utils.js`

跨平台的Biblioteca-de-Funções-de-Utilitários,适用于 Windows、macOS 和 Linux。

### 平台检测

```javascript
const { isWindows, isMacOS, isLinux } = require('./utils');

// isWindows: true on Windows
// isMacOS: true on macOS
// isLinux: true on Linux
```

### 目录操作

```javascript
const {
  getHomeDir,      // 获取用户主目录
  getClaudeDir,    // 获取 ~/.claude 目录
  getSessionsDir,  // 获取 ~/.claude/session-data 目录
  getTempDir,      // 获取临时目录
  ensureDir,       // 确保目录存在,不存在则创建
  findFiles        // 查找匹配模式的文件
} = require('./utils');

// 创建目录(如果不存在)
ensureDir('/path/to/dir');

// 查找文件
const files = findFiles('/path', '*.js', { recursive: true });
```

### 日期时间

```javascript
const {
  getDateString,      // 返回 YYYY-MM-DD 格式
  getTimeString,      // 返回 HH:MM 格式
  getDateTimeString   // 返回 YYYY-MM-DD HH:MM:SS 格式
} = require('./utils');
```

### 会话和项目

```javascript
const {
  sanitizeSessionId,    // 清理会话 ID,去除无效字符
  getSessionIdShort,    // 获取短会话 ID
  getGitRepoName,       // 获取 git 仓库名称
  getProjectName        // 获取项目名称
} = require('./utils');

// 清理会话 ID
const cleanId = sanitizeSessionId('my-session@123'); // 'my-session-123'

// 获取项目名
const project = getProjectName();
```

### 文件操作

```javascript
const {
  readFile,        // 安全读取文件,失败返回 null
  writeFile,       // 写入文件
  appendFile,      // 追加到文件
  replaceInFile,   // 替换文件内容
  countInFile,     // 统计模式出现次数
  grepFile         // 搜索文件中的模式
} = require('./utils');

// 读取文件
const content = readFile('/path/to/file.txt');

// 替换文件内容(首次匹配)
replaceInFile('/path', 'old', 'new');

// 替换所有匹配
replaceInFile('/path', 'old', 'new', { all: true });

// 统计出现次数
const count = countInFile('/path', /pattern/g);

// 搜索并返回匹配行
const matches = grepFile('/path', /regex/);
// 返回: [{ lineNumber: 1, content: '...' }]
```

### 字符串处理

```javascript
const { stripAnsi } = require('./utils');

// 移除 ANSI 转义序列(颜色等)
const clean = stripAnsi('\x1b[32mHello\x1b[0m'); // 'Hello'
```

### Hook I/O

```javascript
const { readStdinJson, log, output } = require('./utils');

// 从 stdin 读取 JSON(用于 hooks)
const input = await readStdinJson({ timeoutMs: 5000 });

// 输出到 stderr(对用户可见)
log('Debug message');

// 输出到 stdout(返回给 Claude)
output({ result: 'data' });
```

### 系统命令

```javascript
const { commandExists, runCommand, isGitRepo, getGitModifiedFiles } = require('./utils');

// 检查命令是否存在
if (commandExists('git')) { /* ... */ }

// 运行命令(仅限白名单命令)
const result = runCommand('git rev-parse --show-toplevel');
if (result.success) {
  console.log(result.output);
}

// 检查是否是 git 仓库
if (isGitRepo()) { /* ... */ }

// 获取修改的文件
const files = getGitModifiedFiles(['\\.js$', '\\.md$']);
```

---

## package-manager.js

**路径**: `scripts/lib/package-manager.js`

包管理器检测和选择工具。

### 支持的包管理器

- npm
- pnpm
- yarn
- bun

### 检测优先级

1. 环境变量 `CLAUDE_PACKAGE_MANAGER`
2. 项目配置 `.claude/package-manager.json`
3. `package.json` 的 `packageManager` 字段
4. lock 文件检测
5. 全局用户偏好 `~/.claude/package-manager.json`
6. 默认值 npm

### 核心 API

```javascript
const {
  getPackageManager,           // 获取当前项目的包管理器
  setPreferredPackageManager,  // 设置全局偏好
  setProjectPackageManager,    // 设置项目偏好
  getAvailablePackageManagers, // 获取系统上可用的包管理器
  detectFromLockFile,          // 从 lock 文件检测
  detectFromPackageJson,       // 从 package.json 检测
  getRunCommand,               // 获取运行脚本的命令
  getExecCommand,              // 获取执行二进制文件的命令
  getSelectionPrompt,          // 获取交互式选择提示
  getCommandPattern            // 生成命令匹配正则
} = require('./package-manager');

// 获取当前包管理器
const { name, config, source } = getPackageManager();
// name: 'pnpm', 'npm', 'yarn', 或 'bun'
// config: { installCmd, runCmd, execCmd, testCmd, ... }
// source: 'environment', 'project-config', 'lock-file', ...

// 设置全局偏好
setPreferredPackageManager('pnpm');

// 设置项目偏好
setProjectPackageManager('yarn', '/path/to/project');

// 获取运行命令
const testCmd = getRunCommand('test'); // 'pnpm test' 或 'npm test' 等
const buildCmd = getRunCommand('build');

// 获取执行命令
const eslintCmd = getExecCommand('eslint', '--fix .');
```

### 安全检查

`getRunCommand` 和 `getExecCommand` 会验证输入:
- 脚本名只允许字母、数字、dash、underscore、dot、slash、@
- 参数只允许字母、数字、空白、dash、dots、slashes、equals、colons、commas、quotes、@

---

## session-manager.js

**路径**: `scripts/lib/session-manager.js`

会话管理库,提供会话的 CRUD 操作。

### 会话存储

- 新格式: `~/.claude/session-data/YYYY-MM-DD-[session-id]-session.tmp`
- 旧格式兼容: `~/.claude/sessions/YYYY-MM-DD-session.tmp`

### 核心 API

```javascript
const {
  parseSessionFilename,      // 解析会话文件名
  getSessionPath,            // 获取会话文件完整路径
  getSessionContent,         // 读取会话内容
  parseSessionMetadata,      // 解析会话元数据
  getSessionStats,           // 获取会话统计
  getSessionTitle,           // 获取会话标题
  getSessionSize,            // 获取会话大小(人类可读格式)
  getAllSessions,            // 获取所有会话(支持分页和筛选)
  getSessionById,            // 按 ID 获取单个会话
  writeSessionContent,       // 写入会话内容
  appendSessionContent,      // 追加会话内容
  deleteSession,             // 删除会话
  sessionExists              // 检查会话是否存在
} = require('./session-manager');

// 获取所有会话
const { sessions, total, offset, limit, hasMore } = getAllSessions({
  limit: 50,
  offset: 0,
  date: '2026-05-26',
  search: 'abc123'
});

// 获取单个会话
const session = getSessionById('abc123', true);
// session: { filename, shortId, date, sessionPath, content, metadata, stats, ... }

// 解析会话元数据
const metadata = parseSessionMetadata(content);
// metadata: { title, date, started, lastUpdated, project, branch, worktree, completed, inProgress, notes, context }

// 获取会话统计
const stats = getSessionStats(sessionPathOrContent);
// stats: { totalItems, completedItems, inProgressItems, lineCount, hasNotes, hasContext }

// 写入会话
writeSessionContent(sessionPath, '# New Session\n\nContent...');

// 追加内容
appendSessionContent(sessionPath, '\n\nMore content...');

// 删除会话
deleteSession(sessionPath);
```

### 会话元数据格式

会话内容中的元数据:

```markdown
# Session Title

**Date:** 2026-05-26
**Started:** 14:30
**Last Updated:** 15:45
**Project:** my-project
**Branch:** main
**Worktree:** frontend

### Completed
- [x] Task 1
- [x] Task 2

### In Progress
- [ ] Task 3

### Notes for Next Session
Remember to check the config file.

### Context to Load
```
previous-context-content
```
```

---

## install-executor.js

**路径**: `scripts/lib/install-executor.js`

ECC 安装执行器,负责实际执行安装操作。

### 主要功能

- 创建安装计划
- 执行文件复制操作
- 处理安装状态记录

### 核心 API

```javascript
const {
  applyInstallPlan,       // 应用安装计划
  listAvailableLanguages  // 列出可用语言
} = require('./install-executor');

// 列出可用语言
const languages = listAvailableLanguages();
// ['typescript', 'javascript', 'python', ...]
```

---

## install-lifecycle.js

**路径**: `scripts/lib/install-lifecycle.js`

安装生命周期管理,处理安装的各个阶段。

---

## install-manifests.js

**路径**: `scripts/lib/install-manifests.js`

安装清单定义,包括:
- 支持的安装目标
- 可用语言列表
- 支持的本地化列表

---

## install-state.js

**路径**: `scripts/lib/install-state.js`

安装状态管理,记录哪些文件被安装到哪里。

---

## project-detect.js

**路径**: `scripts/lib/project-detect.js`

项目类型检测,检测当前目录的项目类型。

---

## observer-sessions.js

**路径**: `scripts/lib/observer-sessions.js`

会话观察器,用于监控会话活动。

---

## shell-substitution.js

**路径**: `scripts/lib/shell-substitution.js`

Shell 变量替换工具,处理环境变量和 shell 表达式。

---

## tmux-worktree-orchestrator.js

**路径**: `scripts/lib/tmux-worktree-orchestrator.js`

Tmux + Git Worktree 编排器,管理复杂的开发环境。

---

## mcp-config.js

**路径**: `scripts/lib/mcp-config.js`

MCP (Model Context Protocol) 配置管理。

---

## 其他工具

| 文件 | 说明 |
|------|------|
| `agent-compress.js` | Agent 上下文压缩 |
| `cost-estimate.js` | 成本估算 |
| `github-discussions.js` | GitHub 讨论集成 |
| `hook-flags.js` | Hook 标志管理 |
| `inspection.js` | 检查工具 |
| `orchestration-session.js` | 编排会话管理 |
| `resolve-ecc-root.js` | ECC 根目录解析 |
| `resolve-formatter.js` | 格式化器解析 |
| `session-aliases.js` | 会话别名管理 |
| `session-bridge.js` | 会话桥接 |
| `shell-split.js` | Shell 命令分割 |
| `state-store/` | 状态存储子模块 |
| `install/` | 安装相关子模块 |
| `skill-evolution/` | 技能演进子模块 |
| `skill-improvement/` | 技能改进子模块 |
