# 04 - Skills Development

> Learn to create and use Skills, sediment workflow patterns into reusable skills

## Module Objectives

After completing this module, you will be able to:

- Understand standard Skill format and structure
- Use `/learn` to extract patterns from sessions
- Use `/skill-create` to create Skills from git history
- Create and manage custom Skills

## Skill Format

Each Skill is a Markdown file with standardized sections:

```markdown
# Skill Name

## When to Use
Describe when to use this Skill

## How It Works
Detailed explanation of how the Skill works

## Examples
Usage examples

## Related Skills
Related Skills
```

### Required Sections

| Section | Description | Required |
|---------|-------------|----------|
| When to Use | Usage scenario description | Yes |
| How It Works | Working principle description | Yes |
| Examples | Usage examples | Yes |

### Optional Sections

| Section | Description |
|---------|-------------|
| Related Skills | Related Skills |
| Prerequisites | Prerequisites |
| Tips | Usage tips |

## Skill Types

### 1. Workflow Skill

Describes a complete workflow:

```markdown
# TDD Workflow

## When to Use
Use TDD methodology when developing new features or fixing bugs

## How It Works
1. Write tests (Red)
2. Implement functionality (Green)
3. Refactor (Blue)

## Examples
/step 1: Write failing tests
/step 2: Implement minimal code
/step 3: Run tests to verify
/step 4: Refactor
```

### 2. Pattern Skill

Sediments code patterns:

```markdown
# API Response Pattern

## When to Use
Use when designing API response formats

## How It Works
Use unified response format:
- success: Success status
- data: Response data
- error: Error message (optional)

## Examples
{ "success": true, "data": {...}, "error": null }
```

### 3. Best Practices Skill

Records best practices:

```markdown
# Git Commit Best Practices

## When to Use
Check commit messages before committing code

## How It Works
Follow Conventional Commits format:
- feat: New feature
- fix: Fix
- docs: Documentation
- test: Test
- refactor: Refactor

## Examples
feat: add user authentication
fix: resolve login bug
docs: update README
```

## Ways to Create Skills

### Method 1: Use `/learn`

Extract patterns from successful sessions:

```
/learn "Extract workflow patterns from this session"
```

### Method 2: Use `/skill-create`

Create Skill from git history:

```
/skill-create "Fix build error workflow"
```

### Method 3: Manual Creation

Write Skill file directly:

```markdown
# My Custom Skill

## When to Use
...

## How It Works
...
```

## Skill Storage Locations

| Type | Location | Description |
|------|----------|-------------|
| Official Skills | `~/.claude/skills/` | Claude Code built-in |
| Custom Skills | Project `skills/` | Project-specific |
| Rule Skills | `~/.claude/rules/` | Global rules |

## Skill Management Commands

| Command | Function |
|---------|----------|
| `/skill-scout` | Search available Skills |
| `/skill-stocktake` | List all Skills |
| `/skill-create` | Create new Skill |
| `/skill-health` | Check Skill health status |

## Using Skills

### Direct Usage

```
/skill-name
```

### In Tasks

Reference Skill in complex tasks:

```
I need to develop a payment module using TDD workflow
```

## Skill Best Practices

1. **Single Responsibility**: Each Skill does one thing
2. **Clear Naming**: Use descriptive names
3. **Complete Documentation**: Include all required sections
4. **Verified in Practice**: Based on real workflows
5. **Continuously Iterate**: Improve based on usage feedback

## Reference Materials

Complete Skills documentation is located at: `../../Reference-Docs/skills/Best-Practices.md`

## Next Steps

- Learn [exercises](./exercises/Getting-Started-Exercises.md)
- Read [complete Skills documentation](../../Reference-Docs/skills/Best-Practices.md)