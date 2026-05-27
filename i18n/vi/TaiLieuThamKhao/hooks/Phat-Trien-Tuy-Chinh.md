# Phát triển Tùy chỉnh

## Tổng quan

Hướng dẫn này giới thiệu cách phát triển custom hooks cho hệ thống ECC Hooks, bao gồm cấu trúc cơ bản, best practices và yêu cầu exit 0.

## Cấu trúc cơ bản

### Template hook tối thiểu

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Truy cập thông tin tool
  const toolName = input.tool_name;        // "Edit", "Bash", "Write", v.v.
  const toolInput = input.tool_input;      // Tham số đặc thù của tool
  const toolOutput = input.tool_output;    // Chỉ khả dụng ở PostToolUse

  // Cảnh báo (non-blocking): ghi vào stderr
  console.error('[Hook] Thông báo cảnh báo');

  // Block (chỉ PreToolUse): exit code 2
  // process.exit(2);

  // Luôn output dữ liệu gốc vào stdout
  console.log(data);
});
```

---

## Yêu cầu Exit Code

### Quy tắc quan trọng: exit 0 on lỗi không nghiêm trọng

**Quan trọng**: Tất cả hooks phải exit 0 khi có lỗi không nghiêm trọng, tránh blocking tool execution không mong muốn.

| Exit code | Ý nghĩa | Trường hợp sử dụng |
|--------|------|----------|
| 0 | Thành công | Tiếp tục thực thi, hoặc cảnh báo không nghiêm trọng |
| 2 | Block | Chỉ dùng cho blocking nghiêm trọng ở PreToolUse |
| Các giá trị khác | Lỗi | Chỉ ghi log, **không sử dụng** |

### Tại sao exit code khác 0 không tốt?

Nếu hook exit với code khác 0 (ngoài 2), Claude Code sẽ đánh dấu toàn bộ tool call là thất bại, điều này có thể dẫn đến:
- Tool execution bị interrupt không mong muốn
- Trải nghiệm người dùng bị ảnh hưởng
- Vấn đề khó debug

### Xử lý lỗi đúng cách

```javascript
// Ví dụ sai - Không làm như thế này
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Logic xử lý
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // Lỗi: sẽ block tool
  }
  console.log(data);
});

// Ví dụ đúng - Làm như thế này
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Logic xử lý
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // Lỗi không nghiêm trọng, exit 0 để tiếp tục
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## Mẫu Hook Input

### Giao diện HookInput

```typescript
interface HookInput {
  tool_name: string;          // Tên tool
  tool_input: {               // Tham số đầu vào của tool
    command?: string;         // Bash: lệnh
    file_path?: string;        // Edit/Write/Read: đường dẫn file
    old_string?: string;       // Edit: text bị thay thế
    new_string?: string;       // Edit: text thay thế
    content?: string;          // Write: nội dung file
  };
  tool_output?: {             // Chỉ PostToolUse
    output?: string;          // Output của lệnh/tool
  };
}
```

### Truy cập thông tin tool

```javascript
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Lấy tên tool
  console.log('Tool:', input.tool_name);

  // Xử lý theo loại tool
  if (input.tool_name === 'Bash') {
    console.log('Command:', input.tool_input.command);
  }

  if (input.tool_name === 'Edit' || input.tool_name === 'Write') {
    console.log('File:', input.tool_input.file_path);
  }

  // PostToolUse có thể truy cập output
  if (input.tool_output) {
    console.log('Output:', input.tool_output.output);
  }
});
```

---

## Các Recipe Hook phổ biến

### Cảnh báo TODO/FIXME comments

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "Cảnh báo khi thêm TODO/FIXME comments"
}
```

### Block file quá lớn

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');console.error('[Hook] Split into smaller, focused modules');process.exit(2)}console.log(d)})\""
  }],
  "description": "Block file vượt quá 800 dòng"
}
```

### Tự động format Python file bằng ruff

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "Tự động format Python files bằng ruff sau khi edit"
}
```

### Yêu cầu test khi thêm source file mới

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p);console.error('[Hook] Expected: '+testPath);console.error('[Hook] Consider writing tests first (/tdd)')}}console.log(d)})\""
  }],
  "description": "Nhắc tạo test khi thêm source file mới"
}
```

---

## Async Hooks

Đối với hooks không nên block main flow (như background analysis):

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

### Lưu ý về Async Hooks

- Async hooks chạy ở background
- Không thể block tool execution
- Nên hoàn thành trong 30 giây
- Phù hợp cho: logging, analytics, telemetry

---

## Blocking Hooks

Đối với trường hợp phải block tool execution (PreToolUse only):

```javascript
// Block trong PreToolUse
process.exit(2);  // Exit code 2 để block
```

### Khi nào sử dụng blocking

- Security check thất bại (phát hiện keys)
- Vi phạm hard policy
- Operations có thể导致数据丢失

### Khi nào KHÔNG sử dụng blocking

- Vấn đề code style (sử dụng cảnh báo thay thế)
- Checks không nghiêm trọng
- Suggestions

---

## Runtime Configuration

