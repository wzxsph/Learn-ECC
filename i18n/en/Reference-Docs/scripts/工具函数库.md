# Utility Functions Library

This document introduces the core utility function library in the `scripts/lib/` directory.

---

## utils.js

**Path**: `scripts/lib/utils.js`

Cross-platform utility functions, works on Windows, macOS, and Linux.

### Platform Detection

```javascript
const { isWindows, isMacOS, isLinux } = require('./utils');

// isWindows: true on Windows
// isMacOS: true on macOS
// isLinux: true on Linux
```

### Directory Operations

```javascript
const {
  getHomeDir,      // Get user home directory
  getClaudeDir,    // Get ~/.claude directory
  getSessionsDir,  // Get ~/.claude/session-data directory
  getTempDir,      // Get temp directory
  ensureDir,       // Ensure directory exists, create if not
  findFiles        // Find files matching pattern
} = require('./utils');

// Create directory (if not exists)
ensureDir('/path/to/dir');

// Find files
const files = findFiles('/path', '*.js', { recursive: true });
```

### Date and Time

```javascript
const {
  getDateString,      // Returns YYYY-MM-DD format
  getTimeString,      // Returns HH:MM format
  getDateTimeString   // Returns YYYY-MM-DD HH:MM:SS format
} = require('./utils');
```

### Session and Project

```javascript
const {
  sanitizeSessionId,    // Sanitize session ID, remove invalid characters
  getSessionIdShort,    // Get short session ID
  getGitRepoName,       // Get git repository name
  getProjectName        // Get project name
} = require('./utils');

// Sanitize session ID
const cleanId = sanitizeSessionId('my-session@123'); // 'my-session-123'

// Get project name
const project = getProjectName();
```

### File Operations

```javascript
const {
  readFile,        // Safely read file, returns null on failure
  writeFile,       // Write file
  appendFile,      // Append to file
  replaceInFile,   // Replace file content
  countInFile,     // Count pattern occurrences
  grepFile         // Search for pattern in file
} = require('./utils');

// Read file
const content = readFile('/path/to/file.txt');

// Replace file content (first match)
replaceInFile('/path', 'old', 'new');

// Replace all matches
replaceInFile('/path', 'old', 'new', { all: true });

// Count occurrences
const count = countInFile('/path', /pattern/g);

// Search and return matching lines
const matches = grepFile('/path', /regex/);
// Returns: [{ lineNumber: 1, content: '...' }]
```

### String Processing

```javascript
const { stripAnsi } = require('./utils');

// Remove ANSI escape sequences (colors, etc.)
const clean = stripAnsi('\x1b[32mHello\x1b[0m'); // 'Hello'
```

### Hook I/O

```javascript
const { readStdinJson, log, output } = require('./utils');

// Read JSON from stdin (for hooks)
const input = await readStdinJson({ timeoutMs: 5000 });

// Output to stderr (visible to user)
log('Debug message');

// Output to stdout (return to Claude)
output({ result: 'data' });
```

### System Commands

```javascript
const { commandExists, runCommand, isGitRepo, getGitModifiedFiles } = require('./utils');

// Check if command exists
if (commandExists('git')) { /* ... */ }

// Run command (whitelist only)
const result = runCommand('git rev-parse --show-toplevel');
if (result.success) {
  console.log(result.output);
}

// Check if git repository
if (isGitRepo()) { /* ... */ }

// Get modified files
const files = getGitModifiedFiles(['\\.js$', '\\.md$']);
```

---

## package-manager.js

**Path**: `scripts/lib/package-manager.js`

Package manager detection and selection utility.

### Supported Package Managers

- npm
- pnpm
- yarn
- bun

### Detection Priority

1. `CLAUDE_PACKAGE_MANAGER` environment variable
2. Project config `.claude/package-manager.json`
3. `packageManager` field in `package.json`
4. Lock file detection
5. Global user preference `~/.claude/package-manager.json`
6. Default to npm

### Core API

```javascript
const {
  getPackageManager,           // Get current project's package manager
  setPreferredPackageManager,  // Set global preference
  setProjectPackageManager,    // Set project preference
  getAvailablePackageManagers, // Get available package managers on system
  detectFromLockFile,          // Detect from lock file
  detectFromPackageJson,       // Detect from package.json
  getRunCommand,               // Get command to run script
  getExecCommand,              // Get command to execute binary
  getSelectionPrompt,          // Get interactive selection prompt
  getCommandPattern            // Generate command matching regex
} = require('./package-manager');

// Get current package manager
const { name, config, source } = getPackageManager();
// name: 'pnpm', 'npm', 'yarn', or 'bun'
// config: { installCmd, runCmd, execCmd, testCmd, ... }
// source: 'environment', 'project-config', 'lock-file', ...

// Set global preference
setPreferredPackageManager('pnpm');

// Set project preference
setProjectPackageManager('yarn', '/path/to/project');

// Get run command
const testCmd = getRunCommand('test'); // 'pnpm test' or 'npm test' etc.
const buildCmd = getRunCommand('build');

// Get exec command
const eslintCmd = getExecCommand('eslint', '--fix .');
```

