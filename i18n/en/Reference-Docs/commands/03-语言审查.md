# Language Review Commands

This document introduces specialized commands for code review across various programming languages in ECC.

---

## /python-review

**Purpose**: Comprehensive Python code review. Checks PEP 8 compliance, type hints, security, and Python idioms.

**Usage**:
```
/python-review
```

**Use Cases**:
- After writing or modifying Python code
- Before committing Python changes
- Reviewing PRs containing Python code
- Learning Python idioms and best practices

**Review Categories**:

### CRITICAL (Must Fix)
- SQL/command injection vulnerabilities
- Unsafe eval/exec usage
- Unsafe pickle deserialization
- Hardcoded credentials
- Unsafe YAML loading
- Bare except clauses that swallow errors

### HIGH (Should Fix)
- Public functions missing type hints
- Mutable default arguments
- Silently swallowing exceptions
- Resource management without context managers
- C-style loops instead of comprehensions
- Using type() instead of isinstance()

### MEDIUM (Consider Fixing)
- PEP 8 format violations
- Public functions missing docstrings
- Print statements instead of logging
- Inefficient string operations
- Magic numbers without named constants
- Not using f-strings

**Automatic Check Commands**:
```bash
mypy .                              # Type checking
ruff check .                        # Linting
black --check .                     # Format check
isort --check-only .                # Import sorting
bandit -r .                         # Security scan
pip-audit                           # Dependency audit
pytest --cov=app --cov-report=term-missing  # Test coverage
```

**Common Issue Fixes**:

```python
# SQL injection fix
# Wrong
query = f"SELECT * FROM users WHERE id = {user_id}"

# Correct - use parameterized query
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))

# Mutable default argument fix
# Wrong
def process_items(items=[]):
    items.append("new")
    return items

# Correct
def process_items(items=None):
    if items is None:
        items = []
    items.append("new")
    return items

# Use context manager
# Wrong
f = open("config.json")
data = f.read()
f.close()

# Correct
with open("config.json") as f:
    data = f.read()
```

---

## /go-review

**Purpose**: Comprehensive Go code review. Checks idiomatic patterns, concurrency safety, error handling, and security.

**Usage**:
```
/go-review
```

**Use Cases**:
- After writing or modifying Go code
- Before committing Go changes
- Reviewing PRs containing Go code
- Learning Go idiomatic patterns

**Review Categories**:

### CRITICAL (Must Fix)
- SQL/command injection vulnerabilities
- Race conditions without synchronization
- Goroutine leaks
- Hardcoded credentials
- Unsafe pointer usage
- Ignoring errors in critical paths

### HIGH (Should Fix)
- Missing error context wrapping
- Panic instead of error return
- Context not propagated
- Unbuffered channels causing deadlocks
- Interface not implemented errors
- Missing mutex protection

### MEDIUM (Consider Fixing)
- Non-idiomatic code patterns
- Exported functions missing godoc comments
- Inefficient string concatenation
- Slices not pre-allocated
- Not using table-driven tests

**Automatic Check Commands**:
```bash
go vet ./...                         # Static analysis
staticcheck ./...                    # Advanced checks (if installed)
golangci-lint run                   # Linting
go build -race ./...                # Race detection
govulncheck ./...                   # Security vulnerabilities
```

---

## /kotlin-review

**Purpose**: Comprehensive Kotlin code review. Checks idiomatic patterns, null safety, coroutine safety, and security.

**Usage**:
```
/kotlin-review
```

**Use Cases**:
- After writing or modifying Kotlin code
- Before committing Kotlin changes
- Reviewing PRs for Android/KMP projects
- Learning Kotlin idiomatic patterns

**Review Categories**:

### CRITICAL (Must Fix)
- SQL/command injection vulnerabilities
- Force unwrap `!!` without good reason
- Platform type null safety violations
- GlobalScope usage (structured concurrency violation)
- Hardcoded credentials
- Unsafe deserialization

### HIGH (Should Fix)
- Mutable state (when immutability is possible)
- Blocking calls in coroutine context
- Missing cancellation checks in long loops
- Non-exhaustive sealed class `when`
- Large functions (>50 lines)
- Deep nesting (>4 levels)

### MEDIUM (Consider Fixing)
- Non-idiomatic Kotlin (Java-style patterns)
- Missing trailing commas
- Scope function misuse or nesting
- Large collection chains missing Sequence
- Redundant explicit types

**Automatic Check Commands**:
```bash
./gradlew build                      # Build check
./gradlew detekt                     # Static analysis
./gradlew ktlintCheck                # Format check
./gradlew test                       # Test
```

---

## /rust-review

**Purpose**: Comprehensive Rust code review. Checks ownership lifetimes, error handling, unsafe usage, and idiomatic patterns.

**Usage**:
```
/rust-review
```

**Use Cases**:
- After writing or modifying Rust code
- Before committing Rust changes
- Reviewing PRs for Rust projects
- Learning Rust idiomatic patterns

**Review Categories**:

