# Testing

This section covers testing strategies, TDD workflows, and E2E testing patterns for various programming languages.

---

## TDD Workflow

### Purpose
TDD workflow ensures all code development follows test-driven development principles with comprehensive test coverage.

### When to Use
- Writing new features or functionality
- Fixing bugs or issues
- Refactoring existing code
- Adding API endpoints
- Creating new components

### Core Concepts

**1. Test First**
Always write tests first, then implement code to make tests pass.

**2. Coverage Requirements**
Minimum 80% coverage (unit tests + integration tests + E2E tests)

**3. Test Types**
- Unit tests: Independent functions and components
- Integration tests: API endpoints and database operations
- E2E tests: Critical user flows and browser automation

**4. RED-GREEN-REFACTOR Loop**
- RED: Write failing test first
- GREEN: Write minimal code to make test pass
- REFACTOR: Improve code quality while keeping tests green

### TDD Process Steps

```bash
# 1. Write test (RED)
npm test  # Test should fail

# 2. Implement code (GREEN)
# Write minimal code to make test pass

# 3. Refactor (REFACTOR)
# Improve code quality, keep tests green

# 4. Verify coverage
npm run test:coverage  # Verify 80%+ coverage
```

### Test Isolation Principle

```typescript
// Wrong: tests depend on each other
test('creates user', () => { /* ... */ })
test('updates same user', () => { /* depends on previous test */ })

// Correct: each test sets up independently
test('creates user', () => {
  const user = createTestUser()
  // test logic
})

test('updates user', () => {
  const user = createTestUser()
  // update logic
})
```

---

## Python Testing

### Purpose
Python testing strategies use pytest, TDD methodology, fixtures, mocking, parametrization, and coverage requirements.

### When to Use
- Writing new Python code following TDD methodology
- Designing test suites for Python projects
- Reviewing Python test coverage
- Setting up test infrastructure

### Core Concepts

**1. pytest Fixtures**
Use `@pytest.fixture` to create reusable test data.

**2. Parametrized Tests**
Use `@pytest.mark.parametrize` to run tests with different inputs.

**3. Mocking**
Use `@patch` from `unittest.mock` to mock external dependencies.

**4. Coverage**
Use `--cov` to measure coverage, target 80%+.

### Usage Examples

```python
import pytest
from unittest.mock import patch

# Fixtures
@pytest.fixture
def database():
    db = Database(":memory:")
    db.create_tables()
    yield db
    db.close()

# Parametrized tests
@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("world", "WORLD"),
    ("PyThOn", "PYTHON"),
])
def test_uppercase(input, expected):
    assert input.upper() == expected

# Mocking
@patch("mypackage.external_api_call")
def test_with_mock(api_call_mock):
    api_call_mock.return_value = {"status": "success"}
    result = my_function()
    assert result["status"] == "success"

# Exception testing
with pytest.raises(ValueError, match="invalid input"):
    raise ValueError("invalid input provided")

# Run commands
pytest --cov=mypackage --cov-report=html
```

---

## Go Testing

### Purpose
Go testing patterns include table-driven tests, subtests, benchmark tests, fuzz tests, and test coverage, following TDD methodology and idiomatic Go practices.

### When to Use
- Writing new Go functions or methods
- Adding test coverage for existing code
- Creating benchmark tests for performance-critical code
- Implementing fuzz tests for input validation
- Following TDD workflow in Go projects

### Core Concepts

**1. Table-Driven Tests**
Use table-driven pattern for comprehensive coverage.

**2. Subtests**
Use `t.Run` to organize related tests.

**3. Test Helpers**
Use `t.Helper()` to mark helper functions.

**4. Interface Mocking**
Use interfaces for dependency decoupling and testing.

**5. Benchmark and Fuzz Tests**
Performance testing and random input testing.

### Usage Examples

```go
// Table-driven tests
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"negative numbers", -1, -2, -3},
        {"zero values", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.expected)
            }
        })
    }
}

// Test helper function
func setupTestDB(t *testing.T) *sql.DB {
    t.Helper()
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatalf("failed to open database: %v", err)
    }
    t.Cleanup(func() { db.Close() })
    return db
}

// Interface Mocking
type UserRepository interface {
    GetUser(id string) (*User, error)
    SaveUser(user *User) error
}

type MockUserRepository struct {
    GetUserFunc  func(id string) (*User, error)
    SaveUserFunc func(user *User) error
}

func (m *MockUserRepository) GetUser(id string) (*User, error) {
    return m.GetUserFunc(id)
}

// Benchmark test
func BenchmarkProcess(b *testing.B) {
    data := generateTestData(1000)
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        Process(data)
    }
}

// Run commands
go test -cover ./...
go test -bench=. -benchmem
go test -fuzz=FuzzParse -fuzztime=30s
```

