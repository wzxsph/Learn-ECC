# Agentes de Revisão de Código

Agentes de revisão de código são especializados em trabalho de revisão de qualidade, segurança e manutenibilidade de código.

## Lista de Agentes

| Nome do Agente | Propósito | Modelo | Ferramentas Principais |
|----------------|-----------|--------|----------------------|
| code-reviewer | Especialista geral em revisão de código | sonnet | Read, Grep, Glob, Bash |
| python-reviewer | Revisão de código Python (PEP 8, type hints, segurança) | sonnet | Read, Grep, Glob, Bash |
| go-reviewer | Revisão de código Go (concorrência, tratamento de erros, performance) | sonnet | Read, Grep, Glob, Bash |
| kotlin-reviewer | Revisão de código Kotlin/Android (coroutines, Compose) | sonnet | Read, Grep, Glob, Bash |
| rust-reviewer | Revisão de código Rust (ownership, lifetimes, segurança) | sonnet | Read, Grep, Glob, Bash |
| cpp-reviewer | Revisão de código C++ (segurança de memória, idiomática C++ moderna) | sonnet | Read, Grep, Glob, Bash |
| flutter-reviewer | Revisão de código Flutter/Dart (gerenciamento de estado, performance) | sonnet | Read, Grep, Glob, Bash |
| typescript-reviewer | Revisão de código TypeScript/JavaScript (type safety) | sonnet | Read, Grep, Glob, Bash |
| swift-reviewer | Revisão de código Swift (protocolos, concorrência, gerenciamento de memória ARC) | sonnet | Read, Grep, Glob, Bash |
| csharp-reviewer | Revisão de código C# (.NET convenções, padrões assíncronos) | sonnet | Read, Grep, Glob, Bash |
| fsharp-reviewer | Revisão de código F# (idiomas funcionais, pattern matching) | sonnet | Read, Grep, Glob, Bash |
| java-reviewer | Revisão de código Java (Spring Boot/Quarkus frameworks) | sonnet | Read, Grep, Glob, Bash |

---

## code-reviewer

### Nome e Propósito
Revisor de código geral, revisa proativamente qualidade, segurança e manutenibilidade de código. Deve ser usado após toda mudança de código.

### Capacidades
- Verificações de qualidade de código (tamanho de função, profundidade de aninhamento, duplicação de código)
- Detecção de vulnerabilidades de segurança (SQL injection, XSS, credenciais hardcoded)
- Verificações de padrão React/Next.js
- Verificações de padrão Node.js/backend
- Identificação de problemas de performance
- Sugestões de melhores práticas

### Cenários de Uso
- Após escrever ou modificar código
- Revisão de Pull Request
- Antes de fazer merge em branch compartilhado

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos
- Bash: Executar git diff e comandos de lint

### Colaboração com Outros Agentes
- Escalation para security-reviewer quando problemas de segurança são descobertos
- Usar build-error-resolver quando erros de build são encontrados
- Usar refactor-cleaner quando refatoração é necessária

---

## python-reviewer

### Nome e Propósito
Revisor de código Python especializado, focando em conformidade PEP 8, idiomática Python, type hints, segurança e performance.

### Capacidades
- Verificações de estilo PEP 8
- Promoção de idiomática Python (list comprehensions, enums, with statements)
- Validação de type hints
- Detecção de SQL injection
- Verificações de padrões assíncronos e coroutines
- Verificações específicas de framework (Django, FastAPI, Flask)

### Cenários de Uso
- Todas as mudanças de código em projetos Python
- Projetos específicos Django/FastAPI/Flask

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos Python
- Bash: Executar mypy, ruff, black, bandit, pytest

### Colaboração com Outros Agentes
- Compartilhar regras de verificação de segurança com security-reviewer
- Usar tdd-guide para garantir cobertura de testes

---

## go-reviewer

### Nome e Propósito
Revisor de código Go especializado, focando em Go idiomático, padrões de concorrência, tratamento de erros e performance.

