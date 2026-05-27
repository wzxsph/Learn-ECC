# Programmiersprachen-Faehigkeiten

Dieses Dokument stellt die Entwicklungsfaehigkeiten vor, die im ECC-Projekt fuer verschiedene Programmiersprachen verwendet werden.

---

## Python Patterns (python-patterns)

### Verwendungszweck
Bietet pythonische idiomatische Muster, PEP 8-Standards, Type Hints und Best-Practices fuer den Aufbau robuster, effizienter und wartbarer Python-Anwendungen.

### Verwendung
- Neuen Python-Code schreiben
- Python-Code ueberpruefen
- Bestehenden Python-Code refaktorieren
- Python-Pakete/-Module entwerfen

### Kernkonzepte
1. **Lesbarkeit zuerst** - Code sollte klar und offensichtlich sein
2. **Explizit besser als implizit** - Keine Magie vermeiden
3. **EAFP-Stil** - Leichter um Verzeihung bitten als Erlaubnis
4. **Type Hints** - Type Annotations zur Verbesserung der Codesicherheit verwenden
5. **Kontext-Manager** - `with` fuer Ressourcenverwaltung verwenden
6. **Listenkomprehension** - Einfache Datentransformationen
7. **Generatoren** - Faules Evaluation und grosse Datensaetze

### Verwendungsbeispiel

**Type Hints:**
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

**Kontext-Manager:**
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

**Fehlerbehandlung:**
```python
try:
    parsed = json.loads(data)
except json.JSONDecodeError as e:
    raise ValueError(f"Failed to parse data: {data}") from e
```

---

## Go Patterns (golang-patterns)

### Verwendungszweck
Bietet idiomatische Go-Muster, Best-Practices und Konventionen fuer den Aufbau robuster, effizienter und wartbarer Go-Anwendungen.

### Verwendung
- Neuen Go-Code schreiben
- Go-Code ueberpruefen
- Bestehenden Go-Code refaktorieren
- Go-Pakete/-Module entwerfen

### Kernkonzepte
1. **Einfachheit und Klarheit** - Go bevorzugt Einfachheit gegenueber Tricks
2. **Zero-Value ist nuetzlich** - Typen so entwerfen, dass Zero-Value sofort verwendbar ist
3. **Interfaces annehmen, Structs zurueckgeben** - Funktionen nehmen Interface-Parameter an, geben konkrete Typen zurueck
4. **Fehlerbehandlung** - Fehler sind Werte, dem `error`-Return-Pattern folgen
5. **Concurrency-Muster** - Goroutines und Channels fuer Concurrency verwenden
6. **Interface-Design** - Kleine, fokussierte Interfaces
7. **Paketorganisation** - Standard-Projekt-Layout

### Verwendungsbeispiel

**Fehler-Wrapping:**
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

**Functional Options Pattern:**
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

### Verwendungszweck
Bietet idiomatische Rust-Muster, Ownership-System, Fehlerbehandlung, Traits, Concurrency und Best-Practices fuer den Aufbau sicherer, effizienter Anwendungen.

### Verwendung
- Neuen Rust-Code schreiben
- Rust-Code ueberpruefen
- Bestehenden Rust-Code refaktorieren
- Crate-Strukturen und Modullayouts entwerfen

### Kernkonzepte
1. **Ownership und Borrowing** - Kompilierzeit verhindert Datenrennen und Speicherfehler
2. **Result und ?-Operator** - `?` fuer Fehlerpropagation verwenden
3. **Enums und Exhaustives Matching** - Enums verwenden, damit unmoegliche Zustände nicht dargestellt werden koennen
4. **Traits und Generics** - Zero-Cost Abstraktionen
5. **Sichere Concurrency** - `Arc<Mutex<T>>`, Channels und async/await verwenden
6. **Minimale pub-Oberflaeche** - Module nach Domain organisieren

### Verwendungsbeispiel

**Fehlerbehandlung:**
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

**Cow fuer flexible Ownership:**
```rust
use std::borrow::Cow;

fn normalize(input: &str) -> Cow<'_, str> {
    if input.contains(' ') {
        Cow::Owned(input.replace(' ', "_"))
    } else {
        Cow::Borrowed(input) // Zero-cost wenn keine Aenderung
    }
}
```

**Arc<Mutex<T>> fuerShared Mutable State:**
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

### Verwendungszweck
Bietet idiomatische Kotlin-Muster, Best-Practices und Konventionen fuer den Aufbau robuster, effizienter und wartbarer Kotlin-Anwendungen, einschliesslich Coroutines, Null-Safety und DSL-Builder.

### Verwendung
- Neuen Kotlin-Code schreiben
- Kotlin-Code ueberpruefen
- Bestehenden Kotlin-Code refaktorieren
- Kotlin-Module oder -Bibliotheken entwerfen
- Gradle Kotlin DSL-Build konfigurieren

