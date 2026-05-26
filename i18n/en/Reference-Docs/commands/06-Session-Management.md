# Session Management Commands

## Overview

Session management commands are used to save, restore, and manage Claude Code session state. These commands are essential for maintaining context during long sessions, handing off work between sessions, and tracking progress.

## Command List

### /save-session

**Purpose**: Save session state - write complete context of current session to date file

**Description**: Save all context from the current session to a dated file under `~/.claude/session-data/`, including: what was built, what worked, what failed, remaining work, file state, decisions made, and open blockers.

**Session File Naming Convention**:
- Format: `YYYY-MM-DD-<short-id>-session.tmp`
- Valid characters: letters `a-z`/`A-Z`, digits `0-9`, hyphen `-`, underscore `_`
- Minimum length: 1 character (not recommended to be too short)
- Recommended: 8+ characters of lowercase letters/digits/hyphens to avoid same-day conflicts

**When to Use**:
- When ending work (before session ends)
- Near context limit (save first, then start new session)
- After solving complex problems (save successful patterns)
- When handing off to future session

**Workflow**:
1. **Collect context** - Read all modified files, review discussion
2. **Create directory** - `mkdir -p ~/.claude/session-data`
3. **Write file** - Create session file with timestamp
4. **Show to user** - Display content and request confirmation

**Session File Format**:

```markdown
# Session: YYYY-MM-DD

**Started:** [approximate time if known]
**Last Updated:** [current time]
**Project:** [project name or path]
**Topic:** [one-line summary of what this session was about]

---

## What We Are Building
[1-3 paragraphs describing the feature, bug fix, or task]

## What WORKED (with evidence)
- **[thing that works]** — confirmed by: [specific evidence]

## What Did NOT Work (and why)
- **[approach tried]** — failed because: [exact reason / error message]

## What Has NOT Been Tried Yet
- [approach / idea]

## Current State of Files
| File              | Status         | Notes                      |
| ----------------- | -------------- | -------------------------- |
| `path/to/file.ts` | PASS: Complete | [what it does]             |

## Decisions Made
- **[decision]** — reason: [why this was chosen over alternatives]

## Blockers & Open Questions
- [blocker / open question]

## Exact Next Step
[The single most important thing to do when resuming]
```

**Key Principles**:
- "What Did NOT Work" section is most important - future sessions will blindly retry failed approaches
- Each session gets its own file - never append to previous sessions
- Files are read by next session via `/resume-session`

**Best Practices**:
- When saving mid-session (not at end), clearly mark in-progress items
- Include specific error messages and failure reasons ("threw X error because Y")
- Next specific step should be precise enough to know where to start without thinking

---

### /resume-session

**Purpose**: Restore saved session

**Description**: Load and restore previously saved session context from `~/.claude/session-data/`.

**Use Cases**:
- Starting new session
- Needing to continue previous work
- Needing to review previous problem-solving process

---

### /sessions

**Purpose**: Browse + search + manage session history

**Description**: Browse, search, and manage all saved session history.

**Features**:
- List all sessions
- Search by date
- Manage session aliases
- Clean up old sessions

---

### /checkpoint

**Purpose**: Create/validate workflow checkpoints

**Description**: Create checkpoints at key workflow milestones to ensure traceable progress.

**Use Cases**:
- Important steps completed
- Needing to validate key milestones
- Multi-phase tasks

---

### /aside

**Purpose**: Answer side questions without losing context

**Description**: Switch to lateral conversation mode, handle secondary issues, then seamlessly return to main task.

**Use Cases**:
- Needing to ask a quick question
- Needing to look up documentation
- Needing to perform auxiliary tasks

---

## Related Commands

- `/save-session` - Save current session
- `/resume-session` - Restore session
- `/sessions` - Manage session history