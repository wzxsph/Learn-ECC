# Habilidades de Linguagens de Programação

Este documento apresenta habilidades de desenvolvimento para várias Linguagens-de-Programação no projeto ECC.

---

## Python Patterns (python-patterns)

### Propósito
Fornecer padrões Python idiomáticos, padrões PEP 8, type hints e Melhores-Práticas para construir aplicações Python robustas, eficientes e mantíveis.

### Quando Usar
- Escrever novo código Python
- Revisar código Python
- Refatorar código Python existente
- Projetar pacotes/módulos Python

### Conceitos Centrais
1. **Legibilidade Primeiro** - Código deve ser claro e óbvio
2. **Explícito Melhor que Implícito** - Evitar mágicas
3. **Estilo EAFP** - Easier to ask forgiveness than permission
4. **Type Hints** - Usar anotações de tipo para maior segurança de código
5. **Context Managers** - Usar `with` para gerenciamento de recursos
6. **List Comprehensions** - Transformações de dados simples
7. **Generators** - Avaliação lazy e grandes datasets

### Exemplo de Uso

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

**Tratamento de erros:**
```python
try:
    parsed = json.loads(data)
except json.JSONDecodeError as e:
    raise ValueError(f"Failed to parse data: {data}") from e
```

---

## Go Patterns (golang-patterns)

### Propósito
Fornecer padrões Go idiomáticos, Melhores-Práticas e convenções para construir aplicações Go robustas, eficientes e mantíveis.

### Quando Usar
- Escrever novo código Go
- Revisar código Go
- Refatorar código Go existente
- Projetar pacotes/módulos Go

### Conceitos Centrais
1. **Simplicidade e Clareza** - Go prefere simplicidade sobre esperteza
2. **Zero Valor Útil** - Projetar tipos para que zero value seja utilizável
3. **Aceitar Interfaces, Retornar Structs** - Funções aceitam interfaces, retornam tipos concretos
4. **Tratamento de Erros** - Erros são valores, seguir padrão de retorno de `error`
5. **Padrões de Concorrência** - Usar goroutines e channels para concorrência
6. **Design de Interfaces** - Interfaces pequenas e focadas
7. **Organização de Pacotes** - Layout padrão de projeto

### Exemplo de Uso

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

### Propósito
Fornecer padrões Rust idiomáticos, sistema de ownership, tratamento de erros, traits, concorrência e Melhores-Práticas para construir aplicações seguras e eficientes.

### Quando Usar
- Escrever novo código Rust
- Revisar código Rust
- Refatorar código Rust existente
- Projetar estrutura de crates e layout de módulos

### Conceitos Centrais
1. **Ownership e Borrowing** - Compilador previne corridas de dados e erros de memória em tempo de compilação
2. **Result e ? Operator** - Usar `?` para propagação de erros
3. **Enums e Match Exhaustive** - Usar enums para tornar estados impossíveis inexpressáveis
4. **Traits e Generics** - Abstrações de custo zero
5. **Concorrência Segura** - Usar `Arc<Mutex<T>>`, channels e async/await
6. **Mínimo Pub Surface** - Organizar módulos por domínio

### Exemplo de Uso

**Tratamento de erros:**
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

**Usando Cow para flexibilidade de ownership:**
```rust
use std::borrow::Cow;

fn normalize(input: &str) -> Cow<'_, str> {
    if input.contains(' ') {
        Cow::Owned(input.replace(' ', "_"))
    } else {
        Cow::Borrowed(input) // Sem custo quando não precisa modificar
    }
}
```

**Arc<Mutex<T>> compartilhando estado mutável:**
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

### Propósito
Fornecer padrões Kotlin idiomáticos, Melhores-Práticas e convenções para construir aplicações Kotlin robustas, eficientes e mantíveis, incluindo coroutines, null safety e DSL builders.

### Quando Usar
- Escrever novo código Kotlin
- Revisar código Kotlin
- Refatorar código Kotlin existente
- Projetar módulos ou bibliotecas Kotlin
- Configurar builds Gradle Kotlin DSL

### Conceitos Centrais
1. **Null Safety** - Usar sistema de tipos e operadores de chamada segura
2. **Imutável por Padrão** - Preferir `val` ao invés de `var`
3. **Data Classes** - Para tipos que principalmente armazenam dados
4. **Sealed Classes** - Hierarquias de tipos exhaustivas
5. **Coroutines** - Concorrência estruturada
6. **Extension Functions** - Adicionar funcionalidades sem herança
7. **DSL Builders** - Usar `@DslMarker` e lambda receivers

### Exemplo de Uso

