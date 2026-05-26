# Build Scripts

This document introduces the build and release scripts of the ECC project.

---

## release.sh

**Path**: `scripts/release.sh`

Plugin version upgrade and release script.

### Features

1. **Version Validation**
   - Confirm semver format version number provided (X.Y.Z or X.Y.Z-prerelease)
   - Confirm current branch is main
   - Confirm working tree is clean

2. **File Updates**
   - Update version numbers in all package/plugin manifests
   - Update version references in documentation
   - Update agent configurations

3. **Build Validation**
   - Run OpenCode build
   - Run build tests
   - Run plugin manifest tests

4. **Git Operations**
   - Stage all changes
   - Create commit
   - Create and push tag

### Usage

```bash
# Release new version
./scripts/release.sh 1.5.0

# Pre-release version
./scripts/release.sh 2.0.0-rc.1
```

### Updated Files

| File | Update Content |
|------|----------------|
| `package.json` | version field |
| `package-lock.json` | version and packages[""].version |
| `AGENTS.md` | Version line |
| `docs/tr/AGENTS.md` | Sürüm line |
| `docs/zh-CN/AGENTS.md` | 版本 line |
| `agent.yaml` | version field |
| `VERSION` | version file |
| `.claude-plugin/plugin.json` | version field |
| `.claude-plugin/marketplace.json` | version field |
| `.agents/plugins/marketplace.json` | ecc plugin version |
| `.codex-plugin/plugin.json` | version field |
| `.opencode/package.json` | version field |
| `.opencode/package-lock.json` | version and packages[""].version |
| `.opencode/plugins/ecc-hooks.ts` | banner version |
| `README.md` | version table row |
| `README.zh-CN.md` | version table row |
| `docs/tr/README.md` | latest release heading |
| `docs/pt-BR/README.md` | latest release heading |
| `docs/zh-CN/README.md` | latest release heading |
| `docs/SELECTIVE-INSTALL-ARCHITECTURE.md` | repoVersion example |

### Prerequisites

- Git working tree must be clean
- Must be on main branch
- All manifest files must exist

---

## build-opencode.js

**Path**: `scripts/build-opencode.js`

Build script for OpenCode plugin TypeScript code.

### Build Process

1. Clean `dist` directory
2. Find TypeScript compiler (`typescript/bin/tsc`)
3. Compile `.opencode` directory using tsconfig.json from project root

### Usage

```bash
# Build OpenCode
node scripts/build-opencode.js

# Automatically run as part of release
./scripts/release.sh 1.5.0
```

### Prerequisites

Requires development dependencies installed in root directory, ensuring TypeScript compiler is available.

---

## Other Scripts

### gan-harness.sh

**Path**: `scripts/gan-harness.sh`

GAN (General Agent Network) test tool related.

### orchestrate-codex-worker.sh

**Path**: `scripts/orchestrate-codex-worker.sh`

Orchestrate Codex worker processes.

### sync-ecc-to-codex.sh

**Path**: `scripts/sync-ecc-to-codex.sh`

Sync ECC to Codex.

---

## Release Workflow

```
1. Ensure on main branch with clean working tree
2. Run release script with version number
3. Script automatically completes:
   ├── Validate prerequisites
   ├── Update all version references
   ├── Build OpenCode
   ├── Run tests for validation
   ├── Create Git commit
   └── Push tag
4. GitHub Actions auto-publish when tag detected
```
