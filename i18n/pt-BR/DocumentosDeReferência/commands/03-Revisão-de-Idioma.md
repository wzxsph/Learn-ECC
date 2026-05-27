# Comandos de Revisão de Linguagem

Este documento apresenta comandos especializados no ECC para revisão de código em várias linguagens de programação.

---

## /python-review

**Propósito**: Revisão completa de código Python. Verifica conformidade PEP 8, type hints, segurança e idiomaticidade Python.

**Como Usar**:
```
/python-review
```

**Cenários de Uso**:
- Após escrever ou modificar código Python
- Antes de fazer commit de mudanças Python
- Revisar PR que contém código Python
- Aprender idiomática Python e melhores práticas

**Categorias de Revisão**:

### CRITICAL (Deve corrigir)
- Vulnerabilidades de SQL/command injection
- Uso inseguro de eval/exec
- Desserialização insegura de Pickle
- Credenciais hardcoded
- Carregamento inseguro de YAML
- Cláusulas except nuas que escondem erros

### HIGH (Deveria corrigir)
- Funções públicas sem type hints
- Argumentos padrão mutáveis
- Engolir exceções silenciosamente
- Gerenciamento de recursos sem context manager
- Loops estilo C ao invés de list comprehensions
- Uso de type() ao invés de isinstance()

### MEDIUM (Sugere considerar)
- Violações de formatação PEP 8
- Funções públicas sem docstrings
- Instruções print ao invés de logging
- Operações de string ineficientes
- Números mágicos sem constantes nomeadas
- Não usar f-strings

**Comandos de Verificação Automática**:
```bash
mypy .                              # Verificação de tipo
ruff check .                        # Linting
black --check .                     # Verificação de formatação
isort ---check-only .                # Ordenação de imports
bandit -r .                         # Varredura de segurança
pip-audit                           # Auditoria de dependências
pytest --cov=app --cov-report=term-missing  # Cobertura de teste
```

**Correção de Problemas Comuns**:

```python
# Correção de SQL injection
# Errado
query = f"SELECT * FROM users WHERE id = {user_id}"

# Correto - usar query parametrizada
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))

# Correção de argumentos padrão mutáveis
# Errado
def process_items(items=[]):
    items.append("new")
    return items

# Correto
def process_items(items=None):
    if items is None:
        items = []
    items.append("new")
    return items

# Usar context manager
# Errado
f = open("config.json")
data = f.read()
f.close()

# Correto
with open("config.json") as f:
    data = f.read()
```

---

## /go-review

**Propósito**: Revisão completa de código Go. Verifica padrões idiomáticos, segurança de concorrência, tratamento de erros e segurança.

**Como Usar**:
```
/go-review
```

**Cenários de Uso**:
- Após escrever ou modificar código Go
- Antes de fazer commit de mudanças Go
- Revisar PR que contém código Go
- Aprender padrões Go idiomáticos

**Categorias de Revisão**:

### CRITICAL (Deve corrigir)
- Vulnerabilidades de SQL/command injection
- Condições de corrida sem sincronização
- Vazamento de Goroutine
- Credenciais hardcoded
- Uso inseguro de ponteiros
- Ignorar erros em caminhos críticos

### HIGH (Deveria corrigir)
- Falta wrapper de contexto de erro
- Panic ao invés de retorno de erro
- Context não propagado
- Canais não bufferizados causando deadlock
- Interface não implementada corretamente
- Falta proteção de mutex

### MEDIUM (Sugere considerar)
- Padrões de código não idiomáticos
- Funções exportadas sem comentários godoc
- Concatenação de string ineficiente
- Slice não pre-alocado
- Não usar testes table-driven

**Comandos de Verificação Automática**:
```bash
go vet ./...                         # Análise estática
staticcheck ./...                    # Verificações avançadas (se instalado)
golangci-lint run                   # Linting
go build -race ./...                # Detecção de corrida
govulncheck ./...                   # Vulnerabilidades de segurança
```

---

## /kotlin-review

