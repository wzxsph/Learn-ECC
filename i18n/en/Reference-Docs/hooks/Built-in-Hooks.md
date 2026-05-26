# Built-in Hooks

## Overview

The ECC plugin provides rich built-in hook implementations in the `ECC/scripts/hooks/` directory.

## PreToolUse Built-in Hooks

### pre:bash:dispatcher

**Type**: PreToolUse
**matcher**: Bash
**Description**: Bash pre-check dispatcher combining quality checks, tmux reminders, git push reminders, and GateGuard checks

**Functions**:
- Development server blocker - blocks running `npm run dev` etc. outside tmux
- Tmux reminder - suggests using tmux for long-running commands (npm test, cargo build, docker)
- Git push reminder - reminds to review changes before `git push`
- Pre-commit quality check - runs quality checks: lint staged files, validate commit message format, detect console.log/debugger/keys

**Exit Codes**:
- 0: Warn but continue
- 2: Block execution

---

### pre:write:doc-file-warning

**Type**: PreToolUse
**matcher**: Write
**Description**: Warn about creating non-standard documentation files

**Functions**:
- Allow standard files: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- Allow directories: docs/, skills/
- Warn about other .md/.txt files
- Cross-platform path handling

**Exit Code**: 0 (warning only)

---

### pre:edit-write:suggest-compact

**Type**: PreToolUse
**matcher**: Edit|Write
**Description**: Suggest manual `/compact` at logical intervals (approximately every 50 tool calls)

**Functions**:
- Track tool call count
- Remind about context compression at appropriate intervals
- Non-blocking, only provides suggestions

**Exit Code**: 0 (suggestion)

---

### pre:observe:continuous-learning

**Type**: PreToolUse
**matcher**: *
**Description**: Record tool intent to support continuous learning signals

**Functions**:
- Capture tool use observations
- Used for pattern extraction and analysis
- Execute asynchronously, non-blocking

**Exit Code**: 0

---

### pre:governance-capture

**Type**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Description**: Capture governance events (keys, policy violations, approval requests)

**Functions**:
- Detect secret/key leaks
- Capture policy violations
- Log approval requests

**Enable Condition**: `ECC_GOVERNANCE_CAPTURE=1`

**Exit Code**: 0

---

### pre:config-protection

**Type**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**Description**: Block modification of linter/formatter config files

**Functions**:
- Block .eslintrc, .prettierrc, etc. modifications
- Guide fixing code instead of weakening config

**Exit Code**: 0

---

### pre:mcp-health-check

**Type**: PreToolUse
**matcher**: *
**Description**: Check MCP server health status before MCP tool execution

**Functions**:
- Check MCP server status
- Block calls to unhealthy MCP servers

**Exit Code**: 0

---

### pre:edit-write:gateguard-fact-force

**Type**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**Description**: Fact-force GateGuard: block first edit/write of each file, require investigation before allowing

**Functions**:
- Block first edit
- Require confirmation: imports, data schema, user instructions
- Ensure sufficient reason before modifying

**Exit Code**: 0

---

## PostToolUse Built-in Hooks

### post:bash:dispatcher

**Type**: PostToolUse
**matcher**: Bash
**Description**: Bash post-check dispatcher for logging, PR and build notifications

**Functions**:
- PR logging - log PR URL and review command after `gh pr create`
- Build analysis - background analysis after build commands (async, non-blocking)

**Exit Code**: 0

---

### post:quality-gate

**Type**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Description**: Run quick quality checks after file edits

**Functions**:
- Code quality checks
- Execute asynchronously
- Don't block edit operations

**Exit Code**: 0

---

### post:edit:design-quality-check

**Type**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Description**: Warn about frontend edits deviating toward generic template appearance

**Functions**:
- Detect UI design quality
- Prevent overly generic template appearance

**Exit Code**: 0

---

### post:edit:accumulator

**Type**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Description**: Record paths of edited JS/TS files for batch formatting and type checking at Stop

**Functions**:
- Accumulate list of edited files
- For use by stop:format-typecheck

**Exit Code**: 0

---

### post:edit:console-warn

**Type**: PostToolUse
**matcher**: Edit
**Description**: Warn about console.log statements after edit

**Functions**:
- Detect console.log in code
- Warn developers to clean up debug code

**Exit Code**: 0

---

### post:governance-capture

**Type**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Description**: Capture governance events from tool output

**Functions**:
- Analyze tool output
- Detect potential issues

**Enable Condition**: `ECC_GOVERNANCE_CAPTURE=1`

**Exit Code**: 0

---

### post:session-activity-tracker

**Type**: PostToolUse
**matcher**: *
**Description**: Record tool calls and file activity per session

**Functions**:
- Track tool usage statistics
- File activity logging
- For ECC2 metrics

**Exit Code**: 0

---

### post:observe:continuous-learning

**Type**: PostToolUse
**matcher**: *
**Description**: Record tool results to support continuous learning signals

**Functions**:
- Capture tool execution results
- Support pattern analysis

**Exit Code**: 0

---

### post:ecc-metrics-bridge

**Type**: PostToolUse
**matcher**: *
**Description**: Maintain running session metrics aggregation

**Functions**:
- Track token and cost metrics
- For status bar and context monitor use

