# Testbefehle

Dieses Dokument beschreibt die ECC-Befehle fuer Tests, die mehrere Programmiersprachen und Test-Frameworks unterstuetzen.

---

## /go-test

**Zweck**: TDD-Workflow fuer Go. Table-driven Tests verwenden, Tests zuerst schreiben, dann implementieren, 80%+ Abdeckung verifizieren.

**Verwendung**:
```
/go-test [Funktionsbeschreibung]
```

**Anwendungsszenarien**:
- Neue Go-Funktionen implementieren
- Testabdeckung fuer bestehenden Go-Code hinzufuegen
- Bugs beheben (erst fehlschlagenden Test schreiben)
- Go-TDD-Methodik lernen

**Beispiel-Workflow**:
```go
// Schritt 1: Interface definieren
func ValidateEmail(email string) error {
    panic("not implemented")
}

// Schritt 2: Table-driven Tests schreiben (ROT)
tests := []struct {
    name    string
    email   string
    wantErr bool
}{
    {"simple email", "user@example.com", false},
    {"empty string", "", true},
    {"no at sign", "userexample.com", true},
}

// Schritt 3: Tests ausfuehren - FAIL verifizieren

// Schritt 4: Minimalen Code implementieren (GRUEN)
func ValidateEmail(email string) error {
    if email == "" {
        return ErrEmailEmpty
    }
    if !emailRegex.MatchString(email) {
        return ErrEmailInvalid
    }
    return nil
}

// Schritt 5: PASS verifizieren

// Schritt 6: Abdeckung pruefen
go test -cover ./...
// coverage: 100.0%
```

**Abdeckungsbefehle**:
```bash
go test -cover ./...                    # Basisabdeckung
go test -coverprofile=coverage.out      # Abdeckungsdatei generieren
go tool cover -html=coverage.out        # Im Browser anzeigen
go test -race -cover ./...              # Race-Detektion
```

---

## /kotlin-test

**Zweck**: TDD-Workflow fuer Kotlin. Kotest-Framework und MockK verwenden, Tests zuerst schreiben, dann implementieren, 80%+ Abdeckung verifizieren (Kover).

**Verwendung**:
```
/kotlin-test [Funktionsbeschreibung]
```

**Anwendungsszenarien**:
- Neue Kotlin-Funktionen oder -Klassen implementieren
- Testabdeckung fuer Android/KMP-Projekte hinzufuegen
- Bugs beheben (erst fehlschlagenden Test schreiben)
- Kotlin-TDD-Methodik lernen

**Kotest-Teststile**:
```kotlin
// StringSpec (einfachster Stil)
class CalculatorTest : StringSpec({
    "add two positive numbers" {
        Calculator.add(2, 3) shouldBe 5
    }
})

// FunSpec (standardmaessige Unit-Tests)
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

// BehaviorSpec (BDD-Stil)
class OrderServiceTest : BehaviorSpec({
    Given("a valid order") {
        When("placed") {
            Then("should be confirmed") { /* ... */ }
        }
    }
})
```

**Koroutinen-Tests**:
```kotlin
test("concurrent fetch completes") {
    runTest {
        val result = service.fetchAll()
        result.shouldNotBeEmpty()
    }
}
```

**Abdeckungsbefehle**:
```bash
./gradlew koverHtmlReport     # HTML-Bericht
./gradlew koverVerify         # Abdeckungsschwelle verifizieren
./gradlew koverXmlReport      # CI-XML-Bericht
./gradlew test                # Tests ausfuehren
```

---

## /rust-test

**Zweck**: TDD-Workflow fuer Rust. `#[test]`, rstest, proptest und mockall verwenden, Tests zuerst schreiben, dann implementieren, 80%+ Abdeckung verifizieren (cargo-llvm-cov).

**Verwendung**:
```
/rust-test [Funktionsbeschreibung]
```

**Anwendungsszenarien**:
- Neue Rust-Funktionen, -Methoden oder -Traits implementieren
- Testabdeckung fuer Rust-Projekte hinzufuegen
- Bugs beheben (erst fehlschlagenden Test schreiben)
- Rust-TDD-Methodik lernen

**Testmuster**:
```rust
// Unit-Tests
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

// Parameterisierte Tests (rstest)
#[rstest]
#[case("hello", 5)]
#[case("", 0)]
#[case("rust", 4)]
fn test_string_length(#[case] input: &str, #[case] expected: usize) {
    assert_eq!(input.len(), expected);
}

// Asynchrone Tests
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Property-Tests (proptest)
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        assert_eq!(input, decoded);
    }
}
```

**Abdeckungsbefehle**:
```bash
cargo llvm-cov                    # Abdeckungszusammenfassung
cargo llvm-cov --html             # HTML-Bericht
cargo llvm-cov --fail-under-lines 80  # Bei Unterschreitung fehlschlagen
cargo test                        # Tests ausfuehren
```

---

## /cpp-test

