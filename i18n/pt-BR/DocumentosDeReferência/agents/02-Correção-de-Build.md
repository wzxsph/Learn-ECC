# Agentes de Build e Correção

Agentes de build e correção são especializados em diagnosticar e corrigir erros de build, erros de compilação e problemas de dependência em várias linguagens de programação.

## Lista de Agentes

| Nome do Agente | Propósito | Modelo | Ferramentas Principais |
|----------------|-----------|--------|----------------------|
| build-error-resolver | Correção de erros TypeScript/build genéricos | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| go-build-resolver | Correção de erros build/compilação Go | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| kotlin-build-resolver | Correção de erros Kotlin/Gradle build | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| rust-build-resolver | Correção de erros Rust build/borrow checker | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| cpp-build-resolver | Correção de erros C++/CMake build | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| java-build-resolver | Correção de erros Java/Maven/Gradle build | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| swift-build-resolver | Correção de erros Swift/Xcode build | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| dart-build-resolver | Correção de erros Dart/Flutter build | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| django-build-resolver | Correção de problemas Django/Python build | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| pytorch-build-resolver | Correção de problemas PyTorch/CUDA build | sonnet | Read, Write, Edit, Bash, Grep, Glob |

---

## build-error-resolver

### Nome e Propósito
Especialista em resolver erros de build e tipo TypeScript. Corrige build failing ou erros de tipo, focado em deixar o build verde, sem fazer mudanças de arquitetura.

### Capacidades
- Resolução de erros TypeScript
- Correção de erros de build
- Correção de problemas de dependência
- Resolução de erros de configuração
- Correção de diff mínima

### Cenários de Uso
- Quando o build falha
- Quando ocorrem erros de tipo TypeScript
- Quando há problemas de resolução de módulo

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar tsc, npm build, eslint
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- Não fazer refatoração → usar refactor-cleaner
- Não fazer mudanças de arquitetura → usar architect
- Não adicionar novas funcionalidades → usar planner
- Não corrigir testes → usar tdd-guide
- Não lidar com problemas de segurança → usar security-reviewer

### Princípios Centrais
- Apenas corrigir erros, não refatorar
- Mudanças mínimas
- Verificar após cada correção se o build passa

---

## go-build-resolver

### Nome e Propósito
Especialista em resolver erros de build Go, vet e compilação. Corrige erros de build Go, problemas de go vet e avisos de linter.

### Capacidades
- Diagnóstico de erros de compilação Go
- Correção de avisos go vet
- Correção de problemas staticcheck/golangci-lint
- Resolução de problemas de dependência de módulo
- Tratamento de erros de tipo e incompatibilidade de interface

### Cenários de Uso
- Quando o build Go falha
- Quando go vet reporta problemas
- Quando há conflitos de dependência de módulo

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar go build, go vet, staticcheck, golangci-lint
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- go-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

### Padrões Comuns de Correção de Erros

| Tipo de Erro | Causa | Método de Correção |
|--------------|-------|-------------------|
| undefined: X | Importação faltando, erro de digitação | Adicionar importação ou corrigir maiúsculas/minúsculas |
| cannot use X as type Y | Tipo incompatível | Conversão de tipo ou dereferência |
| X does not implement Y | Método faltando | Implementar método com receiver correto |
| import cycle not allowed | Dependência circular | Extrair tipo compartilhado para novo pacote |
| cannot find package | Dependência faltando | go get pkg@version ou go mod tidy |

---

## kotlin-build-resolver

### Nome e Propósito
Especialista em resolver erros de build, compilação e dependência Kotlin/Gradle. Corrige erros de build Kotlin, erros do compilador Kotlin e problemas do Gradle.

### Capacidades
- Diagnóstico de erros de compilação Kotlin
- Correção de problemas de configuração de build Gradle
- Resolução de conflitos de dependência e incompatibilidade de versão
- Tratamento de erros do compilador Kotlin
- Correção de violações detekt e ktlint

### Cenários de Uso
- Quando o build Kotlin falha
- Quando há conflitos de dependência Gradle
- Quando o compilador Kotlin reporta erros

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar ./gradlew build, detekt, ktlintCheck
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- kotlin-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

### Padrões Comuns de Correção de Erros

