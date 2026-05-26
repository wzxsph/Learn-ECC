# Configuration Format

## hooks.json Structure Overview

ECC Hooks system configuration files use JSON format, with the following main structure:

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

## Top-level Structure

| Field | Type | Description |
|------|------|------|
| `$schema` | string | JSON Schema URL for configuration validation |
| `hooks` | object | Object containing all hook types |

---

## Hook Array Structure

Each event type (PreToolUse, PostToolUse, etc.) contains a hook array, where each hook object has the following structure:

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
  "description": "Hook description",
  "id": "unique-hook-id"
}
```

### matcher Field

#### Type Description
The matcher field specifies which tool types trigger the hook, supporting single or multiple tools (separated by `|`).

#### Available Values
| matcher Value | Matched Tools |
|-----------|-----------------|
| `*` | All tools |
| `Bash` | Bash tool |
| `Edit` | Edit tool |
| `Write` | Write tool |
| `Read` | Read tool |
| `MultiEdit` | MultiEdit tool |
| `Bash\|Edit\|Write` | Bash, Edit, or Write |
| `Edit\|Write\|MultiEdit` | Edit, Write, or MultiEdit |

#### Usage Examples
```json
// Match all tools
"matcher": "*"

// Match only Bash tool
"matcher": "Bash"

// Match multiple tools
"matcher": "Edit|Write|MultiEdit"
```

---

## hooks Array Structure

The hooks array contains one or more hook command objects:

### type Field

| type Value | Description |
|--------|------|
| `command` | Execute Node.js command |

### command Field

The command to execute, usually a Node.js script path.

### async Field (Optional)

```json
"async": true
```

- `true`: Execute asynchronously, does not block tool execution
- `false` or omitted: Execute synchronously, blocks tool execution

### timeout Field (Optional)

```json
"timeout": 30
```

Maximum execution time in seconds. Recommendations:
- Sync hooks: <200ms
- Async hooks: ≤30 seconds

---

## Complete Configuration Examples

### PreToolUse Hook Example

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/pre-bash-dispatcher.js"
    }
  ],
  "description": "Bash pre-check dispatcher for quality, tmux, push, and GateGuard checks",
  "id": "pre:bash:dispatcher"
}
```

### PostToolUse Hook Example

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
  "description": "Run quality gate checks after file edits",
  "id": "post:quality-gate"
}
```

### Stop Hook Example

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
  "description": "Save session state after each response",
  "id": "stop:session-end"
}
```

### SessionStart Hook Example

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/session-start-bootstrap.js"
    }
  ],
  "description": "Load bounded prior context and detect package manager for new session",
  "id": "session:start"
}
```

---

## Lifecycle Event Configuration Format

For lifecycle events like SessionStart, SessionEnd, PreCompact, use a different configuration structure:

```json
{
  "description": "Lifecycle hook definitions for memory persistence",
  "events": [
    {
      "event": "SessionStart",
      "id": "session:start",
      "script": "scripts/hooks/session-start-bootstrap.js",
      "purpose": "Load bounded prior context and detect project state at session start",
      "blocking": false
    },
    {
      "event": "PreCompact",
      "id": "pre:compact",
      "script": "scripts/hooks/pre-compact.js",
      "purpose": "Persist session state before context compression",
      "blocking": false
    }
  ]
}
```

### Lifecycle Event Fields

| Field | Description |
|------|------|
| `event` | Event type (SessionStart, PreCompact, SessionEnd, etc.) |
| `id` | Unique identifier |
| `script` | Script path |
| `purpose` | Purpose description |
| `blocking` | Whether to block (usually false) |

---

## Disabling Hooks

### Disable via Configuration Edit

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Override: Allow all .md file creation"
      }
    ]
  }
}
```

### Disable via Environment Variables

```bash
# Disable specific hooks (comma-separated)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Disable GateGuard
export ECC_GATEGUARD=off
```

---

## Hook ID Naming Convention

ECC uses a colon-separated naming convention:

| Prefix | Purpose | Example |
|------|------|------|
| `pre:` | PreToolUse hooks | `pre:bash:dispatcher` |
| `post:` | PostToolUse hooks | `post:quality-gate` |
| `stop:` | Stop hooks | `stop:session-end` |
| `session:` | Session lifecycle hooks | `session:start` |

---

## run-with-flags.js Wrapper

ECC uses the `run-with-flags.js` wrapper to run hooks, supporting runtime gating from configuration files:

```
node scripts/hooks/run-with-flags.js <hook-id> <script-path> <profiles>
```

### Parameter Description
| Parameter | Description |
|------|------|
| `hook-id` | Unique hook identifier |
| `script-path` | Actual script path to run |
| `profiles` | Comma-separated list of profiles (minimal, standard, strict) |

### How It Works
1. Read `ECC_HOOK_PROFILE` environment variable (default: standard)
2. Check if hook ID is in `ECC_DISABLED_HOOKS`
3. Run script if allowed

---

## Cross-Platform Path Handling

ECC hook commands use Node.js scripts for cross-platform behavior:

```javascript
const path = require('path');
const homedir = require('os').homedir();

// Cross-platform path resolution
const configDir = path.join(homedir, '.claude');
const hookScript = path.join(configDir, 'scripts', 'hooks', 'my-hook.js');
```

---

## Configuration Validation

It is recommended to use JSON Schema for configuration validation:
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json"
}
```