**Exit Code**: 0

---

### post:ecc-context-monitor

**Type**: PostToolUse
**matcher**: *
**Description**: Inject agent warnings when context is exhausted, high cost, scope creep, or tool looping

**Functions**:
- Context usage monitoring
- Cost warnings
- Scope creep detection
- Tool loop detection

**Exit Code**: 0

---

### post:mcp-health-check

**Type**: PostToolUseFailure
**matcher**: *
**Description**: Track failed MCP tool calls, mark unhealthy servers and attempt reconnect

**Functions**:
- Failure tracking
- Health status marking
- Automatic reconnect attempts

**Exit Code**: 0

---

## Stop Built-in Hooks

### stop:format-typecheck

**Type**: Stop
**matcher**: *
**Description**: Batch format (Biome/Prettier) and type check (tsc) all JS/TS files edited in this response

**Functions**:
- Prettier/Biome formatting
- TypeScript type checking
- Run once at Stop instead of after each edit

**Exit Code**: 0

**Timeout**: 300 seconds

---

### stop:check-console-log

**Type**: Stop
**matcher**: *
**Description**: Check for console.log in modified files after each response

**Functions**:
- Scan modified files
- Report console.log usage

**Exit Code**: 0

---

### stop:session-end

**Type**: Stop
**matcher**: *
**Description**: Persist session state after each response (Stop carries transcript_path)

**Functions**:
- Session state saving
- Context persistence

**Exit Code**: 0

---

### stop:evaluate-session

**Type**: Stop
**matcher**: *
**Description**: Evaluate session to extract learnable patterns

**Functions**:
- Pattern recognition
- Continuous learning support
- Execute asynchronously

**Exit Code**: 0

---

### stop:cost-tracker

**Type**: Stop
**matcher**: *
**Description**: Track token and cost metrics per session

**Functions**:
- Token usage statistics
- Cost estimation
- Lightweight operational cost telemetry tagging

**Exit Code**: 0

---

### stop:desktop-notify

**Type**: Stop
**matcher**: *
**Description**: Send macOS/WSL desktop notification when Claude responds, with task summary

**Functions**:
- Desktop notification
- Task summary display

**Exit Code**: 0

---

## SessionStart Built-in Hooks

### session:start

**Type**: SessionStart
**matcher**: *
**Description**: Load prior context and detect package manager for new session

**Functions**:
- Load bounded prior context
- Project state detection
- Package manager detection (npm/pnpm/yarn/bun)

**Environment Variables**:
- `ECC_SESSION_START_MAX_CHARS`: Control extra context size (default 8000 characters)
- `ECC_SESSION_START_CONTEXT=off`: Completely disable extra context

**Exit Code**: 0

---

## PreCompact Built-in Hooks

### pre:compact

**Type**: PreCompact
**matcher**: *
**Description**: Save state before context compression

**Functions**:
- Session state persistence
- Prepare context for compression

**Exit Code**: 0

---

## SessionEnd Built-in Hooks

### session:end:marker

**Type**: SessionEnd
**matcher**: *
**Description**: Session end lifecycle marker

**Functions**:
- Lifecycle event marking
- Cleanup logging

**Exit Code**: 0

---

## Built-in Hook Quick Reference

### PreToolUse Hooks

| ID | Matcher | Function | Blocking |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | Quality/tmux/push/GateGuard checks | Can block |
| pre:write:doc-file-warning | Write | Doc file warning | No |
| pre:edit-write:suggest-compact | Edit\|Write | Suggest compact | No |
| pre:observe:continuous-learning | * | Continuous learning observation | No |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit | Governance event capture | No |
| pre:config-protection | Write\|Edit\|MultiEdit | Config protection | No |
| pre:mcp-health-check | * | MCP health check | Can block |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | First edit GateGuard | Can block |

### PostToolUse Hooks

| ID | Matcher | Function | Async |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR logging/build notifications | Yes |
| post:quality-gate | Edit\|Write\|MultiEdit | Quality gate checks | Yes |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | Design quality check | No |
| post:edit:accumulator | Edit\|Write\|MultiEdit | Edit accumulator | No |
| post:edit:console-warn | Edit | console.log warning | No |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit | Governance event capture | No |
| post:session-activity-tracker | * | Session activity tracking | No |
| post:observe:continuous-learning | * | Continuous learning observation | Yes |
| post:ecc-metrics-bridge | * | Metrics bridge | No |
| post:ecc-context-monitor | * | Context monitor | No |
| post:mcp-health-check | * (PostToolUseFailure) | MCP health check | No |

### Stop Hooks

| ID | Matcher | Function | Timeout |
|----|---------|------|------|
| stop:format-typecheck | * | Batch format and type check | 300s |
| stop:check-console-log | * | console.log check | 30s |
| stop:session-end | * | Session state persistence | 10s |
| stop:evaluate-session | * | Session evaluation | 10s |
| stop:cost-tracker | * | Cost tracking | 10s |
| stop:desktop-notify | * | Desktop notification | 10s |

### Lifecycle Hooks

| ID | Event | Function |
|----|-------|------|
| session:start | SessionStart | Load context and detect package manager |
| pre:compact | PreCompact | State save before compression |
| session:end:marker | SessionEnd | Session end marker |