| Tipo de Erro | Causa | Método de Correção |
|--------------|-------|-------------------|
| Unresolved reference: X | Importação faltando, erro de digitação | Adicionar importação ou dependência |
| Type mismatch | Erro de tipo | Adicionar conversão ou corrigir tipo |
| Smart cast impossible | Propriedade mutável ou acesso concorrente | Usar cópia val local ou let |
| when expression must be exhaustive | Falta branch em sealed class when | Adicionar branch faltando ou else |
| Suspend function can only be called from coroutine | Falta modificador suspend ou escopo de coroutine | Adicionar modificador suspend |

---

## rust-build-resolver

### Nome e Propósito
Especialista em resolver erros de build, compilação e dependência Rust. Corrige erros de cargo build, problemas do borrow checker e problemas de Cargo.toml.

### Capacidades
- Diagnóstico de erros cargo build/check
- Correção de erros de borrow checker e lifetime
- Resolução de incompatibilidade de implementação de trait
- Tratamento de problemas de dependência e feature do Cargo
- Correção de avisos cargo clippy

### Cenários de Uso
- Quando o build Rust falha
- Quando o borrow checker reporta erros
- Quando há conflitos de dependência Cargo

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar cargo check, clippy, fmt, tree
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- rust-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

### Padrões Comuns de Correção de Erros

| Tipo de Erro | Causa | Método de Correção |
|--------------|-------|-------------------|
| cannot borrow as mutable | Empréstimo imutável ainda ativo | Refatorar para terminar empréstimo imutável primeiro |
| does not live long enough | Valor descartado enquanto ainda emprestado | Expandir escopo do lifetime |
| cannot move out of | Mover após referência | Usar .clone() ou refatorar |
| mismatched types | Erro de tipo | Adicionar .into(), as ou conversão explícita |
| trait X is not implemented for Y | Falta impl ou derive | Adicionar #[derive(Trait)] |

---

## cpp-build-resolver

### Nome e Propósito
Especialista em resolver erros de build, CMake e compilação C++. Corrige erros de build, problemas de linker e erros de template.

### Capacidades
- Diagnóstico de erros de compilação C++
- Correção de problemas de configuração CMake
- Resolução de erros de linker (referências indefinidas, definições duplicadas)
- Tratamento de erros de instanciação de template
- Correção de problemas de inclusão e dependência

### Cenários de Uso
- Quando o build C++ falha
- Quando há erros de configuração CMake
- Quando o linker reporta erros

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar cmake --build, clang-tidy, cppcheck
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- cpp-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

### Padrões Comuns de Correção de Erros

| Tipo de Erro | Causa | Método de Correção |
|--------------|-------|-------------------|
| undefined reference to X | Implementação ou biblioteca faltando | Adicionar arquivo fonte ou linkar biblioteca |
| no matching function for call | Erro de tipo de parâmetro | Corrigir tipo ou adicionar overload |
| multiple definition of | Símbolo duplicado | Usar inline ou mover para .cpp |
| cannot convert X to Y | Tipo incompatível | Adicionar conversão ou corrigir tipo |
| template argument deduction failed | Erro de parâmetro de template | Corrigir parâmetros de template |

---

## java-build-resolver

### Nome e Propósito
Especialista em resolver erros de build, compilação e dependência Java/Maven/Gradle. Detecta automaticamente Spring Boot ou Quarkus e aplica correções específicas do framework.

### Capacidades
- Diagnóstico de erros de compilação Java
- Correção de problemas de configuração de build Maven e Gradle
- Resolução de conflitos de dependência e incompatibilidade de versão
- Tratamento de erros de processador de anotação (Lombok, MapStruct, Spring, Quarkus)
- Correção de violações Checkstyle e SpotBugs

### Cenários de Uso
- Quando o build Java falha
- Quando há conflitos de dependência Maven/Gradle
- Quando há problemas específicos de build de framework

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar ./mvnw compile, ./gradlew build, dependency:tree
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- java-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

### Detecção de Framework
Detecção automática de framework do projeto:
- Se contém `quarkus` → aplicar regras [QUARKUS]
- Se contém `spring-boot` → aplicar regras [SPRING]
- Ambos → aplicar dois conjuntos de regras

### Padrões Comuns de Correção de Erros

| Tipo de Erro | Causa | Método de Correção |
|--------------|-------|-------------------|
| cannot find symbol | Importação faltando, erro de digitação | Adicionar importação ou dependência |
| incompatible types | Tipo incompatível | Adicionar conversão explícita |
| package X does not exist | Dependência faltando | Adicionar ao pom.xml/build.gradle |
| No qualifying bean of type X | Falta @Component ou scanning de componente | Adicionar anotação ou corrigir pacote de scanning |

---

## swift-build-resolver

