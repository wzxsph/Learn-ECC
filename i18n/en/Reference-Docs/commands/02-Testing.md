# Testing Commands

This document introduces specialized commands for testing in ECC, supporting multiple programming languages and test frameworks.

---

## /go-test

**Purpose**: TDD workflow for Go language. Uses table-driven tests, write tests first then implement, verify 80%+ coverage.

**Usage**:
```
/go-test [feature description]
```

**Use Cases**:
- Implementing new Go functions
- Adding test coverage for existing Go code
- Fixing bugs (write failing test first)
- Learning Go TDD methodology

**Example Workflow**:
```go
// Step 1: Define interface
func ValidateEmail(email string) error {
    panic("not implemented")
}

// Step 2: Write table-driven test (RED)
tests := []struct {
    name    string
    email   string
    wantErr bool
}{
    {"simple email", "user@example.com", false},
    {"empty string", "", true},
    {"no at sign", "userexample.com", true},
}

// Step 3: Run test - verify FAIL

// Step 4: Implement minimal code (GREEN)
func ValidateEmail(email string) error {
    if email == "" {
        return ErrEmailEmpty
    }
    if !emailRegex.MatchString(email) {
        return ErrEmailInvalid
    }
    return nil
}

// Step 5: Verify PASS

// Step 6: Check coverage
go test -cover ./...
// coverage: 100.0%
```

**Coverage Commands**:
```bash
go test -cover ./...                    # Basic coverage
go test -coverprofile=coverage.out      # Generate coverage file
go tool cover -html=coverage.out        # View in browser
go test -race -cover ./...              # Race detection
```

---

## /kotlin-test

**Purpose**: TDD workflow for Kotlin language. Uses Kotest framework and MockK, write tests first then implement, verify 80%+ coverage (Kover).

**Usage**:
```
/kotlin-test [feature description]
```

**Use Cases**:
- Implementing new Kotlin functions or classes
- Adding test coverage for Android/KMP projects
- Fixing bugs (write failing test first)
- Learning Kotlin TDD methodology

**Kotest Test Styles**:
```kotlin
// StringSpec (simplest)
class CalculatorTest : StringSpec({
    "add two positive numbers" {
        Calculator.add(2, 3) shouldBe 5
    }
})

// FunSpec (standard unit test)
class RegistrationValidatorTest : FunSpec({
    test("valid registration returns Valid") {
        val request = RegistrationRequest(
            name = "Alice",
            email = "alice@example.com",
            password = "SecureP@ss1"
        )
        val result = validateRegistration(request)
        result.shouldBeInstanceOf<ValidationResult.Valid>()
    }
})

// BehaviorSpec (BDD style)
class OrderServiceTest : BehaviorSpec({
    Given("a valid order") {
        When("placed") {
            Then("should be confirmed") { /* ... */ }
        }
    }
})
```

**Coroutine Testing**:
```kotlin
test("concurrent fetch completes") {
    runTest {
        val result = service.fetchAll()
        result.shouldNotBeEmpty()
    }
}
```

**Coverage Commands**:
```bash
./gradlew koverHtmlReport     # HTML report
./gradlew koverVerify         # Verify coverage threshold
./gradlew koverXmlReport      # CI XML report
./gradlew test                # Run tests
```

---

## /rust-test

**Purpose**: TDD workflow for Rust language. Uses `#[test]`, rstest, proptest and mockall, write tests first then implement, verify 80%+ coverage (cargo-llvm-cov).

**Usage**:
```
/rust-test [feature description]
```

**Use Cases**:
- Implementing new Rust functions, methods, or traits
- Adding test coverage for Rust projects
- Fixing bugs (write failing test first)
- Learning Rust TDD methodology

**Test Patterns**:
```rust
// Unit test
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn adds_two_numbers() {
        assert_eq!(add(2, 3), 5);
    }

    #[test]
    fn handles_error() -> Result<(), Box<dyn std::error::Error>> {
        let result = parse_config(r#"port = 8080"#)?;
        assert_eq!(result.port, 8080);
        Ok(())
    }
}

// Parametrized test (rstest)
#[rstest]
#[case("hello", 5)]
#[case("", 0)]
#[case("rust", 4)]
fn test_string_length(#[case] input: &str, #[case] expected: usize) {
    assert_eq!(input.len(), expected);
}

// Async test
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Property test (proptest)
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        assert_eq!(input, decoded);
    }
}
```

**Coverage Commands**:
```bash
cargo llvm-cov                    # Coverage summary
cargo llvm-cov --html             # HTML report
cargo llvm-cov --fail-under-lines 80  # Fail if below threshold
cargo test                        # Run tests
```

---

## /cpp-test

**Purpose**: TDD workflow for C++ language. Uses GoogleTest/GoogleMock and CMake/CTest, write tests first then implement, verify coverage (gcov/lcov).

**Usage**:
```
/cpp-test [feature description]
```

**Use Cases**:
- Implementing new C++ classes or functions
- Adding test coverage
- Fixing bugs (write failing test first)
- Learning C++ TDD methodology

