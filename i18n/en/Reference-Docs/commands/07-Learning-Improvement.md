# Learning and Improvement Commands

## Overview

Learning and improvement commands are used to extract patterns from sessions, track instincts, and manage organizational knowledge.

## Command List

### /learn

**Purpose**: Extract reusable patterns from current session and save as candidate skills

**Description**: Analyze current session, extract any patterns worth saving as skills. Run `/learn` at any time during a session.

**Extracted Content Types**:

| Type | Description |
|---|---|
| **Error solution patterns** | What error? Root cause? What fixed it? Can similar errors reuse this? |
| **Debugging techniques** | Non-obvious debugging steps, effective tool combinations, diagnostic patterns |
| **Workarounds** | Library quirks, API limitations, version-specific fixes |
| **Project-specific patterns** | Discovered codebase conventions, architecture decisions, integration patterns |

**Output Format**: Saved to `~/.claude/skills/learned/[pattern-name].md`

```markdown
# [Descriptive Pattern Name]

**Extracted:** [Date]
**Context:** [Brief description of when this applies]

## Problem
[What problem this solves - be specific]

## Solution
[The pattern/technique/workaround]

## Example
[Code example if applicable]

## When to Use
[Trigger conditions - what should activate this skill]
```

**Workflow**:
1. Review patterns in session that can be extracted
2. Identify most valuable/reusable insights
3. Draft skill file
4. Request user confirmation before saving
5. Save to `~/.claude/skills/learned/`

**Best Practices**:
- Don't extract trivial fixes (spelling errors, simple syntax errors)
- Don't extract one-time issues (specific API outages, etc.)
- Focus on patterns that save time in future sessions
- Keep skills focused - one pattern per skill

---

### /learn-eval

**Purpose**: Extract patterns + self-assess quality

**Description**: Quality assessment while extracting patterns.

---

### /evolve

**Purpose**: Analyze instincts + suggest evolution structure

**Description**: Analyze learned instincts, provide structured evolution suggestions.

---

### /promote

**Purpose**: Promote project instincts to global scope

**Description**: Elevate project-specific instincts to globally usable knowledge.

---

### /instinct-status

**Purpose**: Show all learned instincts

**Description**: Display status and confidence of all current instincts.

---

### /instinct-export

**Purpose**: Export instincts to file

**Description**: Export instincts to shareable file format.

---

### /instinct-import

**Purpose**: Import instincts from file/URL

**Description**: Import instincts into the system from file or URL.

---

### /skill-create

**Purpose**: Analyze git history + generate skill file

**Description**: Analyze project git history, extract reusable patterns to generate skill files.

**Workflow**:
1. Analyze git commit history
2. Identify recurring patterns
3. Generate skill file
4. Verify and save

---

### /skill-health

**Purpose**: Skill portfolio health dashboard

**Description**: Display health status and usage statistics of skill portfolio.

---

### /rules-distill

**Purpose**: Scan skills + extract cross-domain principles

**Description**: Scan all skills, extract cross-domain general principles.

---

## Related Commands

- `/learn` - Extract patterns
- `/skill-create` - Generate skill from git history
- `/instinct-status` - View instinct status