# Định dạng Cấu hình

## Tổng quan cấu trúc hooks.json

File cấu hình hệ thống ECC Hooks sử dụng định dạng JSON, với cấu trúc chính như sau:

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

## Cấu trúc cấp cao nhất

| Trường | Loại | Mô tả |
|------|------|------|
| `$schema` | string | JSON Schema URL, dùng để xác minh cấu hình |
| `hooks` | object | Object chứa tất cả các loại hook |

---

## Cấu trúc mảng hook

Mỗi loại sự kiện (PreToolUse, PostToolUse, v.v.) chứa một mảng hooks, mỗi object hook có cấu trúc sau:

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
  "description": "Mô tả hook",
  "id": "unique-hook-id"
}
```

### Trường matcher

#### Mô tả loại
Trường matcher dùng để chỉ định loại tool kích hoạt hook, hỗ trợ một tool hoặc nhiều tool (phân cách bằng `|`).

#### Giá trị khả dụng
| Giá trị matcher | Tool được match |
|-----------|-----------------|
| `*` | Tất cả tools |
| `Bash` | Tool Bash |
| `Edit` | Tool Edit |
| `Write` | Tool Write |
| `Read` | Tool Read |
| `MultiEdit` | Tool MultiEdit |
| `Bash\|Edit\|Write` | Bash, Edit hoặc Write |
| `Edit\|Write\|MultiEdit` | Edit, Write hoặc MultiEdit |

#### Ví dụ sử dụng
```json
// Match tất cả tools
"matcher": "*"

// Chỉ match tool Bash
"matcher": "Bash"

// Match nhiều tools
"matcher": "Edit|Write|MultiEdit"
```

---

## Cấu trúc mảng hooks

Mảng hooks chứa một hoặc nhiều object lệnh hook:

### Trường type

| Giá trị type | Mô tả |
|--------|------|
| `command` | Thực thi lệnh Node.js |

### Trường command

Đường dẫn script Node.js cần thực thi.

### Trường async (tùy chọn)

```json
"async": true
```

- `true`: Thực thi không đồng bộ, không blocking tool execution
- `false` hoặc không có: Thực thi đồng bộ, blocking tool execution

### Trường timeout (tùy chọn)

```json
"timeout": 30
```

Thời gian thực thi tối đa (giây). Khuyến nghị:
- Hooks đồng bộ: <200ms
- Hooks không đồng bộ: ≤30 giây

---

## Ví dụ cấu hình đầy đủ

### Ví dụ PreToolUse Hook

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/pre-bash-dispatcher.js"
    }
  ],
  "description": "Bash pre-check dispatcher cho quality, tmux, push và GateGuard checks",
  "id": "pre:bash:dispatcher"
}
```

### Ví dụ PostToolUse Hook

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
  "description": "Chạy quality gate checks sau khi chỉnh sửa file",
  "id": "post:quality-gate"
}
```

### Ví dụ Stop Hook

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
  "description": "Lưu session state sau mỗi response",
  "id": "stop:session-end"
}
```

### Ví dụ SessionStart Hook

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/session-start-bootstrap.js"
    }
  ],
  "description": "Tải previous context có giới hạn và phát hiện package manager khi session mới",
  "id": "session:start"
}
```

---

## Định dạng cấu hình lifecycle events

Đối với các lifecycle events như SessionStart, SessionEnd, PreCompact, sử dụng cấu trúc cấu hình khác:

```json
{
  "description": "Định nghĩa lifecycle hooks cho memory persistence",
  "events": [
    {
      "event": "SessionStart",
      "id": "session:start",
      "script": "scripts/hooks/session-start-bootstrap.js",
      "purpose": "Tải previous context có giới hạn và phát hiện trạng thái project khi session bắt đầu",
      "blocking": false
    },
    {
      "event": "PreCompact",
      "id": "pre:compact",
      "script": "scripts/hooks/pre-compact.js",
      "purpose": "Persist session state trước khi context compact",
      "blocking": false
    }
  ]
}
```

### Trường lifecycle event

| Trường | Mô tả |
|------|------|
| `event` | Loại sự kiện (SessionStart, PreCompact, SessionEnd, v.v.) |
| `id` | Identifier duy nhất |
| `script` | Đường dẫn script |
| `purpose` | Mô tả mục đích |
| `blocking` | Có blocking không (thường là false) |

---

## Vô hiệu hóa Hooks

### Vô hiệu hóa qua chỉnh sửa cấu hình

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Override: Cho phép tạo tất cả file .md"
      }
    ]
  }
}
```

### Vô hiệu hóa qua biến môi trường

```bash
# Vô hiệu hóa hooks cụ thể (phân cách bằng dấu phẩy)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Vô hiệu hóa GateGuard
export ECC_GATEGUARD=off
```

---

## Quy ước đặt tên Hook ID

ECC sử dụng quy ước đặt tên phân cách bằng dấu hai chấm:

| Prefix | Mục đích | Ví dụ |
|------|------|------|
| `pre:` | PreToolUse hooks | `pre:bash:dispatcher` |
| `post:` | PostToolUse hooks | `post:quality-gate` |
| `stop:` | Stop hooks | `stop:session-end` |
| `session:` | Session lifecycle hooks | `session:start` |

---

## Wrapper run-with-flags.js

ECC sử dụng wrapper `run-with-flags.js` để chạy hooks, hỗ trợ runtime gating từ cấu hình:

```
node scripts/hooks/run-with-flags.js <hook-id> <script-path> <profiles>
```

### Mô tả tham số
| Tham số | Mô tả |
|------|------|
| `hook-id` | Identifier duy nhất của hook |
| `script-path` | Đường dẫn script thực tế cần chạy |
| `profiles` | Danh sách cấu hình phân cách bằng dấu phẩy (minimal, standard, strict) |

### Cách hoạt động
1. Đọc biến môi trường `ECC_HOOK_PROFILE` (mặc định: standard)
2. Kiểm tra hook ID có trong `ECC_DISABLED_HOOKS` không
3. Nếu được phép, chạy script

---

## Xử lý đường dẫn đa nền tảng

Lệnh hook ECC sử dụng script Node.js để implement hành vi đa nền tảng:

```javascript
const path = require('path');
const homedir = require('os').homedir();

// Cross-platform path resolution
const configDir = path.join(homedir, '.claude');
const hookScript = path.join(configDir, 'scripts', 'hooks', 'my-hook.js');
```

---

## Xác minh cấu hình

Khuyến nghị sử dụng JSON Schema để xác minh cấu hình:
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json"
}
```