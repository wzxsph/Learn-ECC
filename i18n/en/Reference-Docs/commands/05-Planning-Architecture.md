# Planning and Architecture Commands

## Overview

Planning and architecture commands are used for product planning, feature design, and system architecture decisions. These commands ensure clear plans exist before writing code, reducing rework and improving code quality.

## Command List

### /plan

**Purpose**: General implementation planning - restate requirements, assess risks, create step-by-step implementation plan

**Description**: Create a comprehensive implementation plan before touching any code, wait for user confirmation. Accept freeform requirements or PRD markdown files.

**Input Modes**:

| Input | Mode | Behavior |
|-------|------|----------|
| `path/to/name.prd.md` | PRD artifact mode | Read PRD, select next deliverable milestone, generate `.claude/plans/{name}.plan.md` |
| Other markdown path | Reference mode | Read file as context, produce inline plan |
| Freeform text | Conversation mode | Produce inline plan |
| Empty input | Clarification mode | Ask what to plan |

**Key Parameters**:
- `$ARGUMENTS`: `[feature description | path/to/*.prd.md]` - Feature description or PRD file path

**Workflow**:
1. **Restate requirements** - Clarify what needs to be built
2. **Identify risks** - List potential issues and blockers
3. **Create step plan** - Break down into phased implementation
4. **Wait for confirmation** - Must receive user approval before proceeding

**Output Format** (PRD artifact mode):
```markdown
# Plan: {Feature Name}

**Source PRD**: {path}
**Selected Milestone**: {milestone or phase name}
**Complexity**: {Small | Medium | Large}

## Summary
{2-3 sentences}

## Patterns to Mirror
| Category | Source | Pattern |
|---|---|---|
| Naming | `path:line` | {short description} |
| Errors | `path:line` | {short description} |
| Tests | `path:line` | {short description} |

## Files to Change
| File | Action | Why |
|---|---|---|
| `path` | CREATE / UPDATE / DELETE | {reason} |

## Tasks
### Task 1: {name}
- **Action**: {what to do}
- **Mirror**: {pattern to follow}
- **Validate**: {command that proves correctness}

## Validation
```bash
{project-specific validation commands}
```

## Risks
| Risk | Likelihood | Mitigation |
|---|---|---|
```

**Best Practices**:
- Research existing patterns in codebase before using
- Create `.claude/plans/` directory if it doesn't exist in PRD artifact mode
- If PRD contains `Delivery Milestones` table, only update selected row status

**Common Pitfalls**:
- Trying to plan without asking clarification on empty input
- Starting to write code without waiting for user confirmation
- Skipping Pattern Grounding step

**Integration with Other Commands**:
- Use `tdd-workflow` skill for test-driven development after planning
- Use `/build-fix` to fix build errors
- Use `/code-review` to review completed implementation
- Use `/pr` or `/prp-pr` to open Pull Request

---

### /plan-prd

**Purpose**: Interactive PRD generator

**Description**: Generate problem-centric product requirements document with problem definition, user personas, success metrics, and MVP scope.

**Use Cases**:
- Deciding what to build
- Needing clear product requirements
- Planning new feature from scratch

**Workflow**:
1. FRAME - Understand user and problem
2. GROUND - Gather evidence
3. DECIDE - Determine scope and assumptions
4. GENERATE - Generate PRD file

---

### /prp-plan

**Purpose**: Comprehensive feature planning

**Description**: Complete project planning covering codebase analysis, risk assessment, and step-by-step implementation plan.

**Use Cases**:
- Detailed planning for complex features
- Needing codebase context analysis
- Multi-phase implementation projects

---

### /prp-prd

**Purpose**: PRP workflow PRD generator

**Description**: Generate product requirements document using PRP (Plan-Record-Produce) workflow.

---

### /prp-implement

**Purpose**: Execute PRP plan + validation loop

**Description**: Execute plan following PRP workflow with continuous validation and checks.

---

### /prp-pr

**Purpose**: Create PR from PRP workflow

**Description**: Create GitHub Pull Request from PRP workflow results.

---

### /prp-commit

**Purpose**: PRP validation commit

**Description**: Execute validation and commit code in PRP workflow.

---

### /multi-plan

**Purpose**: Multi-model collaborative planning

**Description**: Combine Codex and Gemini dual-model analysis to generate comprehensive implementation plan.

**Use Cases**:
- Needing multi-perspective analysis
- Complex cross-domain projects
- Balancing frontend and backend considerations

---

## Related Commands

- `/plan` - General implementation planning
- `/code-review` - Code review
- `/build-fix` - Build fix