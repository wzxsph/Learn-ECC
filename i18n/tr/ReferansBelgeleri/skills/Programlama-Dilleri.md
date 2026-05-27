# Programlama Dilleri技能 (Programming Language Skills)

本文档介绍ECC项目中Kullanılan各种Programlama Dilleri的开发技能。

---

## Python Patterns (python-patterns)

### KullanımAçıklama
SağlaPythonic惯用模式、PEP 8标准、Türİpucu和En İyi Uygulamalar，Kullanılan构建健壮、高效和可维护的Python应用程序。

### Kullanım Zamanı
- 编写新的Python代码
- 审查Python代码
- 重构现有Python代码
- TasarımPython包/模块

### Temel Kavramlar
1. **可读性优先** - 代码Olmalı清晰明显
2. **显式优于隐式** - 避免魔法操作
3. **EAFP风格** - 更容易请求原谅而不是许可
4. **Türİpucu** - 使用Tür注解提高代码Güvenlik性
5. **上下文Yönet器** - 使用`with`进行资源Yönet
6. **列表推导式** - 简单的数据转换
7. **生成器** - 惰性求值和大数据集

### 使用Örnek

**Türİpucu：**
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

**上下文Yönet器：**
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

**Hata Yönetimi：**
```python
try:
    parsed = json.loads(data)
except json.JSONDecodeError as e:
    raise ValueError(f"Failed to parse data: {data}") from e
```

---

## Go Patterns (golang-patterns)

### KullanımAçıklama
Sağla惯用的Go模式、En İyi Uygulamalar和约定，Kullanılan构建健壮、高效和可维护的Go应用程序。

### Kullanım Zamanı
- 编写新的Go代码
- 审查Go代码
- 重构现有Go代码
- TasarımGo包/模块

### Temel Kavramlar
1. **简洁和清晰** - Go偏好简洁而不是技巧
2. **零值有用** - TasarımTür使其零值可直接使用
3. **接受接口，Geri Dön结构体** - 函数接受接口参数，Geri Dön具体Tür
4. **Hata Yönetimi** - 错误是值，遵循`error`Geri Dön值模式
5. **并发模式** - 使用goroutine和channel进行并发
6. **接口Tasarım** - 小而专注的接口
7. **包组织** - 标准项目布局

### 使用Örnek

**错误包装：**
```go
func LoadConfig(path string) (*Config, error) {
    data, err := os.ReadFile(path)
    if err != nil {
        return nil, fmt.Errorf("load config %s: %w", path, err)
    }
    // ...
}
```

**Worker Pool：**
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

**功能选项模式：**
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

### KullanımAçıklama
Sağla惯用的Rust模式、所有权系统、Hata Yönetimi、trait、并发和En İyi Uygulamalar，Kullanılan构建Güvenlik、高效的应用程序。

### Kullanım Zamanı
- 编写新的Rust代码
- 审查Rust代码
- 重构现有Rust代码
- Tasarımcrate结构和模块布局

### Temel Kavramlar
1. **所有权和借用** - 编译时防止数据竞争和内存错误
2. **Result和?操作符** - 使用`?`进行错误传播
3. **枚举和穷尽匹配** - 使用枚举使不可能的状态无法表示
4. **Trait和泛型** - 零成本抽象
5. **Güvenlik并发** - 使用`Arc<Mutex<T>>`、channels和async/await
6. **最小pub表面** - 按领域组织模块

### 使用Örnek

**Hata Yönetimi：**
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

**使用Cow灵活处理所有权：**
```rust
use std::borrow::Cow;

fn normalize(input: &str) -> Cow<'_, str> {
    if input.contains(' ') {
        Cow::Owned(input.replace(' ', "_"))
    } else {
        Cow::Borrowed(input) // 无需修改时零成本
    }
}
```

**Arc<Mutex<T>>共享可变状态：**
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

### KullanımAçıklama
Sağla惯用的Kotlin模式、En İyi Uygulamalar和约定，Kullanılan构建健壮、高效和可维护的Kotlin应用程序，İçerir协程、空Güvenlik和DSL构建器。

### Kullanım Zamanı
- 编写新的Kotlin代码
- 审查Kotlin代码
- 重构现有Kotlin代码
- TasarımKotlin模块或库
- 配置Gradle Kotlin DSL构建