### Capacidades
- Verificações de idiomática Go (early returns, error wrapping)
- Verificações de segurança de concorrência (goroutine leaks, channel deadlocks)
- Melhores práticas de tratamento de erros
- Reconhecimento de padrões de performance
- Detecção de vulnerabilidades de segurança

### Cenários de Uso
- Todas as mudanças de código em projetos Go
- Projetos que precisam seguir convenções de código Go

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos Go
- Bash: Executar go vet, staticcheck, golangci-lint

### Colaboração com Outros Agentes
- go-build-resolver lidar com erros de build
- security-reviewer lidar com problemas relacionados a segurança

---

## kotlin-reviewer

### Nome e Propósito
Revisor de código Kotlin e Android/KMP especializado, focando em padrões idiomáticos, segurança de coroutines, melhores práticas de Compose e violações de Clean Architecture.

### Capacidades
- Verificações de idiomática Kotlin
- Detecção de anti-padrões de coroutines e Flow
- Identificação de problemas de performance de Compose
- Aplicação de limites de módulo de Clean Architecture
- Verificações de problemas específicos de Android

### Cenários de Uso
- Desenvolvimento nativo Android
- Projetos Kotlin Multiplatform (KMP)
- Projetos Jetpack Compose

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos Kotlin/KTS
- Bash: Executar verificações Gradle

### Colaboração com Outros Agentes
- kotlin-build-resolver lidar com problemas de build
- security-reviewer lidar com problemas críticos de segurança

---

## rust-reviewer

### Nome e Propósito
Revisor de código Rust especializado, focando em ownership, segurança de lifetimes, tratamento de erros, uso de unsafe e padrões idiomáticos.

### Capacidades
- Verificações de ownership e lifetimes
- Diagnóstico de erros de borrow checker
- Revisão de segurança de código unsafe
- Validação de padrões de tratamento de erros
- Verificações de segurança de concorrência

### Cenários de Uso
- Todas as mudanças de código em projetos Rust
- Projetos que precisam de garantias de segurança de memória

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos Rust
- Bash: Executar cargo check, clippy, fmt, test, audit

### Colaboração com Outros Agentes
- rust-build-resolver lidar com erros de build
- Compartilhar regras de revisão de segurança com security-reviewer

---

## cpp-reviewer

### Nome e Propósito
Revisor de código C++ especializado, focando em segurança de memória, idiomática C++ moderna, concorrência e performance.

### Capacidades
- Verificações de segurança de memória (new/delete crus, buffer overflow)
- Promoção de padrões C++ modernos (smart pointers, RAII)
- Verificações de segurança de concorrência
- Reconhecimento de padrões de performance
- Detecção de vulnerabilidades de segurança

### Cenários de Uso
- Todas as mudanças de código em projetos C++
- Projetos que precisam de melhores práticas C++ modernas

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos C++
- Bash: Executar clang-tidy, cppcheck, cmake

### Colaboração com Outros Agentes
- cpp-build-resolver lidar com problemas de build
- security-reviewer lidar com problemas críticos de segurança

---

## flutter-reviewer

### Nome e Propósito
Revisor de código Flutter e Dart especializado, revisando melhores práticas de widget, padrões de gerenciamento de estado, idiomática Dart, armadilhas de performance, acessibilidade e violações de Clean Architecture.

### Capacidades
- Detecção de anti-padrões de gerenciamento de estado (uso indevido de setState)
- Otimização de performance de widget build
- Aplicação de limites de arquitetura
- Gerenciamento de ciclo de vida de recursos
- Verificações de acessibilidade

### Cenários de Uso
- Todas as mudanças de código em projetos Flutter
- Desenvolvimento de aplicações móveis cross-platform

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos Dart
- Bash: Executar flutter analyze

### Colaboração com Outros Agentes
- dart-build-resolver lidar com problemas de build
- security-reviewer lidar com problemas críticos de segurança
- e2e-runner executar testes end-to-end

