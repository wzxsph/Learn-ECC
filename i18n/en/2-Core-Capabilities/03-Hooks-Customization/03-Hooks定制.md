# 03 - Hooks Customization

Learn ECC [Hook](../../Reference-Docs/hooks/Hook类型.md) system for workflow automation.

## Overview

ECC Hooks system is an event-driven automation mechanism that triggers before and after Claude Code tool execution, used to enforce code quality, catch errors early, and automate repetitive checks.

```
User request → Claude selects tool → PreToolUse hook runs → Tool executes → PostToolUse hook runs
```

---

## 1. Hook Types Detailed

### 1.1 PreToolUse Hook

Runs **before** tool execution, can block (exit code 2) or warn (stderr output without blocking).

#### Trigger Timing
- Before any tool (Edit, Write, Bash, Read, etc.) executes
- Applicable to pre-execution checks for all tool operations

#### Exit Codes
| Exit Code | Meaning | Behavior |
|-----------|---------|----------|
| 0 | Success | Continue execution, no warning |
| 2 | Block | Block tool execution |
| Other non-zero | Error | Log but don't block |

#### Usage Examples

**Dev server blocker:**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "Block running dev server outside tmux"
}
```

**Documentation file warning:**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Warn about non-standard documentation file creation"
}
```

### 1.2 PostToolUse Hook

Runs **after** tool execution, can analyze output but cannot block tool execution.

#### Trigger Timing
- Runs after any tool completes
- Can access tool's output results

#### Usage Examples

**PR logging:**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Log PR URL and review command after creation"
}
```

**Quality gate check:**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Run quality gate check after file edits"
}
```

### 1.3 Stop Hook

Runs after each Claude response, used for batch processing, final checks, and session state persistence.

#### Usage Examples

**Console log check:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "Check if console.log exists in modified files"
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
  "description": "Save session state"
}
```

### 1.4 SessionStart / SessionEnd Hooks

Lifecycle hooks for context loading at session start and state persistence at session end.

---

## 2. Hook Types Comparison Table

| Type | Trigger Timing | Can Block | Can Block | Typical Uses |
|------|----------------|-----------|------------|--------------|
| PreToolUse | Before tool | Yes (exit 2) | Yes (sync) | Quality checks, security validation |
| PostToolUse | After tool | No | Optional (async) | Logging, analysis |
| Stop | After each response | No | Optional (async) | Batch formatting, session summary |
| SessionStart | Session start | No | No | Load context, detect project |
| SessionEnd | Session end | No | No | Persist summary, cleanup |

---

## 3. Custom Hook Development

### 3.1 Basic Structure

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

  // Always output original data to stdout
  console.log(data);
});
```

### 3.2 HookInput Interface

```typescript
interface HookInput {
  tool_name: string;          // Tool name
  tool_input: {               // Tool input parameters
    command?: string;         // Bash: command
    file_path?: string;       // Edit/Write/Read: file path
    old_string?: string;      // Edit: replaced text
    new_string?: string;      // Edit: replacement text
    content?: string;         // Write: file content
  };
  tool_output?: {             // PostToolUse only
    output?: string;          // Command/tool output
  };
}
```

### 3.3 Key Rule: exit 0 on Non-Critical Errors

**Important**: All hooks must exit 0 on non-critical errors to avoid accidentally blocking tool execution.

| Exit Code | Meaning | Use Case |
|-----------|---------|----------|
| 0 | Success | Continue execution, or non-critical warning |
| 2 | Block | Only for critical blocking in PreToolUse |
| Other non-zero | Error | Only log, **don't use** |

### 3.4 Correct Error Handling

```javascript
// Wrong example - Don't do this
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Processing logic
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // Wrong: will block tool
  }
  console.log(data);
});

// Correct example - Do this
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Processing logic
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

## 4. Common Hook Recipes

### 4.1 Warn About TODO/FIXME Comments

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "Warn about adding TODO/FIXME comments"
}
```

### 4.2 Block Creating Oversized Files

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');process.exit(2)}console.log(d)})\""
  }],
  "description": "Block creating files over 800 lines"
}
```

### 4.3 Auto-Format Python Files with ruff

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

### 4.4 Require Tests for New Source Files

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p)}}console.log(d)})\""
  }],
  "description": "Remind to create tests when adding new source files"
}
```

---

## 5. Configuration Management

### 5.1 Using run-with-flags.js

```bash
# Using run-with-flags.js wrapper
node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict
```

### 5.2 Environment Variable Control

```bash
# minimal | standard | strict (default: standard)
export ECC_HOOK_PROFILE=standard

# Disable specific hook IDs (comma-separated)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Control SessionStart extra context size (default: 8000 chars)
export ECC_SESSION_START_MAX_CHARS=4000

# Completely disable SessionStart extra context
export ECC_SESSION_START_CONTEXT=off
```

### 5.3 Integration into hooks.json

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

---

## 6. Async Hooks

For hooks that should not block the main flow (like background analysis):

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

**Async hook notes**:
- Async hooks run in background
- Cannot block tool execution
- Should complete within 30 seconds
- Suitable for: Logging, analysis, telemetry

---

## 7. Performance Best Practices

| Hook Type | Performance Requirements |
|-----------|-------------------------|
| PreToolUse | <200ms |
| PostToolUse (sync) | <1 second |
| Async hooks | <30 seconds |

### Avoid Blocking Operations

```javascript
// Bad: Synchronous file reading
const content = fs.readFileSync('large-file.txt', 'utf8');

// Good: Async or lazy loading
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // Process
});
```

---

## 8. Debugging Hooks

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

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| Tool unexpectedly blocked | Hook exits non-zero (except 2) | Change to exit 0 and stderr warning |
| Hanging | Hook doesn't end | Ensure always output to stdout |
| Data corruption | Output non-JSON to stdout | Only output original data |
| Performance issues | Hook too slow | Optimize or make async |

---

## Exercises

Complete tasks in [exercises](./exercises/练习.md).

---

[Return to Core Capabilities README](../README.md)