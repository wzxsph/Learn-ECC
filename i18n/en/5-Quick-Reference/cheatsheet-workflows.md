# Workflows Cheatsheet

## Common Workflow Templates

### 1. TDD Development Flow

```
Requirements → Write tests (RED) → Implement (GREEN) → Refactor
```

**Usage**: `/tdd`

---

### 2. Code Review Flow

```
Commit code → Start review → Issue classification → Fix → Re-review → Merge
```

**Usage**: `/code-review`

---

### 3. Error Fix Flow

```
Discover error → Analyze cause → Locate code → Fix → Verify → Test
```

**Usage**: `/build-fix`

---

### 4. Skill Generation Flow

```
Analyze requirements → Reference existing patterns → Write template → Test & verify
```

**Usage**: `/skill-create`

---

## Custom Workflows

Workflow definition format:

```yaml
name: my-workflow
description: My workflow
steps:
  - name: step1
    command: do-something
  - name: step2
    command: do-other
    requires: [step1]
```

---

[Return to Quick Reference README](./README.md)