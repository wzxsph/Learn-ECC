# Custom Development

## Overview

This guide introduces how to develop custom hooks for the ECC Hooks system, including basic structure, best practices, and exit 0 requirements.

## Basic Structure

### Minimum Hook Template

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Access tool information
  const toolName = input.tool_name;        // "Edit", "Bash", "Write", etc.
  const toolInput = input.tool_input;      // Tool-specific parameters
  const toolOutput = input.tool_output;    // Only available in PostToolUse

  // Warn (non-blocking): Write to stderr
  console.error('[Hook] Warning message');

  // Block (PreToolUse only): exit code 2
  // process.exit(2);

  // Always output raw data to stdout
  console.log(data);
});
```

---

## Exit Code Requirements

### Key Rule: exit 0 on Non-Critical Errors

**Important**: All hooks must exit 0 on non-critical errors to avoid accidentally blocking tool execution.

| Exit Code | Meaning | Use Case |
|--------|------|----------|
| 0 | Success | Continue execution, or non-critical warning |
| 2 | Block | Only for critical blocking in PreToolUse |
| Other non-zero | Error | Only log, **do not use** |

### Why Non-Zero Exit Codes Are Bad?

If a hook exits with a non-zero exit code (except 2), Claude Code will mark the entire tool call as failed, which could cause:
- Tool execution to be interrupted unexpectedly
- Degraded user experience
- Hard-to-debug issues

### Correct Error Handling

```javascript
// WRONG example - don't do this
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // processing logic
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // WRONG: will block tool
  }
  console.log(data);
});

// CORRECT example - do this
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // processing logic
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // Non-critical error, exit 0 to continue
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
  tool_input: {               // Tool input parameters
    command?: string;         // Bash: command
    file_path?: string;        // Edit/Write/Read: file path
    old_string?: string;       // Edit: replaced text
    new_string?: string;       // Edit: replacement text
    content?: string;          // Write: file content
  };
  tool_output?: {             // PostToolUse only
    output?: string;          // Command/tool output
  };
}
```

### Accessing Tool Information

```javascript
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Get tool name
  console.log('Tool:', input.tool_name);

  // Handle based on tool type
  if (input.tool_name === 'Bash') {
    console.log('Command:', input.tool_input.command);
  }

  if (input.tool_name === 'Edit' || input.tool_name === 'Write') {
    console.log('File:', input.tool_input.file_path);
  }

  // PostToolUse can access output
  if (input.tool_output) {
    console.log('Output:', input.tool_output.output);
  }
});
```

---

## Common Hook Recipes

### Warn TODO/FIXME Comments

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "Warn when TODO/FIXME comments are added"
}
```

### Block Files Exceeding Size Limit

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');console.error('[Hook] Split into smaller, focused modules');process.exit(2)}console.log(d)})\""
  }],
  "description": "Block files over 800 lines"
}
```

### Auto-format Python Files with ruff

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "Auto-format Python files with ruff after edit"
}
```

### Require Tests for New Source Files

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p);console.error('[Hook] Expected: '+testPath);console.error('[Hook] Consider writing tests first (/tdd)')}}console.log(d)})\""
  }],
  "description": "Remind to create tests when adding new source files"
}
```

---

## Async Hooks

For hooks that should not block the main flow (e.g., background analysis):

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

### Async Hook Notes

- Async hooks run in background
- Cannot block tool execution
- Should complete within 30 seconds
- Suitable for: logging, analysis, telemetry

---

## Blocking Hooks

For cases where tool execution must be blocked (PreToolUse only):

```javascript
// Block in PreToolUse
process.exit(2);  // Exit code 2 means block
```

### When to Use Blocking

- Security check fails (e.g., detected keys)
- Hard policy violation
- Operations that could cause data loss

### When Not to Use Blocking

- Code style issues (use warning instead)
- Non-critical checks
- Suggestion-type checks

---

## Runtime Configuration

### Using run-with-flags.js

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

Use script path directly (if plugin root is known):

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

## Cross-Platform Notes

### Path Handling

```javascript
const path = require('path');
const os = require('os');

// Use path.join instead of hardcoding path separators
const configPath = path.join(os.homedir(), '.claude', 'config.json');

// Detect platform
if (process.platform === 'win32') {
  // Windows-specific handling
}
```

### Process Output

```javascript
// Use console.error for warnings (visible to user)
console.error('[Hook] Warning: some issue detected');

// Use console.log for raw data (required)
console.log(data);

// Never use console.log to output message text (will interfere with stdout data flow)
```

---

## Performance Best Practices

### Keep Fast

- PreToolUse hooks: <200ms
- PostToolUse sync hooks: <1 second
- Async hooks: <30 seconds

### Avoid Blocking Operations

```javascript
// BAD: Synchronous file read
const content = fs.readFileSync('large-file.txt', 'utf8');

// GOOD: Async or lazy load
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // handle
});
```

### Cache Results

```javascript
// Cache expensive operations
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

## Testing Hooks

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
console.log(data);  // Original input data

// Correct stderr output (warnings)
console.error('[Hook] Warning message');

// Incorrect - don't output other content to stdout
console.log('Some message');  // This will corrupt data flow
```

---

## Debugging Hooks

### Add Debug Output

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // processing logic
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### Common Issues

| Issue | Reason | Solution |
|------|------|----------|
| Tool accidentally blocked | Hook exits non-zero (except 2) | Change to exit 0 with stderr warning |
| Hanging | Hook doesn't end | Ensure always output to stdout |
| Data corruption | Non-JSON output to stdout | Only output original data |
| Performance issues | Hook too slow | Optimize or make async |

---

## Integrating into hooks.json

### Adding New Hook

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

### Disabling Built-in Hooks

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

## Best Practices Summary

1. **Always exit 0 on non-critical errors** - Don't use non-zero exit codes (except 2)
2. **Use console.error for warnings** - Don't use console.log
3. **Keep fast** - PreToolUse <200ms
4. **Always output raw data to stdout** - Don't modify data flow
5. **Use async for long-running operations** - Set async: true
6. **Cross-platform path handling** - Use path.join
7. **Test output format** - Ensure stdout is raw JSON
8. **Document your hooks** - Add clear description