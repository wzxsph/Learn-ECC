# Hookify System Commands

## Overview

The Hookify system is used to create and manage hooks to prevent undesirable behaviors and automate workflows.

## Command List

### /hookify

**Purpose**: Create hooks to prevent undesirable behaviors

**Description**: Create custom hooks to prevent unwanted behaviors.

**Use Cases**:
- Prevent accidental delete operations
- Block dangerous commands
- Auto-format
- Quality checks

**Hook Types**:
- **PreToolUse**: Before tool execution
- **PostToolUse**: After tool execution
- **Stop**: When session ends

---

### /hookify-list

**Purpose**: List all configured hookify rules

**Description**: Display all currently configured hookify rules.

---

### /hookify-configure

**Purpose**: Interactively enable/disable hookify rules

**Description**: Interactively configure enabled/disabled state of hookify rules.

---

### /hookify-help

**Purpose**: Hookify system help

**Description**: Get detailed help information for the Hookify system.

---

## Hook Configuration Example

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## Related Commands

- `/hookify` - Create hook
- `/hookify-list` - List rules
- `/hookify-configure` - Configure rules
- `/hookify-help` - Get help