# Lệnh Liên Quan Đến Test

Tài liệu này giới thiệu các lệnh chuyên dụng trong ECC cho testing, hỗ trợ nhiều ngôn ngữ lập trình và test framework.

---

## /go-test

**Mục đích**: Workflow TDD cho ngôn ngữ Go. Sử dụng table-driven test, viết test trước, rồi implement, xác minh 80%+ coverage.

**Cách sử dụng**:
```
/go-test [mô tả tính năng]
```

**Khi nào sử dụng**:
- Implement hàm Go mới
- Thêm test coverage cho code Go hiện có
- Fix bug (viết test thất bại trước)
- Học phương pháp TDD của Go

**Ví dụ workflow**:
```go
// Step 1: Định nghĩa interface
func ValidateEmail(email string) error {
    panic("not implemented")
}

// Step 2: Viết table-driven test (RED)
tests := []struct {
    name    string
    email   string
    wantErr bool
}{
    {"simple email", "user@example.com", false},
    {"empty string", "", true},
    {"no at sign", "userexample.com", true},
}

// Step 3: Chạy test - Xác minh FAIL

// Step 4: Implement code tối thiểu (GREEN)
func ValidateEmail(email string) error {
    if email == "" {
        return ErrEmailEmpty
    }
    if !emailRegex.MatchString(email) {
        return ErrEmailInvalid
    }
    return nil
}

// Step 5: Xác minh PASS

// Step 6: Kiểm tra coverage
go test -cover ./...
// coverage: 100.0%
```

**Lệnh coverage**:
```bash
go test -cover ./...                    # Coverage cơ bản
go test -coverprofile=coverage.out      # Tạo file coverage
go tool cover -html=coverage.out        # Xem bằng browser
go test -race -cover ./...              # Phát hiện race
```

---

## /kotlin-test

**Mục đích**: Workflow TDD cho ngôn ngữ Kotlin. Sử dụng Kotest framework và MockK, viết test trước, rồi implement, xác minh 80%+ coverage (Kover).

**Cách sử dụng**:
```
/kotlin-test [mô tả tính năng]
```

**Khi nào sử dụng**:
- Implement hàm hoặc class Kotlin mới
- Thêm test coverage cho Android/KMP project
- Fix bug (viết test thất bại trước)
- Học phương pháp TDD của Kotlin

**Kotest test style**:
```kotlin
// StringSpec (đơn giản nhất)
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

**Coroutine test**:
```kotlin
test("concurrent fetch completes") {
    runTest {
        val result = service.fetchAll()
        result.shouldNotBeEmpty()
    }
}
```

**Lệnh coverage**:
```bash
./gradlew koverHtmlReport     # HTML report
./gradlew koverVerify         # Xác minh coverage threshold
./gradlew koverXmlReport      # XML report cho CI
./gradlew test                # Chạy test
```

---

## /rust-test

**Mục đích**: Workflow TDD cho ngôn ngữ Rust. Sử dụng `#[test]`, rstest, proptest và mockall, viết test trước, rồi implement, xác minh 80%+ coverage (cargo-llvm-cov).

**Cách sử dụng**:
```
/rust-test [mô tả tính năng]
```

**Khi nào sử dụng**:
- Implement hàm, method hoặc trait Rust mới
- Thêm test coverage cho Rust project
- Fix bug (viết test thất bại trước)
- Học phương pháp TDD của Rust

**Test pattern**:
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

// Parametric test (rstest)
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

**Lệnh coverage**:
```bash
cargo llvm-cov                    # Coverage summary
cargo llvm-cov --html             # HTML report
cargo llvm-cov --fail-under-lines 80  # Fail nếu dưới threshold
cargo test                        # Chạy test
```

---

## /cpp-test

**Mục đích**: Workflow TDD cho ngôn ngữ C++. Sử dụng GoogleTest/GoogleMock và CMake/CTest, viết test trước, rồi implement, xác minh coverage (gcov/lcov).

**Cách sử dụng**:
```
/cpp-test [mô tả tính năng]
```