---

## typescript-reviewer

### Nome e Propósito
Revisor de código TypeScript/JavaScript especializado, focando em type safety, correção assíncrona, segurança Node.js/web e padrões idiomáticos.

### Capacidades
- Verificações de type safety (abuso de any, assertions não-null)
- Validação de correção assíncrona
- Detecção de vulnerabilidades de segurança
- Verificações específicas React/Next.js
- Verificações específicas Node.js

### Cenários de Uso
- Todas as mudanças de código em projetos TypeScript/JavaScript
- Projetos Next.js e React

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos TS/TSX/JS/JSX
- Bash: Executar tsc, eslint, prettier, npm audit

### Colaboração com Outros Agentes
- build-error-resolver lidar com erros de build
- security-reviewer lidar com problemas críticos de segurança

---

## swift-reviewer

### Nome e Propósito
Revisor de código Swift especializado, focando em design orientado a protocolo, semântica de valor, gerenciamento de memória ARC, Swift Concurrency e padrões idiomáticos.

### Capacidades
- Verificações de segurança de Swift Concurrency
- Verificações de gerenciamento de memória (strong reference cycles, delegate references)
- Validação de design orientado a protocolo
- Melhores práticas de tratamento de erros
- Reconhecimento de padrões de performance

### Cenários de Uso
- Todas as mudanças de código em projetos Swift
- Desenvolvimento de aplicações iOS/macOS

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos Swift
- Bash: Executar swift build, swiftlint, swift test

### Colaboração com Outros Agentes
- swift-build-resolver lidar com problemas de build
- security-reviewer lidar com problemas críticos de segurança

---

## csharp-reviewer

### Nome e Propósito
Revisor de código C# especializado, focando em convenções .NET, padrões assíncronos, segurança, nullable reference types e performance.

### Capacidades
- Verificações de idiomática .NET
- Validação de padrões assíncronos
- Segurança de tipos anuláveis
- Detecção de vulnerabilidades de segurança
- Verificações específicas de EF Core

### Cenários de Uso
- Todas as mudanças de código em projetos C#
- Desenvolvimento de aplicações .NET/.NET Core

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos C#
- Bash: Executar dotnet build, dotnet format, dotnet test

### Colaboração com Outros Agentes
- build-error-resolver lidar com problemas de build .NET
- security-reviewer lidar com problemas críticos de segurança

---

## java-reviewer

### Nome e Propósito
Revisor de código Java especializado para projetos Spring Boot e Quarkus. Detecta automaticamente o framework e aplica regras de revisão apropriadas.

### Capacidades
- Detecção automática de framework (Spring Boot/Quarkus)
- Verificações de arquitetura de camadas
- Verificações JPA/Panache/MongoDB
- Verificações de segurança de concorrência
- Detecção de vulnerabilidades de segurança

### Cenários de Uso
- Projetos Spring Boot
- Projetos Quarkus
- Projetos Java 11+

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos Java
- Bash: Executar verificações Maven/Gradle

### Colaboração com Outros Agentes
- java-build-resolver lidar com problemas de build
- security-reviewer lidar com problemas críticos de segurança

---

## fsharp-reviewer

### Nome e Propósito
Revisor de código F# especializado, focando em idiomáticos funcionais, type safety, pattern matching, computation expressions e performance.

### Capacidades
- Verificações de idiomáticos funcionais
- Validação de completude de pattern matching
- Verificações de type safety
- Melhores práticas de computation expressions
- Reconhecimento de padrões de performance

### Cenários de Uso
- Todas as mudanças de código em projetos F#
- Projetos de programação funcional

### Ferramentas Utilizadas
- Read: Ler conteúdo de arquivo
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos F#
- Bash: Executar dotnet build, fantomas, dotnet test

### Colaboração com Outros Agentes
- Compartilhar regras de revisão de segurança com security-reviewer
- Usar tdd-guide para garantir cobertura de testes
[Voltar ao Índice de Agentes](../README.md)