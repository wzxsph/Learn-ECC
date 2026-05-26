# Loops and Automation Commands

## Overview

Loops and automation commands are used to start and manage autonomous loop work modes.

## Command List

### /loop-start

**Purpose**: Start managed autonomous loop mode with safe defaults and clear stop conditions

**Description**: Start a managed autonomous loop mode with safe default settings and clear stop conditions.

**Usage**: `/loop-start [pattern] [--mode safe|fast]`

**Parameters**:

| Parameter | Value | Description |
|---|---|---|
| `pattern` | `sequential` | Execute tasks sequentially, one after another |
| | `continuous-pr` | Continuous PR creation and merge loop |
| | `rfc-dag` | RFC-based directed acyclic graph workflow |
| | `infinite` | Infinite loop (requires explicit stop condition) |
| `--mode` | `safe` (default) | Strict quality gates and checkpoints |
| | `fast` | Reduced gates for speed |

**Workflow**:
1. Confirm repository state and branch strategy
2. Select loop pattern and related model tier strategy
3. Enable required hooks/profile for selected pattern
4. Create loop plan and write runbook under `.claude/plans/`
5. Print startup and monitoring commands

**Required Safety Checks**:
- Verify tests pass before first loop iteration
- Ensure `ECC_HOOK_PROFILE` is not globally disabled
- Ensure loop has clear stop condition

**Parameter Passing**:
- `$ARGUMENTS`: `[pattern]` and optional `[--mode safe|fast]`
- Pattern is optional (defaults to sequential)
- Mode flag is optional (defaults to safe)

**Best Practices**:
- Ensure clear stop conditions before using infinite mode
- Only use fast mode on well-tested code
- Monitor loop progress using `/loop-status` to check status

**Common Pitfalls**:
- Starting loop without verifying tests
- Using infinite mode without clear stop condition
- Trying to use safe mode when hooks are globally disabled

**Integration with Other Commands**:
- Use `/loop-status` to monitor running loop
- Use `/santa-loop` for Santa-style loop
- Use `/checkpoint` to create key milestone checkpoints

---

### /loop-status

**Purpose**: Check status of running loop

**Description**: View current running loop status and progress.

---

### /santa-loop

**Purpose**: Santa-style autonomous loop

**Description**: A special style of autonomous loop mode.

---

## Related Commands

- `/loop-start` - Start loop
- `/loop-status` - Check status
- `/santa-loop` - Santa-style