**Propósito**: Revisão completa de código Kotlin. Verifica padrões idiomáticos, null safety, segurança de coroutines e segurança.

**Como Usar**:
```
/kotlin-review
```

**Cenários de Uso**:
- Após escrever ou modificar código Kotlin
- Antes de fazer commit de mudanças Kotlin
- Revisar PR de projetos Android/KMP
- Aprender padrões Kotlin idiomáticos

**Categorias de Revisão**:

### CRITICAL (Deve corrigir)
- Vulnerabilidades de SQL/command injection
- Desembrulhar forçado `!!` sem razão suficiente
- Violações de null safety de tipo de plataforma
- Uso de GlobalScope (violação de concorrência estrutural)
- Credenciais hardcoded
- Desserialização insegura

### HIGH (Deveria corrigir)
- Estado mutável (quando imutável é possível)
- Chamadas bloqueantes dentro de contexto de coroutine
- Falta verificação de cancelamento em loops longos
- Expressão `when` de sealed class não exaustiva
- Funções grandes (>50 linhas)
- Aninhamento profundo (>4 níveis)

### MEDIUM (Sugere considerar)
- Kotlin não idiomático (padrões estilo Java)
- Falta de vírgulas à direita
- Uso incorreto ou aninhamento de scope functions
- Falta de Sequence em chains de collections grandes
- Tipos explícitos redundantes

**Comandos de Verificação Automática**:
```bash
./gradlew build                      # Verificação de build
./gradlew detekt                     # Análise estática
./gradlew ktlintCheck                # Verificação de formatação
./gradlew test                       # Testes
```

---

## /rust-review

**Propósito**: Revisão completa de código Rust. Verifica ownership, lifetime, tratamento de erros, uso de unsafe e padrões idiomáticos.

**Como Usar**:
```
/rust-review
```

**Cenários de Uso**:
- Após escrever ou modificar código Rust
- Antes de fazer commit de mudanças Rust
- Revisar PR de projetos Rust
- Aprender padrões Rust idiomáticos

**Categorias de Revisão**:

### CRITICAL (Deve corrigir)
- `unwrap()`/`expect()` sem checagem em caminho de produção
- `unsafe` sem comentário `// SAFETY:`
- SQL injection com interpolação de string
- Input de comando não validado
- Credenciais hardcoded
- Use-after-free de ponteiros crus

### HIGH (Deveria corrigir)
- `.clone()` desnecessário para satisfazer borrow checker
- Parâmetros `String` deveriam usar `&str` ou `impl AsRef<str>`
- Bloqueio em contexto assíncrono (`std::thread::sleep`)
- Tipos compartilhados sem constraints `Send`/`Sync`
- Match com curinga `_ =>` em enums críticos
- Funções grandes (>50 linhas)

### MEDIUM (Sugere considerar)
- Alocações desnecessárias em hot paths
- Falta `with_capacity` quando tamanho é conhecido
- Suprimir avisos clippy sem razão suficiente
- APIs públicas sem documentação `///`
- Considerar usar `#[must_use]` em tipos de retorno que podem ser ignorados

**Comandos de Verificação Automática**:
```bash
cargo check                           # Verificação de build
cargo clippy -- -D warnings            # Lints
cargo fmt --check                      # Verificação de formatação
cargo test                            # Testes
cargo audit                           # Auditoria de segurança (se disponível)
```

---

## /cpp-review

**Propósito**: Revisão completa de código C++. Verifica segurança de memória, idiomática C++ moderna, concorrência e segurança.

**Como Usar**:
```
/cpp-review
```

**Cenários de Uso**:
- Após escrever ou modificar código C++
- Antes de fazer commit de mudanças C++
- Revisar PR de projetos C++
- Verificar problemas de segurança de memória

**Categorias de Revisão**:

### CRITICAL (Deve corrigir)
- `new`/`delete` crus sem RAII
- Buffer overflow e use-after-free
- Condições de corrida de dados sem sincronização
- Command injection através de `system()`
- Variáveis não inicializadas lidas
- Dereferência de ponteiro nulo