### Temel Kavramlar
1. **空Güvenlik** - 使用Tür系统和Güvenlik调用操作符
2. **默认不可变** - 优先使用`val`而不是`var`
3. **数据类** - Kullanılan主要持有数据的Tür
4. **密封类** - 穷尽Tür层次结构
5. **协程** - 结构化并发
6. **扩展函数** - 无需继承添加功能
7. **DSL构建器** - 使用`@DslMarker`和lambda接收器

### 使用Örnek

**空Güvenlik：**
```kotlin
fun getUserEmail(userId: String): String {
    val user = userRepository.findById(userId)
    return user?.email ?: "unknown@example.com"
}
```

**密封类Kullanılan穷尽结果：**
```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Failure(val error: AppError) : Result<Nothing>()
    data object Loading : Result<Nothing>()
}
```

**结构化并发：**
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

### KullanımAçıklama
基于C++核心指南的现代C++（C++17/20/23）编码标准。强制TürGüvenlik、资源Güvenlik、不可变性和清晰度。

### Kullanım Zamanı
- 编写新的C++代码
- 审查或重构C++代码
- 做出架构决策
- 强制一致的Kod Stili
- 选择语言特性

### Temel Kavramlar
1. **RAII无处不在** - 绑定资源生命周期到对象生命周期
2. **默认不可变** - 从`const`/`constexpr`Başla
3. **TürGüvenlik** - 使用Tür系统防止编译时错误
4. **表达意图** - Ad、Tür和概念应传达目的
5. **最小化复杂度** - 简单代码是正确的代码
6. **值语义优先于指针语义** - 优先Geri Dön值和作用域对象

### 使用Örnek

**智能指针使用：**
```cpp
// R.11 + R.20 + R.21: RAII和智能指针
auto widget = std::make_unique<Widget>("config");  // 唯一所有权
auto cache = std::make_shared<Cache>(1024);         // 共享所有权

// R.3: 原始指针是非拥有的观察者
void render(const Widget* w) {  // 不拥有w
    if (w) w->draw();
}
```

**Rule of Zero：**
```cpp
// C.20: 让编译器生成特殊成员
struct Employee {
    std::string name;
    std::string department;
    int id;
    // 无需定义析构函数、复制/移动构造函数或赋值运算符
};
```

---

## Java Coding Standards (java-coding-standards)

### KullanımAçıklama
Spring Boot和Quarkus服务的Java编码标准，İçerir命名、不可变性、Optional使用、流处理、异常、泛型和项目布局。

### Kullanım Zamanı
- 在Spring Boot或Quarkus项目中编写或审查Java代码
- 强制命名、不可变性或异常处理约定
- 使用records、密封类或模式匹配（Java 17+）
- 审查Optional、流或泛型的使用

### Temel Kavramlar
1. **Çerçeveler检测** - 根据构建文件检测Spring Boot或Quarkus
2. **命名约定** - 类/Records用PascalCase，方法/字段用camelCase
3. **不可变性** - 优先使用records和final字段
4. **Optional使用** - 从find*方法Geri DönOptional
5. **依赖注入** - 构造函数注入优于字段注入
6. **响应式模式（Quarkus）** - Geri DönUni/Multi

### 使用Örnek

**Spring Boot构造函数注入：**
```java
@Service
public class MarketService {
    private final MarketRepository marketRepository;

    public MarketService(MarketRepository marketRepository) {
        this.marketRepository = marketRepository;
    }
}
```

**Optional使用：**
```java
return market
    .map(MarketResponse::from)
    .orElseThrow(() -> new EntityNotFoundException("Market not found"));
```

---

## Swift 6.2 Approachable Concurrency (swift-concurrency-6-2)

### KullanımAçıklama
Swift 6.2的"可接近并发"模式——默认单线程，使用`@concurrent`显式后台卸载，Vasıtasıyla隔离一致性为主actorTürSağla协议Destekle。

### Kullanım Zamanı
- 将Swift 5.x或6.0/6.1项目迁移到Swift 6.2
- 解决数据竞争Güvenlik编译器错误
- TasarımMainActor-based应用架构
- Gerçekleştir协议一致性

### Temel Kavramlar
1. **单线程默认** - 大多数代码是数据竞争Güvenlik的；并发是显式 opt-in
2. **Async保持在调用actor** - 消除导致数据竞争错误的隐式卸载
3. **隔离一致性** - MainActorTürOlabilirGüvenlik地Gerçekleştir非隔离协议
4. **@concurrent显式opt-in** - 后台执行是有意的性能选择

