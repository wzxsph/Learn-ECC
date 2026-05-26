# Programming Language Skills

This document introduces development skills for various programming languages in the ECC project.

---

## Python Patterns (python-patterns)

### Purpose
Provides Pythonic idiomatic patterns, PEP 8 standards, type hints, and best practices for building robust, efficient, and maintainable Python applications.

### When to Use
- Writing new Python code
- Reviewing Python code
- Refactoring existing Python code
- Designing Python packages/modules

### Core Concepts
1. **Readability first** - Code should be obvious
2. **Explicit over implicit** - Avoid magic operations
3. **EAFP style** - Easier to ask forgiveness than permission
4. **Type hints** - Use type annotations to improve code safety
5. **Context managers** - Use `with` for resource management
6. **List comprehensions** - Simple data transformations
7. **Generators** - Lazy evaluation and large datasets

### Usage Examples

**Type hints:**
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

**Context managers:**
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

**Error handling:**
```python
try:
    parsed = json.loads(data)
except json.JSONDecodeError as e:
    raise ValueError(f"Failed to parse data: {data}") from e
```

---

## Go Patterns (golang-patterns)

### Purpose
Provides idiomatic Go patterns, best practices, and conventions for building robust, efficient, and maintainable Go applications.

### When to Use
- Writing new Go code
- Reviewing Go code
- Refactoring existing Go code
- Designing Go packages/modules

### Core Concepts
1. **Simplicity and clarity** - Go prefers simplicity over cleverness
2. **Zero value is useful** - Design types so their zero value is directly usable
3. **Accept interfaces, return structs** - Functions accept interface parameters, return concrete types
4. **Error handling** - Errors are values, follow `error` return value pattern
5. **Concurrency patterns** - Use goroutines and channels for concurrency
6. **Interface design** - Small and focused interfaces
7. **Package organization** - Standard project layout

### Usage Examples

**Error wrapping:**
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

**Functional options pattern:**
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

### Purpose
Provides idiomatic Rust patterns, ownership system, error handling, traits, concurrency, and best practices for building safe, efficient applications.

### When to Use
- Writing new Rust code
- Reviewing Rust code
- Refactoring existing Rust code
- Designing crate structure and module layout

### Core Concepts
1. **Ownership and borrowing** - Compile-time prevention of data races and memory errors
2. **Result and ? operator** - Use `?` for error propagation
3. **Enums and exhaustive matching** - Use enums to make impossible states unrepresentable
4. **Traits and generics** - Zero-cost abstractions
5. **Safe concurrency** - Use `Arc<Mutex<T>>`, channels, and async/await
6. **Minimize pub surface** - Organize modules by domain

### Usage Examples

**Error handling:**
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

**Using Cow for flexible ownership:**
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

**Arc<Mutex<T>> for shared mutable state:**
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

### Purpose
Provides idiomatic Kotlin patterns, best practices, and conventions for building robust, efficient, and maintainable Kotlin applications, including coroutines, null safety, and DSL builders.

### When to Use
- Writing new Kotlin code
- Reviewing Kotlin code
- Refactoring existing Kotlin code
- Designing Kotlin modules or libraries
- Configuring Gradle Kotlin DSL builds

### Core Concepts
1. **Null safety** - Use type system and safe call operators
2. **Immutable by default** - Prefer `val` over `var`
3. **Data classes** - For types that primarily hold data
4. **Sealed classes** - Exhaustive type hierarchies
5. **Coroutines** - Structured concurrency
6. **Extension functions** - Add functionality without inheritance
7. **DSL builders** - Use `@DslMarker` and lambda receivers

### Usage Examples

**Null safety:**
```kotlin
fun getUserEmail(userId: String): String {
    val user = userRepository.findById(userId)
    return user?.email ?: "unknown@example.com"
}
```

**Sealed class for exhaustive results:**
```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Failure(val error: AppError) : Result<Nothing>()
    data object Loading : Result<Nothing>()
}
```

**Structured concurrency:**
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

### Purpose
Modern C++ (C++17/20/23) coding standards based on C++ Core Guidelines. Enforces type safety, resource safety, immutability, and clarity.

### When to Use
- Writing new C++ code
- Reviewing or refactoring C++ code
- Making architectural decisions
- Enforcing consistent code style
- Choosing language features

### Core Concepts
1. **RAII everywhere** - Bind resource lifetimes to object lifetimes
2. **Immutable by default** - Start with `const`/`constexpr`
3. **Type safety** - Use type system to prevent compile-time errors
4. **Express intent** - Names, types, and concepts should convey purpose
5. **Minimize complexity** - Simple code is correct code
6. **Value semantics over pointer semantics** - Prefer return values and scoped objects

### Usage Examples

**Smart pointer usage:**
```cpp
// R.11 + R.20 + R.21: RAII and smart pointers
auto widget = std::make_unique<Widget>("config");  // Unique ownership
auto cache = std::make_shared<Cache>(1024);         // Shared ownership

// R.3: Raw pointers are non-owning observers
void render(const Widget* w) {  // Does not own w
    if (w) w->draw();
}
```

**Rule of Zero:**
```cpp
// C.20: Let the compiler generate special members
struct Employee {
    std::string name;
    std::string department;
    int id;
    // No need to define destructor, copy/move constructors, or assignment operators
};
```

---

## Java Coding Standards (java-coding-standards)

