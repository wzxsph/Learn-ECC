# Custom Development

## Tổng quan

Hướng dẫn này giới thiệu cách phát triển custom hook cho hệ thống ECC Hooks, bao gồm basic structure, best practice và exit 0 requirement.

## Basic Structure

### Minimum Hook Template

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Access tool info
  const toolName = input.tool_name;        // "Edit", "Bash", "Write", etc.
  const toolInput = input.tool_input;      // Tool-specific parameter
  const toolOutput = input.tool_output;    // Chỉ available trong PostToolUse

  // Warning (non-blocking): write to stderr
  console.error('[Hook] Warning message');

  // Block (chỉ PreToolUse): exit code 2
  // process.exit(2);

  // Always output raw data to stdout
  console.log(data);
});
```

---

## Exit Code Requirement

### Key rule: exit 0 on non-critical error

**Important**: Tất cả hook phải exit 0 khi có non-critical error, tránh blocking tool execution.

| Exit code | Ý nghĩa | Use case |
|--------|------|----------|
| 0 | Thành công | Tiếp tục thực thi, hoặc non-critical warning |
| 2 | Block | Chỉ dùng cho PreToolUse critical blocking |
| Khác 0 | Error | Chỉ ghi log, **không nên dùng** |

### Tại sao non-zero exit code không tốt?

Nếu hook exit với non-zero exit code (ngoài 2), Claude Code sẽ đánh dấu entire tool call là failed, điều này có thể gây ra:
- Tool execution bị interrupt
- User experience bị ảnh hưởng
- Problem khó debug

### Xử lý lỗi đúng cách

```javascript
// Sai - Đừng làm thế này
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Processing logic
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // Sai: sẽ block tool
  }
  console.log(data);
});

// Đúng - Làm thế này
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Processing logic
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // Non-critical error, exit 0 continue
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## Hook Input Pattern

### HookInput Interface

```typescript
interface HookInput {
  tool_name: string;          // Tool name
  tool_input: {               // Tool input parameter
    command?: string;         // Bash: command
    file_path?: string;        // Edit/Write/Read: file path
    old_string?: string;       // Edit: text bị replace
    new_string?: string;       // Edit: replacement text
    content?: string;          // Write: file content
  };
  tool_output?: {             // Chỉ PostToolUse
    output?: string;          // Command/tool output
  };
}
```

### Access Tool Info

```javascript
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Get tool name
  console.log('Tool:', input.tool_name);

  // Handle theo tool type
  if (input.tool_name === 'Bash') {
    console.log('Command:', input.tool_input.command);
  }

  if (input.tool_name === 'Edit' || input.tool_name === 'Write') {
    console.log('File:', input.tool_input.file_path);
  }

  // PostToolUse có thể access output
  if (input.tool_output) {
    console.log('Output:', input.tool_output.output);
  }
});
```

---

## Common Hook Recipe

### Warn TODO/FIXME Comment

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "Warn when adding TODO/FIXME comment"
}
```

### Block Creating Too Large File

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');console.error('[Hook] Split into smaller, focused modules');process.exit(2)}console.log(d)})\""
  }],
  "description": "Block creating file over 800 lines"
}
```

### Auto-format Python File with ruff

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "Auto-format Python file with ruff after edit"
}
```

### Require Test When Adding New Source File

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p);console.error('[Hook] Expected: '+testPath);console.error('[Hook] Consider writing tests first (/tdd)')}}console.log(d)})\""
  }],
  "description": "Remind to create test when adding new source file"
}
```

---

## Async Hook

Với hook không nên block main flow (như background analysis):

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

### Async Hook Lưu ý

- Async hook chạy ở background
- Không thể block tool execution
- Nên hoàn thành trong 30 giây
- Phù hợp cho: logging, analysis, telemetry

---

## Blocking Hook

Với trường hợp phải block tool execution (PreToolUse only):

```javascript
// Block trong PreToolUse
process.exit(2);  // Exit code 2 means block
```

### Khi nào dùng Block

- Security check fail (như detect secret)
- Hard policy violation
- Operation có thể cause data loss

### Khi nào không dùng Block

- Code style issue (dùng warning thay thế)
- Non-critical check
- Suggestion-type check

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

### Simplified Version

Sử dụng trực tiếp script path (nếu plugin root known):

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

## Cross-Platform Note

### Path Handling

```javascript
const path = require('path');
const os = require('os');

// Dùng path.join thay vì hard-code path separator
const configPath = path.join(os.homedir(), '.claude', 'config.json');

// Detect platform
if (process.platform === 'win32') {
  // Windows-specific handling
}
```

### Process Output

```javascript
// Dùng console.error output warning (hiển thị cho user)
console.error('[Hook] Warning: some issue detected');

// Dùng console.log output raw data (bắt buộc)
console.log(data);

// Không bao giờ dùng console.log output message text (sẽ interfere stdout data flow)
```

---

## Performance Best Practice

### Keep Fast

- PreToolUse hook: <200ms
- PostToolUse sync hook: <1 second
- Async hook: <30 seconds

### Avoid Blocking Operation

```javascript
// Bad: Sync file read
const content = fs.readFileSync('large-file.txt', 'utf8');

// Good: Async hoặc lazy load
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // Process
});
```

### Cache Result

```javascript
// Cache expensive operation
let cachedResult = null;
let cacheTime = 0;
const CACHE_TTL = 60000; // 1 minute

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

## Testing Hook

### Manual Testing

```bash
# Test PreToolUse hook
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"}}' | node scripts/hooks/my-hook.js

# Test PostToolUse hook
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"},"tool_output":{"output":"test\n"}}' | node scripts/hooks/my-hook.js
```

### Test Output Format

```javascript
// Correct stdout output (JSON)
console.log(data);  // Raw input data

// Correct stderr output (warning)
console.error('[Hook] Warning message');

// Incorrect - Không output other content to stdout
console.log('Some message');  // This will corrupt data flow
```

---

## Debugging Hook

### Add Debug Output

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // Processing logic
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### Common Problem

| Problem | Reason | Solution |
|------|------|----------|
| Tool bị block | Hook exit non-zero (ngoài 2) | Đổi thành exit 0 và stderr warning |
| Hang | Hook không end | Ensure luôn output to stdout |
| Data corruption | Output non-JSON to stdout | Chỉ output raw data |
| Performance issue | Hook quá chậm | Optimize hoặc đổi thành async |

---

## Integration vào hooks.json

### Add New Hook

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
        "description": "My new hook description",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

### Disable Built-in Hook

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Disable doc file warning"
      }
    ]
  }
}
```

---

## Best Practice Summary

1. **Luôn exit 0 on non-critical error** - Không dùng non-zero exit code (ngoài 2)
2. **Dùng console.error output warning** - Không dùng console.log
3. **Giữ nhanh** - PreToolUse <200ms
4. **Luôn output raw data to stdout** - Không thay đổi data flow
5. **Dùng async xử lý long operation** - Set async: true
6. **Cross-platform path handling** - Dùng path.join
7. **Test output format** - Ensure stdout là raw JSON
8. **Document hook của bạn** - Thêm clear description