### Sử dụng run-with-flags.js

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const p=require('path');const r=(()=>{var e=process.env.CLAUDE_PLUGIN_ROOT;if(e&&e.trim())return e.trim();var p=require('path'),f=require('fs'),h=require('os').homedir(),d=p.join(h,'.claude'),q=p.join('scripts','lib','utils.js');if(f.existsSync(p.join(d,q)))return d;for(var s of [['ecc'],['ecc@ecc'],['marketplaces','ecc'],['everything-claude-code'],['everything-claude-code@everything-claude-code'],['marketplaces','everything-claude-code']]){var l=p.join(d,'plugins',...s);if(f.existsSync(p.join(l,q)))return l}try{for(var g of ['ecc','everything-claude-code']){var b=p.join(d,'plugins','cache',g);for(var o of f.readdirSync(b,{withFileTypes:true})){if(!o.isDirectory())continue;for(var v of f.readdirSync(p.join(b,o.name),{withFileTypes:true})){if(!v.isDirectory())continue;var c=p.join(b,o.name,v.name);if(f.existsSync(p.join(c,q)))return c}}}}catch(x){}return d})();const s=p.join(r,'scripts/hooks/plugin-hook-bootstrap.js');process.env.CLAUDE_PLUGIN_ROOT=r;process.argv.splice(1,0,s);require(s)\" node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict"
  }]
}
```

### Phiên bản đơn giản

Sử dụng trực tiếp script path (nếu plugin root đã biết):

```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/my-hook.js"
  }]
}
```

---

## Lưu ý đa nền tảng

### Xử lý đường dẫn

```javascript
const path = require('path');
const os = require('os');

// Sử dụng path.join thay vì hardcode path separator
const configPath = path.join(os.homedir(), '.claude', 'config.json');

// Detect platform
if (process.platform === 'win32') {
  // Windows-specific handling
}
```

### Output process

```javascript
// Sử dụng console.error cho cảnh báo (hiển thị cho user)
console.error('[Hook] Warning: some issue detected');

// Sử dụng console.log cho dữ liệu gốc (bắt buộc)
console.log(data);

// Không bao giờ dùng console.log để output message text (sẽ interfere với stdout data stream)
```

---

## Performance Best Practices

### Giữ nhanh

- PreToolUse hooks: <200ms
- PostToolUse synchronous hooks: <1 giây
- Async hooks: <30 giây

### Tránh blocking operations

```javascript
// Không tốt: synchronous file read
const content = fs.readFileSync('large-file.txt', 'utf8');

// Tốt: asynchronous hoặc deferred loading
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // Xử lý
});
```

### Cache kết quả

```javascript
// Cache expensive operations
let cachedResult = null;
let cacheTime = 0;
const CACHE_TTL = 60000; // 1 phút

function getCachedResult() {
  const now = Date.now();
  if (!cachedResult || now - cacheTime > CACHE_TTL) {
    cachedResult = expensiveOperation();
    cacheTime = now;
  }
  return cachedResult;
}
```

---

## Testing Hooks

### Manual testing

```bash
# Test PreToolUse hook
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"}}' | node scripts/hooks/my-hook.js

# Test PostToolUse hook
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"},"tool_output":{"output":"test\n"}}' | node scripts/hooks/my-hook.js
```

### Output format testing

```javascript
// Correct stdout output (JSON)
console.log(data);  // Dữ liệu đầu vào gốc

// Correct stderr output (warnings)
console.error('[Hook] Warning message');

// Incorrect - Không output gì khác vào stdout
console.log('Some message');  // Điều này sẽ phá vỡ data stream
```

---

## Debugging Hooks

### Thêm debug output

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // Logic xử lý
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### Vấn đề thường gặp

| Vấn đề | Nguyên nhân | Giải pháp |
|------|------|------|
| Tool bị block không mong muốn | Hook exit khác 0 (ngoài 2) | Đổi thành exit 0 và stderr warnings |
| Hang | Hook không kết thúc | Đảm bảo luôn output vào stdout |
| Data corruption | Output khác JSON vào stdout | Chỉ output data gốc |
| Performance issues | Hook quá chậm | Tối ưu hoặc chuyển thành async |

---

## Tích hợp vào hooks.json

### Thêm hook mới

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
        "description": "Mô tả hook mới của tôi",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

### Vô hiệu hóa built-in hook

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Vô hiệu hóa cảnh báo file tài liệu"
      }
    ]
  }
}
```

---

## Tóm tắt Best Practices

1. **Luôn exit 0 on lỗi không nghiêm trọng** - Không dùng exit code khác 0 (ngoài 2)
2. **Sử dụng console.error cho cảnh báo** - Không dùng console.log
3. **Giữ nhanh** - PreToolUse <200ms
4. **Luôn output data gốc vào stdout** - Không thay đổi data stream
5. **Sử dụng async cho operations lâu** - Đặt async: true
6. **Xử lý đường dẫn đa nền tảng** - Sử dụng path.join
7. **Test output format** - Đảm bảo stdout là JSON gốc
8. **Document hook của bạn** - Thêm description rõ ràng