# 02 - Installation & Configuration

## Environment Requirements

- Node.js >= 18
- npm / pnpm / yarn / bun (one of these)
- Claude Code CLI v2.1.0+ installed

## Installation Methods

### Method 1: Plugin Installation (Recommended)

#### 1. Add Plugin Marketplace

```bash
/plugin marketplace add https://github.com/affaan-m/ECC
```

#### 2. Install Plugin

```bash
/plugin install ecc@ecc
```

#### 3. Install Rules (Plugin does not auto-install)

```bash
# Clone repository
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Copy rules to user directory
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
# Choose your tech stack
cp -r rules/typescript ~/.claude/rules/ecc/   # or python, golang, swift, php
```

### Method 2: Manual Installation

If you want finer control over installation:

```bash
# Clone repository
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Install dependencies
npm install        # or pnpm install | yarn install | bun install

# Install agents
cp agents/*.md ~/.claude/agents/

# Install skills (primary workflow interface)
mkdir -p ~/.claude/skills/ecc
cp -r .agents/skills/* ~/.claude/skills/ecc/

# Install rules
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
cp -r rules/typescript ~/.claude/rules/ecc/

# Install commands (optional, ECC is migrating to skills)
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```

## Verify Installation

```bash
# Check if plugin installed successfully
/plugin list ecc@ecc

# Check available commands
/ecc:plan
/ecc:code-review
```

## Basic Configuration

### Set Package Manager

ECC auto-detects your preferred package manager, or you can set it manually:

```bash
# Environment variable method
export CLAUDE_PACKAGE_MANAGER=pnpm

# Or use command
/setup-pm
```

Priority: Environment variable > Project config > package.json > lock file > Global config

### Hook Runtime Control

```bash
# Hook strictness level (default: standard)
export ECC_HOOK_PROFILE=standard

# Disable specific Hooks
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# SessionStart context limit (default: 8000 chars)
export ECC_SESSION_START_MAX_CHARS=4000
```

### Model Settings

Recommended to set in `~/.claude/settings.json`:

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50"
  }
}
```

## Next Steps

- [Basic Commands](./03-Basic-Commands.md) - Learn common commands
- [Return to Getting Started README](../README.md)