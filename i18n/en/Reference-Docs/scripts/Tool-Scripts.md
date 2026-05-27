# Utility Scripts

This document introduces the main utility scripts in the ECC (Everything Claude Code) project.

## Table of Contents

- [ecc.js](#eccjs) - Main CLI entry point
- [install-apply.js](#install-applyjs) - Installation executor
- [claw.js](#clawjs) - NanoClaw REPL
- [harness-audit.js](#harness-auditjs) - Toolchain audit
- [catalog.js](#catalogjs) - Component catalog viewer
- [build-opencode.js](#build-opencodejs) - OpenCode build

---

## ecc.js

**Path**: `scripts/ecc.js`

ECC selective installation CLI main entry point, unified dispatcher for all subcommands.

### Command List

| Command | Script | Description |
|---------|--------|-------------|
| `install` | install-apply.js | Install ECC content to target |
| `plan` | install-plan.js | Check installation manifest and parse plan |
| `catalog` | catalog.js | Discover installation config files and component IDs |
| `consult` | consult.js | Recommend components via natural language query |
| `list-installed` | list-installed.js | Check installation status of current context |
| `doctor` | doctor.js | Diagnose missing or drifted ECC management files |
| `repair` | repair.js | Restore drifted or missing ECC management files |
| `auto-update` | auto-update.js | Pull latest ECC changes and reinstall |
| `status` | status.js | Query SQLite state store summary |
| `platform-audit` | platform-audit.js | Audit GitHub queue, discussions, roadmap, etc. |
| `security-ioc-scan` | ci/scan-supply-chain-iocs.js | Scan supply chain IOCs for dependency and AI tool persistence |
| `sessions` | sessions-cli.js | List or inspect sessions in SQLite state store |
| `work-items` | work-items.js | Track linked Linear, GitHub work items |
| `session-inspect` | session-inspect.js | Emit canonical session snapshot from dmux or Claude history target |
| `loop-status` | loop-status.js | Check stale loop wakeups and pending tool results in Claude scripts |
| `uninstall` | uninstall.js | Delete ECC management files recorded in install-state |

### Usage Examples

```bash
# Direct language installation
ecc typescript

# Installation with config file
ecc install --profile developer --target claude

# View installation plan
ecc plan --profile core --target cursor

# List available components
ecc catalog profiles
ecc catalog components --family language

# Show component details
ecc catalog show framework:nextjs

# Consult recommendations
ecc consult "security reviews"

# Check installed
ecc list-installed --json

# Diagnose issues
ecc doctor --target cursor
ecc repair --dry-run

# Auto update
ecc auto-update --dry-run

# Status query
ecc status --json
ecc status --exit-code
ecc status --markdown --write status.md

# Platform audit
ecc platform-audit --json --allow-untracked docs/drafts/

# Security scan
ecc security-ioc-scan --home

# Session management
ecc sessions
ecc sessions session-active --json

# Work item tracking
ecc work-items upsert linear-ecc-20 --source linear --source-id ECC-20 --title "Review control-plane contract" --status blocked
ecc work-items sync-github --repo affaan-m/ECC

# Check loop status
ecc loop-status --json

# Uninstall
ecc uninstall --target antigravity --dry-run
```

---

## install-apply.js

**Path**: `scripts/install-apply.js`

ECC installation runtime, keeping traditional language installation entry points intact while moving target-specific change logic into testable Node code.

### Supported Targets

| Target | Description |
|--------|-------------|
| `claude` | Install to ~/.claude/, rules/skills go to rules/ecc and skills/ecc |
| `claude-project` | Install to ./.claude/ (project-level) |
| `cursor` | Install rules, hooks, cursor config to ./.cursor/ |
| `antigravity` | Install rules, workflows, skills, agents to ./.agent/ |
| `codex` | Install shared agents/config to ~/.codex/ |
| `gemini` | Install project-level Gemini config to ./.gemini/ |
| `opencode` | Install shared commands/hooks/config to ~/.opencode/ |
| `codebuddy` | Install commands, agents, skills to ./.codebuddy/ |
| `joycode` | Install commands, agents, skills to ./.joycode/ |
| `qwen` | Install to ~/.qwen/ |
| `zed` | Install to ./.zed/ |

### Installation Modes

1. **Traditional Language Mode**: `install.sh <language>` (e.g., `ecc-install typescript`)
2. **Profile Mode**: `install.sh --profile <name> [--with <component>]... [--without <component>]...`
3. **Module Mode**: `install.sh --modules <id,id,...>`
4. **Skill Mode**: `install.sh --skills <skill-id[,skill-id...]>`
5. **Localization Mode**: `install.sh --target claude|claude-project --locale <locale-code>`

### Options

- `--dry-run` - Show installation plan without copying files
- `--json` - Emit machine-readable JSON format
- `--config <path>` - Load installation intent from file

---

## claw.js

**Path**: `scripts/claw.js`

NanoClaw v2 - Zero-dependency, session-aware REPL for Everything Claude Code, built on `claude -p`.

### Features

- Session history persisted in `~/.claude/claw/`
- Support for dynamic skill loading as context
- Session compression for context length management
- Session branching and export
- Cross-session search

### REPL Commands

| Command | Description |
|---------|-------------|
| `/help` | Show help |
| `/clear` | Clear current session history |
| `/history` | Print full conversation history |
| `/sessions` | List saved sessions |
| `/model [name]` | Show/set model |
| `/load <skill-name>` | Load skill into active context |
| `/branch <session-name>` | Branch current session to new session |
| `/search <query>` | Cross-session search |
| `/compact` | Keep recent rounds, compress old context |
| `/export <md|json|txt> [path]` | Export session |
| `/metrics` | Show session metrics |
| `exit` | Exit REPL |

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `CLAW_SESSION` | `default` | Initial session name |
| `CLAW_MODEL` | `sonnet` | Default model |
| `CLAW_SKILLS` | empty | List of skills to load (comma-separated) |

### Usage Examples

```bash
# Start with default session
node scripts/claw.js

# Start with specified session
CLAW_SESSION=my-session node scripts/claw.js

# Start with skills loaded
CLAW_SKILLS="javascript-patterns,python-testing" node scripts/claw.js
```

---

## harness-audit.js

**Path**: `scripts/harness-audit.js`

Deterministic toolchain audit tool, based on explicit file/rule checks. Automatically detects ECC repository mode vs consumer project mode.

### Audit Categories

| Category | Description |
|----------|-------------|
| Tool Coverage | Tool coverage completeness |
| Context Efficiency | Context efficiency |
| Quality Gates | Quality gates |
| Memory Persistence | Memory persistence |
| Eval Coverage | Evaluation coverage |
| Security Guardrails | Security guardrails |
| Cost Efficiency | Cost efficiency |
| GitHub Integration | GitHub integration |
| Vercel/Netlify/Cloudflare/Fly Integration | Platform integrations |

### Usage Examples

```bash
# Audit current directory
node scripts/harness-audit.js

# Specify scope
node scripts/harness-audit.js --scope repo
node scripts/harness-audit.js --scope hooks
node scripts/harness-audit.js --scope skills

# Specify root directory
node scripts/harness-audit.js --root /path/to/project

# JSON format output
node scripts/harness-audit.js --format json

# Combined usage
node scripts/harness-audit.js repo --format json --root .
```

---

## catalog.js

**Path**: `scripts/catalog.js`

Tool to discover ECC installation components and configuration files.

### Commands

#### profiles

List all installation profiles.

```bash
node scripts/catalog.js profiles
node scripts/catalog.js profiles --json
```

#### components

List installation components, filterable by family and target.

```bash
node scripts/catalog.js components
node scripts/catalog.js components --family language
node scripts/catalog.js components --family framework
node scripts/catalog.js components --target claude
node scripts/catalog.js components --json
```

#### show

Show detailed information for a specific component.

```bash
node scripts/catalog.js show <component-id>
node scripts/catalog.js show framework:nextjs --json
```

### Component Families

- `baseline` - Baseline components
- `language` - Language components (javascript, typescript, python, etc.)
- `framework` - Framework components (nextjs, react, vue, etc.)
- `capability` - Capability components
- `agent` - Agent components
- `skill` - Skill components

---

## build-opencode.js

**Path**: `scripts/build-opencode.js`

Build script for OpenCode plugin TypeScript code.

### Features

1. Clean `dist` directory
2. Compile code in `.opencode` directory using TypeScript compiler
3. Output to `.opencode/dist`

### Prerequisites

Requires development dependencies installed in root directory, ensuring TypeScript compiler is available.

### Usage

```bash
node scripts/build-opencode.js
```