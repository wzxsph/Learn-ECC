# Lệnh Review Ngôn Ngữ

Tài liệu này giới thiệu các lệnh chuyên dụng trong ECC để review code theo từng ngôn ngữ lập trình.

---

## /python-review

**Mục đích**: Review toàn diện code Python. Kiểm tra tuân thủ PEP 8, type hint, bảo mật và Python idioms.

**Cách sử dụng**:
```
/python-review
```

**Khi nào sử dụng**:
- Sau khi viết hoặc sửa code Python
- Trước khi commit thay đổi Python
- Review PR chứa code Python
- Học Python idioms và best practice

**Các category review**:

### CRITICAL (Phải sửa)
- SQL/Command injection vulnerability
- Unsafe eval/exec usage
- Unsafe pickle deserialization
- Hardcoded credentials
- Unsafe YAML loading
- Bare except clause that swallows errors

### HIGH (Nên sửa)
- Hàm public thiếu type hint
- Mutable default argument
- Silently swallowing exceptions
- Resource management không dùng context manager
- C-style loop thay vì comprehension
- Dùng type() thay vì isinstance()

### MEDIUM (Nên cân nhắc)
- PEP 8 format violation
- Hàm public thiếu docstring
- Print statement thay vì logging
- Inefficient string manipulation
- Magic number không dùng named constant
- Chưa dùng f-strings

**Lệnh kiểm tra tự động**:
```bash
mypy .                              # Kiểm tra kiểu
ruff check .                        # Linting
black --check .                     # Kiểm tra format
isort --check-only .                # Sắp xếp import
bandit -r .                         # Security scan
pip-audit                           # Audit phụ thuộc
pytest --cov=app --cov-report=term-missing  # Test coverage
```

**Sửa lỗi thường gặp**:

```python
# Sửa SQL injection
# Sai
query = f"SELECT * FROM users WHERE id = {user_id}"

# Đúng - Dùng parameterized query
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))

# Sửa mutable default argument
# Sai
def process_items(items=[]):
    items.append("new")
    return items

# Đúng
def process_items(items=None):
    if items is None:
        items = []
    items.append("new")
    return items

# Dùng context manager
# Sai
f = open("config.json")
data = f.read()
f.close()

# Đúng
with open("config.json") as f:
    data = f.read()
```

---

## /go-review

**Mục đích**: Review toàn diện code Go. Kiểm tra idiomatic pattern, concurrency safety, error handling và security.

**Cách sử dụng**:
```
/go-review
```

**Khi nào sử dụng**:
- Sau khi viết hoặc sửa code Go
- Trước khi commit thay đổi Go
- Review PR chứa code Go
- Học Go idioms

**Các category review**:

### CRITICAL (Phải sửa)
- SQL/Command injection vulnerability
- Race condition không đồng bộ
- Goroutine leak
- Hardcoded credentials
- Unsafe pointer usage
- Ignore error in critical path

### HIGH (Nên sửa)
- Thiếu context wrapper cho error
- Panic thay vì return error
- Context không được propagate
- Unbuffered channel gây deadlock
- Interface không implement đúng
- Thiếu mutex bảo vệ

### MEDIUM (Nên cân nhắc)
- Non-idiomatic code pattern
- Export function thiếu godoc comment
- Inefficient string concatenation
- Slice không preallocate
- Chưa dùng table-driven test

**Lệnh kiểm tra tự động**:
```bash
go vet ./...                         # Phân tích tĩnh
staticcheck ./...                    # Kiểm tra nâng cao (nếu cài)
golangci-lint run                   # Linting
go build -race ./...                # Phát hiện race
govulncheck ./...                   # Security vulnerability
```

---

## /kotlin-review

**Mục đích**: Review toàn diện code Kotlin. Kiểm tra idiomatic pattern, null safety, coroutine safety và security.

**Cách sử dụng**:
```
/kotlin-review
```

**Khi nào sử dụng**:
- Sau khi viết hoặc sửa code Kotlin
- Trước khi commit thay đổi Kotlin
- Review PR cho Android/KMP project
- Học Kotlin idioms

**Các category review**:

### CRITICAL (Phải sửa)
- SQL/Command injection vulnerability
- Forced unwrap `!!` không có lý do
- Platform type null safety violation
- GlobalScope usage (structured concurrency violation)
- Hardcoded credentials
- Unsafe deserialization

### HIGH (Nên sửa)
- Mutable state (khi có thể dùng immutable)
- Blocking call trong coroutine context
- Thiếu cancellation check trong long loop
- Sealed class `when` không exhaustive
- Hàm lớn (>50 dòng)
- Deep nesting (>4 levels)

### MEDIUM (Nên cân nhắc)
- Non-idiomatic Kotlin (Java-style pattern)
- Thiếu trailing comma
- Scope function misuse hoặc nesting
- Chain collection lớn thiếu Sequence
- Redundant explicit type

**Lệnh kiểm tra tự động**:
```bash
./gradlew build                      # Build check
./gradlew detekt                     # Phân tích tĩnh
./gradlew ktlintCheck                # Kiểm tra format
./gradlew test                       # Test
```

---

## /rust-review

**Mục đích**: Review toàn diện code Rust. Kiểm tra ownership lifecycle, error handling, unsafe usage và idiomatic pattern.

**Cách sử dụng**:
```
/rust-review
```

**Khi nào sử dụng**:
- Sau khi viết hoặc sửa code Rust
- Trước khi commit thay đổi Rust
- Review PR cho Rust project
- Học Rust idioms

**Các category review**:

### CRITICAL (Phải sửa)
- `unwrap()`/`expect()` không checked trong production code path
- `unsafe` không có comment `// SAFETY:`
- SQL injection qua string interpolation
- Command injection từ input không validated
- Hardcoded credentials
- Use-after-free của raw pointer

### HIGH (Nên sửa)
- Unnecessary `.clone()` để satisfy borrow checker
- `String` parameter nên dùng `&str` hoặc `impl AsRef<str>`
- Blocking trong async context (`std::thread::sleep`)
- Shared type thiếu `Send`/`Sync` bounds
- Wildcard `_ =>` match trên critical enum
- Hàm lớn (>50 dòng)

### MEDIUM (Nên cân nhắc)
- Unnecessary allocation trong hot path
- Thiếu `with_capacity` khi kích thước đã biết
- Suppress clippy warning không có lý do
- Public API thiếu `///` documentation
- Cân nhắc dùng `#[must_use]` cho return type có thể ignore giá trị

**Lệnh kiểm tra tự động**:
```bash
cargo check                           # Build check
cargo clippy -- -D warnings            # Lints
cargo fmt --check                      # Kiểm tra format
cargo test                            # Test
cargo audit                           # Security audit (nếu có)
```

---

## /cpp-review

**Mục đích**: Review toàn diện code C++. Kiểm tra memory safety, modern C++ idioms, concurrency và security.

**Cách sử dụng**:
```
/cpp-review
```

**Khi nào sử dụng**:
- Sau khi viết hoặc sửa code C++
- Trước khi commit thay đổi C++
- Review PR cho C++ project
- Kiểm tra vấn đề memory safety

**Các category review**:

### CRITICAL (Phải sửa)
- Bare `new`/`delete` không có RAII
- Buffer overflow và use-after-free
- Data race không đồng bộ
- Command injection qua `system()`
- Uninitialized variable read
- Null pointer dereference

### HIGH (Nên sửa)
- Rule of five violation
- Thiếu `std::lock_guard` / `std::scoped_lock`
- Lifetime management không đúng cho detached thread
- C-style cast thay vì `static_cast`/`dynamic_cast`
- Thiếu `const` correctness

### MEDIUM (Nên cân nhắc)
- Unnecessary copy (truyền giá trị thay vì `const&`)
- Thiếu `reserve()` khi kích thước đã biết
- `using namespace std;` trong header file
- Important return value thiếu `[[nodiscard]]`
- Template metaprogramming quá phức tạp

**Lệnh kiểm tra tự động**:
```bash
clang-tidy --checks='*,-llvmlibc-*' src/*.cpp -- -std=c++17
cppcheck --enable=all --suppress=missingIncludeSystem src/
cmake --build build -- -Wall -Wextra -Wpedantic
```

---

## /flutter-review

**Mục đích**: Review code Flutter/Dart. Kiểm tra idiomatic pattern, component best practice, state management, performance, accessibility và security.

**Cách sử dụng**:
```
/flutter-review
```

**Khi nào sử dụng**:
- Trước khi commit PR chứa thay đổi Flutter/Dart (sau khi build và test pass)
- Phát hiện vấn đề sớm sau khi implement tính năng mới
- Review code Flutter của người khác
- Audit component, state management component hoặc service class
- Trước khi phát hành production

**Các vùng review**:

| Vùng | Mức độ nghiêm trọng |
|------|----------|
| Hardcoded key, plaintext HTTP | CRITICAL |
| Architecture violation, state management anti-pattern | CRITICAL |
| Widget rebuild issue, resource leak | HIGH |
| Thiếu `dispose()`, BuildContext sau `await` | HIGH |
| Dart null safety, thiếu error/loading state | HIGH |
| Const propagation, widget composition | HIGH |
| Expensive work trong `build()` | HIGH |
| Accessibility, semantic label | MEDIUM |
| State transition thiếu test | HIGH |
| Hardcoded string (l10n) | MEDIUM |

---

## /fastapi-review

**Mục đích**: Review FastAPI application. Kiểm tra architecture, async correctness, dependency injection, Pydantic schema, security, performance và testability.

**Cách sử dụng**:
```
/fastapi-review [file hoặc directory]
```

**Các vùng review**:
- App factory, route boundary, middleware và exception handler
- Pydantic request và response schema tách biệt
- Dependency injection cho database session, authentication, pagination và settings
- Async database và external HTTP pattern
- CORS, authentication, rate limiting, logging và secret handling
- OpenAPI metadata và documented response model
- Test client setup và dependency override

---

## Bảng so sánh lệnh review ngôn ngữ

| Lệnh | Ngôn ngữ | Kiểm tra chính | Mức độ nghiêm trọng |
|------|------|----------|----------|
| `/python-review` | Python | PEP 8, type hint, SQL injection | CRITICAL/HIGH/MEDIUM |
| `/go-review` | Go | Concurrency safety, error handling, goroutine | CRITICAL/HIGH/MEDIUM |
| `/kotlin-review` | Kotlin | Null safety, coroutine, GlobalScope | CRITICAL/HIGH/MEDIUM |
| `/rust-review` | Rust | Ownership, lifecycle, unsafe | CRITICAL/HIGH/MEDIUM |
| `/cpp-review` | C++ | Memory safety, RAII, concurrency | CRITICAL/HIGH/MEDIUM |
| `/flutter-review` | Flutter | State management, accessibility, performance | CRITICAL/HIGH/MEDIUM |
| `/fastapi-review` | Python | Pydantic, DI, async, security | CRITICAL/HIGH/MEDIUM |