### Kernkonzepte
1. **Null-Safety** - Typsystem und Safe-Call-Operatoren verwenden
2. **Standardmaessig immutable** - `val` gegenueber `var` bevorzugen
3. **Data Classes** - fuer hauptsaechlich datenhaltende Typen
4. **Sealed Classes** - Exhaustives Typhierarchien
5. **Coroutines** - Strukturierte Concurrency
6. **Extension Functions** - Funktionen ohne Vererbung hinzufuegen
7. **DSL Builder** - `@DslMarker` und Lambda-Receiver verwenden

### Verwendungsbeispiel

**Null-Safety:**
```kotlin
fun getUserEmail(userId: String): String {
    val user = userRepository.findById(userId)
    return user?.email ?: "unknown@example.com"
}
```

**Sealed Classes fuer exhaustive Results:**
```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Failure(val error: AppError) : Result<Nothing>()
    data object Loading : Result<Nothing>()
}
```

**Strukturierte Concurrency:**
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

### Verwendungszweck
Modernes C++ (C++17/20/23) Coding-Standards basierend auf den C++ Core Guidelines. Erzwingt Typsicherheit, Ressourcensicherheit, Immutabilitaet und Klarheit.

### Verwendung
- Neuen C++-Code schreiben
- C++-Code ueberpruefen oder refaktorieren
- Architekturentscheidungen treffen
- Konsistenten Codestil erzwingen
- Sprachfeatures auswaehlen

### Kernkonzepte
1. **RAII ueberall** - Ressourcenlebenszyklus an Objektlebenszyklus binden
2. **Standardmaessig immutable** - Mit `const`/`constexpr` beginnen
3. **Typsicherheit** - Typsystem verwenden, um Kompilierzeitfehler zu verhindern
4. **Absicht ausdruecken** - Namen, Typen und Konzepte sollen Zweck vermitteln
5. **Komplexitaet minimieren** - Einfacher Code ist korrekter Code
6. **Wertsemantik bevorzugt gegenueber Zeigersemantik** - Returns und Scoped-Objekte bevorzugen

### Verwendungsbeispiel

**Smart Pointer-Verwendung:**
```cpp
// R.11 + R.20 + R.21: RAII und Smart Pointer
auto widget = std::make_unique<Widget>("config");  // Unique Ownership
auto cache = std::make_shared<Cache>(1024);         // Shared Ownership

// R.3: Raw Pointer sind nicht-besitzende Observer
void render(const Widget* w) {  // Besitzt w nicht
    if (w) w->draw();
}
```

**Rule of Zero:**
```cpp
// C.20: Spezielle Member von Compiler generieren lassen
struct Employee {
    std::string name;
    std::string department;
    int id;
    // Keine Destruktor-, Copy/Move-Konstruktoren oder Zuweisungsoperatoren definieren
};
```

---

## Java Coding Standards (java-coding-standards)

### Verwendungszweck
Java-Coding-Standards fuer Spring Boot- und Quarkus-Services, einschliesslich Benennung, Immutabilitaet, Optional-Verwendung, Stream-Handling, Exceptions, Generics und Projektlayout.

### Verwendung
- Java-Code in Spring Boot- oder Quarkus-Projekten schreiben oder ueberpruefen
- Benennungs-, Immutabilitaets- oder Exception-Handling-Konventionen erzwingen
- Records, Sealed Classes oder Pattern Matching (Java 17+) verwenden
- Optional-, Stream- oder Generics-Verwendung ueberpruefen

### Kernkonzepte
1. **Framework-Erkennung** - Spring Boot oder Quarkus aus Build-Dateien erkennen
2. **Benennungskonventionen** - Klassen/Records: PascalCase, Methoden/Felder: camelCase
3. **Immutabilitaet** - Records und final-Felder bevorzugen
4. **Optional-Verwendung** - Optional von find*-Methoden zurueckgeben
5. **Dependency Injection** - Konstruktor-Injection gegenueber Field-Injection
6. **Reaktive Patterns (Quarkus)** - Uni/Multi zurueckgeben

### Verwendungsbeispiel

**Spring Boot Konstruktor-Injection:**
```java
@Service
public class MarketService {
    private final MarketRepository marketRepository;

    public MarketService(MarketRepository marketRepository) {
        this.marketRepository = marketRepository;
    }
}
```

**Optional-Verwendung:**
```java
return market
    .map(MarketResponse::from)
    .orElseThrow(() -> new EntityNotFoundException("Market not found"));
```

---

## Swift 6.2 Approachable Concurrency (swift-concurrency-6-2)

### Verwendungszweck
Swift 6.2 "Approachable Concurrency"-Muster - standardmaessig Single-Threaded, `@concurrent` fuer explizites Background-Offloading, Isolation-Konformity fuer MainActor-Typen mit Protocol-Support.

### Verwendung
- Swift 5.x oder 6.0/6.1-Projekte auf Swift 6.2 migrieren
- Data-Race-Sicherheits-Compiler-Fehler beheben
- MainActor-basierte Anwendungsarchitektur entwerfen
- Protocol-Conformance implementieren

### Kernkonzepte
1. **Single-Threaded Standard** - Meiste Code ist data-race-safe; Concurrency ist expliziter Opt-in
2. **Async bleibt im aufrufenden Actor** - Eliminiert implizites Offloading, das Data-Race-Fehler verursacht
3. **Isolation-Konformity** - MainActor-Typen koennen sicher non-isolated Protocols implementieren
4. **@concurrent expliziter Opt-in** - Background-Ausfuehrung ist beabsichtigte Performance-Wahl

