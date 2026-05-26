# 测试相关命令

本文档介绍 ECC 中用于测试的专门命令，支持多种Linguagens-de-Programação和测试框架。

---

## /go-test

**用途说明**: Go 语言的 TDD 工作流。使用表驱动测试，先写测试再实现，验证 80%+ 覆盖率。

**使用方法**:
```
/go-test [功能描述]
```

**使用场景**:
- 实现新的 Go 函数
- 为现有 Go 代码添加测试覆盖
- 修复 bug（先写失败的测试）
- 学习 Go TDD 方法论

**示例工作流**:
```go
// Step 1: 定义接口
func ValidateEmail(email string) error {
    panic("not implemented")
}

// Step 2: 写表驱动测试 (RED)
tests := []struct {
    name    string
    email   string
    wantErr bool
}{
    {"simple email", "user@example.com", false},
    {"empty string", "", true},
    {"no at sign", "userexample.com", true},
}

// Step 3: 运行测试 - 验证 FAIL

// Step 4: 实现最小代码 (GREEN)
func ValidateEmail(email string) error {
    if email == "" {
        return ErrEmailEmpty
    }
    if !emailRegex.MatchString(email) {
        return ErrEmailInvalid
    }
    return nil
}

// Step 5: 验证 PASS

// Step 6: 检查覆盖率
go test -cover ./...
// coverage: 100.0%
```

**覆盖率命令**:
```bash
go test -cover ./...                    # 基础覆盖率
go test -coverprofile=coverage.out      # 生成覆盖率文件
go tool cover -html=coverage.out        # 浏览器查看
go test -race -cover ./...              # 竞态检测
```

---

## /kotlin-test

**用途说明**: Kotlin 语言的 TDD 工作流。使用 Kotest 框架和 MockK，先写测试再实现，验证 80%+ 覆盖率（Kover）。

**使用方法**:
```
/kotlin-test [功能描述]
```

**使用场景**:
- 实现新的 Kotlin 函数或类
- 为 Android/KMP 项目添加测试覆盖
- 修复 bug（先写失败的测试）
- 学习 Kotlin TDD 方法论

**Kotest 测试风格**:
```kotlin
// StringSpec (最简单)
class CalculatorTest : StringSpec({
    "add two positive numbers" {
        Calculator.add(2, 3) shouldBe 5
    }
})

// FunSpec (标准单元测试)
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

// BehaviorSpec (BDD 风格)
class OrderServiceTest : BehaviorSpec({
    Given("a valid order") {
        When("placed") {
            Then("should be confirmed") { /* ... */ }
        }
    }
})
```

**协程测试**:
```kotlin
test("concurrent fetch completes") {
    runTest {
        val result = service.fetchAll()
        result.shouldNotBeEmpty()
    }
}
```

**覆盖率命令**:
```bash
./gradlew koverHtmlReport     # HTML 报告
./gradlew koverVerify         # 验证覆盖率阈值
./gradlew koverXmlReport      # CI 用 XML 报告
./gradlew test                # 运行测试
```

---

## /rust-test

**用途说明**: Rust 语言的 TDD 工作流。使用 `#[test]`、rstest、proptest 和 mockall，先写测试再实现，验证 80%+ 覆盖率（cargo-llvm-cov）。

**使用方法**:
```
/rust-test [功能描述]
```

**使用场景**:
- 实现新的 Rust 函数、方法或 trait
- 为 Rust 项目添加测试覆盖
- 修复 bug（先写失败的测试）
- 学习 Rust TDD 方法论

**测试模式**:
```rust
// 单元测试
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

// 参数化测试 (rstest)
#[rstest]
#[case("hello", 5)]
#[case("", 0)]
#[case("rust", 4)]
fn test_string_length(#[case] input: &str, #[case] expected: usize) {
    assert_eq!(input.len(), expected);
}

// 异步测试
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// 属性测试 (proptest)
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        assert_eq!(input, decoded);
    }
}
```

**覆盖率命令**:
```bash
cargo llvm-cov                    # 覆盖率摘要
cargo llvm-cov --html             # HTML 报告
cargo llvm-cov --fail-under-lines 80  # 低于阈值则失败
cargo test                        # 运行测试
```

---

## /cpp-test

