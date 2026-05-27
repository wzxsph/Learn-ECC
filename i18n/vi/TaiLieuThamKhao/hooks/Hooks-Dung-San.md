# Built-in Hooks

## Tổng quan

ECC plugin cung cấp rich built-in Hook implementation, nằm trong `ECC/scripts/hooks/` directory.

## PreToolUse Built-in Hooks

### pre:bash:dispatcher

**Loại**: PreToolUse
**matcher**: Bash
**Mô tả**: Bash pre-check dispatcher, tích hợp quality check, tmux reminder, git push reminder và GateGuard check

**Chức năng**:
- Dev server blocker - Block chạy `npm run dev` bên ngoài tmux
- Tmux reminder - Suggest dùng tmux cho long-running command (npm test, cargo build, docker)
- Git push reminder - Remind review changes trước `git push`
- Pre-commit quality check - Chạy quality check: lint staged file, verify commit message format, detect console.log/debugger/secret

**Exit code**:
- 0: Warning nhưng tiếp tục
- 2: Block execution

---

### pre:write:doc-file-warning

**Loại**: PreToolUse
**matcher**: Write
**Mô tả**: Warning khi tạo non-standard doc file

**Chức năng**:
- Allow standard file: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- Allow directory: docs/, skills/
- Warning other .md/.txt file
- Cross-platform path handling

**Exit code**: 0 (chỉ warning)

---

### pre:edit-write:suggest-compact

**Loại**: PreToolUse
**matcher**: Edit|Write
**Mô tả**: Suggest manual `/compact` tại interval logic (khoảng mỗi 50 tool call)

**Chức năng**:
- Track số lần tool call
- Remind context compress ở appropriate interval
- Non-blocking, chỉ provide suggestion

**Exit code**: 0 (suggestion)

---

### pre:observe:continuous-learning

**Loại**: PreToolUse
**matcher**: *
**Mô tả**: Record tool intent để support continuous learning signal

**Chức năng**:
- Capture tool usage observation
- Dùng cho pattern extraction và analysis
- Async execution, không block

**Exit code**: 0

---

### pre:governance-capture

**Loại**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Mô tả**: Capture governance event (secret, policy violation, approval request)

**Chức năng**:
- Detect secret/key leak
- Capture policy violation
- Record approval request

**Enable condition**: `ECC_GOVERNANCE_CAPTURE=1`

**Exit code**: 0

---

### pre:config-protection

**Loại**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**Mô tả**: Block modify linter/formatter config file

**Chức năng**:
- Block .eslintrc, .prettierrc, etc. modification
- Guide fix code thay vì weaken config

**Exit code**: 0

---

### pre:mcp-health-check

**Loại**: PreToolUse
**matcher**: *
**Mô tả**: Check MCP server health status trước MCP tool execution

**Chức năng**:
- Check MCP server status
- Block call đến unhealthy MCP server

**Exit code**: 0

---

### pre:edit-write:gateguard-fact-force

**Loại**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**Mô tả**: Fact-force GateGuard: block first edit/write của mỗi file, yêu cầu investigation trước khi allow

**Chức năng**:
- Block first edit
- Require confirmation: import, data pattern, user instruction
- Ensure có đủ lý do trước khi modify

**Exit code**: 0

---

## PostToolUse Built-in Hooks

### post:bash:dispatcher

**Loại**: PostToolUse
**matcher**: Bash
**Mô tả**: Bash post-check dispatcher, cho logging, PR và build notification

**Chức năng**:
- PR logging - Log PR URL và review command sau `gh pr create`
- Build analysis - Background analysis sau build command (async, non-blocking)

**Exit code**: 0

---

### post:quality-gate

**Loại**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Mô tả**: Run fast quality check sau khi file edit

**Chức năng**:
- Code quality check
- Async execution
- Không block edit operation

**Exit code**: 0

---

### post:edit:design-quality-check

**Loại**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Mô tả**: Warning khi frontend edit deviate về generic template-looking UI

**Chức năng**:
- Detect UI design quality
- Prevent quá generic template-looking appearance

**Exit code**: 0

---

### post:edit:accumulator

**Loại**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Mô tả**: Record JS/TS file path đã edit để batch format và type check ở Stop

**Chức năng**:
- Accumulate edited file list
- Dùng cho stop:format-typecheck

**Exit code**: 0

---

### post:edit:console-warn

**Loại**: PostToolUse
**matcher**: Edit
**Mô tả**: Warning console.log statement sau edit

**Chức năng**:
- Detect console.log trong code
- Warning developer clean up debug code

**Exit code**: 0

---

### post:governance-capture

**Loại**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Mô tả**: Capture governance event từ tool output

**Chức năng**:
- Analyze tool output
- Detect potential issue

**Enable condition**: `ECC_GOVERNANCE_CAPTURE=1`

**Exit code**: 0

---

### post:session-activity-tracker

**Loại**: PostToolUse
**matcher**: *
**Mô tả**: Record tool call và file activity mỗi session

**Chức năng**:
- Track tool usage statistics
- File activity recording
- Dùng cho ECC2 metrics

**Exit code**: 0

---

### post:observe:continuous-learning

**Loại**: PostToolUse
**matcher**: *
**Mô tả**: Record tool result để support continuous learning signal

**Chức năng**:
- Capture tool execution result
- Support pattern analysis

**Exit code**: 0

---

### post:ecc-metrics-bridge

**Loại**: PostToolUse
**matcher**: *
**Mô tả**: Maintain running session metrics aggregation

**Chức năng**:
- Track token và cost metrics
- Dùng cho status bar và context monitor

