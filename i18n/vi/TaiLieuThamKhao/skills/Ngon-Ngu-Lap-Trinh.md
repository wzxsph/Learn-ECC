# Ngôn Ngữ Lập Trình

Tài liệu này giới thiệu development skill cho various programming languages trong ECC project.

---

## Python Patterns (python-patterns)

### Mục đích
Cung cấp Pythonic idiomatic pattern, PEP 8 standard, type hint và best practice để build robust, efficient và maintainable Python application.

### Khi nào sử dụng
- Viết new Python code
- Review Python code
- Refactor existing Python code
- Design Python package/module

### Core concept
1. **Readability First** - Code nên rõ ràng và obvious
2. **Explicit Over Implicit** - Tránh magic operation
3. **EAFP Style** - Easier to ask for forgiveness than permission
4. **Type Hint** - Sử dụng type annotation improve code safety
5. **Context Manager** - Sử dụng `with` cho resource management
6. **List Comprehension** - Simple data transformation
7. **Generator** - Lazy evaluation và large dataset

### Usage Example

**Type Hint:**
```python
from typing import Optional, List, Dict, Any

def process_user(
    user_id: str,
    data: Dict[str, Any],
    active: bool = True
) -> Optional[User]:
    if not active:
        return None
    return User(user_id, data)
```

**Context Manager:**
```python
from contextlib import contextmanager

@contextmanager
def timer(name: str):
    start = time.perf_counter()
    yield
    elapsed = time.perf_counter() - start
    print(f"{name} took {elapsed:.4f} seconds")

with timer("data processing"):
    process_large_dataset()
```

**Error Handling:**
```python
try:
    parsed = json.loads(data)
except json.JSONDecodeError as e:
    raise ValueError(f"Failed to parse data: {data}") from e
```

---

## Go Patterns (golang-patterns)

### Mục đích
Cung cấp idiomatic Go pattern, best practice và convention để build robust, efficient và maintainable Go application.

### Khi nào sử dụng
- Viết new Go code
- Review Go code
- Refactor existing Go code
- Design Go package/module

### Core concept
1. **Simplicity and Clarity** - Go prefer simplicity over cleverness
2. **Zero Value Useful** - Design type sao cho zero value có thể dùng được ngay
3. **Accept Interface, Return Struct** - Function accept interface parameter, return concrete type
4. **Error Handling** - Error is value, follow `error` return value pattern
5. **Concurrency Pattern** - Sử dụng goroutine và channel cho concurrency
6. **Interface Design** - Small và focused interface
7. **Package Organization** - Standard project layout

### Usage Example

**Error Wrapping:**
```go
func LoadConfig(path string) (*Config, error) {
    data, err := os.ReadFile(path)
    if err != nil {
        return nil, fmt.Errorf("load config %s: %w", path, err)
    }
    // ...
}
```

**Worker Pool:**
```go
func WorkerPool(jobs <-chan Job, results chan<- Result, numWorkers int) {
    var wg sync.WaitGroup
    for i := 0; i < numWorkers; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for job := range jobs {
                results <- process(job)
            }
        }()
    }
    wg.Wait()
}
```

**Functional Option Pattern:**
```go
type Option func(*Server)

func WithTimeout(d time.Duration) Option {
    return func(s *Server) { s.timeout = d }
}

func NewServer(addr string, opts ...Option) *Server {
    s := &Server{addr: addr, timeout: 30 * time.Second}
    for _, opt := range opts {
        opt(s)
    }
    return s
}
```

---

## Rust Patterns (rust-patterns)

### Mục đích
Cung cấp idiomatic Rust pattern, ownership system, error handling, trait, concurrency và best practice để build safe, efficient application.

### Khi nào sử dụng
- Viết new Rust code
- Review Rust code
- Refactor existing Rust code
- Design crate structure và module layout

