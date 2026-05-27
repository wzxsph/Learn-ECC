# Testes

Esta seção cobre estratégias de teste para várias Linguagens-de-Programação, fluxos de trabalho TDD e padrões de testes E2E.

---

## Fluxo de Trabalho TDD

### Propósito
Fluxos de trabalho TDD garantem que todo desenvolvimento de código siga princípios de desenvolvimento orientado a testes, com cobertura abrangente de testes.

### Quando Usar
- Escrever novas funcionalidades ou features
- Corrigir bugs ou problemas
- Refatorar código existente
- Adicionar endpoints de API
- Criar novos componentes

### Conceitos Centrais

**1. Testar Primeiro**
Sempre escrever testes primeiro, depois implementar código para fazer os testes passarem.

**2. Requisitos de Cobertura**
Mínimo de 80% de cobertura (testes unitários + testes de integração + testes E2E)

**3. Tipos de Testes**
- Testes unitários: Funções e componentes independentes
- Testes de integração: Endpoints de API e operações de banco de dados
- Testes E2E: Fluxos críticos de usuário e automação de navegador

**4. Loop RED-GREEN-REFACTOR**
- RED: Escrever teste que falha primeiro
- GREEN: Escrever código mínimo para fazer o teste passar
- REFACTOR: Melhorar qualidade do código mantendo testes verdes

### Passos do Fluxo TDD

```bash
# 1. Escrever teste (RED)
npm test  # Teste deve falhar

# 2. Implementar código (GREEN)
# Escrever código mínimo para fazer o teste passar

# 3. Refatorar (REFACTOR)
# Melhorar qualidade do código mantendo testes verdes

# 4. Verificar cobertura
npm run test:coverage  # Verificar 80%+ de cobertura
```

### Princípios de Isolamento de Testes

```typescript
// Errado: testes dependem uns dos outros
test('creates user', () => { /* ... */ })
test('updates same user', () => { /* depende do teste anterior */ })

// Correto: cada teste configura independentemente
test('creates user', () => {
  const user = createTestUser()
  // lógica de teste
})

test('updates user', () => {
  const user = createTestUser()
  // lógica de update
})
```

---

## Testes Python

### Propósito
Estratégias de teste Python usam pytest, metodologia TDD, fixtures, mocking, parametrização e requisitos de cobertura.

### Quando Usar
- Escrever novo código Python seguindo metodologia TDD
- Projetar suite de testes para projetos Python
- Revisar cobertura de testes Python
- Configurar infraestrutura de testes

### Conceitos Centrais

**1. Fixtures pytest**
Usar `@pytest.fixture` para criar dados de teste reutilizáveis.

**2. Testes Parametrizados**
Usar `@pytest.mark.parametrize` para executar testes com diferentes inputs.

**3. Mocking**
Usar `@patch` de `unittest.mock` para simular dependências externas.

**4. Cobertura**
Usar `--cov` para medir cobertura, meta de 80%+.

### Exemplo de Uso

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

# Testes parametrizados
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

# Testes de exceção
with pytest.raises(ValueError, match="invalid input"):
    raise ValueError("invalid input provided")

# Comandos de execução
pytest --cov=mypackage --cov-report=html
```

---

## Testes Go

### Propósito
Padrões de teste Go incluem testes table-driven, subtests, benchmark tests, fuzz tests e cobertura, seguindo metodologia TDD e práticas Go idiomáticas.

### Quando Usar
- Escrever novas funções ou métodos Go
- Adicionar cobertura de testes para código existente
- Criar benchmark tests para código crítico de performance
- Implementar fuzz tests para validação de input
- Seguir fluxo de trabalho TDD em projetos Go

### Conceitos Centrais

**1. Testes Table-Driven**
Usar padrão table-driven para cobertura abrangente.

**2. Subtests**
Usar `t.Run` para organizar testes relacionados.

**3. Funções Auxiliares de Teste**
Usar `t.Helper()` para marcar funções auxiliares.

**4. Mocking de Interface**
Usar interfaces para desacoplamento de dependências e testes.

**5. Benchmark Tests e Fuzz Tests**
Testes de performance e inputs aleatórios.

### Exemplo de Uso

```go
// Testes table-driven
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

