# 03 - Tùy chỉnh Hooks

Học hệ thống [Hook](../../TaiLieuThamKhao/hooks/Loai-Hook.md) của ECC, thực hiện tự động hóa workflow.

## Tổng quan

Hệ thống Hooks của ECC là cơ chế tự động hóa dựa trên sự kiện, trigger trước và sau khi Claude Code thực thi công cụ, dùng để enforce chất lượng code, phát hiện lỗi sớm và tự động hóa kiểm tra lặp đi lặp lại.

```
Yêu cầu người dùng → Claude chọn công cụ → PreToolUse hook chạy → Công cụ thực thi → PostToolUse hook chạy
```

---

## 1. Chi tiết loại Hook

### 1.1 Hook PreToolUse

Chạy **trước** khi công cụ thực thi, có thể chặn (exit code 2) hoặc cảnh báo (stderr output nhưng không chặn).

#### Thời điểm trigger
- Trước khi bất kỳ công cụ nào (Edit, Write, Bash, Read, v.v.) thực thi
- Phù hợp với kiểm tra pre-check cho tất cả thao tác công cụ

#### Mã thoát
| Mã thoát | Ý nghĩa | Hành vi |
|--------|------|------|
| 0 | Thành công | Tiếp tục thực thi, không output cảnh báo |
| 2 | Chặn | Chặn công cụ thực thi |
| Khác 0 | Lỗi | Ghi log nhưng không chặn |

#### Ví dụ sử dụng

**Chặn server phát triển:**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "Chặn chạy development server bên ngoài tmux"
}
```

**Cảnh báo file tài liệu:**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Cảnh báo tạo file tài liệu không chuẩn"
}
```

### 1.2 Hook PostToolUse

Chạy **sau** khi công cụ thực thi, có thể phân tích output nhưng không thể chặn công cụ thực thi.

#### Thời điểm trigger
- Sau khi bất kỳ công cụ nào hoàn thành
- Có thể truy cập kết quả output của công cụ

#### Ví dụ sử dụng

**Ghi log PR:**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Sau khi tạo PR, ghi URL và lệnh review"
}
```

**Quality gate:**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Sau khi edit file, chạy quality gate check"
}
```

### 1.3 Hook Stop

Chạy sau mỗi lần Claude phản hồi, dùng cho batch processing, check cuối cùng và persistence trạng thái session.

#### Ví dụ sử dụng

**Kiểm tra console log:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "Kiểm tra các file đã sửa có console.log không"
}
```

**Persistence tóm tắt session:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "Lưu trạng thái session"
}
```

### 1.4 Hook SessionStart / SessionEnd

Lifecycle hook, dùng cho việc load context khi bắt đầu session và persistence trạng thái khi kết thúc session.

---

## 2. Bảng so sánh các loại Hook

| Loại | Thời điểm trigger | Có thể chặn | Có thể blocking | Mục đích điển hình |
|------|----------|----------|----------|----------|
| PreToolUse | Trước khi công cụ thực thi | Có (exit 2) | Có (đồng bộ) | Kiểm tra chất lượng, xác thực bảo mật |
| PostToolUse | Sau khi công cụ thực thi | Không | Tùy chọn (async) | Ghi log, phân tích |
| Stop | Sau mỗi phản hồi | Không | Tùy chọn (async) | Batch formatting, tóm tắt session |
| SessionStart | Bắt đầu session | Không | Không | Load context, phát hiện dự án |
| SessionEnd | Kết thúc session | Không | Không | Persistence tóm tắt, cleanup |

---

## 3. Phát triển Hook tùy chỉnh