### Core concept
1. **Ownership và Borrowing** - Compile-time prevent data race và memory error
2. **Result và ? Operator** - Sử dụng `?` cho error propagation
3. **Enum và Exhaustive Match** - Sử dụng enum làm impossible state unrepresentable
4. **Trait và Generic** - Zero-cost abstraction
5. **Safe Concurrency** - Sử dụng `Arc<Mutex<T>>`, channel và async/await
6. **Minimal pub Surface** - Organize module by domain

### Usage Example

**Error Handling:**
```rust
use anyhow::{Context, Result};

fn load_config(path: &str) -> Result<Config> {
    let content = std::fs::read_to_string(path)
        .with_context(|| format!("failed to read config from {path}"))?;
    let config: Config = toml::from_str(&content)
        .with_context(|| format!("failed to parse config from {path}"))?;
    Ok(config)
}
```

**Using Cow for Flexible Ownership:**
```rust
use std::borrow::Cow;

fn normalize(input: &str) -> Cow<'_, str> {
    if input.contains(' ') {
        Cow::Owned(input.replace(' ', "_"))
    } else {
        Cow::Borrowed(input) // Zero-cost when no modification needed
    }
}
```

**Arc<Mutex<T>> Shared Mutable State:**
```rust
use std::sync::{Arc, Mutex};

let counter = Arc::new(Mutex::new(0));
let handles: Vec<_> = (0..10).map(|_| {
    let counter = Arc::clone(&counter);
    std::thread::spawn(move || {
        let mut num = counter.lock().expect("mutex poisoned");
        *num += 1;
    })
}).collect();
```

---

## Kotlin Patterns (kotlin-patterns)

### Mục đích
Cung cấp idiomatic Kotlin pattern, best practice và convention để build robust, efficient và maintainable Kotlin application, bao gồm coroutine, null safety và DSL builder.

### Khi nào sử dụng
- Viết new Kotlin code
- Review Kotlin code
- Refactor existing Kotlin code
- Design Kotlin module hoặc library
- Configure Gradle Kotlin DSL build

### Core concept
1. **Null Safety** - Sử dụng type system và safe call operator
2. **Default Immutable** - Prefer `val` over `var`
3. **Data Class** - Dùng cho type chủ yếu hold data
4. **Sealed Class** - Exhaust type hierarchy
5. **Coroutine** - Structured concurrency
6. **Extension Function** - Thêm function mà không cần inherit
7. **DSL Builder** - Sử dụng `@DslMarker` và lambda receiver

### Usage Example

**Null Safety:**
```kotlin
fun getUserEmail(userId: String): String {
    val user = userRepository.findById(userId)
    return user?.email ?: "unknown@example.com"
}
```

**Sealed Class for Exhaust Results:**
```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Failure(val error: AppError) : Result<Nothing>()
    data object Loading : Result<Nothing>()
}
```

**Structured Concurrency:**
```kotlin
suspend fun fetchUserWithPosts(userId: String): UserProfile =
    coroutineScope {
        val user = async { userService.getUser(userId) }
        val posts = async { postService.getUserPosts(userId) }
        UserProfile(user = user.await(), posts = posts.await())
    }
```

---

## C++ Coding Standards (cpp-coding-standards)

### Mục đích
Modern C++ (C++17/20/23) coding standard dựa trên C++ Core Guidelines. Enforce type safety, resource safety, immutability và clarity.

### Khi nào sử dụng
- Viết new C++ code
- Review hoặc refactor C++ code
- Make architectural decision
- Enforce consistent code style
- Choose language feature

### Core concept
1. **RAII Everywhere** - Bind resource lifecycle to object lifecycle
2. **Default Immutable** - Start from `const`/`constexpr`
3. **Type Safety** - Use type system prevent compile-time error
4. **Express Intent** - Name, type và concept nên convey purpose
5. **Minimize Complexity** - Simple code is correct code
6. **Value Semantics Over Pointer Semantics** - Prefer return value và scope object

### Usage Example