### Purpose
Java coding standards for Spring Boot and Quarkus services, including naming, immutability, Optional usage, stream processing, exceptions, generics, and project layout.

### When to Use
- Writing or reviewing Java code in Spring Boot or Quarkus projects
- Enforcing naming, immutability, or exception handling conventions
- Using records, sealed classes, or pattern matching (Java 17+)
- Reviewing usage of Optional, streams, or generics

### Core Concepts
1. **Framework detection** - Detect Spring Boot or Quarkus from build files
2. **Naming conventions** - Classes/Records use PascalCase, methods/fields use camelCase
3. **Immutability** - Prefer records and final fields
4. **Optional usage** - Return Optional from find* methods
5. **Dependency injection** - Constructor injection over field injection
6. **Reactive patterns (Quarkus)** - Return Uni/Multi

### Usage Examples

**Spring Boot constructor injection:**
```java
@Service
public class MarketService {
    private final MarketRepository marketRepository;

    public MarketService(MarketRepository marketRepository) {
        this.marketRepository = marketRepository;
    }
}
```

**Optional usage:**
```java
return market
    .map(MarketResponse::from)
    .orElseThrow(() -> new EntityNotFoundException("Market not found"));
```

---

## Swift 6.2 Approachable Concurrency (swift-concurrency-6-2)

### Purpose
Swift 6.2's "approachable concurrency" patterns - single-threaded by default, explicit `@concurrent` for background offloading, protocol support for actor isolation consistency with MainActor-based types.

### When to Use
- Migrating Swift 5.x or 6.0/6.1 projects to Swift 6.2
- Resolving data race safety compiler errors
- Designing MainActor-based application architecture
- Implementing protocol conformance

### Core Concepts
1. **Single-threaded by default** - Most code is data race safe; concurrency is explicit opt-in
2. **Async stays in calling actor** - Eliminates implicit offloading that causes data race errors
3. **Isolation consistency** - MainActor types can safely implement non-isolated protocols
4. **@concurrent explicit opt-in** - Background execution is an intentional performance choice

### Usage Examples

**Isolation consistency:**
```swift
extension StickerModel: @MainActor Exportable {
    func export() {
        photoProcessor.exportAsPNG()
    }
}
```

**@concurrent for background work:**
```swift
nonisolated final class PhotoProcessor {
    @concurrent
    static func extractSubject(from data: Data) async -> Sticker {
        // Expensive work done in background
    }
}
```

---

## Dart/Flutter Patterns (dart-flutter-patterns)

### Purpose
Production-ready Dart and Flutter patterns covering null safety, immutable state, async composition, component architecture, popular state management frameworks (BLoC, Riverpod, Provider), GoRouter navigation, Dio HTTP requests, Freezed code generation, and clean architecture.

### When to Use
- Starting new Flutter features needing patterns for state management, navigation, or data access
- Reviewing or writing Dart code needing guidance on null safety, sealed types, or async composition
- Setting up new Flutter projects choosing between BLoC, Riverpod, or Provider
- Implementing secure HTTP clients, WebView integration, or local storage
- Writing tests for Flutter components, Cubits, or Riverpod providers

### Core Concepts
1. **Null safety** - Avoid `!`, prefer `?.`/`??`/pattern matching
2. **Immutable state** - Sealed classes, `freezed`, `copyWith`
3. **Async composition** - Concurrent `Future.wait`, safe BuildContext usage after await
4. **Component architecture** - Extract as classes (not methods), `const` propagation, scoped rebuilds
5. **State management** - BLoC/Cubit events, Riverpod notifiers, and derived providers

### Usage Examples

**Sealed state classes:**
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

**Riverpod derived provider:**
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

### Purpose
F# testing patterns using xUnit, FsUnit, Unquote, FsCheck property testing, integration testing, and test organization best practices.

### When to Use
- Writing new tests for F# code
- Reviewing test quality and coverage
- Setting up test infrastructure for F# projects
- Debugging flaky or slow tests

### Core Concepts
1. **xUnit** - Standard .NET ecosystem testing framework
2. **FsUnit.xUnit** - F#-friendly assertion syntax for xUnit
3. **Unquote** - Assertion library using F# quotations, clear failure messages
4. **FsCheck.xUnit** - Property testing integrated into xUnit

### Usage Examples

**Unquote assertions:**
```fsharp
[<Fact>]
let ``order total sums item prices`` () =
    let items = [ { Sku = "A"; Quantity = 2; Price = 10m }
                  { Sku = "B"; Quantity = 1; Price = 5m } ]
    let total = Order.calculateTotal items
    test <@ total = 25m @>
```

**FsCheck property testing:**
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

| Skill | Primary Use | Key Patterns |
|-------|-------------|---------------|
| python-patterns | Python development | Type hints, context managers, list comprehensions |
| golang-patterns | Go development | Error wrapping, worker pool, interface design |
| rust-patterns | Rust development | Ownership, Result, traits, concurrency |
| kotlin-patterns | Kotlin development | Null safety, coroutines, DSL builders |
| cpp-coding-standards | C++ development | RAII, smart pointers, concepts |
| java-coding-standards | Java development | Dependency injection, Optional, stream processing |
| swift-concurrency-6-2 | Swift concurrency | @MainActor, @concurrent, isolation consistency |
| dart-flutter-patterns | Flutter development | State management, routing, component architecture |
| fsharp-testing | F# testing | xUnit, FsUnit, property testing |