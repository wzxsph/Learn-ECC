# Testing

Dieser Abschnitt behandelt Teststrategien fuer verschiedene Programmiersprachen, TDD-Workflows und E2E-Testmuster.

---

## TDD-Workflow

### Verwendungszweck
TDD-Workflow stellt sicher, dass jegliche Codeentwicklung testgetriebenen Entwicklungsprinzipien folgt, mit umfassender Testabdeckung.

### Verwendung
- Neue Features oder Funktionalitaeten schreiben
- Bugs oder Probleme beheben
- Bestehenden Code refaktorieren
- API-Endpunkte hinzufuegen
- Neue Komponenten erstellen

### Kernkonzepte

**1. Test First**
Immer zuerst Tests schreiben, dann Code implementieren, damit Tests bestehen.

**2. Abdeckungsanforderung**
Minimum 80% Abdeckung (Unit-Tests + Integration-Tests + E2E-Tests)

**3. Testtypen**
- Unit-Tests: Eigenstaendige Funktionen und Komponenten
- Integration-Tests: API-Endpunkte und Datenbankoperationen
- E2E-Tests: Kritische Benutzerfloesse und Browser-Automatisierung

**4. RED-GREEN-REFACTOR-Zyklus**
- RED: Zuerst fehlgeschlagenen Test schreiben
- GREEN: Minimalen Code schreiben, damit Test besteht
- REFACTOR: Codequalitaet verbessern, Tests gruen halten

### TDD-Flow-Schritte

```bash
# 1. Test schreiben (RED)
npm test  # Test sollte fehlschlagen

# 2. Code implementieren (GREEN)
# Minimalen Code schreiben, damit Test besteht

# 3. Refaktorieren (REFACTOR)
# Codequalitaet verbessern, Tests gruen halten

# 4. Abdeckung verifizieren
npm run test:coverage  # 80%+ Abdeckung verifizieren
```

### Testisolationsprinzipien

```typescript
// Falsch: Tests sind voneinander abhaengig
test('creates user', () => { /* ... */ })
test('updates same user', () => { /* Haengt von vorherigem Test ab */ })

// Richtig: Jeder Test unabhaengig einrichten
test('creates user', () => {
  const user = createTestUser()
  // Testlogik
})

test('updates user', () => {
  const user = createTestUser()
  // Update-Logik
})
```

---

## Python-Testing

### Verwendungszweck
Python-Teststrategien verwenden pytest, TDD-Methodik, Fixtures, Mocking, Parametrisierung und Abdeckungsanforderungen.

### Verwendung
- Neuen Python-Code nach TDD-Methodik schreiben
- Python-Projekt-Testsuite entwerfen
- Python-Testabdeckung ueberpruefen
- Testinfrastruktur einrichten

### Kernkonzepte

**1. pytest Fixtures**
`@pytest.fixture` verwenden, um wiederverwendbare Testdaten zu erstellen.

**2. Parametrisierte Tests**
`@pytest.mark.parametrize` verwenden, um Tests mit verschiedenen Eingaben auszufuehren.

**3. Mocking**
`@patch` von `unittest.mock` verwenden, um externe Abhaengigkeiten zu mocken.

**4. Abdeckung**
`--cov` verwenden, um Abdeckung zu messen, Ziel 80%+.

### Verwendungsbeispiel

```python
import pytest
from unittest.mock import patch

# Fixtures
@pytest.fixture
def database():
    db = Database(":memory:")
    db.create_tables()
    yield db
    db.close()

# Parametrisierte Tests
@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("world", "WORLD"),
    ("PyThOn", "PYTHON"),
])
def test_uppercase(input, expected):
    assert input.upper() == expected

# Mocking
@patch("mypackage.external_api_call")
def test_with_mock(api_call_mock):
    api_call_mock.return_value = {"status": "success"}
    result = my_function()
    assert result["status"] == "success"

# Exception-Tests
with pytest.raises(ValueError, match="invalid input"):
    raise ValueError("invalid input provided")

# Ausfuehrungsbefehle
pytest --cov=mypackage --cov-report=html
```

---

## Go-Testing

### Verwendungszweck
Go-Testmuster umfassen Table-Driven-Tests, Subtests, Benchmark-Tests, Fuzz-Tests und Testabdeckung, nach TDD-Methodik und idiomatischen Go-Praktiken.

### Verwendung
- Neue Go-Funktionen oder -Methoden schreiben
- Testabdeckung fuer bestehenden Code hinzufuegen
- Benchmark-Tests fuer performancekritischen Code erstellen
- Fuzz-Tests fuer Eingabevalidierung implementieren
- TDD-Workflow in Go-Projekten folgen

### Kernkonzepte

**1. Table-Driven Tests**
Table-Driven-Pattern fuer umfassende Abdeckung verwenden.

**2. Subtests**
`t.Run` verwenden, um verwandte Tests zu organisieren.

**3. Test-Helper**
`t.Helper()` verwenden, um Helper-Funktionen zu markieren.

**4. Interface Mocking**
Interfaces fuer Dependency-Decoupling und Testing verwenden.

**5. Benchmark-Tests und Fuzz-Tests**
Performance-Tests und Zufallseingabe-Tests.

### Verwendungsbeispiel

```go
// Table-Driven Tests
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"negative numbers", -1, -2, -3},
        {"zero values", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.expected)
            }
        })
    }
}

// Test-Helper
func setupTestDB(t *testing.T) *sql.DB {
    t.Helper()
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatalf("failed to open database: %v", err)
    }
    t.Cleanup(func() { db.Close() })
    return db
}

// Interface Mocking
type UserRepository interface {
    GetUser(id string) (*User, error)
    SaveUser(user *User) error
}

type MockUserRepository struct {
    GetUserFunc  func(id string) (*User, error)
    SaveUserFunc func(user *User) error
}

func (m *MockUserRepository) GetUser(id string) (*User, error) {
    return m.GetUserFunc(id)
}

// Benchmark
func BenchmarkProcess(b *testing.B) {
    data := generateTestData(1000)
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        Process(data)
    }
}

// Ausfuehrungsbefehle
go test -cover ./...
go test -bench=. -benchmem
go test -fuzz=FuzzParse -fuzztime=30s
```

