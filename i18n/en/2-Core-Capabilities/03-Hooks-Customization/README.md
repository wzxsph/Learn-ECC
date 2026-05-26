# 03 - Hooks Customization

> Learn to write PreToolUse, PostToolUse, and Stop Hooks for automated quality gates

## Module Objectives

After completing this module, you will be able to:

- Understand Claude Code's Hook types and triggering mechanisms
- Create PreToolUse Hooks for pre-tool invocation validation
- Create PostToolUse Hooks for post-tool invocation processing
- Configure quality gate Hooks for automated checks

## Hook Types Overview

| Hook Type | Trigger Timing | Typical Uses |
|-----------|----------------|--------------|
| **PreToolUse** | Before tool invocation | Parameter validation, permission checks, security scans |
| **PostToolUse** | After tool invocation | Result formatting, logging, automated processing |
| **Stop** | When session ends | Session summary, state saving, cleanup |

## PreToolUse Hook

### How It Works

```
User input → PreToolUse Hook → Tool execution → Return result
```

### Common Scenarios

1. **Security validation**
   - Check dangerous commands (like `rm -rf`)
   - Validate file path safety
   - Check sensitive data access

2. **Parameter validation**
   - Validate tool parameter format
   - Check required parameters exist
   - Type checking

3. **Permission control**
   - Check if operation is authorized
   - Verify project access permissions

### Example Hook Configuration

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('rm -rf')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "Warning: Dangerous command rm -rf detected, please confirm before executing!"
    }
  ]
}
```

## PostToolUse Hook

### How It Works

```
Tool execution → PostToolUse Hook → Return result → User
```

### Common Scenarios

1. **Result formatting**
   - Beautify output format
   - Extract key information
   - Simplify long output

2. **Automated processing**
   - Run tests automatically after successful build
   - Automatically log after test failure
   - Auto-format code before commit

3. **Quality checks**
   - Check returned error messages
   - Verify output security
   - Analyze performance data

### Example Hook Configuration

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm test')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Test complete: Checking coverage...'",
      "executeAfter": true
    }
  ]
}
```

## Stop Hook

### How It Works

```
Session end signal → Stop Hook → Execute cleanup → Session terminates
```

### Common Scenarios

1. **Session summary**
   - Generate session summary
   - Record completed tasks
   - List todo items

2. **State saving**
   - Save draft files
   - Persist progress
   - Clean up temporary files

3. **Notification sending**
   - Send completion notification
   - Summary report
   - Trigger next process

## Hook Configuration Structure

```json
{
  "matcher": {
    "tool": "Tool name",
    "operation": "Operation type",
    "condition": "Condition expression"
  },
  "hooks": [
    {
      "type": "notification | command | stop",
      "when": "before | after",
      "message": "Notification message",
      "command": "Command to execute",
      "continue": true
    }
  ]
}
```

## Common Hook Scenarios

### Scenario 1: Pre-Git-Commit Checks

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('git commit')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "npm test",
      "continue": false
    }
  ]
}
```

### Scenario 2: Build Success Notification

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm run build')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "Build complete! Running tests...",
      "executeAfter": true
    }
  ]
}
```

### Scenario 3: Session End Summary

```json
{
  "matcher": {
    "type": "stop"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "Session ended. Completed: {summary}",
      "command": "node scripts/save-session.js"
    }
  ]
}
```

## Hook Development Best Practices

1. **Keep it simple**: Hook logic should be simple and direct
2. **Fast execution**: PreToolUse Hooks should complete within 200ms
3. **Graceful degradation**: Should continue execution on failure, not block tools
4. **Clear logging**: Use `[HookName]` prefix to mark logs
5. **Test coverage**: Write tests for Hooks

## Reference Materials

Complete Hooks documentation is located at: `../../Reference-Docs/hooks/Hook类型.md`

## Next Steps

- Learn [exercises](./exercises/练习.md)
- Read [complete Hooks documentation](../../Reference-Docs/hooks/Hook类型.md)