### Security Checks

`getRunCommand` and `getExecCommand` validate input:
- Script names only allow letters, numbers, dash, underscore, dot, slash, @
- Arguments only allow letters, numbers, whitespace, dash, dots, slashes, equals, colons, commas, quotes, @

---

## session-manager.js

**Path**: `scripts/lib/session-manager.js`

Session management library providing CRUD operations for sessions.

### Session Storage

- New format: `~/.claude/session-data/YYYY-MM-DD-[session-id]-session.tmp`
- Old format compatible: `~/.claude/sessions/YYYY-MM-DD-session.tmp`

### Core API

```javascript
const {
  parseSessionFilename,      // Parse session filename
  getSessionPath,            // Get full session file path
  getSessionContent,         // Read session content
  parseSessionMetadata,      // Parse session metadata
  getSessionStats,           // Get session statistics
  getSessionTitle,           // Get session title
  getSessionSize,            // Get session size (human-readable format)
  getAllSessions,            // Get all sessions (with pagination and filtering)
  getSessionById,            // Get single session by ID
  writeSessionContent,       // Write session content
  appendSessionContent,      // Append session content
  deleteSession,             // Delete session
  sessionExists              // Check if session exists
} = require('./session-manager');

// Get all sessions
const { sessions, total, offset, limit, hasMore } = getAllSessions({
  limit: 50,
  offset: 0,
  date: '2026-05-26',
  search: 'abc123'
});

// Get single session
const session = getSessionById('abc123', true);
// session: { filename, shortId, date, sessionPath, content, metadata, stats, ... }

// Parse session metadata
const metadata = parseSessionMetadata(content);
// metadata: { title, date, started, lastUpdated, project, branch, worktree, completed, inProgress, notes, context }

// Get session statistics
const stats = getSessionStats(sessionPathOrContent);
// stats: { totalItems, completedItems, inProgressItems, lineCount, hasNotes, hasContext }

// Write session
writeSessionContent(sessionPath, '# New Session\n\nContent...');

// Append content
appendSessionContent(sessionPath, '\n\nMore content...');

// Delete session
deleteSession(sessionPath);
```

### Session Metadata Format

Metadata in session content:

```markdown
# Session Title

**Date:** 2026-05-26
**Started:** 14:30
**Last Updated:** 15:45
**Project:** my-project
**Branch:** main
**Worktree:** frontend

### Completed
- [x] Task 1
- [x] Task 2

### In Progress
- [ ] Task 3

### Notes for Next Session
Remember to check the config file.

### Context to Load
```
previous-context-content
```
```

---

## install-executor.js

**Path**: `scripts/lib/install-executor.js`

ECC installation executor, responsible for actually performing installation operations.

### Main Functions

- Create installation plan
- Execute file copy operations
- Handle installation state recording

### Core API

```javascript
const {
  applyInstallPlan,       // Apply installation plan
  listAvailableLanguages  // List available languages
} = require('./install-executor');

// List available languages
const languages = listAvailableLanguages();
// ['typescript', 'javascript', 'python', ...]
```

---

## install-lifecycle.js

**Path**: `scripts/lib/install-lifecycle.js`

Installation lifecycle management, handles various phases of installation.

---

## install-manifests.js

**Path**: `scripts/lib/install-manifests.js`

Installation manifest definitions, including:
- Supported installation targets
- Available language list
- Supported localization list

---

## install-state.js

**Path**: `scripts/lib/install-state.js`

Installation state management, records which files were installed where.

---

## project-detect.js

**Path**: `scripts/lib/project-detect.js`

Project type detection, detects project type of current directory.

---

## observer-sessions.js

**Path**: `scripts/lib/observer-sessions.js`

Session observer, used for monitoring session activity.

---

## shell-substitution.js

**Path**: `scripts/lib/shell-substitution.js`

Shell variable substitution utility, handles environment variables and shell expressions.

---

## tmux-worktree-orchestrator.js

**Path**: `scripts/lib/tmux-worktree-orchestrator.js`

Tmux + Git Worktree orchestrator, manages complex development environments.

---

## mcp-config.js

**Path**: `scripts/lib/mcp-config.js`

MCP (Model Context Protocol) configuration management.

---

## Other Utilities

| File | Description |
|------|------|
| `agent-compress.js` | Agent context compression |
| `cost-estimate.js` | Cost estimation |
| `github-discussions.js` | GitHub discussions integration |
| `hook-flags.js` | Hook flags management |
| `inspection.js` | Inspection tools |
| `orchestration-session.js` | Orchestration session management |
| `resolve-ecc-root.js` | ECC root directory resolution |
| `resolve-formatter.js` | Formatter resolution |
| `session-aliases.js` | Session alias management |
| `session-bridge.js` | Session bridge |
| `shell-split.js` | Shell command splitting |
| `state-store/` | State storage submodule |
| `install/` | Installation-related submodule |
| `skill-evolution/` | Skill evolution submodule |
| `skill-improvement/` | Skill improvement submodule |