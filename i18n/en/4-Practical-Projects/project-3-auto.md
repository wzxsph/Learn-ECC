# Project 3: Workflow Automation

Build an end-to-end automated workflow system.

## Project Goal

**Estimated Duration**: 2-3 hours

Design and implement an automated workflow engine for handling common development tasks.

## Functional Requirements

### 1. Workflow Definition
- YAML format definition
- Step series and parallel execution
- Conditional branches
- Loop handling

### 2. Built-in Workflows
- **Code Check** - lint + type-check + test
- **Build & Deploy** - build + test + deploy
- **Problem Fix** - analyze + fix + verify

### 3. Monitoring & Alerts
- Execution status tracking
- Failure alert notifications
- Time consumption statistics

## Technical Solution

### Workflow Definition Example

```yaml
name: code-check
steps:
  - name: lint
    command: npm run lint
  - name: type-check
    command: npm run type-check
  - name: test
    command: npm test
    requires: [lint, type-check]
```

### Hook Integration
- PreToolUse - Parameter validation
- PostToolUse - Output validation
> Reference: [Hook Types](../Reference-Docs/hooks/Hook-Types.md) | [Automation & Scripting](../Reference-Docs/skills/Automation-Scripting.md)

## Acceptance Criteria

- [ ] Supports YAML definition
- [ ] Step dependency resolution is correct
- [ ] Parallel execution optimization
- [ ] Failure retry mechanism
- [ ] Execution logs are complete

---

[Return to Practical Projects README](./README.md)