### CRITICAL (Must Fix)
- Unchecked `unwrap()`/`expect()` in production code paths
- `unsafe` without `// SAFETY:` comment
- SQL injection via string interpolation
- Unvalidated input for command injection
- Hardcoded credentials
- Use-after-free of raw pointers

### HIGH (Should Fix)
- Unnecessary `.clone()` to satisfy borrow checker
- `String` parameters should use `&str` or `impl AsRef<str>`
- Blocking in async context (`std::thread::sleep`)
- Shared types missing `Send`/`Sync` bounds
- Wildcard `_ =>` match on critical enums
- Large functions (>50 lines)

### MEDIUM (Consider Fixing)
- Unnecessary allocations in hot paths
- Missing `with_capacity` when size is known
- Suppressing clippy warnings without good reason
- Public APIs missing `///` documentation
- Consider using `#[must_use]` for return types that may be ignored

**Automatic Check Commands**:
```bash
cargo check                           # Build check
cargo clippy -- -D warnings            # Lints
cargo fmt --check                      # Format check
cargo test                            # Test
cargo audit                           # Security audit (if available)
```

---

## /cpp-review

**Purpose**: Comprehensive C++ code review. Checks memory safety, modern C++ idioms, concurrency, and security.

**Usage**:
```
/cpp-review
```

**Use Cases**:
- After writing or modifying C++ code
- Before committing C++ changes
- Reviewing PRs for C++ projects
- Checking memory safety issues

**Review Categories**:

### CRITICAL (Must Fix)
- Raw `new`/`delete` without RAII
- Buffer overflow and use-after-free
- Data races without synchronization
- Command injection via `system()`
- Uninitialized variable reads
- Null pointer dereference

### HIGH (Should Fix)
- Rule of Five violations
- Missing `std::lock_guard` / `std::scoped_lock`
- Improper lifetime management of detached threads
- C-style casts instead of `static_cast`/`dynamic_cast`
- Missing `const` correctness

### MEDIUM (Consider Fixing)
- Unnecessary copies (pass by value instead of `const&`)
- Missing `reserve()` when size is known
- `using namespace std;` in header files
- Important return values missing `[[nodiscard]]`
- Overly complex template metaprogramming

**Automatic Check Commands**:
```bash
clang-tidy --checks='*,-llvmlibc-*' src/*.cpp -- -std=c++17
cppcheck --enable=all --suppress=missingIncludeSystem src/
cmake --build build -- -Wall -Wextra -Wpedantic
```

---

## /flutter-review

**Purpose**: Flutter/Dart code review. Checks idiomatic patterns, component best practices, state management, performance, accessibility, and security.

**Usage**:
```
/flutter-review
```

**Use Cases**:
- Before submitting PRs with Flutter/Dart changes (after build and tests pass)
- Early discovery of issues after feature implementation
- Reviewing others' Flutter code
- Auditing components, state management components, or service classes
- Before production release

**Review Areas**:

| Area | Severity |
|------|----------|
| Hardcoded keys, plaintext HTTP | CRITICAL |
| Architecture violations, state management anti-patterns | CRITICAL |
| Widget rebuild issues, resource leaks | HIGH |
| Missing `dispose()`, BuildContext after await | HIGH |
| Dart null safety, missing error/loading states | HIGH |
| Const propagation, widget composition | HIGH |
| Expensive work in `build()` | HIGH |
| Accessibility, semantic labels | MEDIUM |
| State transitions missing tests | HIGH |
| Hardcoded strings (l10n) | MEDIUM |

---

## /fastapi-review

**Purpose**: FastAPI application review. Checks architecture, async correctness, dependency injection, Pydantic schemas, security, performance, and testability.

**Usage**:
```
/fastapi-review [file or directory]
```

**Review Areas**:
- App factory, route boundaries, middleware, and exception handlers
- Pydantic request and response schema separation
- Dependency injection for database sessions, authentication, pagination, and settings
- Async database and external HTTP patterns
- CORS, authentication, rate limiting, logging, and key handling
- OpenAPI metadata and documented response models
- Test client setup and dependency overrides

---

## Language Review Commands Comparison

| Command | Language | Key Checks | Severity Levels |
|---------|----------|-----------|-----------------|
| `/python-review` | Python | PEP 8, type hints, SQL injection | CRITICAL/HIGH/MEDIUM |
| `/go-review` | Go | Concurrency safety, error handling, goroutine | CRITICAL/HIGH/MEDIUM |
| `/kotlin-review` | Kotlin | Null safety, coroutines, GlobalScope | CRITICAL/HIGH/MEDIUM |
| `/rust-review` | Rust | Ownership, lifetime, unsafe | CRITICAL/HIGH/MEDIUM |
| `/cpp-review` | C++ | Memory safety, RAII, concurrency | CRITICAL/HIGH/MEDIUM |
| `/flutter-review` | Flutter | State management, accessibility, performance | CRITICAL/HIGH/MEDIUM |
| `/fastapi-review` | Python | Pydantic, DI, async, security | CRITICAL/HIGH/MEDIUM |