### Nome e Propósito
Especialista em resolver erros de build, compilação e dependência Swift/Xcode. Corrige erros de build swift, falhas de build Xcode, problemas de dependência SPM e problemas de code signing.

### Capacidades
- Diagnóstico de erros swift build/xcodebuild
- Correção de erros de type checker e conformidade de protocolo
- Resolução de problemas de Swift Concurrency e Sendable
- Tratamento de falhas de resolução e versão de dependência SPM
- Correção de problemas de configuração de projeto Xcode e code signing

### Cenários de Uso
- Quando o build Swift falha
- Quando o build Xcode falha
- Quando há conflitos de dependência SPM

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar swift build, xcodebuild, swiftlint
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- swift-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

### Padrões Comuns de Correção de Erros

| Tipo de Erro | Causa | Método de Correção |
|--------------|-------|-------------------|
| cannot find type 'X' in scope | Importação faltando ou erro de digitação | Adicionar import ou corrigir nome |
| type 'X' does not conform to protocol 'Y' | Faltam membros requeridos | Implementar requisitos de protocolo faltando |
| non-sendable type passed | Violação de Sendable | Adicionar conformidade Sendable ou refatorar |
| @MainActor function cannot be called | Isolamento Main actor | Adicionar await ou usar MainActor.run |

---

## dart-build-resolver

### Nome e Propósito
Especialista em resolver erros de build, análise e dependência Dart/Flutter. Corrige erros de dart analyze, falhas de compilação Flutter, conflitos de dependência pub e problemas de build_runner.

### Capacidades
- Diagnóstico de erros dart analyze/flutter analyze
- Correção de erros de tipo Dart, violações de null safety e importações faltando
- Resolução de conflitos de dependência pubspec.yaml e restrições de versão
- Correção de falhas de geração de código build_runner
- Tratamento de erros específicos de build Flutter (Android Gradle, iOS CocoaPods, web)

### Cenários de Uso
- Quando o build Dart/Flutter falha
- Quando há conflitos de dependência pub
- Quando a geração de build_runner falha

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar flutter analyze, dart analyze, flutter pub get, build_runner
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- flutter-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

### Padrões Comuns de Correção de Erros

| Tipo de Erro | Causa | Método de Correção |
|--------------|-------|-------------------|
| The name 'X' isn't defined | Importação faltando ou erro de digitação | Adicionar import correto |
| A value of type 'X?' can't be assigned to type 'X' | Null safety - tipo anulável não tratado | Adicionar !, ?? default ou verificação null |
| Because X depends on Y >=A and Z depends on Y <B | Conflito de versão pub | Ajustar restrições de versão ou adicionar dependency_overrides |
| build_runner: No actions were run | Entrada de geração de código sem mudanças | Usar --delete-conflicting-outputs para forçar reconstrução |

---

## django-build-resolver

### Nome e Propósito
Especialista em corrigir problemas de build e dependência Django/Python. Trata problemas específicos de build Django e conflitos de dependência Python.

### Capacidades
- Correção de problemas de configuração de projeto Django
- Resolução de conflitos de dependência Python
- Tratamento de problemas de migração Django
- Gerenciamento de dependências requirements.txt/pipfile

### Cenários de Uso
- Quando o build de projeto Django falha
- Quando há conflitos de dependência Python
- Quando há problemas de migração Django

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar pip, pipenv, poetry, django-admin
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- python-reviewer para revisar qualidade de código
- security-reviewer para lidar com problemas relacionados a segurança

---

## pytorch-build-resolver

### Nome e Propósito
Especialista em corrigir problemas de build PyTorch/CUDA. Trata problemas específicos de build PyTorch, problemas de compatibilidade CUDA e erros de operação de tensor.

### Capacidades
- Diagnóstico de erros de build PyTorch
- Correção de problemas de compatibilidade CUDA
- Tratamento de erros de forma e posicionamento de tensor
- Correção de falhas de DataLoader e AMP
- Problemas de configuração de ambiente PyTorch

### Cenários de Uso
- Quando o build PyTorch falha
- Quando há problemas de compatibilidade CUDA
- Quando há falhas de training/inference

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Write: Escrever correções
- Edit: Editar arquivos
- Bash: Executar python, pip, nvcc, nvidia-smi
- Grep: Pesquisar padrões de erro
- Glob: Encontrar arquivos relacionados

### Colaboração com Outros Agentes
- mle-reviewer para revisar código ML
- security-reviewer para lidar com problemas relacionados a segurança
[Voltar ao Índice de Agentes](../README.md)