# Kiểm Thử

Phần này cover testing strategy, TDD workflow và E2E testing pattern cho các ngôn ngữ lập trình.

---

## TDD Workflow

### Mục đích
TDD workflow đảm bảo tất cả code development tuân theo test-driven development principle, có comprehensive test coverage.

### Khi nào sử dụng
- Viết new feature hoặc function
- Fix bug hoặc issue
- Refactor existing code
- Thêm API endpoint
- Tạo new component

### Core concept

**1. Test First**
Luôn viết test trước, sau đó implement code để test pass.

**2. Coverage Requirement**
Tối thiểu 80% coverage (unit test + integration test + E2E test)

**3. Test Type**
- Unit test: Independent function và component
- Integration test: API endpoint và database operation
- E2E test: Critical user flow và browser automation

**4. RED-GREEN-REFACTOR Loop**
- RED: Viết test fail trước
- GREEN: Viết minimal code để test pass
- REFACTOR: Cải thiện code quality trong khi giữ test xanh

### TDD Process Steps

```bash
# 1. Viết test (RED)
npm test  # Test should fail

# 2. Implement code (GREEN)
# Viết minimal code để test pass

# 3. Refactor (REFACTOR)
# Cải thiện code quality, giữ test xanh

# 4. Verify coverage
npm run test:coverage  # Verify 80%+ coverage
```

### Test Isolation Principle

```typescript
// Wrong: Test phụ thuộc lẫn nhau
test('creates user', () => { /* ... */ })
test('updates same user', () => { /* Depends on previous test */ })

// Correct: Mỗi test independent setup
test('creates user', () => {
  const user = createTestUser()
  // Test logic
})

test('updates user', () => {
  const user = createTestUser()
  // Update logic
})
```

---

## Python Testing

### Mục đích
Python testing strategy sử dụng pytest, TDD methodology, fixture, mocking, parametrize và coverage requirement.

### Khi nào sử dụng
- Viết new Python code theo TDD methodology
- Design Python project test suite
- Review Python test coverage
- Setup test infrastructure

### Core concept

**1. pytest Fixtures**
Sử dụng `@pytest.fixture` tạo reusable test data.

**2. Parametrize Test**
Sử dụng `@pytest.mark.parametrize` run test với different input.

**3. Mocking**
Sử dụng `@patch` từ `unittest.mock` mock external dependency.

**4. Coverage**
Sử dụng `--cov` measure coverage, target 80%+.

### Usage Example

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

# Parametrize Test
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

# Exception Test
with pytest.raises(ValueError, match="invalid input"):
    raise ValueError("invalid input provided")

# Run command
pytest --cov=mypackage --cov-report=html
```

---

## Go Testing

### Mục đích
Go testing pattern bao gồm table-driven test, subtest, benchmark, fuzz test và coverage, follow TDD method và idiomatic Go practice.

### Khi nào sử dụng
- Viết new Go function hoặc method
- Thêm test coverage cho existing code
- Tạo benchmark cho performance-critical code
- Implement fuzz test cho input validation
- Follow TDD workflow trong Go project

### Core concept

**1. Table-Driven Test**
Sử dụng table-driven pattern implement comprehensive coverage.

**2. Subtest**
Sử dụng `t.Run` organize related test.

**3. Test Helper**
Sử dụng `t.Helper()` mark helper function.

**4. Interface Mocking**
Sử dụng interface để decouple dependency và test.

**5. Benchmark và Fuzz Test**
Performance test và random input test.

### Usage Example

```go
// Table-Driven Test
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

// Test Helper
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

// Benchmark
func BenchmarkProcess(b *testing.B) {
    data := generateTestData(1000)
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        Process(data)
    }
}

// Run command
go test -cover ./...
go test -bench=. -benchmem
go test -fuzz=FuzzParse -fuzztime=30s
```

---

## Rust Testing

### Mục đích
Rust testing pattern bao gồm unit test, integration test, async test, property test, mocking và coverage, follow TDD methodology.

### Khi nào sử dụng
- Viết new Rust function, method hoặc trait
- Thêm test coverage cho existing code
- Tạo benchmark cho performance-critical code
- Implement property test cho input validation
- Follow TDD workflow trong Rust project

### Core concept

**1. Module-Level Test**
Viết unit test trong `#[cfg(test)]` module.

**2. Integration Test**
Viết integration test trong `tests/` directory.

**3. Async Test**
Sử dụng `#[tokio::test]` cho async test.

**4. Property Test**
Sử dụng `proptest` cho property-based testing.

**5. Mocking**
Sử dụng `mockall` mock trait.

### Usage Example

```rust
// Module-Level Test
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

// Async Test
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Property Test
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

// Run command
cargo test
cargo llvm-cov --fail-under-lines 80
cargo bench
```

---

## E2E Testing

### Mục đích
Playwright E2E testing pattern cung cấp comprehensive pattern để build stable, fast, maintainable E2E test suite, bao gồm page object model, configuration, CI/CD integration và flaky test strategy.

### Khi nào sử dụng
- Tạo end-to-end test verify complete user flow
- Sử dụng Playwright cho browser automation
- Setup E2E test trong CI/CD
- Handle flaky test

### Core concept

**1. Page Object Model (POM)**
Encapsulate page interaction trong page object class.

**2. Test Isolation**
Mỗi test independent data setup.

**3. Locator Strategy**
Sử dụng semantic locator thay vì CSS class name.

**4. Flaky Test Handling**
Identify và handle flaky test.

**5. Artifacts Management**
Screenshot, video, trace recording cho debugging.

### Usage Example

```typescript
import { test, expect } from '@playwright/test'

// Page Object Model
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

// Test Structure
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

// Playwright Configuration
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

// Flaky Test Handling
test('conditional skip', async ({ page }) => {
  test.skip(process.env.CI, 'Flaky in CI - Issue #123')
})

test('flaky: complex search', async ({ page }) => {
  test.fixme(true, 'Known flaky test - Issue #123')
})
```

---

## Related Skill

| Skill | Usage |
|------|------|
| `coding-standards` | Coding standard và best practice |
| `error-handling` | Error handling pattern |
| `security-review` | Security review checklist |