# Code Style Rules

## Rule Overview

ECC Rules code style rules define core principles and best practices for code organization. These rules ensure code remains readable, maintainable, and consistent, covering everything from immutability to naming conventions.

## Core Requirements

### Immutability (Critical Principle)

**MUST**: Always create new objects, never modify existing ones.

```typescript
// WRONG - mutating original object
function modify(user, name) {
  user.name = name;  // changes original
  return user;
}

// CORRECT - returning new object
function update(user, name) {
  return { ...user, name };  // creates new copy
}
```

**Rationale**: Immutable data prevents hidden side effects, makes debugging easier, and enables safe concurrency.

### Core Design Principles

| Principle | Description | Practice |
|------|------|------|
| **KISS** | Keep It Simple | Prefer simplest working solution, avoid premature optimization, clarity over cleverness |
| **DRY** | Don't Repeat Yourself | Extract repeated logic into shared functions or utilities, avoid copy-paste implementation drift |
| **YAGNI** | You Aren't Gonna Need It | Don't build features/abstractions before needed, start simple, refactor when pressure is real |

### File Organization

| Requirement | Description |
|------|------|
| Many small files > Few large files | High cohesion, low coupling |
| Typical line count | 200-400 lines, max 800 lines |
| Large module splitting | Extract utilities from large modules |
| Organization method | By feature/domain, not by type |

### Error Handling

| Requirement | Description |
|------|------|
| Explicit handling | Handle errors explicitly at every level |
| UI code | Provide user-friendly error messages |
| Server-side | Log detailed error context |
| Forbidden | Never silently swallow errors |

### Input Validation

| Requirement | Description |
|------|------|
| Boundary validation | Validate all input at system boundaries |
| Schema validation | Use schema-based validation where available |
| Fail fast | Fail fast with clear error messages |
| External data | Never trust external data (API responses, user input, file content) |

## Implementation Details

### Naming Conventions

| Type | Convention | Example |
|------|------|------|
| Variables and functions | camelCase + descriptive names | `calculateTotal`, `userName` |
| Booleans | is/has/should/can prefix | `isActive`, `hasPermission` |
| Interfaces, types, components | PascalCase | `UserProfile`, `ButtonComponent` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_TIMEOUT` |
| Custom Hooks | camelCase + use prefix | `useUserData`, `useFetch` |

### Code Smells

#### Deep Nesting

Refactor nesting over 4 levels deep using early returns:

```typescript
// AVOID
function processOrder(order) {
  if (order) {
    if (order.items) {
      if (order.items.length > 0) {
        // processing logic
      }
    }
  }
}

// RECOMMENDED
function processOrder(order) {
  if (!order || !order.items || order.items.length === 0) {
    return;
  }
  // processing logic
}
```

#### Magic Numbers

Use named constants instead of magic numbers:

```typescript
// AVOID
setTimeout(callback, 300000);

// RECOMMENDED
const SESSION_TIMEOUT_MS = 5 * 60 * 1000; // 5 minutes
setTimeout(callback, SESSION_TIMEOUT_MS);
```

#### Long Functions

Split large functions into small focused functions with clear responsibilities:

| Function Length | Suggestion |
|----------|----------|
| <50 lines | Ideal |
| 50-100 lines | Acceptable, consider splitting |
| >100 lines | Must split |

## Violation Handling

### Code Quality Checklist

Must check before marking work complete:

- [ ] Code is readable and well-named
- [ ] Functions are small (<50 lines)
- [ ] Files are focused (<800 lines)
- [ ] No deep nesting (>4 levels)
- [ ] Proper error handling
- [ ] No hardcoded values (use constants or config)
- [ ] Using immutable patterns (no mutation)

### Severity Levels

| Level | Meaning | Action |
|------|------|------|
| CRITICAL | Security vulnerability or data loss risk | **BLOCK** - Must fix before merge |
| HIGH | Bug or significant quality issue | **WARN** - Should fix before merge |
| MEDIUM | Maintainability concern | **INFO** - Consider fixing |
| LOW | Style or minor suggestion | **NOTE** - Optional |

## Related Rules

| Related Rule | Description |
|----------|------|
| Testing Rules | Test coverage requirements |
| Security Rules | Security check items |
| Code Review Rules | Detailed review standards |
| Development Workflow | TDD and code review process |