**Zweck**: TDD-Workflow fuer C++. GoogleTest/GoogleMock und CMake/CTest verwenden, Tests zuerst schreiben, dann implementieren, Abdeckung verifizieren (gcov/lcov).

**Verwendung**:
```
/cpp-test [Funktionsbeschreibung]
```

**Anwendungsszenarien**:
- Neue C++-Klassen oder -Funktionen implementieren
- Testabdeckung hinzufuegen
- Bugs beheben (erst fehlschlagenden Test schreiben)
- C++-TDD-Methodik lernen

**GoogleTest-Muster**:
```cpp
// Basis-Tests
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

// Parameterisierte Tests
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

**Abdeckungsbefehle**:
```bash
cmake -DCMAKE_CXX_FLAGS="--coverage" -B build
cmake --build build
ctest --test-dir build
lcov --capture --directory build --output-file coverage.info
genhtml coverage.info --output-directory coverage_html
```

---

## /flutter-test

**Zweck**: Flutter/Dart-Tests. Fehlschlaege melden und schrittweise Testprobleme beheben, deckt Unit-, Widget-, Golden- und Integrationstests ab.

**Verwendung**:
```
/flutter-test                # Alle Tests ausfuehren
/flutter-test --coverage     # Mit Abdeckung
/flutter-test test/file.dart # Bestimmte Testdatei ausfuehren
```

**Anwendungsszenarien**:
- Verifizieren dass nichts nach Funktionsimplementierung kaputt ist
- Testabdeckung fuer neuen Code pruefen
- Bestimmte Testdatei reparieren
- Vor dem Committen eines PRs

**Haeufige Testfehler und -reparaturen**:

| Fehlertyp | Typische Reparatur |
|----------|-------------------|
| `Expected: <X> Actual: <Y>` | Assertion oder Implementierung aktualisieren |
| `Widget not found` | Finder-Selektor oder Widget-Namen reparieren |
| `Golden file not found` | `flutter test --update-goldens` ausfuehren um zu generieren |
| `MissingPluginException` | Plattform-Kanäle im Test-Setup mocken |
| `pumpAndSettle timed out` | Durch explizites `pump(Duration)` ersetzen |

---

## /e2e

**Zweck**: End-to-End-Testing-Workflow. E2E-Tests fuer kritische Benutzerflows generieren und ausfuehren.

**Verwendung**:
```
/e2e [Funktionsbeschreibung]
```

**Anwendungsszenarien**:
- Kritische Benutzerflows testen
- Systemuebergreifende Integrationstests
- Abnahmetests
- Regressionstests

---

## /test-coverage

**Zweck**: Testabdeckung analysieren, Luecken identifizieren und fehlende Tests generieren um Zielschwelle zu erreichen (standardmaessig 80%).

**Verwendung**:
```
/test-coverage
```

**Workflow**:
1. **Test-Framework erkennen** - Passenden Abdeckungsbefehl basierend auf Projekttyp auswaehlen
2. **Abdeckungsbericht analysieren** - Dateien unter 80% auflisten
3. **Fehlende Tests generieren** - Tests nach Prioritaet generieren
4. **Verifizieren** - Abdeckung erneut ausfuehren um Verbesserung zu bestaetigen

**Test-Framework-Erkennung**:

| Indikator | Abdeckungsbefehl |
|-----------|-----------------|
| `jest.config.*` | `npx jest --coverage` |
| `vitest.config.*` | `npx vitest run --coverage` |
| `pytest.ini` | `pytest --cov=src --cov-report=json` |
| `Cargo.toml` | `cargo llvm-cov --json` |
| `go.mod` | `go test -coverprofile=coverage.out` |

---

## /python-testing

**Zweck**: Python-Testing-Workflow. Pytest mit Abdeckungsanalyse verwenden.

**Verwendung**:
```
/python-testing [Funktionsbeschreibung]
```

**Anwendungsszenarien**:
- Neue Python-Funktionen implementieren
- Testabdeckung hinzufuegen
- Bugs beheben
- FastAPI/Django/Flask-Projekt-Tests

**Pytest-Features**:
```python
# Parameterisierte Tests
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

# Asynchrone Tests
@pytest.mark.asyncio
async def test_async_fetch():
    result = await fetch_data()
    assert result is not None
```

---

## Befehlsvergleichstabelle

| Befehl | Sprache | Test-Framework | Abdeckungstool | TDD-Zyklus |
|--------|---------|-----------------|-----------------|------------|
| `/go-test` | Go | Standard testing | go test -cover | ✓ |
| `/kotlin-test` | Kotlin | Kotest + MockK | Kover | ✓ |
| `/rust-test` | Rust | #[test], rstest, proptest | cargo-llvm-cov | ✓ |
| `/cpp-test` | C++ | GoogleTest | gcov/lcov | ✓ |
| `/flutter-test` | Dart | Flutter test | coverage | - |
| `/python-testing` | Python | pytest | pytest-cov | - |
| `/e2e` | Mehrere | Projektspezifisch | - | - |
| `/test-coverage` | Allgemein | Mehrere | Mehrere | - |