**GoogleTest Patterns**:
```cpp
// Basic test
TEST(SuiteName, TestName) {
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_NE(result, nullptr);
    EXPECT_TRUE(is_valid);
    EXPECT_THROW(func(), std::invalid_argument);
}

// Fixture
class DatabaseTest : public ::testing::Test {
protected:
    void SetUp() override { db_ = create_test_db(); }
    void TearDown() override { db_.reset(); }
    std::unique_ptr<Database> db_;
};

TEST_F(DatabaseTest, InsertsRecord) {
    db_->insert("key", "value");
    EXPECT_EQ(db_->get("key"), "value");
}

// Parametrized test
class PrimeTest : public ::testing::TestWithParam<std::pair<int, bool>> {};

TEST_P(PrimeTest, ChecksPrimality) {
    auto [input, expected] = GetParam();
    EXPECT_EQ(is_prime(input), expected);
}

INSTANTIATE_TEST_SUITE_P(Primes, PrimeTest,
    ::testing::Values(
        std::make_pair(2, true),
        std::make_pair(4, false),
        std::make_pair(7, true)
    ));
```

**Coverage Commands**:
```bash
cmake -DCMAKE_CXX_FLAGS="--coverage" -B build
cmake --build build
ctest --test-dir build
lcov --capture --directory build --output-file coverage.info
genhtml coverage.info --output-directory coverage_html
```

---

## /flutter-test

**Purpose**: Flutter/Dart testing. Reports failures and incrementally fixes test issues, covering unit, widget, golden, and integration tests.

**Usage**:
```
/flutter-test                # Run all tests
/flutter-test --coverage     # With coverage
/flutter-test test/file.dart # Run specific test file
```

**Use Cases**:
- Verifying no breakage after feature implementation
- Checking test coverage for new code
- Fixing specific test files
- Before submitting PR

**Common Test Failures and Fixes**:

| Failure Type | Typical Fix |
|-------------|-------------|
| `Expected: <X> Actual: <Y>` | Update assertion or fix implementation |
| `Widget not found` | Fix finder selector or update widget name |
| `Golden file not found` | Run `flutter test --update-goldens` to generate |
| `MissingPluginException` | Mock platform channels in test setup |
| `pumpAndSettle timed out` | Replace with explicit `pump(Duration)` |

---

## /e2e

**Purpose**: End-to-end testing workflow. Generate and run E2E tests for critical user flows.

**Usage**:
```
/e2e [feature description]
```

**Use Cases**:
- Testing critical user flows
- Cross-system integration testing
- Acceptance testing
- Regression testing

---

## /test-coverage

**Purpose**: Analyze test coverage, identify gaps, and generate missing tests to reach target threshold (default 80%).

**Usage**:
```
/test-coverage
```

**Workflow**:
1. **Detect test framework** - Select coverage command based on project type
2. **Analyze coverage report** - List files below 80%
3. **Generate missing tests** - Generate tests by priority
4. **Verify** - Re-run coverage to confirm improvement

**Test Framework Detection**:

| Indicator | Coverage Command |
|-----------|-----------------|
| `jest.config.*` | `npx jest --coverage` |
| `vitest.config.*` | `npx vitest run --coverage` |
| `pytest.ini` | `pytest --cov=src --cov-report=json` |
| `Cargo.toml` | `cargo llvm-cov --json` |
| `go.mod` | `go test -coverprofile=coverage.out` |

---

## /python-testing

**Purpose**: Python testing workflow. Uses pytest with coverage analysis support.

**Usage**:
```
/python-testing [feature description]
```

**Use Cases**:
- Implementing new Python features
- Adding test coverage
- Fixing bugs
- FastAPI/Django/Flask project testing

**Pytest Features**:
```python
# Parametrized test
@pytest.mark.parametrize("input,expected", [
    ("hello", 5),
    ("", 0),
    ("world", 5),
])
def test_string_length(input, expected):
    assert len(input) == expected

# Fixtures
@pytest.fixture
def client():
    with TestClient(app) as c:
        yield c

# Async test
@pytest.mark.asyncio
async def test_async_fetch():
    result = await fetch_data()
    assert result is not None
```

---

## Command Comparison Table

| Command | Language | Test Framework | Coverage Tool | TDD Loop |
|---------|----------|----------------|---------------|----------|
| `/go-test` | Go | Built-in testing | go test -cover | Yes |
| `/kotlin-test` | Kotlin | Kotest + MockK | Kover | Yes |
| `/rust-test` | Rust | #[test], rstest, proptest | cargo-llvm-cov | Yes |
| `/cpp-test` | C++ | GoogleTest | gcov/lcov | Yes |
| `/flutter-test` | Dart | Flutter test | coverage | - |
| `/python-testing` | Python | pytest | pytest-cov | - |
| `/e2e` | Multi | Project-specific | - | - |
| `/test-coverage` | Generic | Multiple | Multiple | - |