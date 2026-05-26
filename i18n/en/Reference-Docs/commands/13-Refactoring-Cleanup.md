# Refactoring and Cleanup Commands

## Overview

Refactoring and cleanup commands are used for code refactoring, dead code cleanup, and automatic updates.

## Command List

### /refactor-clean

**Purpose**: Delete dead code + merge duplicates

**Description**: Identify and delete unused code, merge duplicate code fragments.

**Analysis Includes**:
- Unused functions and variables
- Duplicate code blocks
- Outdated comments
- Unused imports

**Tool Support**:
- `knip` - Detect dead code
- `depcheck` - Detect unused dependencies
- `ts-prune` - TypeScript dead code detection

---

### /auto-update

**Purpose**: Automatic update capability

**Description**: Automatically update project dependencies and configuration to latest versions.

**Updates**:
- npm/pip package updates
- Configuration file migrations
- Code migrations
- Version compatibility checks

---

## Related Commands

- `/refactor-clean` - Refactor cleanup
- `/auto-update` - Auto update