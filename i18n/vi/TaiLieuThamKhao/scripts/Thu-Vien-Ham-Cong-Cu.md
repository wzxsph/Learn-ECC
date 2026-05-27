# Thư viện hàm công cụ

Tài liệu này giới thiệu các thư viện hàm công cụ cốt lõi trong thư mục `scripts/lib/`.

---

## utils.js

**Đường dẫn**: `scripts/lib/utils.js`

Thư viện hàm công cụ cross-platform, hoạt động trên Windows, macOS và Linux.

### Phát hiện platform

```javascript
const { isWindows, isMacOS, isLinux } = require('./utils');

// isWindows: true trên Windows
// isMacOS: true trên macOS
// isLinux: true trên Linux
```

### Thao tác thư mục

```javascript
const {
  getHomeDir,      // Lấy thư mục home người dùng
  getClaudeDir,    // Lấy thư mục ~/.claude
  getSessionsDir,  // Lấy thư mục ~/.claude/session-data
  getTempDir,      // Lấy thư mục tạm
  ensureDir,       // Đảm bảo thư mục tồn tại, tạo nếu không
  findFiles        // Tìm file theo pattern
} = require('./utils');

// Tạo thư mục nếu không tồn tại
ensureDir('/path/to/dir');

// Tìm file
const files = findFiles('/path', '*.js', { recursive: true });
```

### Ngày giờ

```javascript
const {
  getDateString,      // Trả về định dạng YYYY-MM-DD
  getTimeString,      // Trả về định dạng HH:MM
  getDateTimeString   // Trả về định dạng YYYY-MM-DD HH:MM:SS
} = require('./utils');
```

### Phiên và dự án

```javascript
const {
  sanitizeSessionId,    // Làm sạch session ID, loại bỏ ký tự không hợp lệ
  getSessionIdShort,    // Lấy short session ID
  getGitRepoName,       // Lấy tên git repo
  getProjectName        // Lấy tên dự án
} = require('./utils');

// Làm sạch session ID
const cleanId = sanitizeSessionId('my-session@123'); // 'my-session-123'

// Lấy tên dự án
const project = getProjectName();
```

### Thao tác file

```javascript
const {
  readFile,        // Đọc file an toàn, trả về null nếu fail
  writeFile,       // Ghi file
  appendFile,      // Nối vào file
  replaceInFile,   // Thay thế nội dung file
  countInFile,     // Đếm số lần pattern xuất hiện
  grepFile         // Tìm kiếm pattern trong file
} = require('./utils');

// Đọc file
const content = readFile('/path/to/file.txt');

// Thay thế nội dung (thay thế lần xuất hiện đầu tiên)
replaceInFile('/path', 'old', 'new');

// Thay thế tất cả lần xuất hiện
replaceInFile('/path', 'old', 'new', { all: true });

// Đếm số lần xuất hiện
const count = countInFile('/path', /pattern/g);

// Tìm và trả về các dòng khớp
const matches = grepFile('/path', /regex/);
// Trả về: [{ lineNumber: 1, content: '...' }]
```

### Xử lý chuỗi

```javascript
const { stripAnsi } = require('./utils');

// Loại bỏ escape sequence ANSI (màu sắc, etc.)
const clean = stripAnsi('\x1b[32mHello\x1b[0m'); // 'Hello'
```

### Hook I/O

```javascript
const { readStdinJson, log, output } = require('./utils');

// Đọc JSON từ stdin (cho hooks)
const input = await readStdinJson({ timeoutMs: 5000 });

// Output ra stderr (hiển thị cho người dùng)
log('Debug message');

// Output ra stdout (trả về cho Claude)
output({ result: 'data' });
```

### Lệnh hệ thống

```javascript
const { commandExists, runCommand, isGitRepo, getGitModifiedFiles } = require('./utils');

// Kiểm tra lệnh có tồn tại không
if (commandExists('git')) { /* ... */ }

// Chạy lệnh (chỉ lệnh whitelist)
const result = runCommand('git rev-parse --show-toplevel');
if (result.success) {
  console.log(result.output);
}

// Kiểm tra có phải git repo không
if (isGitRepo()) { /* ... */ }

// Lấy các file đã sửa
const files = getGitModifiedFiles(['\\.js$', '\\.md$']);
```

