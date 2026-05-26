# Project Management Commands

## Overview

Project management commands are used for project configuration, infrastructure setup, and toolchain management.

## Command List

### /projects

**Purpose**: List known projects + instinct statistics

**Description**: List all user's known projects and their related instinct statistics.

---

### /harness-audit

**Purpose**: Audit agent harness configuration

**Description**: Check and audit Claude Code harness configuration.

**Checks**:
- Toolchain configuration
- Agent settings
- MCP server status
- Rules configuration

---

### /model-route

**Purpose**: Route tasks to appropriate models

**Description**: Select most suitable model (Haiku/Sonnet/Opus) based on task type.

**Model Selection Guide**:
- **Haiku**: Lightweight tasks, code generation, frequent invocations
- **Sonnet**: Main development work, complex coding tasks
- **Opus**: Architecture decisions, deep reasoning, research analysis

---

### /pm2

**Purpose**: PM2 process manager initialization

**Description**: Initialize PM2 process manager configuration.

---

### /setup-pm

**Purpose**: Configure package manager

**Description**: Configure package manager used by project.

**Supported Package Managers**:
- npm
- pnpm
- yarn
- bun

**Can be configured via** `CLAUDE_PACKAGE_MANAGER` **environment variable**

---

### /project-init

**Purpose**: Project initialization

**Description**: Initialize standard structure and configuration for new projects.

---

## Related Commands

- `/projects` - List projects
- `/harness-audit` - Audit configuration
- `/model-route` - Model routing