### Verwendungsbeispiel

**Isolation-Konformity:**
```swift
extension StickerModel: @MainActor Exportable {
    func export() {
        photoProcessor.exportAsPNG()
    }
}
```

**@concurrent fuer Background-Arbeit:**
```swift
nonisolated final class PhotoProcessor {
    @concurrent
    static func extractSubject(from data: Data) async -> Sticker {
        // Teure Arbeit, die im Hintergrund ausgefuehrt wird
    }
}
```

---

## Dart/Flutter Patterns (dart-flutter-patterns)

### Verwendungszweck
Produktionsreife Dart- und Flutter-Muster, einschliesslich Null-Safety, immutable State, async Composition, Komponentenarchitektur, beliebte State-Management-Frameworks (BLoC, Riverpod, Provider), GoRouter-Navigation, Dio-HTTP-Requests, Freezed-Code-Generierung und Clean Architecture.

### Verwendung
- Neue Flutter-Features starten und State-Management-, Navigations- oder Datenzugriffsmuster benoetigen
- Dart-Code ueberpruefen oder schreiben und Null-Safety, Sealed-Types oder async Composition Guidance benoetigen
- Neues Flutter-Projekt einrichten und BLoC, Riverpod oder Provider auswaehlen
- Sicheren HTTP-Client, WebView-Integration oder lokalen Storage implementieren
- Tests fuer Flutter-Komponenten, Cubits oder Riverpod-Provider schreiben

### Kernkonzepte
1. **Null-Safety** - `!` vermeiden, `?.`/`??`/Pattern-Matching bevorzugen
2. **Immutable State** - Sealed Classes, `freezed`, `copyWith`
3. **Async Composition** - `Future.wait`, nach await sichere BuildContext-Nutzung
4. **Komponentenarchitektur** - Als Klassen extrahieren (nicht als Methoden), `const` propagieren, Scoping Rebuilds
5. **State-Management** - BLoC/Cubit Events, Riverpod Notifiers und Derived Providers

### Verwendungsbeispiel

**Sealed State Classes:**
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

### Verwendungszweck
F#-Testmuster mit xUnit, FsUnit, Unquote, FsCheck Property-Testing, Integrationstests und Testorganisations-Best-Practices.

### Verwendung
- Neue Tests fuer F#-Code schreiben
- Testqualitaet und -abdeckung ueberpruefen
- Testinfrastruktur fuer F#-Projekte einrichten
- Flaky oder langsame Tests debuggen

### Kernkonzepte
1. **xUnit** - Standard-.NET-Ecosystem-Test-Framework
2. **FsUnit.xUnit** - F#-freundliche Assertion-Syntax fuer xUnit
3. **Unquote** - Assertions-Bibliothek mit F#-Quotes, klare Fehlermeldungen
4. **FsCheck.xUnit** - Property-Testing integriert in xUnit

### Verwendungsbeispiel

**Unquote Assertions:**
```fsharp
[<Fact>]
let ``order total sums item prices`` () =
    let items = [ { Sku = "A"; Quantity = 2; Price = 10m }
                  { Sku = "B"; Quantity = 1; Price = 5m } ]
    let total = Order.calculateTotal items
    test <@ total = 25m @>
```

**FsCheck Property Testing:**
```fsharp
[<Property>]
let ``order total is always non-negative`` (items: NonEmptyList<PositiveInt * decimal>) =
    let orderItems = items.Get |> List.map (fun (qty, price) ->
        { Sku = "SKU"; Quantity = qty.Get; Price = abs price })
    let total = Order.calculateTotal orderItems
    total >= 0m
```

---

## Andere Sprachmuster

### C# Testing (csharp-testing)
- xUnit + FluentAssertions
- Mocking with Moq/NSubstitute
- Integration tests with Testcontainers

### Perl Patterns (perl-patterns)
- Modern Perl syntax
- Moose objects
- DBIx::Class ORM

---

## Schnellreferenz-Tabelle

| Faehigkeit | Hauptverwendung | Wichtige Muster |
|------|---------|---------|
| python-patterns | Python-Entwicklung | Type Hints, Kontext-Manager, Listenkomprehension |
| golang-patterns | Go-Entwicklung | Fehler-Wrapping, Worker Pool, Interface-Design |
| rust-patterns | Rust-Entwicklung | Ownership, Result, Traits, Concurrency |
| kotlin-patterns | Kotlin-Entwicklung | Null-Safety, Coroutines, DSL-Builder |
| cpp-coding-standards | C++-Entwicklung | RAII, Smart Pointer, Concepts |
| java-coding-standards | Java-Entwicklung | Dependency Injection, Optional, Stream-Handling |
| swift-concurrency-6-2 | Swift-Concurrency | @MainActor, @concurrent, Isolation-Konformity |
| dart-flutter-patterns | Flutter-Entwicklung | State-Management, Routing, Komponentenarchitektur |
| fsharp-testing | F#-Testing | xUnit, FsUnit, Property-Testing |