---

## Rust-Testing

### Verwendungszweck
Rust-Testmuster umfassen Unit-Tests, Integration-Tests, Async-Tests, Property-Tests, Mocking und Abdeckung, nach TDD-Methodik.

### Verwendung
- Neue Rust-Funktionen, -Methoden oder -Traits schreiben
- Testabdeckung fuer bestehenden Code hinzufuegen
- Benchmark-Tests fuer performancekritischen Code erstellen
- Property-Tests fuer Eingabevalidierung implementieren
- TDD-Workflow in Rust-Projekten folgen

### Kernkonzepte

**1. Module-Level Tests**
Unit-Tests in `#[cfg(test)]`-Modulen schreiben.

**2. Integration-Tests**
Integration-Tests im `tests/`-Verzeichnis schreiben.

**3. Async-Tests**
`#[tokio::test]` fuer Async-Tests verwenden.

**4. Property-Tests**
`proptest` fuer Property-Based-Testing verwenden.

**5. Mocking**
`mockall` fuer Trait-Mocking verwenden.

### Verwendungsbeispiel

```rust
// Module-Level Tests
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn creates_user_with_valid_email() {
        let user = User::new("Alice", "alice@example.com").unwrap();
        assert_eq!(user.display_name(), "Alice");
    }

    #[test]
    fn rejects_invalid_email() {
        let result = User::new("Bob", "not-an-email");
        assert!(result.is_err());
    }
}

// Async Tests
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Property Tests
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        prop_assert_eq!(input, decoded);
    }
}

// Mocking with mockall
use mockall::{automock, predicate::eq};

#[automock]
trait UserRepository {
    fn find_by_id(&self, id: u64) -> Option<User>;
    fn save(&self, user: &User) -> Result<(), StorageError>;
}

#[test]
fn service_returns_user_when_found() {
    let mut mock = MockUserRepository::new();
    mock.expect_find_by_id()
        .with(eq(42))
        .times(1)
        .returning(|_| Some(User { id: 42, name: "Alice".into() }));

    let service = UserService::new(Box::new(mock));
    let user = service.get_user(42).unwrap();
    assert_eq!(user.name, "Alice");
}

// Ausfuehrungsbefehle
cargo test
cargo llvm-cov --fail-under-lines 80
cargo bench
```

---

## E2E-Testing

### Verwendungszweck
Playwright E2E-Testmuster bieten umfassende Muster fuer den Aufbau stabiler, schneller, wartbarer E2E-Testsuites, einschliesslich Page-Object-Model, Konfiguration, CI/CD-Integration und Flaky-Test-Strategien.

### Verwendung
- End-to-End-Tests erstellen, um vollstaendige Benutzerfloesse zu verifizieren
- Playwright fuer Browser-Automatisierung verwenden
- E2E-Tests in CI/CD einrichten
- Mit instabilen Tests umgehen

### Kernkonzepte

**1. Page-Object-Model (POM)**
Seiteninteraktionen in Page-Object-Klassen kapseln.

**2. Testisolierung**
Jeder Test richtet unabhaengige Daten ein.

**3. Locator-Strategien**
Semantische Locators statt CSS-Klassennamen verwenden.

**4. Flaky-Test-Strategien**
Instabile Tests erkennen und behandeln.

**5. Artifacts-Management**
Screenshots, Videos, Trace-Aufzeichnungen fuer Debugging verwenden.

### Verwendungsbeispiel

```typescript
import { test, expect } from '@playwright/test'

// Page-Object-Model
export class ItemsPage {
  readonly page: Page
  readonly searchInput: Locator
  readonly itemCards: Locator

  constructor(page: Page) {
    this.page = page
    this.searchInput = page.locator('[data-testid="search-input"]')
    this.itemCards = page.locator('[data-testid="item-card"]')
  }

  async goto() {
    await this.page.goto('/items')
    await this.page.waitForLoadState('networkidle')
  }

  async search(query: string) {
    await this.searchInput.fill(query)
    await this.page.waitForResponse(resp => resp.url().includes('/api/search'))
  }
}

// Teststruktur
test.describe('Item Search', () => {
  let itemsPage: ItemsPage

  test.beforeEach(async ({ page }) => {
    itemsPage = new ItemsPage(page)
    await itemsPage.goto()
  })

  test('should search by keyword', async ({ page }) => {
    await itemsPage.search('test')
    const count = await itemsPage.getItemCount()
    expect(count).toBeGreaterThan(0)
    await expect(itemsPage.itemCards.first()).toContainText(/test/i)
  })
})

// Playwright-Konfiguration
export default defineConfig({
  testDir: './tests/e2e',
  retries: process.env.CI ? 2 : 0,
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
  ],
})

// Flaky-Test
test('conditional skip', async ({ page }) => {
  test.skip(process.env.CI, 'Flaky in CI - Issue #123')
})

test('flaky: complex search', async ({ page }) => {
  test.fixme(true, 'Known flaky test - Issue #123')
})
```

---

## Zugehoerige Faehigkeiten

| Faehigkeit | Verwendungszweck |
|------|------|
| `coding-standards` | Coding-Standards und Best-Practices |
| `error-handling` | Fehlerbehandlungsmuster |
| `security-review` | Sicherheits-Review-Checkliste |