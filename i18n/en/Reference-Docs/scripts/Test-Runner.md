# Test Runner

This document introduces the test structure and running methods of the ECC project.

## Test Entry Point

**Path**: `tests/run-all.js`

Centralized test runner, automatically discovers and runs all test files.

### Usage

```bash
# Run all tests
node tests/run-all.js

# Run single test file
node tests/lib/utils.test.js
node tests/hooks/hooks.test.js
```

### Test File Discovery

The test runner automatically discovers test files via glob pattern `tests/**/*.test.js`.

Rules:
- Files must be located in `tests/` directory
- File names must end with `.test.js`
- Directory structure is preserved for display

### Output Format

```
╔══════════════════════════════════════════════════════════╗
║           Everything Claude Code - Test Suite             ║
╚══════════════════════════════════════════════════════════╝

━━━ Running lib/utils.test.js ━━━
(test output...)
━━━ Running hooks/hooks.test.js ━━━
(test output...)

╔══════════════════════════════════════════════════════════╗
║                     Final Results                        ║
╠══════════════════════════════════════════════════════════╣
║  Total Tests:    42                                      ║
║  Passed:         42  ✓                                   ║
║  Failed:         0                                        ║
╚══════════════════════════════════════════════════════════╝
```

### Test Statistics

The test runner parses output from each test file to aggregate statistics:
- `Passed: <count>`
- `Failed: <count>`

---

## Test Structure

### Directory Layout

```
tests/
├── run-all.js              # Main test runner
├── lib/                    # Library utility tests
│   ├── utils.test.js
│   ├── package-manager.test.js
│   └── ...
├── hooks/                  # Hook integration tests
│   └── hooks.test.js
├── integration/            # Integration tests
├── scripts/                # Script tests
├── commands/               # Command tests
├── docs/                   # Documentation tests
├── opencode-config.test.js
├── plugin-manifest.test.js
├── test_*.py               # Python tests
└── conftest.py            # pytest configuration
```

### Test Types

1. **Unit Tests** (`*.test.js`)
   - Test independent functions and utilities
   - Use Node.js native assert or Jest style

2. **Integration Tests** (`tests/integration/`)
   - Test multiple components collaborating
   - Test complete hook flows

3. **Python Tests** (`test_*.py`)
   - pytest format Python tests
   - Use `conftest.py` for configuration

---

## Test Configuration

### Node.js Tests

Test files use standard Node.js assert module:

```javascript
const assert = require('assert');

test('example test', () => {
  assert.strictEqual(actual, expected, 'Optional message');
});
```

### Python Tests (pytest)

Location: `tests/conftest.py`

pytest configuration file, provides shared fixtures and configuration.

```bash
# Run Python tests
python -m pytest tests/

# Run specific test
python -m pytest tests/test_builder.py
```

---

## Test Best Practices

### AAA Pattern

```
test('test description', () => {
  // Arrange - prepare test data
  const input = 'test data';

  // Act - execute operation under test
  const result = myFunction(input);

  // Assert - verify results
  assert.strictEqual(result, expected);
});
```

### Test Naming

Use descriptive names that clearly explain the behavior under test:

```javascript
test('returns empty array when no markets match query', () => {});
test('throws error when API key is missing', () => {});
test('falls back to substring search when Redis is unavailable', () => {});
```

### Test Isolation

Each test should:
- Run independently, not depend on other tests
- Clean up its own test data
- Use mocks to isolate external dependencies

---

## CI Integration

Configure test scripts in `package.json`:

```json
{
  "scripts": {
    "test": "validate-commands.js && tests/run-all.js"
  }
}
```

The test runner is designed to work in CI environments:
- Non-zero exit code indicates failure
- JSON output for automated parsing
- Clear error messages for debugging
