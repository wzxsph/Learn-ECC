# 01 - [ECC](../Reference-Docs/README.md) Introduction

## What is [ECC](../Reference-Docs/README.md)?

[ECC](../Reference-Docs/README.md) (Everything Claude Code) is a Claude Code plugin system providing production-grade [Agents](../Reference-Docs/agents/1.%20代码审查类.md), [Skills](../Reference-Docs/skills/最佳实践.md), hooks, rules, [Commands](../Reference-Docs/commands/01-核心工作流.md), and MCP configuration solutions. Originated from an Anthropic Hackathon winning project, refined through 10+ months of daily use.

## What Can [ECC](../Reference-Docs/README.md) Do?

### 1. [Agent](../Reference-Docs/agents/1.%20代码审查类.md) System
[ECC](../Reference-Docs/README.md) provides 60+ professional [Agents](../Reference-Docs/agents/1.%20代码审查类.md) covering multi-language review, build fixes, architecture design, and more:

| [Agent](../Reference-Docs/agents/1.%20代码审查类.md) | Purpose |
|-------|---------|
| `planner` | Feature implementation planning |
| `code-reviewer` | Code quality and security review |
| `security-reviewer` | Security vulnerability analysis |
| `build-error-resolver` | Build error fixes |
| `architect` | System architecture design |
| `tdd-guide` | Test-driven development guidance |

### 2. [Skills](../Reference-Docs/skills/最佳实践.md) Workflows
[Skills](../Reference-Docs/skills/最佳实践.md) are the primary workflow interface. [ECC](../Reference-Docs/README.md) provides 232+ [Skills](../Reference-Docs/skills/最佳实践.md):

- `tdd-workflow` - Test-driven development
- `e2e-testing` - End-to-end testing
- `security-review` - Security review checklist
- `continuous-learning-v2` - Instinct-based continuous learning
- `eval-harness` - Verification loop evaluation

### 3. [Commands](../Reference-Docs/commands/01-核心工作流.md)
Preserves traditional slash command compatibility (75+ commands):

| Command | Function |
|---------|----------|
| `/plan` | Create implementation plan |
| `/code-review` | Code review |
| `/build-fix` | Fix build errors |
| `/learn` | Extract patterns from sessions |
| `/skill-create` | Generate Skill from Git history |
| `/sessions` | Session history management |

### 4. Hooks Automation
Flexible hook system (8 event types):
- `SessionStart` - Load context when session starts
- `SessionEnd` - Save state when session ends
- `PreToolUse` - Validate before tool execution
- `PostToolUse` - Format after tool execution

### 5. Rules System
Comprehensive development standards (common + language-specific):
- **common/** - General principles (security, coding style, testing, performance)
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. MCP Services
Supports 14+ MCP server configurations: GitHub, Supabase, Vercel, Railway, Context7, Exa, Playwright, and more.

## Cross-Platform Support

[ECC](../Reference-Docs/README.md) supports multiple AI programming tools:

| Tool | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 types |
| Cursor | 48 | Shared | Shared | 15 types |
| Codex | Shared | Command-style | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 types |
| GitHub Copilot | - | 6 prompts | Command-style | - |

## Core Concepts

### [Agent](../Reference-Docs/agents/1.%20代码审查类.md)
Reusable subtask executors, each [Agent](../Reference-Docs/agents/1.%20代码审查类.md) has clear responsibilities. Build complex workflows by combining multiple [Agents](../Reference-Docs/agents/1.%20代码审查类.md).

### [Skill](../Reference-Docs/skills/最佳实践.md)
Defines workflows for completing specific tasks, is the primary workflow interface for [ECC](../Reference-Docs/README.md).

### Hook
Scripts that automatically trigger at specific points, supporting workflow automation.

### Rule
Constraint rules that Claude Code must follow, ensuring output quality.


- [Installation & Configuration](./02-Installation.md) - Get started with [ECC](../Reference-Docs/README.md)
- [Return to Getting Started README](../README.md)