---

## package-manager.js

**Đường dẫn**: `scripts/lib/package-manager.js`

Công cụ phát hiện và lựa chọn package manager.

### Các package manager được hỗ trợ

- npm
- pnpm
- yarn
- bun

### Ưu tiên phát hiện

1. Biến môi trường `CLAUDE_PACKAGE_MANAGER`
2. File cấu hình dự án `.claude/package-manager.json`
3. Trường `packageManager` trong `package.json`
4. Phát hiện từ lock file
5. preference toàn cục `~/.claude/package-manager.json`
6. Mặc định npm

### API cốt lõi

```javascript
const {
  getPackageManager,           // Lấy package manager của dự án hiện tại
  setPreferredPackageManager,  // Đặt preference toàn cục
  setProjectPackageManager,    // Đặt preference dự án
  getAvailablePackageManagers, // Lấy các package manager khả dụng trên hệ thống
  detectFromLockFile,          // Phát hiện từ lock file
  detectFromPackageJson,       // Phát hiện từ package.json
  getRunCommand,               // Lấy lệnh chạy script
  getExecCommand,              // Lấy lệnh thực thi binary
  getSelectionPrompt,          // Lấy prompt chọn tương tác
  getCommandPattern            // Tạo regex cho pattern lệnh
} = require('./package-manager');

// Lấy package manager hiện tại
const { name, config, source } = getPackageManager();
// name: 'pnpm', 'npm', 'yarn', hoặc 'bun'
// config: { installCmd, runCmd, execCmd, testCmd, ... }
// source: 'environment', 'project-config', 'lock-file', ...

// Đặt preference toàn cục
setPreferredPackageManager('pnpm');

// Đặt preference dự án
setProjectPackageManager('yarn', '/path/to/project');

// Lấy lệnh chạy
const testCmd = getRunCommand('test'); // 'pnpm test' hoặc 'npm test' etc.
const buildCmd = getRunCommand('build');

// Lấy lệnh thực thi
const eslintCmd = getExecCommand('eslint', '--fix .');
```

### Kiểm tra an toàn

`getRunCommand` và `getExecCommand` xác minh input:
- Script name chỉ cho phép chữ cái, số, dash, underscore, dot, slash, @
- Arguments chỉ cho phép chữ cái, số, khoảng trắng, dash, dots, slashes, equals, colons, commas, quotes, @

---

## session-manager.js

**Đường dẫn**: `scripts/lib/session-manager.js`

Thư viện quản lý phiên, cung cấp CRUD operations cho phiên.

### Lưu trữ phiên

- Format mới: `~/.claude/session-data/YYYY-MM-DD-[session-id]-session.tmp`
- Tương thích format cũ: `~/.claude/sessions/YYYY-MM-DD-session.tmp`

### API cốt lõi

```javascript
const {
  parseSessionFilename,      // Parse tên file phiên
  getSessionPath,            // Lấy đường dẫn đầy đủ của phiên
  getSessionContent,         // Đọc nội dung phiên
  parseSessionMetadata,      // Parse metadata phiên
  getSessionStats,           // Lấy thống kê phiên
  getSessionTitle,           // Lấy tiêu đề phiên
  getSessionSize,            // Lấy kích thước phiên (format human readable)
  getAllSessions,            // Lấy tất cả phiên (hỗ trợ phân trang và lọc)
  getSessionById,            // Lấy một phiên theo ID
  writeSessionContent,       // Ghi nội dung phiên
  appendSessionContent,      // Nối nội dung phiên
  deleteSession,             // Xóa phiên
  sessionExists              // Kiểm tra phiên tồn tại
} = require('./session-manager');

// Lấy tất cả phiên
const { sessions, total, offset, limit, hasMore } = getAllSessions({
  limit: 50,
  offset: 0,
  date: '2026-05-26',
  search: 'abc123'
});

// Lấy một phiên
const session = getSessionById('abc123', true);
// session: { filename, shortId, date, sessionPath, content, metadata, stats, ... }

// Parse metadata phiên
const metadata = parseSessionMetadata(content);
// metadata: { title, date, started, lastUpdated, project, branch, worktree, completed, inProgress, notes, context }

// Lấy thống kê phiên
const stats = getSessionStats(sessionPathOrContent);
// stats: { totalItems, completedItems, inProgressItems, lineCount, hasNotes, hasContext }

// Ghi phiên
writeSessionContent(sessionPath, '# New Session\n\nContent...');

// Nối nội dung
appendSessionContent(sessionPath, '\n\nMore content...');

// Xóa phiên
deleteSession(sessionPath);
```