**Smart Pointer Usage:**
```cpp
// R.11 + R.20 + R.21: RAII và smart pointer
auto widget = std::make_unique<Widget>("config");  // Unique ownership
auto cache = std::make_shared<Cache>(1024);         // Shared ownership

// R.3: Raw pointer is non-owning observer
void render(const Widget* w) {  // Does not own w
    if (w) w->draw();
}
```

**Rule of Zero:**
```cpp
// C.20: Let compiler generate special member
struct Employee {
    std::string name;
    std::string department;
    int id;
    // No need to define destructor, copy/move constructor or assignment operator
};
```

---

## Java Coding Standards (java-coding-standards)

### Mục đích
Java coding standard cho Spring Boot và Quarkus service, bao gồm naming, immutability, Optional usage, stream processing, exception, generic và project layout.

### Khi nào sử dụng
- Viết hoặc review Java code trong Spring Boot hoặc Quarkus project
- Enforce naming, immutability hoặc exception convention
- Sử dụng record, sealed class hoặc pattern matching (Java 17+)
- Review Optional, stream hoặc generic usage

### Core concept
1. **Framework Detection** - Detect Spring Boot hoặc Quarkus theo build file
2. **Naming Convention** - Class/Record dùng PascalCase, method/field dùng camelCase
3. **Immutability** - Prefer record và final field
4. **Optional Usage** - Return Optional từ find* method
5. **Dependency Injection** - Constructor injection over field injection
6. **Reactive Pattern (Quarkus)** - Return Uni/Multi

### Usage Example

**Spring Boot Constructor Injection:**
```java
@Service
public class MarketService {
    private final MarketRepository marketRepository;

    public MarketService(MarketRepository marketRepository) {
        this.marketRepository = marketRepository;
    }
}
```

**Optional Usage:**
```java
return market
    .map(MarketResponse::from)
    .orElseThrow(() -> new EntityNotFoundException("Market not found"));
```

---

## Swift 6.2 Approachable Concurrency (swift-concurrency-6-2)

### Mục đích
Swift 6.2 "approachable concurrency" pattern - single-threaded default, sử dụng `@concurrent` explicit background offload, provide protocol conformance for actor types through isolation consistency.

### Khi nào sử dụng
- Migrate Swift 5.x hoặc 6.0/6.1 project sang Swift 6.2
- Resolve data race safety compiler error
- Design MainActor-based application architecture
- Implement protocol conformance

### Core concept
1. **Single-Threaded Default** - Most code is data race safe; concurrency is explicit opt-in
2. **Async Stays on Calling Actor** - Eliminate implicit offload that cause data race error
3. **Isolation Consistency** - MainActor type can safely implement non-isolated protocol
4. **`@concurrent` Explicit Opt-in** - Background execution là intentional performance choice

### Usage Example

**Isolation Consistency:**
```swift
extension StickerModel: @MainActor Exportable {
    func export() {
        photoProcessor.exportAsPNG()
    }
}
```

**`@concurrent` for Background Work:**
```swift
nonisolated final class PhotoProcessor {
    @concurrent
    static func extractSubject(from data: Data) async -> Sticker {
        // Expensive work executed in background
    }
}
```

---

## Dart/Flutter Patterns (dart-flutter-patterns)

### Mục đích
Production-ready Dart và Flutter pattern, cover null safety, immutable state, async composition, component architecture, popular state management framework (BLoC, Riverpod, Provider), GoRouter navigation, Dio network request, Freezed code generation và clean architecture.

### Khi nào sử dụng
- Start new Flutter feature và need pattern cho state management, navigation hoặc data access
- Review hoặc write Dart code và need guidance về null safety, sealed type hoặc async composition
- Setup new Flutter project và chọn BLoC, Riverpod hoặc Provider
- Implement secure HTTP client, WebView integration hoặc local storage
- Write test cho Flutter component, Cubit hoặc Riverpod provider

