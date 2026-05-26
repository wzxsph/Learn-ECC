# 01 - Command System Mastery

> Master Claude Code's 75+ commands, flexibly combine them for efficient development

## Module Objectives

After completing this module, you will be able to:

- Identify and use commands under all command categories
- Select the most appropriate command for each scenario
- Combine multiple commands to create custom workflows
- Use command documentation to quickly look up command usage

## Command Overview

Claude Code provides 75+ commands covering the following categories:

| Category | Commands | Example Commands |
|----------|----------|------------------|
| Development Workflow | 15+ | `/plan`, `/tdd`, `/build-fix`, `/verify` |
| Code Review | 10+ | `/code-review`, `/security-review`, `/python-review` |
| Build & Test | 15+ | `/go-build`, `/rust-build`, `/flutter-build` |
| Project Management | 10+ | `/project-init`, `/projects`, `/santa-loop` |
| Skills Management | 10+ | `/skill-create`, `/skill-stocktake`, `/skill-scout` |
| Learning & Research | 10+ | `/learn`, `/learn-eval`, `/deep-research` |
| Utility Tools | 15+ | `/config`, `/jira`, `/pr`, `/e2e` |

## Command Documentation Structure

Each command has a standard documentation structure:

```
Command Name
├── Command Description
├── Usage Scenarios
├── Parameters
├── Usage Examples
└── Notes
```

## Quick Command Reference

### Development Workflow
- `/plan` - Create implementation plan
- `/tdd` - Test-driven development
- `/build-fix` - Fix build errors
- `/verify` - Verify code changes
- `/review` - Code review

### Build Related
- `/go-build`, `/rust-build`, `/kotlin-build` - Various language builds
- `/cpp-build`, `/flutter-build` - Compiled language builds
- `/docker-build` - Docker image build

### Project Management
- `/project-init` - Initialize new project
- `/projects` - List all projects
- `/santa-loop` - Automated task loop

### Skills Development
- `/skill-create` - Create Skill from git history
- `/skill-scout` - Search available Skills
- `/learn` - Extract patterns from session

## How to Use Command Documentation

The complete accompanying command documentation is located at: `../../Reference-Docs/commands/01-Core-Workflow.md`

Documentation includes:
- Detailed explanation of each command
- Parameters and options
- Usage examples and output
- Best practice suggestions

## Combining Commands

Commands can be combined for complex workflows:

```
/plan → /tdd → /build-fix → /code-review → /verify
```

Output from each command can be passed to the next command, forming an automated pipeline.

## Next Steps

- Learn [exercises](./exercises/Getting-Started-Exercises.md)
- Read [complete command documentation](../../Reference-Docs/commands/01-Core-Workflow.md)