### 3.1 Cấu trúc cơ bản

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Truy cập thông tin công cụ
  const toolName = input.tool_name;        // "Edit", "Bash", "Write", v.v.
  const toolInput = input.tool_input;      // Tham số đặc biệt theo công cụ
  const toolOutput = input.tool_output;    // Chỉ khả dụng ở PostToolUse

  // Cảnh báo (không blocking): Ghi vào stderr
  console.error('[Hook] Thông tin cảnh báo');

  // Chặn (chỉ PreToolUse): exit code 2
  // process.exit(2);

  // Luôn output dữ liệu gốc ra stdout
  console.log(data);
});
```

### 3.2 Interface HookInput

```typescript
interface HookInput {
  tool_name: string;          // Tên công cụ
  tool_input: {               // Tham số đầu vào công cụ
    command?: string;         // Bash: Lệnh
    file_path?: string;       // Edit/Write/Read: Đường dẫn file
    old_string?: string;      // Edit: Văn bản bị thay thế
    new_string?: string;      // Edit: Văn bản thay thế
    content?: string;         // Write: Nội dung file
  };
  tool_output?: {             // Chỉ PostToolUse
    output?: string;          // Output lệnh/công cụ
  };
}
```

### 3.3 Quy tắc quan trọng: exit 0 khi lỗi không nghiêm trọng

**Quan trọng**: Tất cả các hook phải exit 0 khi có lỗi không nghiêm trọng, tránh blocking công cụ thực thi không mong muốn.

| Mã thoát | Ý nghĩa | Kịch bản sử dụng |
|--------|------|----------|
| 0 | Thành công | Tiếp tục thực thi, hoặc cảnh báo không nghiêm trọng |
| 2 | Chặn | Chỉ dùng cho việc chặn quan trọng ở PreToolUse |
| Khác 0 | Lỗi | Chỉ ghi log, **không sử dụng** |

### 3.4 Xử lý lỗi đúng cách

```javascript
// Ví dụ sai - Đừng làm thế này
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Xử lý logic
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // Sai: Sẽ block công cụ
  }
  console.log(data);
});

// Ví dụ đúng - Làm thế này
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Xử lý logic
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // Lỗi không nghiêm trọng, exit 0 tiếp tục thực thi
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## 4. Công thức hook phổ biến

### 4.1 Cảnh báo comment TODO/FIXME

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "Cảnh báo thêm comment TODO/FIXME"
}
```

### 4.2 Chặn tạo file quá lớn

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');process.exit(2)}console.log(d)})\""
  }],
  "description": "Chặn tạo file vượt quá 800 dòng"
}
```

### 4.3 Tự động format file Python bằng ruff

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "Sau khi edit, tự động format file Python bằng ruff"
}
```

### 4.4 Yêu cầu file nguồn mới phải kèm test

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p)}}console.log(d)})\""
  }],
  "description": "Nhắc nhở tạo test khi thêm file nguồn mới"
}
```

---

## 5. Quản lý cấu hình

### 5.1 Sử dụng run-with-flags.js

```bash
# Sử dụng wrapper run-with-flags.js
node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict
```

### 5.2 Kiểm soát biến môi trường

```bash
# minimal | standard | strict (mặc định: standard)
export ECC_HOOK_PROFILE=standard

# Vô hiệu hóa hook ID cụ thể (phân cách bằng dấu phẩy)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Kiểm soát kích thước context SessionStart bổ sung (mặc định: 8000 ký tự)
export ECC_SESSION_START_MAX_CHARS=4000

# Vô hiệu hóa hoàn toàn context bổ sung SessionStart
export ECC_SESSION_START_CONTEXT=off
```

### 5.3 Tích hợp vào hooks.json

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
        "description": "Hook mới của tôi",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

---

## 6. Hook bất đồng bộ

Với các hook không nên block main flow (như phân tích background):

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

**Lưu ý về hook bất đồng bộ**:
- Hook bất đồng bộ chạy ở background
- Không thể block công cụ thực thi
- Nên hoàn thành trong 30 giây
- Phù hợp với: Log, phân tích, telemetry

---

## 7. Best practices hiệu năng

| Loại hook | Yêu cầu hiệu năng |
|----------|----------|
| PreToolUse | <200ms |
| PostToolUse（đồng bộ） | <1 giây |
| Hook bất đồng bộ | <30 giây |

### Tránh thao tác blocking

```javascript
// Không tốt: Đọc file đồng bộ
const content = fs.readFileSync('large-file.txt', 'utf8');

// Tốt: Bất đồng bộ hoặc load lazy
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // Xử lý
});
```

---

## 8. Debug hook

### Thêm output debug

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // Xử lý logic
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### Vấn đề thường gặp

| Vấn đề | Nguyên nhân | Giải pháp |
|------|------|------|
| Công cụ bị chặn ngoài ý muốn | Hook exit non-zero (ngoài 2) | Đổi thành exit 0 và stderr cảnh báo |
| Treo | Hook không kết thúc | Đảm bảo luôn output ra stdout |
| Dữ liệu hỏng | Output ra stdout không phải JSON | Chỉ output data gốc |
| Vấn đề hiệu năng | Hook quá chậm | Tối ưu hoặc chuyển thành async |

---

## Bài tập

Hoàn thành [bài tập](./exercises/练习.md) trong phần này.

---

[Quay lại thư mục khả năng cốt lõi](../README.md)