**Khi nào sử dụng**:
- Implement class hoặc hàm C++ mới
- Thêm test coverage
- Fix bug (viết test thất bại trước)
- Học phương pháp TDD của C++

**GoogleTest pattern**:
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

// Parametric test
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

**Lệnh coverage**:
```bash
cmake -DCMAKE_CXX_FLAGS="--coverage" -B build
cmake --build build
ctest --test-dir build
lcov --capture --directory build --output-file coverage.info
genhtml coverage.info --output-directory coverage_html
```

---

## /flutter-test

**Mục đích**: Test Flutter/Dart. Báo cáo thất bại và từng bước sửa vấn đề test, bao gồm unit, widget, golden và integration test.

**Cách sử dụng**:
```
/flutter-test                # Chạy tất cả test
/flutter-test --coverage     # Có coverage
/flutter-test test/file.dart # Chạy file test cụ thể
```

**Khi nào sử dụng**:
- Xác minh không break sau khi implement tính năng
- Kiểm tra test coverage của code mới
- Sửa file test cụ thể
- Trước khi commit PR

**Test failure thường gặp và cách sửa**:

| Loại thất bại | Sửa lỗi điển hình |
|----------|----------|
| `Expected: <X> Actual: <Y>` | Cập nhật assertion hoặc sửa implementation |
| `Widget not found` | Sửa finder selector hoặc cập nhật tên widget |
| `Golden file not found` | Chạy `flutter test --update-goldens` để tạo |
| `MissingPluginException` | Mock platform channel trong test setup |
| `pumpAndSettle timed out` | Thay bằng explicit `pump(Duration)` |

---

## /e2e

**Mục đích**: Workflow test end-to-end. Tạo và chạy E2E test cho user flow quan trọng.

**Cách sử dụng**:
```
/e2e [mô tả tính năng]
```

**Khi nào sử dụng**:
- Test user flow quan trọng
- Integration test cross-system
- Acceptance test
- Regression test

---

## /test-coverage

**Mục đích**: Phân tích test coverage, xác định gaps và tạo test còn thiếu để đạt target threshold (mặc định 80%).

**Cách sử dụng**:
```
/test-coverage
```

**Quy trình làm việc**:
1. **Phát hiện test framework** - Chọn lệnh coverage phù hợp theo loại project
2. **Phân tích coverage report** - Liệt kê file dưới 80%
3. **Tạo test còn thiếu** - Tạo test theo priority
4. **Xác minh** - Chạy lại coverage để xác nhận cải thiện

**Phát hiện test framework**:

| Indicator | Lệnh coverage |
|--------|----------|
| `jest.config.*` | `npx jest --coverage` |
| `vitest.config.*` | `npx vitest run --coverage` |
| `pytest.ini` | `pytest --cov=src --cov-report=json` |
| `Cargo.toml` | `cargo llvm-cov --json` |
| `go.mod` | `go test -coverprofile=coverage.out` |

---

## /python-testing

**Mục đích**: Workflow test Python. Sử dụng pytest, hỗ trợ phân tích coverage.

**Cách sử dụng**:
```
/python-testing [mô tả tính năng]
```

**Khi nào sử dụng**:
- Implement tính năng Python mới
- Thêm test coverage
- Fix bug
- Test cho FastAPI/Django/Flask project

**Pytest features**:
```python
# Parametric test
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

## Bảng so sánh lệnh

| Lệnh | Ngôn ngữ | Test framework | Coverage tool | TDD loop |
|------|------|----------|------------|----------|
| `/go-test` | Go | testing built-in | go test -cover | ✓ |
| `/kotlin-test` | Kotlin | Kotest + MockK | Kover | ✓ |
| `/rust-test` | Rust | #[test], rstest, proptest | cargo-llvm-cov | ✓ |
| `/cpp-test` | C++ | GoogleTest | gcov/lcov | ✓ |
| `/flutter-test` | Dart | Flutter test | coverage | - |
| `/python-testing` | Python | pytest | pytest-cov | - |
| `/e2e` | Multi | Project-specific | - | - |
| `/test-coverage` | Generic | Multi | Multi | - |