### Định dạng metadata phiên

Metadata trong nội dung phiên:

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

**Đường dẫn**: `scripts/lib/install-executor.js`

Trình thực thi cài đặt ECC, chịu trách nhiệm thực hiện các thao tác cài đặt thực tế.

### Chức năng chính

- Tạo kế hoạch cài đặt
- Thực hiện thao tác sao chép file
- Xử lý ghi trạng thái cài đặt

### API cốt lõi

```javascript
const {
  applyInstallPlan,       // Áp dụng kế hoạch cài đặt
  listAvailableLanguages  // Liệt kê các ngôn ngữ khả dụng
} = require('./install-executor');

// Liệt kê các ngôn ngữ khả dụng
const languages = listAvailableLanguages();
// ['typescript', 'javascript', 'python', ...]
```

---

## install-lifecycle.js

**Đường dẫn**: `scripts/lib/install-lifecycle.js`

Quản lý vòng đời cài đặt, xử lý các giai đoạn cài đặt.

---

## install-manifests.js

**Đường dẫn**: `scripts/lib/install-manifests.js`

Định nghĩa manifest cài đặt, bao gồm:
- Các target cài đặt được hỗ trợ
- Danh sách ngôn ngữ khả dụng
- Các localization được hỗ trợ

---

## install-state.js

**Đường dẫn**: `scripts/lib/install-state.js`

Quản lý trạng thái cài đặt, ghi lại các file được cài đặt ở đâu.

---

## project-detect.js

**Đường dẫn**: `scripts/lib/project-detect.js`

Phát hiện loại dự án trong thư mục hiện tại.

---

## observer-sessions.js

**Đường dẫn**: `scripts/lib/observer-sessions.js`

Session observer, giám sát hoạt động phiên.

---

## shell-substitution.js

**Đường dẫn**: `scripts/lib/shell-substitution.js`

Công cụ thay thế biến shell, xử lý biến môi trường và biểu thức shell.

---

## tmux-worktree-orchestrator.js

**Đường dẫn**: `scripts/lib/tmux-worktree-orchestrator.js`

Tmux + Git Worktree orchestrator, quản lý môi trường phát triển phức tạp.

---

## mcp-config.js

**Đường dẫn**: `scripts/lib/mcp-config.js`

Quản lý cấu hình MCP (Model Context Protocol).

---

## Các công cụ khác

| File | Mô tả |
|------|----------|
| `agent-compress.js` | Nén context agent |
| `cost-estimate.js` | Ước tính chi phí |
| `github-discussions.js` | Tích hợp GitHub discussions |
| `hook-flags.js` | Quản lý hook flags |
| `inspection.js` | Công cụ kiểm tra |
| `orchestration-session.js` | Quản lý orchestration session |
| `resolve-ecc-root.js` | Resolve ECC root directory |
| `resolve-formatter.js` | Resolve formatter |
| `session-aliases.js` | Quản lý session aliases |
| `session-bridge.js` | Session bridge |
| `shell-split.js` | Shell command split |
| `state-store/` | Submodule lưu trữ trạng thái |
| `install/` | Submodule liên quan đến cài đặt |
| `skill-evolution/` | Submodule tiến hóa skill |
| `skill-improvement/` | Submodule cải thiện skill |