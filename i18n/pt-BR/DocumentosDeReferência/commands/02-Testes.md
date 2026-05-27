# Comandos de Testes

Este documento apresenta comandos especializados para testes no ECC, suportando múltiplas Linguagens-de-Programação e frameworks de teste.

---

## /go-test

**Propósito**: Fluxo de trabalho TDD para linguagem Go. Usa testes table-driven, escreve testes primeiro, verifica 80%+ de cobertura.

**Como Usar**:
```
/go-test [descrição da funcionalidade]
```

**Cenários de Uso**:
- Implementar novas funções Go
- Adicionar cobertura de testes para código Go existente
- Corrigir bugs (escrever teste que falha primeiro)
- Aprender metodologia TDD Go

**Fluxo de Trabalho Exemplo**:
```go
// Step 1: Definir interface
func ValidateEmail(email string) error {
    panic("not implemented")
}

// Step 2: Escrever testes table-driven (RED)
tests := []struct {
    name    string
    email   string
    wantErr bool
}{
    {"simple email", "user@example.com", false},
    {"empty string", "", true},
    {"no at sign", "userexample.com", true},
}

// Step 3: Executar testes - verificar FAIL

// Step 4: Implementar código mínimo (GREEN)
func ValidateEmail(email string) error {
    if email == "" {
        return ErrEmailEmpty
    }
    if !emailRegex.MatchString(email) {
        return ErrEmailInvalid
    }
    return nil
}

// Step 5: Verificar PASS

// Step 6: Verificar cobertura
go test -cover ./...
// coverage: 100.0%
```

**Comandos de Cobertura**:
```bash
go test -cover ./...                    # Cobertura básica
go test -coverprofile=coverage.out      # Gerar arquivo de cobertura
go tool cover -html=coverage.out        # Visualizar no navegador
go test -race -cover ./...              # Detecção de race conditions
```

---

## /kotlin-test

**Propósito**: Fluxo de trabalho TDD para linguagem Kotlin. Usa framework Kotest e MockK, escreve testes primeiro, verifica 80%+ de cobertura (Kover).

**Como Usar**:
```
/kotlin-test [descrição da funcionalidade]
```

**Cenários de Uso**:
- Implementar novas funções ou classes Kotlin
- Adicionar cobertura de testes para projetos Android/KMP
- Corrigir bugs (escrever teste que falha primeiro)
- Aprender metodologia TDD Kotlin

**Estilos de Teste Kotest**:
```kotlin
// StringSpec (mais simples)
class CalculatorTest : StringSpec({
    "add two positive numbers" {
        Calculator.add(2, 3) shouldBe 5
    }
})

// FunSpec (testes unitários padrão)
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

// BehaviorSpec (estilo BDD)
class OrderServiceTest : BehaviorSpec({
    Given("a valid order") {
        When("placed") {
            Then("should be confirmed") { /* ... */ }
        }
    }
})
```

**Testes de Coroutines**:
```kotlin
test("concurrent fetch completes") {
    runTest {
        val result = service.fetchAll()
        result.shouldNotBeEmpty()
    }
}
```

**Comandos de Cobertura**:
```bash
./gradlew koverHtmlReport     # Relatório HTML
./gradlew koverVerify         # Verificar limiar de cobertura
./gradlew koverXmlReport      # Relatório XML para CI
./gradlew test                # Executar testes
```

---

## /rust-test

**Propósito**: Fluxo de trabalho TDD para linguagem Rust. Usa `#[test]`, rstest, proptest e mockall, escreve testes primeiro, verifica 80%+ de cobertura (cargo-llvm-cov).

**Como Usar**:
```
/rust-test [descrição da funcionalidade]
```

**Cenários de Uso**:
- Implementar novas funções, métodos ou traits Rust
- Adicionar cobertura de testes para projetos Rust
- Corrigir bugs (escrever teste que falha primeiro)
- Aprender metodologia TDD Rust

**Padrões de Teste**:
```rust
// Testes unitários
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

// Testes parametrizados (rstest)
#[rstest]
#[case("hello", 5)]
#[case("", 0)]
#[case("rust", 4)]
fn test_string_length(#[case] input: &str, #[case] expected: usize) {
    assert_eq!(input.len(), expected);
}

// Testes assíncronos
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Property tests (proptest)
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        assert_eq!(input, decoded);
    }
}
```

**Comandos de Cobertura**:
```bash
cargo llvm-cov                    # Resumo de cobertura
cargo llvm-cov --html             # Relatório HTML
cargo llvm-cov --fail-under-lines 80  # Falhar se abaixo do limiar
cargo test                        # Executar testes
```

---

## /cpp-test