---

## Rust Testing

### Purpose
Rust testing patterns include unit tests, integration tests, async tests, property tests, mocking, and coverage, following TDD methodology.

### When to Use
- Writing new Rust functions, methods, or traits
- Adding test coverage for existing code
- Creating benchmark tests for performance-critical code
- Implementing property tests for input validation
- Following TDD workflow in Rust projects

### Core Concepts

**1. Module-Level Tests**
Write unit tests in `#[cfg(test)]` modules.

**2. Integration Tests**
Write integration tests in `tests/` directory.

**3. Async Tests**
Use `#[tokio::test]` for async testing.

**4. Property Tests**
Use `proptest` for property-based testing.

**5. Mocking**
Use `mockall` to mock traits.

### Usage Examples

```rust
// Module-level tests
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn creates_user_with_valid_email() {
        let user = User::new("Alice", "alice@example.com").unwrap();
        assert_eq!(user.display_name(), "Alice");
    }

    #[test]
    fn rejects_invalid_email() {
        let result = User::new("Bob", "not-an-email");
        assert!(result.is_err());
    }
}

// Async tests
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Property tests
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        prop_assert_eq!(input, decoded);
    }
}

// Mocking with mockall
use mockall::{automock, predicate::eq};

#[automock]
trait UserRepository {
    fn find_by_id(&self, id: u64) -> Option<User>;
    fn save(&self, user: &User) -> Result<(), StorageError>;
}

#[test]
fn service_returns_user_when_found() {
    let mut mock = MockUserRepository::new();
    mock.expect_find_by_id()
        .with(eq(42))
        .times(1)
        .returning(|_| Some(User { id: 42, name: "Alice".into() }));

    let service = UserService::new(Box::new(mock));
    let user = service.get_user(42).unwrap();
    assert_eq!(user.name, "Alice");
}

// Run commands
cargo test
cargo llvm-cov --fail-under-lines 80
cargo bench
```

---

## E2E Testing

### Purpose
Playwright E2E testing patterns provide comprehensive patterns for building stable, fast, maintainable E2E test suites, including page object models, configuration, CI/CD integration, and flaky test strategies.

### When to Use
- Creating end-to-end tests to verify complete user flows
- Using Playwright for browser automation
- Setting up E2E tests in CI/CD
- Handling flaky tests

### Core Concepts

**1. Page Object Model (POM)**
Encapsulate page interactions in page object classes.

**2. Test Isolation**
Each test sets up data independently.

**3. Locator Strategy**
Use semantic locators instead of CSS class names.

**4. Flaky Test Handling**
Identify and handle unstable tests.

**5. Artifacts Management**
Screenshots, videos, trace recordings for debugging.

### Usage Examples

```typescript
import { test, expect } from '@playwright/test'

// Page object model
export class ItemsPage {
  readonly page: Page
  readonly searchInput: Locator
  readonly itemCards: Locator

  constructor(page: Page) {
    this.page = page
    this.searchInput = page.locator('[data-testid="search-input"]')
    this.itemCards = page.locator('[data-testid="item-card"]')
  }

  async goto() {
    await this.page.goto('/items')
    await this.page.waitForLoadState('networkidle')
  }

  async search(query: string) {
    await this.searchInput.fill(query)
    await this.page.waitForResponse(resp => resp.url().includes('/api/search'))
  }
}

// Test structure
test.describe('Item Search', () => {
  let itemsPage: ItemsPage

  test.beforeEach(async ({ page }) => {
    itemsPage = new ItemsPage(page)
    await itemsPage.goto()
  })

  test('should search by keyword', async ({ page }) => {
    await itemsPage.search('test')
    const count = await itemsPage.getItemCount()
    expect(count).toBeGreaterThan(0)
    await expect(itemsPage.itemCards.first()).toContainText(/test/i)
  })
})

// Playwright configuration
export default defineConfig({
  testDir: './tests/e2e',
  retries: process.env.CI ? 2 : 0,
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
  ],
})

// Flaky test handling
test('conditional skip', async ({ page }) => {
  test.skip(process.env.CI, 'Flaky in CI - Issue #123')
})

test('flaky: complex search', async ({ page }) => {
  test.fixme(true, 'Known flaky test - Issue #123')
})
```

---

## Related Skills

| Skill | Purpose |
|-------|---------|
| `coding-standards` | Coding standards and best practices |
| `error-handling` | Error handling patterns |
| `security-review` | Security review checklist |
