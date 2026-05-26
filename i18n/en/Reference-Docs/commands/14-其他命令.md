# Other Commands

## Overview

Other miscellaneous commands including Jira integration, GAN operations, security scanning, and more.

## Command List

### /jira

**Purpose**: Interact with Jira tickets - get, analyze, comment, transition status, search

**Description**: Integrate with Jira project management tool, support working with Jira tickets directly in workflows.

**Usage**:

| Command | Description |
|---|---|
| `/jira get <TICKET-KEY>` | Get and analyze Jira ticket |
| `/jira comment <TICKET-KEY>` | Add progress comment |
| `/jira transition <TICKET-KEY>` | Change ticket status |
| `/jira search <JQL>` | Search issues using JQL |

**`/jira get` Workflow**:
1. Fetch ticket from Jira (via MCP or REST API)
2. Extract all fields: summary, description, acceptance criteria, priority, labels, linked issues
3. Optionally fetch comments for additional context
4. Generate structured analysis

**Prerequisites** - Jira credentials configuration (choose one):

**Option A — MCP Server (recommended)**:
Add `jira` to `mcpServers` configuration.

**Option B — Environment Variables**:
```bash
export JIRA_URL="https://yourorg.atlassian.net"
export JIRA_EMAIL="your.email@example.com"
export JIRA_API_TOKEN="your-api-token"
```

**Integration with Other Commands**:
- Use `/plan` to create implementation plan after analyzing tickets
- Use `tdd-workflow` skill for test-driven development
- Use `/code-review` to review implementation
- Use `/jira comment` or `/jira transition` to update ticket status

---

### /gan-build (To be completed)

**Purpose**: GAN build operations

**Description**: GAN (Generative Agent Network) build operations.

---

### /gan-design (To be completed)

**Purpose**: GAN design operations

**Description**: GAN design-related operations.

---

### /prune

**Purpose**: Delete stale pending instincts older than 30 days

**Description**: Clean up instincts older than 30 days that were auto-generated but never reviewed or promoted.

**Usage**:
```
/prune                    # Delete instincts older than 30 days
/prune --max-age 60      # Custom days threshold
/prune --dry-run         # Preview without deleting
```

**Implementation**:
```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/continuous-learning-v2/scripts/instinct-cli.py" prune
```

Or for manual installation (when `CLAUDE_PLUGIN_ROOT` is not set):
```bash
python3 ~/.claude/skills/continuous-learning-v2/scripts/instinct-cli.py prune
```

**Parameters**:

| Parameter | Description |
|---|---|
| None | Delete instincts older than 30 days |
| `--max-age <days>` | Custom time threshold in days |
| `--dry-run` | Preview what will be deleted without actually deleting |

**Best Practices**:
- Use `--dry-run` first to preview what will be deleted
- Run prune regularly to keep instinct inventory clean
- Note: Only pending status instincts will be deleted, not promoted ones

---

### /security-scan

**Purpose**: Security scanning

**Description**: Scan codebase for security vulnerabilities.

**Scans**:
- Hardcoded credentials
- SQL injection risks
- XSS vulnerabilities
- Dependency vulnerabilities

---

### /feature-dev (To be completed)

**Purpose**: Feature development assistant

**Description**: Provides assisted workflow for feature development.

---

### /cost-report

**Purpose**: Model cost report

**Description**: Generate AI model usage cost report.

---

## Related Commands

- `/security-scan` - Security scanning
- `/jira` - Jira integration
- `/prune` - Clean up instincts