**Exit code**: 0

---

### post:ecc-context-monitor

**Loại**: PostToolUse
**matcher**: *
**Mô tả**: Inject proxy warning khi context exhausted, high cost, scope creep hoặc tool loop

**Chức năng**:
- Context usage monitoring
- Cost warning
- Scope creep detection
- Tool loop detection

**Exit code**: 0

---

### post:mcp-health-check

**Loại**: PostToolUseFailure
**matcher**: *
**Mô tả**: Track failed MCP tool call, mark unhealthy server và attempt reconnect

**Chức năng**:
- Failure tracking
- Health state marking
- Auto reconnect attempt

**Exit code**: 0

---

## Stop Built-in Hooks

### stop:format-typecheck

**Loại**: Stop
**matcher**: *
**Mô tả**: Batch format (Biome/Prettier) và type check (tsc) tất cả JS/TS file đã edit trong response này

**Chức năng**:
- Prettier/Biome formatting
- TypeScript type check
- Run một lần ở Stop thay vì sau mỗi edit

**Exit code**: 0

**Timeout**: 300 giây

---

### stop:check-console-log

**Loại**: Stop
**matcher**: *
**Mô tả**: Check console.log trong modified file sau mỗi response

**Chức năng**:
- Scan modified file
- Report console.log usage

**Exit code**: 0

---

### stop:session-end

**Loại**: Stop
**matcher**: *
**Mô tả**: Persist session state sau mỗi response (Stop mang transcript_path)

**Chức năng**:
- Session state saving
- Context persistence

**Exit code**: 0

---

### stop:evaluate-session

**Loại**: Stop
**matcher**: *
**Mô tả**: Evaluate session để extract learnable pattern

**Chức năng**:
- Pattern recognition
- Continuous learning support
- Async execution

**Exit code**: 0

---

### stop:cost-tracker

**Loại**: Stop
**matcher**: *
**Mô tả**: Track token và cost metrics mỗi session

**Chức năng**:
- Token usage statistics
- Cost estimation
- Lightweight running cost telemetry tagging

**Exit code**: 0

---

### stop:desktop-notify

**Loại**: Stop
**matcher**: *
**Mô tả**: Gửi macOS/WSL desktop notification khi Claude respond, chứa task summary

**Chức năng**:
- Desktop notification
- Task summary display

**Exit code**: 0

---

## SessionStart Built-in Hooks

### session:start

**Loại**: SessionStart
**matcher**: *
**Mô tả**: Load previous context và detect package manager cho session mới

**Chức năng**:
- Load bounded previous context
- Project state detection
- Package manager detection (npm/pnpm/yarn/bun)

**Environment variable**:
- `ECC_SESSION_START_MAX_CHARS`: Control extra context size (default 8000 chars)
- `ECC_SESSION_START_CONTEXT=off`: Disable extra context hoàn toàn

**Exit code**: 0

---

## PreCompact Built-in Hooks

### pre:compact

**Loại**: PreCompact
**matcher**: *
**Mô tả**: Save state trước khi context compact

**Chức năng**:
- Session state persistence
- Prepare context cho compact

**Exit code**: 0

---

## SessionEnd Built-in Hooks

### session:end:marker

**Loại**: SessionEnd
**matcher**: *
**Mô tả**: Session end lifecycle marker

**Chức năng**:
- Lifecycle event marking
- Cleanup log

**Exit code**: 0

---

## Built-in Hook Quick Reference

### PreToolUse Hooks

| ID | Matcher | Chức năng | Blocking |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | Quality/tmux/push/GateGuard check | Có thể blocking |
| pre:write:doc-file-warning | Write | Doc file warning | Không |
| pre:edit-write:suggest-compact | Edit\|Write | Suggest compact | Không |
| pre:observe:continuous-learning | * | Continuous learning observation | Không |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit | Governance event capture | Không |
| pre:config-protection | Write\|Edit\|MultiEdit | Config protection | Không |
| pre:mcp-health-check | * | MCP health check | Có thể blocking |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | First edit GateGuard | Có thể blocking |

### PostToolUse Hooks

| ID | Matcher | Chức năng | Async |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR logging/build notification | Có |
| post:quality-gate | Edit\|Write\|MultiEdit | Quality gate check | Có |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | Design quality check | Không |
| post:edit:accumulator | Edit\|Write\|MultiEdit | Edit accumulator | Không |
| post:edit:console-warn | Edit | console.log warning | Không |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit | Governance event capture | Không |
| post:session-activity-tracker | * | Session activity tracking | Không |
| post:observe:continuous-learning | * | Continuous learning observation | Có |
| post:ecc-metrics-bridge | * | Metrics bridge | Không |
| post:ecc-context-monitor | * | Context monitor | Không |
| post:mcp-health-check | * (PostToolUseFailure) | MCP health check | Không |

### Stop Hooks

| ID | Matcher | Chức năng | Timeout |
|----|---------|------|------|
| stop:format-typecheck | * | Batch format và type check | 300s |
| stop:check-console-log | * | console.log check | 30s |
| stop:session-end | * | Session state persistence | 10s |
| stop:evaluate-session | * | Session evaluation | 10s |
| stop:cost-tracker | * | Cost tracking | 10s |
| stop:desktop-notify | * | Desktop notification | 10s |

### Lifecycle Hooks

| ID | Event | Chức năng |
|----|-------|------|
| session:start | SessionStart | Load context và detect package manager |
| pre:compact | PreCompact | Pre-compact state save |
| session:end:marker | SessionEnd | Session end marker |