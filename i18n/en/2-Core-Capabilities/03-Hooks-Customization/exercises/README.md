# Hooks Customization Exercises

## Exercise Objectives

Master creating, configuring, and debugging Claude Code hooks.

## Exercise Content

### Exercise 1: Create PreToolUse Hook
Implement hook functionality:
- Log invocation parameters before tool execution
- Support whitelist/blacklist filtering

### Exercise 2: Create PostToolUse Hook
Implement hook functionality:
- Format output after tool execution
- Record execution time

### Exercise 3: Configure Async Hooks
Create async hooks for:
- External API calls
- File system monitoring

## Hook Template

```json
{
  "name": "my-hook",
  "matcher": {
    "tool": "Bash",
    "path": "src/**/*.js"
  },
  "hooks": [
    {
      "type": "PostToolUse",
      "command": "echo 'Tool executed: ${tool_name}'"
    }
  ]
}
```

## Verification
1. Trigger condition scenarios
2. Check hook output
3. Verify logging records