// Função auxiliar de teste
func setupTestDB(t *testing.T) *sql.DB {
    t.Helper()
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatalf("failed to open database: %v", err)
    }
    t.Cleanup(func() { db.Close() })
    return db
}

// Mocking de interface
type UserRepository interface {
    GetUser(id string) (*User, error)
    SaveUser(user *User) error
}

type MockUserRepository struct {
    GetUserFunc  func(id string) (*User, error)
    SaveUserFunc func(user *User) error
}

func (m *MockUserRepository) Get(id string) (*User, error) {
    return m.GetUserFunc(id)
}

// Benchmark test
func BenchmarkProcess(b *testing.B) {
    data := generateTestData(1000)
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        Process(data)
    }
}

// Comandos de execução
go test -cover ./...
go test -bench=. -benchmem
go test -fuzz=FuzzParse -fuzztime=30s
```

---

## Testes Rust

### Propósito
Padrões de teste Rust incluem testes unitários, testes de integração, testes assíncronos, property tests, mocking e cobertura, seguindo metodologia TDD.

### Quando Usar
- Escrever novas funções, métodos ou traits Rust
- Adicionar cobertura de testes para código existente
- Criar benchmark tests para código crítico de performance
- Implementar property tests para validação de input
- Seguir fluxo de trabalho TDD em projetos Rust

### Conceitos Centrais

**1. Testes Unitários de Módulo**
Escrever testes unitários em módulos `#[cfg(test)]`.

**2. Testes de Integração**
Escrever testes de integração no diretório `tests/`.

**3. Testes Assíncronos**
Usar `#[tokio::test]` para testes assíncronos.

**4. Property Tests**
Usar `proptest` para testes baseados em propriedades.

**5. Mocking**
Usar `mockall` para fazer mock de traits.

### Exemplo de Uso

```rust
// Testes unitários de módulo
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

// Testes assíncronos
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Property tests
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

// Comandos de execução
cargo test
cargo llvm-cov --fail-under-lines 80
cargo bench
```

---

## Testes E2E

### Propósito
Padrões de teste E2E Playwright fornecem padrões abrangentes para construir suites de testes E2E estáveis, rápidas e mantíveis, incluindo modelo de objeto de página, configuração, integração CI/CD e estratégias de teste flaky.

### Quando Usar
- Criar testes de ponta a ponta para validar fluxos completos de usuário
- Usar Playwright para automação de navegador
- Configurar testes E2E em CI/CD
- Lidar com testes instáveis

### Conceitos Centrais

**1. Page Object Model (POM)**
Encapsular interações de página em classes de objeto de página.

**2. Isolamento de Testes**
Cada teste configura dados independentemente.

**3. Estratégias de Localizadores**
Usar localizadores semânticos ao invés de nomes de classes CSS.

**4. Testes Flaky**
Identificar e lidar com testes instáveis.

**5. Gerenciamento de Artefatos**
Capturas de tela, vídeos, traces para debugging.

### Exemplo de Uso

```typescript
import { test, expect } from '@playwright/test'

// Page Object Model
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

// Estrutura de testes
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

// Configuração Playwright
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

// Testes flaky
test('conditional skip', async ({ page }) => {
  test.skip(process.env.CI, 'Flaky in CI - Issue #123')
})

test('flaky: complex search', async ({ page }) => {
  test.fixme(true, 'Known flaky test - Issue #123')
})
```

---

## Habilidades Relacionadas

| Habilidade | Propósito |
|------|------|
| `coding-standards` | Padrões de codificação e Melhores-Práticas |
| `error-handling` | Padrões de tratamento de erros |
| `security-review` | Checklist de revisão de segurança |