# Workflows Cheat Sheet

## Common Workflow Templates

### 1. TDD Development Process

```
Requirements → Write test (RED) → Implement (GREEN) → Refactor (REFACTOR)
```

**Use**: `/tdd`

---

### 2. Code Review Process

```
Submit code → Start review → Issue classification → Fix → Re-review → Merge
```

**Use**: `/code-review`

---

### 3. Error Fix Process

```
Find error → Analyze cause → Locate code → Fix → Verify → Test
```

**Use**: `/build-fix`

---

### 4. Skill Generation Process

```
Analyze requirements → Reference existing patterns → Write template → Test & verify
```

**Use**: `/skill-create`

---

## Custom Workflows

Workflow definition format:

```yaml
name: my-workflow
description: My workflow description
steps:
  - name: step1
    command: do-something
  - name: step2
    command: do-other
    requires: [step1]
```

---

[Return to quick reference directory](./README.md)
