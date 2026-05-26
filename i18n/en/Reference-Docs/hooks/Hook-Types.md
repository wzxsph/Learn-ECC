# Hook Types

## Overview

ECC Hooks system is an event-driven automation mechanism that triggers before and after Claude Code tool execution, used to enforce code quality, catch errors early, and automate repetitive checks.

```
User request → Claude selects tool → PreToolUse hook runs → Tool executes → PostToolUse hook runs
```

## Hook Types Detailed

### 1. PreToolUse Hook

#### Description
PreToolUse hooks run **before** tool execution, can block (exit code 2) or warn (stderr output without blocking).

#### Trigger Timing
- Before any tool (Edit, Write, Bash, Read, etc.) executes
- Applicable to pre-execution checks for all tool operations

#### Parameters
```typescript
interface HookInput {
  tool_name: string;          // Tool name: "Bash", "Edit", "Write", "Read", etc.
  tool_input: {
    command?: string;         // Bash: command to execute
    file_path?: string;        // Edit/Write/Read: target file path
    old_string?: string;       // Edit: replaced text
    new_string?: string;       // Edit: replacement text
    content?: string;          // Write: file content
  };
}
```

#### Exit Codes
| Exit Code | Meaning | Behavior |
|-----------|---------|----------|
| 0 | Success | Continue execution, no warning |
| 2 | Block | Block tool execution |
| Other non-zero | Error | Log but don't block |

#### Usage Examples

**Dev server blocker (block running npm run dev outside tmux):**
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

**Documentation file warning (block non-standard .md files):**
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

#### Notes
- PreToolUse is **blocking** hook, must execute quickly (<200ms)
- Don't make network calls in PreToolUse hooks
- Blocking operations (exit 2) should be used sparingly, only for critical security checks

---

### 2. PostToolUse Hook

#### Description
PostToolUse hooks run **after** tool execution, can analyze output but cannot block tool execution.

#### Trigger Timing
- Runs after any tool completes
- Can access tool's output results

#### Parameters
```typescript
interface HookInput {
  tool_name: string;          // Tool name
  tool_input: {               // Same as PreToolUse
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // Available only in PostToolUse
    output?: string;          // Command/tool output
  };
}
```

#### Exit Codes
| Exit Code | Meaning |
|-----------|---------|
| 0 | Success, continue execution |
| Non-zero | Error, log but don't block tool execution |

#### Usage Examples

**PR logging (log PR URL after gh pr create):**
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

**Quality gate check (run quality check after edit):**
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

#### Notes
- PostToolUse hooks **cannot block** tool execution
- Can be set to async (async: true) to avoid blocking
- Async hooks should complete within 30 seconds

---

### 3. Stop Hook

#### Description
Stop hooks run after each Claude response, used for batch processing, final checks, and session state persistence.

#### Trigger Timing
- After each user request is processed and Claude generates a response
- Happens after the last response in a session

#### Parameters
Stop hooks receive the same input as PostToolUse, containing complete tool call history.

#### Usage Examples

**Console log checking (check for console.log in modified files after response):**
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

**Batch format and type checking:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "Batch format all edited JS/TS files and type check"
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

#### Notes
- Stop hooks run after every response, suitable for batch operations
- Can use async mode for non-blocking operations
- Formatting/type checking is suitable for batch execution at Stop

---

### 4. SessionStart Hook

#### Description
SessionStart hooks trigger at the start of session lifecycle, used to load previous context and detect project state.

#### Trigger Timing
- When new session starts
- When Claude Code starts or creates new session

#### Usage Examples

**Load previous context and detect package manager:**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "Load bounded previous context and detect project state at session start",
  "blocking": false
}
```

#### Notes
- Lifecycle hooks, must complete quickly (<30 seconds)
- Can control extra context size via `ECC_SESSION_START_MAX_CHARS` environment variable
- Can be completely disabled with `ECC_SESSION_START_CONTEXT=off`

---

### 5. SessionEnd Hook

#### Description
SessionEnd hooks trigger when session ends, used for persisting session end summary and cleanup.

#### Trigger Timing
- When session ends
- When session transcript metadata is available

#### Usage Examples

**Session end marking:**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "Persist session end summary when transcript metadata is available",
  "blocking": false
}
```

#### Notes
- Primarily async execution, should not block session end
- Lifecycle marking for metrics collection and cleanup

---

### 6. PreCompact Hook

#### Description
PreCompact hooks trigger before context compaction, used to save state.

#### Trigger Timing
- Before context compaction/pruning
- When context window is approaching full

#### Usage Examples

**Save state before context compaction:**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "Persist session state before context compaction",
  "blocking": false
}
```

#### Notes
- Suitable for saving state of long-running tasks
- Non-blocking, doesn't affect compaction process

---

### 7. PostToolUseFailure Hook

#### Description
PostToolUseFailure hooks trigger after tool execution failure.

#### Trigger Timing
- After any tool call fails

#### Usage Examples

**MCP health check (track failed MCP calls):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "Track failed MCP tool calls, mark unhealthy servers and attempt reconnection"
}
```

#### Notes
- Can be used for retry logic or health status tracking
- Does not change the failed status of the tool

---

## Hook Types Comparison

| Type | Trigger Timing | Can Block | Can Block | Typical Uses |
|------|----------------|-----------|------------|--------------|
| PreToolUse | Before tool | Yes (exit 2) | Yes (sync) | Quality checks, security validation |
| PostToolUse | After tool | No | Optional (async) | Logging, analysis |
| Stop | After each response | No | Optional (async) | Batch formatting, session summary |
| SessionStart | Session start | No | No | Load context, detect project |
| SessionEnd | Session end | No | No | Persist summary, cleanup |
| PreCompact | Before compaction | No | No | State saving |
| PostToolUseFailure | After tool failure | No | No | Health check, retry |

---

## Environment Variable Control

Hook behavior can be controlled via environment variables:

```bash
# minimal | standard | strict (default: standard)
export ECC_HOOK_PROFILE=standard

# Disable specific hook IDs (comma-separated)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Disable GateGuard
export ECC_GATEGUARD=off

# Control SessionStart extra context size (default: 8000 chars)
export ECC_SESSION_START_MAX_CHARS=4000

# Completely disable SessionStart extra context
export ECC_SESSION_START_CONTEXT=off
```