**Null safety:**
```kotlin
fun getUserEmail(userId: String): String {
    val user = userRepository.findById(userId)
    return user?.email ?: "unknown@example.com"
}
```

**Sealed classes para resultados exhaustivos:**
```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Failure(val error: AppError) : Result<Nothing>()
    data object Loading : Result<Nothing>()
}
```

**Concorrência estruturada:**
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

### Propósito
Padrões de codificação C++ modernos baseados nas C++ Core Guidelines. Enforcement de segurança de tipos, segurança de recursos, immutabilidade e clareza.

### Quando Usar
- Escrever novo código C++
- Revisar ou refatorar código C++
- Tomar decisões de arquitetura
- Aplicar convenções consistentes de estilo de código
- Selecionar features de linguagem

### Conceitos Centrais
1. **RAII em todo lugar** - Vincular ciclo de vida de recursos ao ciclo de vida de objetos
2. **Imutável por Padrão** - Começar com `const`/`constexpr`
3. **Segurança de Tipos** - Usar sistema de tipos para prevenir erros em tempo de compilação
4. **Expressar Intenção** - Nomes, tipos e conceitos devem comunicar propósito
5. **Minimizar Complexidade** - Código simples é código correto
6. **Preferir Semântica de Valor sobre Semântica de Ponteiro** - Preferir retornar valores e objetos com escopo

### Exemplo de Uso

**Uso de smart pointers:**
```cpp
// R.11 + R.20 + R.21: RAII e smart pointers
auto widget = std::make_unique<Widget>("config");  // Unique ownership
auto cache = std::make_shared<Cache>(1024);         // Shared ownership

// R.3: Ponteiros crus são observadores não proprietários
void render(const Widget* w) {  // Não é proprietária de w
    if (w) w->draw();
}
```

**Rule of Zero:**
```cpp
// C.20: Deixar o compilador gerar membros especiais
struct Employee {
    std::string name;
    std::string department;
    int id;
    // Não precisa definir destrutor, copy/move constructors ou operadores de atribuição
};
```

---

## Java Coding Standards (java-coding-standards)

### Propósito
Padrões de codificação Java para serviços Spring Boot e Quarkus, incluindo nomenclatura, immutabilidade, uso de Optional, processamento de streams, exceções, generics e layout de projetos.

### Quando Usar
- Escrever ou revisar código Java em projetos Spring Boot ou Quarkus
- Aplicar convenções de nomenclatura, immutabilidade ou tratamento de exceções
- Usar records, sealed classes ou pattern matching (Java 17+)
- Revisar uso de Optional, streams ou generics

### Conceitos Centrais
1. **Detecção de Framework** - Detectar Spring Boot ou Quarkus baseado em arquivos de build
2. **Convenções de Nomenclatura** - Classes/Records usam PascalCase, métodos/campos usam camelCase
3. **Imutabilidade** - Preferir records e campos final
4. **Uso de Optional** - Retornar Optional de métodos find*
5. **Injeção de Dependência** - Injeção por construtor, preferida sobre injeção por campo
6. **Padrões Reativos (Quarkus)** - Retornar Uni/Multi

### Exemplo de Uso

**Injeção de construtor Spring Boot:**
```java
@Service
public class MarketService {
    private final MarketRepository marketRepository;

    public MarketService(MarketRepository marketRepository) {
        this.marketRepository = marketRepository;
    }
}
```

**Uso de Optional:**
```java
return market
    .map(MarketResponse::from)
    .orElseThrow(() -> new EntityNotFoundException("Market not found"));
```

---

## Swift 6.2 Approachable Concurrency (swift-concurrency-6-2)

### Propósito
Padrões de "concurrency acessível" do Swift 6.2 — single-threaded por padrão, usando `@concurrent` para offloading explícito em background, fornecendo suporte de protocolo para tipos actor via isolation conformance.

### Quando Usar
- Migrar projetos Swift 5.x ou 6.0/6.1 para Swift 6.2
- Resolver erros de compilador de data race safety
- Projetar arquitetura de aplicações baseadas em MainActor
- Implementar conformance de protocolo

### Conceitos Centrais
1. **Single-threaded por padrão** - Maioria do código é data race safe; concorrência é opt-in explícito
2. **Async permanece no calling actor** - Eliminar offloading implícito que causa erros de data race
3. **Isolation conformance** - Tipos MainActor podem implementar protocolos não isolados com segurança
4. **@concurrent para opt-in explícito** - Execução em background é uma escolha de performance intencional

### Exemplo de Uso