### 使用Örnek

**隔离一致性：**
```swift
extension StickerModel: @MainActor Exportable {
    func export() {
        photoProcessor.exportAsPNG()
    }
}
```

**@concurrentKullanılan后台工作：**
```swift
nonisolated final class PhotoProcessor {
    @concurrent
    static func extractSubject(from data: Data) async -> Sticker {
        // 后台执行的昂贵工作
    }
}
```

---

## Dart/Flutter Patterns (dart-flutter-patterns)

### KullanımAçıklama
生产就绪的Dart和Flutter模式，涵盖空Güvenlik、不可变状态、异步组合、组件架构、流行状态YönetÇerçeveler（BLoC、Riverpod、Provider）、GoRouter导航、Dio网络请求、Freezed代码生成和清洁架构。

### Kullanım Zamanı
- 启动新的Flutter功能并Gerekli状态Yönet、导航或数据访问的模式
- 审查或编写Dart代码并Gerekli空Güvenlik、密封Tür或异步组合的指导
- 设置新的Flutter项目并选择BLoC、Riverpod或Provider
- GerçekleştirGüvenlik的HTTP客户端、WebView集成或本地存储
- 编写Flutter组件、Cubit或Riverpod provider的Test

### Temel Kavramlar
1. **空Güvenlik** - 避免`!`，优先使用`?.`/`??`/模式匹配
2. **不可变状态** - 密封类、`freezed`、`copyWith`
3. **异步组合** - 并发`Future.wait`、await后Güvenlik使用BuildContext
4. **组件架构** - 提取为类（不是方法）、`const`传播、作用域重建
5. **状态Yönet** - BLoC/Cubit事件、Riverpod notifiers和派生providers

### 使用Örnek

**密封状态类：**
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

**Riverpod派生provider：**
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

### KullanımAçıklama
使用xUnit、FsUnit、Unquote、FsCheck属性Test的F#Test模式，集成Test和Test组织En İyi Uygulamalar。

### Kullanım Zamanı
- 为F#代码编写新Test
- 审查Test质量和覆盖率
- 为F#项目设置Test基础设施
- 调试不稳定或慢速Test

### Temel Kavramlar
1. **xUnit** - 标准.NET生态系统TestÇerçeveler
2. **FsUnit.xUnit** - xUnit的F#友好断言语法
3. **Unquote** - 使用F#引用的断言库，失败消息清晰
4. **FsCheck.xUnit** - 集成到xUnit的属性Test

### 使用Örnek

**Unquote断言：**
```fsharp
[<Fact>]
let ``order total sums item prices`` () =
    let items = [ { Sku = "A"; Quantity = 2; Price = 10m }
                  { Sku = "B"; Quantity = 1; Price = 5m } ]
    let total = Order.calculateTotal items
    test <@ total = 25m @>
```

**FsCheck属性Test：**
```fsharp
[<Property>]
let ``order total is always non-negative`` (items: NonEmptyList<PositiveInt * decimal>) =
    let orderItems = items.Get |> List.map (fun (qty, price) ->
        { Sku = "SKU"; Quantity = qty.Get; Price = abs price })
    let total = Order.calculateTotal orderItems
    total >= 0m
```

---

## 其他语言模式

### C# Testing (csharp-testing)
- xUnit + FluentAssertions
- Mocking with Moq/NSubstitute
- Integration tests with Testcontainers

### Perl Patterns (perl-patterns)
- Modern Perl syntax
- Moose objects
- DBIx::Class ORM

---

## Hızlı Referans表

| 技能 | 主要Kullanım | 关键模式 |
|------|---------|---------|
| python-patterns | Python开发 | Türİpucu、上下文Yönet器、列表推导式 |
| golang-patterns | Go开发 | 错误包装、Worker Pool、接口Tasarım |
| rust-patterns | Rust开发 | 所有权、Result、trait、并发 |
| kotlin-patterns | Kotlin开发 | 空Güvenlik、协程、DSL构建器 |
| cpp-coding-standards | C++开发 | RAII、智能指针、概念 |
| java-coding-standards | Java开发 | 依赖注入、Optional、流处理 |
| swift-concurrency-6-2 | Swift并发 | @MainActor、@concurrent、隔离一致性 |
| dart-flutter-patterns | Flutter开发 | 状态Yönet、路由、组件架构 |
| fsharp-testing | F#Test | xUnit、FsUnit、属性Test |