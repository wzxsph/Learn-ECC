# Comandos de Build e Correção

Este documento apresenta comandos especializados no ECC para correção de erros de build em várias linguagens.

---

## /go-build

**Propósito**: Corrigir incrementalmente erros de build Go, avisos go vet e problemas de linter.

**Como Usar**:
```
/go-build
```

**Cenários de Uso**:
- `go build ./...` falha
- `go vet ./...` reporta problemas
- `golangci-lint run` mostra avisos
- Dependências de módulo corrompidas
- Build falha após fazer pull de mudanças

**Fluxo de Trabalho**:
1. **Executar Diagnóstico** - Executar `go build`, `go vet`, `staticcheck`
2. **Analisar Erros** - Agrupar por arquivo e ordenar por severidade
3. **Corrigir Incrementally** - Um erro por vez
4. **Verificar Correção** - Executar build novamente após cada mudança
5. **Reportar Resumo** - Mostrar problemas corrigidos e restantes

**Correções de Erros Comuns**:

| Erro | Correção Típica |
|------|----------|
| `undefined: X` | Adicionar importação ou corrigir digitação |
| `cannot use X as Y` | Conversão de tipo ou correção de atribuição |
| `missing return` | Adicionar instrução return |
| `X does not implement Y` | Adicionar método faltando |
| `import cycle` | Refatorar estrutura de pacote |
| `declared but not used` | Remover ou usar variável |
| `cannot find package` | `go get` ou `go mod tidy` |

**Comandos de Diagnóstico**:
```bash
go build ./...                # Verificação principal de build
go vet ./...                  # Análise estática
staticcheck ./...             # Lint avançado (se disponível)
golangci-lint run             # Linting
go mod verify                # Verificação de módulo
go mod tidy -v               # Organizar dependências
```

---

## /kotlin-build

**Propósito**: Corrigir incrementalmente erros de build Kotlin/Gradle, avisos do compilador e problemas de dependência.

**Como Usar**:
```
/kotlin-build
```

**Cenários de Uso**:
- `./gradlew build` falha
- Compilador Kotlin reporta erros
- `./gradlew detekt` reporta violações
- Resolução de dependência Gradle falha
- Build falha após fazer pull de mudanças

**Correções de Erros Comuns**:

| Erro | Correção Típica |
|------|----------|
| `Unresolved reference: X` | Adicionar importação ou dependência |
| `Type mismatch` | Corrigir conversão ou atribuição de tipo |
| `'when' must be exhaustive` | Adicionar branch faltando de sealed class |
| `Suspend function can only be called from coroutine` | Adicionar modificador `suspend` |
| `Smart cast impossible` | Usar `val` local ou `let` |
| `Could not resolve dependency` | Corrigir versão ou adicionar repositório |

**Comandos de Diagnóstico**:
```bash
./gradlew build 2>&1                      # Verificação principal de build
./gradlew detekt 2>&1                      # Análise estática (se configurado)
./gradlew ktlintCheck 2>&1                # Verificação de formatação (se configurado)
./gradlew dependencies --configuration runtimeClasspath | head -100  # Problemas de dependência
./gradlew build --refresh-dependencies     # Refresh profundo (quando cache ou metadados de dependência são suspeitos)
```

---

## /rust-build

**Propósito**: Corrigir incrementalmente erros de build Rust, problemas do borrow checker e problemas de dependência.

**Como Usar**:
```
/rust-build
```

**Cenários de Uso**:
- `cargo build` ou `cargo check` falha
- `cargo clippy` reporta avisos
- Erros de borrow checker ou lifetime impedem compilação
- Resolução de dependência Cargo falha
- Build falha após fazer pull de mudanças

**Correções de Erros Comuns**:

| Erro | Correção Típica |
|------|----------|
| `cannot borrow as mutable` | Refatorar para terminar empréstimo imutável antes de usar acesso mutável |
| `does not live long enough` | Usar tipo ownership ou adicionar anotações de lifetime |
| `cannot move out of` | Refatorar para adquirir ownership; clone como último recurso |
| `mismatched types` | Adicionar `.into()`, `as` ou conversão explícita |
| `trait X not implemented` | Adicionar `#[derive(Trait)]` ou implementação manual |
| `unresolved import` | Adicionar ao Cargo.toml ou corrigir caminho `use` |
| `cannot find value` | Adicionar importação ou corrigir caminho |

**Comandos de Diagnóstico**:
```bash
cargo check 2>&1                               # Verificação principal de build
cargo clippy -- -D warnings 2>&1               # Lints
cargo fmt --check 2>&1                         # Verificação de formatação
cargo tree --duplicates                        # Dependências duplicadas
cargo audit                                   # Auditoria de segurança (se disponível)
```

---

## /cpp-build

**Propósito**: Corrigir incrementalmente erros de build C++, problemas CMake e problemas de linker.

**Como Usar**:
```
/cpp-build
```

**Cenários de Uso**:
- `cmake --build build` falha
- Erros de linker (referências indefinidas, múltiplas definições)
- Falha de instanciação de template
- Problemas de inclusão/dependência
- Build falha após fazer pull de mudanças

**Correções de Erros Comuns**:

| Erro | Correção Típica |
|------|----------|
| `undeclared identifier` | Adicionar `#include` ou corrigir digitação |
| `no matching function` | Corrigir tipo de parâmetro ou adicionar overload |
| `undefined reference` | Linkar biblioteca ou adicionar implementação |
| `multiple definition` | Usar `inline` ou mover para .cpp |
| `incomplete type` | Substituir forward declaration por `#include` |
| `no member named X` | Corrigir nome do membro ou adicionar include |
| `cannot convert X to Y` | Adicionar conversão apropriada |
| `CMake Error` | Corrigir configuração CMakeLists.txt |

**Comandos de Diagnóstico**:
```bash
cmake -B build -S .                        # Configuração CMake
cmake --build build 2>&1 | head -100       # Build
clang-tidy src/*.cpp -- -std=c++17         # Análise estática (se disponível)
cppcheck --enable=all src/                 # Análise adicional (se disponível)
```

---

## /gradle-build

**Propósito**: Corrigir erros de build Gradle para projetos Android e Kotlin Multiplatform (KMP).

**Como Usar**:
```
/gradle-build
```

**Cenários de Uso**:
- Build de projeto Android falha
- Erros de compilação KMP
- Falha de sync Gradle
- Conflitos de dependência

**Detecção de Tipo de Projeto**:

| Indicador | Comando de Build |
|-----------|----------|
| `build.gradle.kts` + `composeApp/` (KMP) | `./gradlew composeApp:compileKotlinMetadata` |
| `build.gradle.kts` + `app/` (Android) | `./gradlew app:compileDebugKotlin` |
| `settings.gradle.kts` com módulos | `./gradlew assemble` |
| detekt configurado | `./gradlew detekt` |

**Correções de Erros Comuns**:

| Erro | Correção |
|------|------|
| `commonMain` referência não resolvida | Verificar se dependência está em `commonMain.dependencies {}` |
| Expect declaration sem actual | Adicionar implementação `actual` em cada conjunto de fontes de plataforma |
| Versão do Compose compiler incompatível | Alinhar versões do Kotlin e Compose compiler em `libs.versions.toml` |
| Classes duplicadas | Verificar dependências conflitantes com `./gradlew dependencies` |
| Erros KSP | Executar `./gradlew kspCommonMainKotlinMetadata` para regenerar |
| Problemas de cache de configuração | Verificar inputs de tarefa não serializáveis |

---

## /flutter-build

**Propósito**: Corrigir incrementalmente erros do analyzer Dart e falhas de build Flutter.

**Como Usar**:
```
/flutter-build
```

**Cenários de Uso**:
- `flutter analyze` reporta erros
- `flutter build` falha em qualquer plataforma
- `flutter pub get` conflitos de versão
- Geração de código `build_runner` falha
- Build falha após fazer pull de mudanças

**Correções de Erros Comuns**:

| Erro | Correção Típica |
|------|----------|
| `A value of type 'X?' can't be assigned to 'X'` | Adicionar `?? default` ou proteção null |
| `The name 'X' isn't defined` | Adicionar importação ou corrigir digitação |
| `Non-nullable instance field must be initialized` | Adicionar inicializador ou `late` |
| `Version solving failed` | Ajustar restrições de versão em pubspec.yaml |
| `Missing concrete implementation of 'X'` | Implementar métodos de interface faltando |
| `build_runner: Part of X expected` | Excluir `.g.dart` desatualizados e reconstruir |

**Comandos de Diagnóstico**:
```bash
flutter analyze 2>&1                  # Análise
flutter pub get 2>&1                  # Dependências
dart run build_runner build --delete-conflicting-outputs 2>&1  # Geração de código (se usando build_runner)
flutter build apk 2>&1                # Build de plataforma
flutter build web 2>&1                # Build web
```

---

## Tabela Comparativa de Comandos de Build

| Comando | Linguagem/Plataforma | Ferramentas Principais | Problemas Comuns |
|---------|----------|----------|----------|
| `/go-build` | Go | go build, go vet | Erros de tipo, ciclos de importação |
| `/kotlin-build` | Kotlin | Gradle, detekt | Tipo incompatível, when não exaustivo |
| `/rust-build` | Rust | cargo check, clippy | Erros de borrow, lifetimes |
| `/cpp-build` | C++ | CMake, clang-tidy | Erros de linker, problemas de template |
| `/gradle-build` | Android/KMP | Gradle | Conflitos de dependência, erros de configuração |
| `/flutter-build` | Flutter/Dart | Flutter analyze | Null safety, erros de análise |

---

## Estratégias Genéricas de Correção

Todos os comandos de correção de build seguem a mesma estratégia básica:

1. **Primeiro corrigir erros de build** - Código deve compilar
2. **Depois corrigir avisos de lint** - Corrigir construções suspeitas
3. **Então corrigir avisos de formatação** - Estilo e melhores práticas
4. **Corrigir um por vez** - Verificar após cada mudança
5. **Mudanças mínimas** - Não refatorar, apenas corrigir

**Condições de Parada**:
O agente parará e reportará se:
- O mesmo erro persiste após 3 tentativas
- A correção introduz mais erros
- É necessária mudança de arquitetura
- Dependência externa faltando