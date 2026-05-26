# Testing Rules

## Rule Overview

ECC Rules testing rules define mandatory testing standards for ensuring code quality. This rule covers Test-Driven Development (TDD), minimum coverage requirements, and test structure specifications, ensuring all code is thoroughly validated.

## Core Requirements

### Minimum Test Coverage: 80%

Must include these three test types:

| Test Type | Description | Coverage Scope |
|----------|------|----------|
| Unit Tests | Individual functions, utilities, components | Individual units |
| Integration Tests | API endpoints, database operations | Integration points |
| End-to-End Tests | Critical user flows | Critical user flows |

### Test-Driven Development (TDD)

**Mandatory Workflow**:

```
1. Write test (RED)   - Write a failing test first
2. Run test           - Verify test fails
3. Write minimal implementation (GREEN) - Write minimal code to pass test
4. Run test           - Verify test passes
5. Refactor (IMPROVE)  - Improve code structure
6. Verify coverage    - Ensure 80%+ coverage
```

## Implementation Details

### AAA Pattern (Arrange-Act-Assert)

All tests must follow AAA structure:

```typescript
test('correctly calculates cosine similarity', () => {
  // Arrange - prepare test data
  const vector1 = [1, 0, 0];
  const vector2 = [0, 1, 0];

  // Act - execute operation under test
  const similarity = calculateCosineSimilarity(vector1, vector2);

  // Assert - verify results
  expect(similarity).toBe(0);
});
```

### Test Naming Conventions

Use descriptive names that explain the behavior under test:

```typescript
// RECOMMENDED - describe expected behavior
test('returns empty array when no markets match query', () => {});
test('throws error when API key is missing', () => {});
test('falls back to substring search when Redis is unavailable', () => {});

// AVOID - vague naming
test('test1', () => {});
test('edge case', () => {});
```

### Test Troubleshooting

When tests fail:

| Step | Action |
|------|------|
| 1 | Use **tdd-guide** agent |
| 2 | Check test isolation |
| 3 | Verify mock correctness |
| 4 | Fix implementation, not tests (unless tests are wrong) |

### Agent Support

| Agent | Purpose | When to Use |
|-------|------|----------|
| **tdd-guide** | Test-driven development guidance | Mandatory for new feature development and bug fixes |
| **code-reviewer** | Code review | After writing code |

## Violation Handling

### Insufficient Coverage

- Code with coverage below 80% must not be merged
- CI/CD pipeline should block low coverage code commits
- Use `c8` or similar tools to generate coverage reports

### Test Failure Handling

```
1. Analyze failure reason
2. Diagnose whether it's implementation issue or test issue
3. If implementation issue - fix implementation code
4. If test issue - fix test code
5. Ensure all tests pass before committing
```

### Common Issues

| Issue | Reason | Solution |
|------|------|----------|
| Flaky tests | Async operations, race conditions | Increase wait times, use mocks |
| Test isolation failures | Shared state pollution | Reset state before each test |
| Incorrect mocks | Expectations don't match reality | Verify mock setup is correct |

## Related Rules

| Related Rule | Description |
|----------|------|
| Code Style Rules | Code readability and maintainability |
| Security Rules | Security testing requirements |
| Code Review Rules | Test coverage checks |
| Development Workflow | TDD process integration |