**Isolation conformance:**
```swift
extension StickerModel: @MainActor Exportable {
    func export() {
        photoProcessor.exportAsPNG()
    }
}
```

**@concurrent para trabalho em background:**
```swift
nonisolated final class PhotoProcessor {
    @concurrent
    static func extractSubject(from data: Data) async -> Sticker {
        // Trabalho caro executado em background
    }
}
```

---

## Dart/Flutter Patterns (dart-flutter-patterns)

### Propósito
Padrões Dart e Flutter prontos para produção, cobrindo null safety, estado imutável, combinação assíncrona, arquitetura de componentes, padrões populares de gerenciamento de estado (BLoC, Riverpod, Provider), navegação GoRouter, requisições HTTP Dio, geração de código Freezed e clean architecture.

### Quando Usar
- Iniciar nova funcionalidade Flutter e precisar de padrões de gerenciamento de estado, navegação ou acesso a dados
- Revisar ou escrever código Dart e precisar de orientação sobre null safety, sealed types ou combinação assíncrona
- Configurar novo projeto Flutter e escolher entre BLoC, Riverpod ou Provider
- Implementar cliente HTTP seguro, integração WebView ou armazenamento local
- Escrever testes para componentes Flutter, Cubits ou Riverpod providers

### Conceitos Centrais
1. **Null safety** - Evitar `!`, preferir `?.`/`??`/pattern matching
2. **Estado imutável** - Sealed classes, `freezed`, `copyWith`
3. **Combinação assíncrona** - `Future.wait` concorrente, usar BuildContext com segurança após await
4. **Arquitetura de componentes** - Extrair para classes (não métodos), propagação de `const`, rebuilds com escopo
5. **Gerenciamento de estado** - Eventos BLoC/Cubit, Riverpod notifiers e providers derivados

### Exemplo de Uso

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

**Riverpod derived providers:**
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

### Propósito
Padrões de teste F# usando xUnit, FsUnit, Unquote, FsCheck property tests, testes de integração e Melhores-Práticas de organização de testes.

### Quando Usar
- Escrever novos testes para código F#
- Revisar qualidade e cobertura de testes
- Configurar infraestrutura de testes para projetos F#
- Depurar testes instáveis ou lentos

### Conceitos Centrais
1. **xUnit** - Framework de testes padrão do ecossistema .NET
2. **FsUnit.xUnit** - Sintaxe de assertions F#-friendly para xUnit
3. **Unquote** - Biblioteca de assertions usando quotes F#, mensagens de falha claras
4. **FsCheck.xUnit** - Property tests integrados com xUnit

### Exemplo de Uso

**Unquote assertions:**
```fsharp
[<Fact>]
let ``order total sums item prices`` () =
    let items = [ { Sku = "A"; Quantity = 2; Price = 10m }
                  { Sku = "B"; Quantity = 1; Price = 5m } ]
    let total = Order.calculateTotal items
    test <@ total = 25m @>
```

**FsCheck property tests:**
```fsharp
[<Property>]
let ``order total is always non-negative`` (items: NonEmptyList<PositiveInt * decimal>) =
    let orderItems = items.Get |> List.map (fun (qty, price) ->
        { Sku = "SKU"; Quantity = qty.Get; Price = abs price })
    let total = Order.calculateTotal orderItems
    total >= 0m
```

---

## Outros Padrões de Linguagem

### C# Testing (csharp-testing)
- xUnit + FluentAssertions
- Mocking with Moq/NSubstitute
- Testes de integração com Testcontainers

### Perl Patterns (perl-patterns)
- Sintaxe Perl moderna
- Objetos Moose
- DBIx::Class ORM

---

## Tabela de Referência Rápida

| Habilidade | Uso Principal | Padrões Chave |
|------|---------|---------|
| python-patterns | Desenvolvimento Python | Type hints, context managers, list comprehensions |
| golang-patterns | Desenvolvimento Go | Error wrapping, Worker Pool, design de interfaces |
| rust-patterns | Desenvolvimento Rust | Ownership, Result, traits, concorrência |
| kotlin-patterns | Desenvolvimento Kotlin | Null safety, coroutines, DSL builders |
| cpp-coding-standards | Desenvolvimento C++ | RAII, smart pointers, conceitos |
| java-coding-standards | Desenvolvimento Java | DI, Optional, processamento de streams |
| swift-concurrency-6-2 | Concorrência Swift | @MainActor, @concurrent, isolation conformance |
| dart-flutter-patterns | Desenvolvimento Flutter | Gerenciamento de estado, roteamento, arquitetura de componentes |
| fsharp-testing | Testes F# | xUnit, FsUnit, property tests |