**用途说明**: C++ 语言的 TDD 工作流。使用 GoogleTest/GoogleMock 和 CMake/CTest，先写测试再实现，验证覆盖率（gcov/lcov）。

**使用方法**:
```
/cpp-test [功能描述]
```

**使用场景**:
- 实现新的 C++ 类或函数
- 添加测试覆盖
- 修复 bug（先写失败的测试）
- 学习 C++ TDD 方法论

**GoogleTest 模式**:
```cpp
// 基本测试
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

// 参数化测试
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

**覆盖率命令**:
```bash
cmake -DCMAKE_CXX_FLAGS="--coverage" -B build
cmake --build build
ctest --test-dir build
lcov --capture --directory build --output-file coverage.info
genhtml coverage.info --output-directory coverage_html
```

---

## /flutter-test

**用途说明**: Flutter/Dart 测试。报告失败并逐步修复测试问题，涵盖单元、组件、金色和集成测试。

**使用方法**:
```
/flutter-test                # 运行所有测试
/flutter-test --coverage     # 带覆盖率
/flutter-test test/file.dart # 运行特定测试文件
```

**使用场景**:
- 验证功能实现后没有破坏
- 检查新代码的测试覆盖率
- 修复特定测试文件
- 提交 PR 之前

**常见测试失败和修复**:

| 失败类型 | 典型修复 |
|----------|----------|
| `Expected: <X> Actual: <Y>` | 更新断言或修复实现 |
| `Widget not found` | 修复 finder 选择器或更新 widget 名称 |
| `Golden file not found` | 运行 `flutter test --update-goldens` 生成 |
| `MissingPluginException` | 在测试设置中 mock 平台通道 |
| `pumpAndSettle timed out` | 用显式 `pump(Duration)` 替换 |

---

## /e2e

**用途说明**: 端到端测试工作流。生成和运行关键用户流程的 E2E 测试。

**使用方法**:
```
/e2e [功能描述]
```

**使用场景**:
- 测试关键用户流程
- 跨系统集成测试
- 验收测试
- 回归测试

---

## /test-coverage

**用途说明**: 分析测试覆盖率，识别差距，并生成缺失测试以达到目标阈值（默认 80%）。

**使用方法**:
```
/test-coverage
```

**工作流程**:
1. **检测测试框架** - 根据项目类型选择覆盖命令
2. **分析覆盖率报告** - 列出低于 80% 的文件
3. **生成缺失测试** - 按优先级生成测试
4. **验证** - 重新运行覆盖率确认改进

**测试框架检测**:

| 指示器 | 覆盖命令 |
|--------|----------|
| `jest.config.*` | `npx jest --coverage` |
| `vitest.config.*` | `npx vitest run --coverage` |
| `pytest.ini` | `pytest --cov=src --cov-report=json` |
| `Cargo.toml` | `cargo llvm-cov --json` |
| `go.mod` | `go test -coverprofile=coverage.out` |

---

## /python-testing

**用途说明**: Python 测试工作流。使用 pytest，支持覆盖率分析。

**使用方法**:
```
/python-testing [功能描述]
```

**使用场景**:
- 实现新的 Python 功能
- 添加测试覆盖
- 修复 bug
- FastAPI/Django/Flask 项目测试

**Pytest 特性**:
```python
# 参数化测试
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

# 异步测试
@pytest.mark.asyncio
async def test_async_fetch():
    result = await fetch_data()
    assert result is not None
```

---

## 命令对比表

| 命令 | 语言 | 测试框架 | 覆盖率工具 | TDD 循环 |
|------|------|----------|------------|----------|
| `/go-test` | Go | 内置 testing | go test -cover | ✓ |
| `/kotlin-test` | Kotlin | Kotest + MockK | Kover | ✓ |
| `/rust-test` | Rust | #[test], rstest, proptest | cargo-llvm-cov | ✓ |
| `/cpp-test` | C++ | GoogleTest | gcov/lcov | ✓ |
| `/flutter-test` | Dart | Flutter test | coverage | - |
| `/python-testing` | Python | pytest | pytest-cov | - |
| `/e2e` | 多语言 | 项目特定 | - | - |
| `/test-coverage` | 通用 | 多种 | 多种 | - |