### Core concept
1. **Null Safety** - Tránh `!`, prefer `?.`/`??`/pattern matching
2. **Immutable State** - Sealed class, `freezed`, `copyWith`
3. **Async Composition** - Concurrent `Future.wait`, safe BuildContext usage after await
4. **Component Architecture** - Extract thành class (không phải method), `const` propagation, scoped rebuild
5. **State Management** - BLoC/Cubit event, Riverpod notifier và derived provider

### Usage Example

**Sealed State Class:**
```dart
sealed class AsyncState<T> {}
final class Loading<T> extends AsyncState<T> {}
final class Success<T> extends AsyncState<T> { final T data; const Success(this.data); }
final class Failure<T> extends AsyncState<T> { final Object error; const Failure(this.error); }

Widget buildFrom(AsyncState<User> state) => switch (state) {
    UserInitial() => const SizedBox.shrink(),
    UserLoading() => const CircularProgressIndicator(),
    UserLoaded(:final user) => UserCard(user: user),
    UserError(:final message) => ErrorText(message),
};
```

**Riverpod Derived Provider:**
```dart
@riverpod
double cartTotal(Ref ref) {
    final cart = ref.watch(cartNotifierProvider);
    final products = ref.watch(productsProvider).valueOrNull ?? [];
    return cart.fold(0.0, (total, item) {
        final product = products.firstWhereOrNull((p) => p.id == item.productId);
        return total + (product?.price ?? 0) * item.quantity;
    });
}
```

---

## F# Testing (fsharp-testing)

### Mục đích
F# testing pattern sử dụng xUnit, FsUnit, Unquote, FsCheck property test, integration test và test organization best practice.

### Khi nào sử dụng
- Write new test cho F# code
- Review test quality và coverage
- Setup test infrastructure cho F# project
- Debug flaky hoặc slow test

### Core concept
1. **xUnit** - Standard .NET ecosystem test framework
2. **FsUnit.xUnit** - F# friendly assertion syntax cho xUnit
3. **Unquote** - Assertion library sử dụng F# quotation, clear failure message
4. **FsCheck.xUnit** - Property testing integrated into xUnit

### Usage Example

**Unquote Assertion:**
```fsharp
[<Fact>]
let ``order total sums item prices`` () =
    let items = [ { Sku = "A"; Quantity = 2; Price = 10m }
                  { Sku = "B"; Quantity = 1; Price = 5m } ]
    let total = Order.calculateTotal items
    test <@ total = 25m @>
```

**FsCheck Property Test:**
```fsharp
[<Property>]
let ``order total is always non-negative`` (items: NonEmptyList<PositiveInt * decimal>) =
    let orderItems = items.Get |> List.map (fun (qty, price) ->
        { Sku = "SKU"; Quantity = qty.Get; Price = abs price })
    let total = Order.calculateTotal orderItems
    total >= 0m
```

---

## Other Language Patterns

### C# Testing (csharp-testing)
- xUnit + FluentAssertions
- Mocking with Moq/NSubstitute
- Integration tests with Testcontainers

### Perl Patterns (perl-patterns)
- Modern Perl syntax
- Moose objects
- DBIx::Class ORM

---

## Quick Reference Table

| Skill | Main Usage | Key Pattern |
|------|---------|---------|
| python-patterns | Python development | Type hint, context manager, list comprehension |
| golang-patterns | Go development | Error wrapping, Worker Pool, interface design |
| rust-patterns | Rust development | Ownership, Result, trait, concurrency |
| kotlin-patterns | Kotlin development | Null safety, coroutine, DSL builder |
| cpp-coding-standards | C++ development | RAII, smart pointer, concept |
| java-coding-standards | Java development | Dependency injection, Optional, stream processing |
| swift-concurrency-6-2 | Swift concurrency | @MainActor, @concurrent, isolation consistency |
| dart-flutter-patterns | Flutter development | State management, routing, component architecture |
| fsharp-testing | F# testing | xUnit, FsUnit, property test |