# Các Loại Hook

## Tổng quan

Hệ thống ECC Hooks là cơ chế tự động event-driven, trigger trước và sau khi Claude Code thực thi tool, dùng để enforce code quality, phát hiện lỗi sớm và automate repeated check。

```
User request → Claude chọn tool → PreToolUse hook chạy → Tool thực thi → PostToolUse hook chạy
```

## Chi tiết các loại Hook

### 1. PreToolUse Hook

#### Loại mô tả
PreToolUse hook chạy **trước khi** tool thực thi, có thể block (exit code 2) hoặc warn (stderr output nhưng không block).

#### Thời điểm trigger
- Trước khi bất kỳ tool nào (Edit, Write, Bash, Read, etc.) thực thi
- Phù hợp cho pre-check của tất cả tool operation

#### Tham số mô tả
```typescript
interface HookInput {
  tool_name: string;          // Tool name: "Bash", "Edit", "Write", "Read", etc.
  tool_input: {
    command?: string;         // Bash: command cần thực thi
    file_path?: string;        // Edit/Write/Read: target file path
    old_string?: string;       // Edit: text bị replace
    new_string?: string;       // Edit: replacement text
    content?: string;          // Write: file content
  };
}
```

#### Exit code
| Exit code | Ý nghĩa | Hành động |
|--------|------|------|
| 0 | Thành công | Tiếp tục thực thi, không output warning |
| 2 | Block | Block tool thực thi |
| Khác không | Error | Ghi log nhưng không block |

#### Ví dụ sử dụng

**Dev server blocker (block chạy npm run dev bên ngoài tmux):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "Block chạy dev server bên ngoài tmux"
}
```

**Doc file warning (block tạo non-standard .md file):**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Warning khi tạo non-standard doc file"
}
```

#### Lưu ý
- PreToolUse là hook **blocking**, phải thực thi nhanh (<200ms)
- Không nên làm network call trong PreToolUse hook
- Block operation (exit 2) nên dùng cẩn thận, chỉ cho critical security check

---

### 2. PostToolUse Hook

#### Loại mô tả
PostToolUse hook chạy **sau khi** tool thực thi, có thể phân tích output nhưng không thể block tool thực thi.

#### Thời điểm trigger
- Sau khi bất kỳ tool nào hoàn thành
- Có thể truy cập tool output result

#### Tham số mô tả
```typescript
interface HookInput {
  tool_name: string;          // Tool name
  tool_input: {               // Giống PreToolUse
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // Chỉ có trong PostToolUse
    output?: string;          // Command/tool output
  };
}
```

#### Exit code
| Exit code | Ý nghĩa |
|--------|------|
| 0 | Thành công, tiếp tục thực thi |
| Khác 0 | Error, ghi log nhưng không block tool thực thi |

#### Ví dụ sử dụng

**PR logging (gh pr create sau đó log PR URL):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Log URL và review command sau khi tạo PR"
}
```

**Quality gate check (chạy quality check sau khi edit):**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Chạy quality gate check sau khi edit file"
}
```

#### Lưu ý
- PostToolUse hook **không thể block** tool thực thi
- Có thể set async (async: true) để tránh blocking
- Async hook nên hoàn thành trong 30 giây

---

### 3. Stop Hook

#### Loại mô tả
Stop hook chạy sau mỗi Claude response, dùng cho batch processing, final check và session state persistence.

#### Thời điểm trigger
- Sau khi mỗi user request được xử lý, Claude generate response
- Xảy ra sau response cuối cùng của session

#### Tham số mô tả
Stop hook nhận cùng input như PostToolUse, chứa full tool call history.

#### Ví dụ sử dụng

**Console log check (check console.log trong modified file sau response):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "Check xem có console.log trong modified file không"
}
```

**Batch format và type check:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "Batch format tất cả JS/TS file đã edit và type check"
}
```

**Session summary persistence:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "Lưu session state"
}
```

#### Lưu ý
- Stop hook chạy sau mỗi response, phù hợp cho batch operation
- Có thể dùng async mode cho non-blocking operation
- Format/type check phù hợp để batch execute lúc Stop

---

### 4. SessionStart Hook

#### Loại mô tả
SessionStart hook trigger khi bắt đầu session lifecycle, dùng để load previous context và detect project state.

#### Thời điểm trigger
- Khi session mới bắt đầu
- Khi Claude Code start hoặc tạo session mới

#### Ví dụ sử dụng

**Load previous context và detect package manager:**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "Load bounded previous context và detect project state khi session bắt đầu",
  "blocking": false
}
```

#### Lưu ý
- Lifecycle hook, phải hoàn thành nhanh (<30 giây)
- Có thể control extra context size qua `ECC_SESSION_START_MAX_CHARS` environment variable
- Có thể disable hoàn toàn bằng `ECC_SESSION_START_CONTEXT=off`

---

### 5. SessionEnd Hook

#### Loại mô tả
SessionEnd hook trigger khi session kết thúc, dùng để persist session end summary và cleanup.

#### Thời điểm trigger
- Khi session kết thúc
- Khi session transcript metadata available

#### Ví dụ sử dụng

**Session end marker:**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "Persist session end summary khi transcript metadata available",
  "blocking": false
}
```

#### Lưu ý
- Chủ yếu async execution, không nên block session end
- Lifecycle marker dùng cho metrics collection và cleanup

---

### 6. PreCompact Hook

#### Loại mô tả
PreCompact hook trigger trước khi context compact, dùng để save state.

#### Thời điểm trigger
- Trước khi context compact/prune
- Khi context window gần đầy

#### Ví dụ sử dụng

**Save state before context compact:**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "Persist session state trước khi context compact",
  "blocking": false
}
```

#### Lưu ý
- Phù hợp để save state của long task
- Non-blocking, không ảnh hưởng compact process

---

### 7. PostToolUseFailure Hook

#### Loại mô tả
PostToolUseFailure hook trigger sau khi tool execution fail.

#### Thời điểm trigger
- Sau bất kỳ tool call fail nào

#### Ví dụ sử dụng

**MCP health check (track failed MCP call):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "Track failed MCP tool call, mark unhealthy server và attempt reconnect"
}
```

#### Lưu ý
- Có thể dùng cho retry logic hoặc health state tracking
- Không thay đổi trạng thái tool failure

---

## Bảng so sánh các loại Hook

| Loại | Thời điểm trigger | Có thể block | Có thể blocking | Use case điển hình |
|------|----------|----------|----------|----------|
| PreToolUse | Trước khi tool thực thi | Có (exit 2) | Có (sync) | Quality check, Security validation |
| PostToolUse | Sau khi tool thực thi | Không | Optional (async) | Logging, Analysis |
| Stop | Sau mỗi response | Không | Optional (async) | Batch format, Session summary |
| SessionStart | Khi session bắt đầu | Không | Không | Load context, Detect project |
| SessionEnd | Khi session kết thúc | Không | Không | Persist summary, Cleanup |
| PreCompact | Trước khi compact | Không | Không | Save state |
| PostToolUseFailure | Sau tool fail | Không | Không | Health check, Retry |

---

## Environment Variable Control

Có thể control hook behavior qua environment variable:

```bash
# minimal | standard | strict (default: standard)
export ECC_HOOK_PROFILE=standard

# Disable specific hook ID (comma separated)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Disable GateGuard
export ECC_GATEGUARD=off

# Control SessionStart extra context size (default: 8000 chars)
export ECC_SESSION_START_MAX_CHARS=4000

# Completely disable SessionStart extra context
export ECC_SESSION_START_CONTEXT=off
```