### HIGH (Deveria corrigir)
- Violação da Regra dos Cinco
- Falta `std::lock_guard` / `std::scoped_lock`
- Gerenciamento incorreto de tempo de vida de threads detach
- Casts estilo C ao invés de `static_cast`/`dynamic_cast`
- Falta de `const` correctness

### MEDIUM (Sugere considerar)
- Cópias desnecessárias (passar `const&` ao invés de valor)
- Falta `reserve()` quando tamanho é conhecido
- `using namespace std;` em arquivos de cabeçalho
- Retorno de valor importante sem `[[nodiscard]]`
- Metaprogramação de template excessivamente complexa

**Comandos de Verificação Automática**:
```bash
clang-tidy --checks='*,-llvmlibc-*' src/*.cpp -- -std=c++17
cppcheck --enable=all --suppress=missingIncludeSystem src/
cmake --build build -- -Wall -Wextra -Wpedantic
```

---

## /flutter-review

**Propósito**: Revisão de código Flutter/Dart. Verifica padrões idiomáticos, melhores práticas de componentes, estado, performance, acessibilidade e segurança.

**Como Usar**:
```
/flutter-review
```

**Cenários de Uso**:
- Antes de fazer commit de PR com mudanças Flutter/Dart (após build e testes passarem)
- Descobrir problemas cedo após implementar nova feature
- Revisar código Flutter de outros
- Auditar componentes, gerenciadores de estado ou classes de serviço
- Antes de release de produção

**Áreas de Revisão**:

| Área | Nível de Severidade |
|------|-------------------|
| Chaves hardcoded, HTTP plain text | CRITICAL |
| Violações de arquitetura, anti-patterns de estado | CRITICAL |
| Problemas de rebuild de widget, vazamento de recursos | HIGH |
| Falta `dispose()`, BuildContext após `await` | HIGH |
| Null safety Dart, falta estados de erro/carregamento | HIGH |
| Propagação de const, composição de widget | HIGH |
| Trabalho caro em `build()` | HIGH |
| Acessibilidade, labels semânticos | MEDIUM |
| Transições de estado sem testes | HIGH |
| Strings hardcoded (l10n) | MEDIUM |

---

## /fastapi-review

**Propósito**: Revisão de aplicações FastAPI. Verifica arquitetura, correção assíncrona, injeção de dependência, esquemas Pydantic, segurança, performance e testabilidade.

**Como Usar**:
```
/fastapi-review [arquivo ou diretório]
```

**Áreas de Revisão**:
- App factory, limites de rota, middleware e exception handlers
- Separação de esquemas Pydantic de request e response
- Injeção de dependência de sessão de banco, auth, paginação e configurações
- Padrões assíncronos de banco e HTTP externo
- CORS, auth, rate limiting, logging e tratamento de chaves
- Metadados OpenAPI e modelos de resposta documentados
- Configuração de test client e overrides de dependência

---

## Tabela Comparativa de Comandos de Revisão de Linguagem

| Comando | Linguagem | Verificações Principais | Níveis de Severidade |
|---------|-----------|-------------------------|---------------------|
| `/python-review` | Python | PEP 8, type hints, SQL injection | CRITICAL/HIGH/MEDIUM |
| `/go-review` | Go | Concorrência segura, tratamento de erros, goroutine | CRITICAL/HIGH/MEDIUM |
| `/kotlin-review` | Kotlin | Null safety, coroutines, GlobalScope | CRITICAL/HIGH/MEDIUM |
| `/rust-review` | Rust | Ownership, lifetimes, unsafe | CRITICAL/HIGH/MEDIUM |
| `/cpp-review` | C++ | Segurança de memória, RAII, concorrência | CRITICAL/HIGH/MEDIUM |
| `/flutter-review` | Flutter | Estado, acessibilidade, performance | CRITICAL/HIGH/MEDIUM |
| `/fastapi-review` | Python | Pydantic, DI, async, segurança | CRITICAL/HIGH/MEDIUM |