**Propósito**: Fluxo de trabalho TDD para linguagem C++. Usa GoogleTest/GoogleMock e CMake/CTest, escreve testes primeiro, verifica cobertura (gcov/lcov).

**Como Usar**:
```
/cpp-test [descrição da funcionalidade]
```

**Cenários de Uso**:
- Implementar novas classes ou funções C++
- Adicionar cobertura de testes
- Corrigir bugs (escrever teste que falha primeiro)
- Aprender metodologia TDD C++

**Padrões GoogleTest**:
```cpp
// Testes básicos
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

// Testes parametrizados
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

**Comandos de Cobertura**:
```bash
cmake -DCMAKE_CXX_FLAGS="--coverage" -B build
cmake --build build
ctest --test-dir build
lcov --capture --directory build --output-file coverage.info
genhtml coverage.info --output-directory coverage_html
```

---

## /flutter-test

**Propósito**: Testes Flutter/Dart. Reporta falhas e corrige problemas de testes progressivamente, cobrindo testes unitários, de componentes, golden e integrados.

**Como Usar**:
```
/flutter-test                # Executar todos os testes
/flutter-test --coverage     # Com cobertura
/flutter-test test/file.dart # Executar arquivo de teste específico
```

**Cenários de Uso**:
- Verificar que funcionalidades não foram quebradas
- Verificar cobertura de testes de novo código
- Corrigir arquivo de teste específico
- Antes de submeter PR

**Falhas Comuns de Testes e Correções**:

| Tipo de Falha | Correção Típica |
|----------|----------|
| `Expected: <X> Actual: <Y>` | Atualizar assertion ou corrigir implementação |
| `Widget not found` | Corrigir seletor finder ou atualizar nome do widget |
| `Golden file not found` | Executar `flutter test --update-goldens` para gerar |
| `MissingPluginException` | Mockar platform channels na configuração de teste |
| `pumpAndSettle timed out` | Substituir por `pump(Duration)` explícito |

---

## /e2e

**Propósito**: Fluxo de trabalho de testes ponta a ponta. Gera e executa testes E2E para fluxos críticos de usuários.

**Como Usar**:
```
/e2e [descrição da funcionalidade]
```

**Cenários de Uso**:
- Testar fluxos críticos de usuários
- Testes de integração entre sistemas
- Testes de aceitação
- Testes de regressão

---

## /test-coverage

**Propósito**: Analisar cobertura de testes, identificar lacunas e gerar testes faltantes para atingir limiar alvo (default 80%).

**Como Usar**:
```
/test-coverage
```

**Fluxo de Trabalho**:
1. **Detectar Framework de Testes** - Selecionar comando de cobertura baseado no tipo de projeto
2. **Analisar Relatório de Cobertura** - Listar arquivos abaixo de 80%
3. **Gerar Testes Faltantes** - Gerar testes por prioridade
4. **Verificar** - Reexecutar cobertura para confirmar melhoria

**Detecção de Framework de Testes**:

| Indicador | Comando de Cobertura |
|--------|----------|
| `jest.config.*` | `npx jest --coverage` |
| `vitest.config.*` | `npx vitest run --coverage` |
| `pytest.ini` | `pytest --cov=src --cov-report=json` |
| `Cargo.toml` | `cargo llvm-cov --json` |
| `go.mod` | `go test -coverprofile=coverage.out` |

---

## /python-testing

**Propósito**: Fluxo de trabalho de testes Python. Usa pytest, suporta análise de cobertura.

**Como Usar**:
```
/python-testing [descrição da funcionalidade]
```

**Cenários de Uso**:
- Implementar novas funcionalidades Python
- Adicionar cobertura de testes
- Corrigir bugs
- Testes de projetos FastAPI/Django/Flask

**Funcionalidades Pytest**:
```python
# Testes parametrizados
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

# Testes assíncronos
@pytest.mark.asyncio
async def test_async_fetch():
    result = await fetch_data()
    assert result is not None
```

---

## Tabela Comparativa de Comandos

| Comando | Linguagem | Framework de Testes | Ferramenta de Cobertura | Loop TDD |
|------|------|----------|------------|----------|
| `/go-test` | Go | testing nativo | go test -cover | ✓ |
| `/kotlin-test` | Kotlin | Kotest + MockK | Kover | ✓ |
| `/rust-test` | Rust | #[test], rstest, proptest | cargo-llvm-cov | ✓ |
| `/cpp-test` | C++ | GoogleTest | gcov/lcov | ✓ |
| `/flutter-test` | Dart | Flutter test | coverage | - |
| `/python-testing` | Python | pytest | pytest-cov | - |
| `/e2e` | Multi-linguagem | Específico do projeto | - | - |
| `/test-coverage` | Genérico | Múltiplos | Múltiplos | - |