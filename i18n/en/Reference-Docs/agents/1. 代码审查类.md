# Code Review Agents

Code Review Agents specialize in code quality, security, and maintainability review.

## Agent List

| Agent Name | Purpose | Model Used | Core Tools |
|------------|---------|------------|------------|
| code-reviewer | General code review expert | sonnet | Read, Grep, Glob, Bash |
| python-reviewer | Python code review (PEP 8, type hints, security) | sonnet | Read, Grep, Glob, Bash |
| go-reviewer | Go code review (concurrency, error handling, performance) | sonnet | Read, Grep, Glob, Bash |
| kotlin-reviewer | Kotlin/Android code review (coroutines, Compose) | sonnet | Read, Grep, Glob, Bash |
| rust-reviewer | Rust code review (ownership, lifetime safety) | sonnet | Read, Grep, Glob, Bash |
| cpp-reviewer | C++ code review (memory safety, modern C++ idioms) | sonnet | Read, Grep, Glob, Bash |
| flutter-reviewer | Flutter/Dart code review (state management, performance) | sonnet | Read, Grep, Glob, Bash |
| typescript-reviewer | TypeScript/JavaScript code review (type safety) | sonnet | Read, Grep, Glob, Bash |
| swift-reviewer | Swift code review (protocols, concurrency, ARC memory management) | sonnet | Read, Grep, Glob, Bash |
| csharp-reviewer | C# code review (.NET conventions, async patterns) | sonnet | Read, Grep, Glob, Bash |
| fsharp-reviewer | F# code review (functional idioms, pattern matching) | sonnet | Read, Grep, Glob, Bash |
| java-reviewer | Java code review (Spring Boot/Quarkus frameworks) | sonnet | Read, Grep, Glob, Bash |

---

## code-reviewer

### Name and Purpose
General code review expert that proactively reviews code quality, security, and maintainability. Must be used after all code changes.

### Capabilities
- Code quality checks (function size, nesting depth, code duplication)
- Security vulnerability detection (SQL injection, XSS, hardcoded credentials)
- React/Next.js pattern checking
- Node.js/backend pattern checking
- Performance issue identification
- Best practice suggestions

### Applicable Scenarios
- After code write or modification
- Pull Request review
- Before merging to shared branches

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find files
- Bash: Run git diff and lint commands

### Collaboration with Other Agents
- Escalate to security-reviewer when security issues found
- Use build-error-resolver when build errors found
- Use refactor-cleaner when refactoring needed

---

## python-reviewer

### Name and Purpose
Python code review expert focused on PEP 8 compliance, Pythonic idioms, type hints, security, and performance.

### Capabilities
- PEP 8 style checking
- Pythonic idiom promotion (list comprehensions, enumerate, with statements)
- Type hint validation
- SQL injection detection
- Coroutine and async pattern checking
- Framework-specific checks (Django, FastAPI, Flask)

### Applicable Scenarios
- All code changes in Python projects
- Django/FastAPI/Flask specific projects

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find Python files
- Bash: Run mypy, ruff, black, bandit, pytest

### Collaboration with Other Agents
- Shares security checking rules with security-reviewer
- Uses tdd-guide to ensure test coverage

---

## go-reviewer

### Name and Purpose
Go code review expert focused on idiomatic Go, concurrency patterns, error handling, and performance.

### Capabilities
- Go idiom checking (early returns, error wrapping)
- Concurrency safety checking (goroutine leaks, channel deadlocks)
- Error handling best practices
- Performance pattern recognition
- Security vulnerability detection

### Applicable Scenarios
- All code changes in Go projects
- Projects requiring Go code standards compliance

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find Go files
- Bash: Run go vet, staticcheck, golangci-lint

### Collaboration with Other Agents
- go-build-resolver handles build errors
- security-reviewer handles security-related issues

---

## kotlin-reviewer

### Name and Purpose
Kotlin and Android/KMP code review expert focused on idiomatic patterns, coroutine safety, Compose best practices, and Clean Architecture violations.

### Capabilities
- Kotlin idiom checking
- Coroutine and Flow anti-pattern detection
- Compose performance issue identification
- Clean Architecture module boundary enforcement
- Android-specific issue checking

### Applicable Scenarios
- Android native development
- Kotlin Multiplatform (KMP) projects
- Jetpack Compose projects

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find Kotlin/KTS files
- Bash: Run Gradle checks

### Collaboration with Other Agents
- kotlin-build-resolver handles build issues
- security-reviewer handles security-critical issues

---

## rust-reviewer

### Name and Purpose
Rust code review expert focused on ownership, lifetime safety, error handling, unsafe usage, and idiomatic patterns.

### Capabilities
- Ownership and lifetime checking
- Borrow checker error diagnosis
- Unsafe code safety review
- Error handling pattern validation
- Concurrency safety checking

### Applicable Scenarios
- All code changes in Rust projects
- Projects requiring memory safety guarantees

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find Rust files
- Bash: Run cargo check, clippy, fmt, test, audit

### Collaboration with Other Agents
- rust-build-resolver handles build errors
- Shares security review rules with security-reviewer

---

## cpp-reviewer

### Name and Purpose
C++ code review expert focused on memory safety, modern C++ idioms, concurrency, and performance.

### Capabilities
- Memory safety checking (raw new/delete, buffer overflow)
- Modern C++ pattern promotion (smart pointers, RAII)
- Concurrency safety checking
- Performance pattern recognition
- Security vulnerability detection

### Applicable Scenarios
- All code changes in C++ projects
- Projects requiring modern C++ best practices

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find C++ files
- Bash: Run clang-tidy, cppcheck, cmake

### Collaboration with Other Agents
- cpp-build-resolver handles build issues
- security-reviewer handles security-critical issues

---

## flutter-reviewer

### Name and Purpose
Flutter and Dart code review expert that reviews Flutter code for widget best practices, state management patterns, Dart idioms, performance pitfalls, accessibility, and Clean Architecture violations.

### Capabilities
- State management anti-pattern detection (State.set solution ignoring)
- Widget build performance optimization
- Architecture boundary enforcement
- Resource lifecycle management
- Accessibility checking

### Applicable Scenarios
- All code changes in Flutter projects
- Cross-platform mobile app development

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find Dart files
- Bash: Run flutter analyze

### Collaboration with Other Agents
- dart-build-resolver handles build issues
- security-reviewer handles security-critical issues
- e2e-runner performs end-to-end testing

---

## typescript-reviewer

### Name and Purpose
TypeScript/JavaScript code review expert focused on type safety, async correctness, Node/web security, and idiomatic patterns.

### Capabilities
- Type safety checking (any abuse, non-null assertions)
- Async correctness validation
- Security vulnerability detection
- React/Next.js specific checks
- Node.js specific checks

### Applicable Scenarios
- All code changes in TypeScript/JavaScript projects
- Next.js and React projects

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find TS/TSX/JS/JSX files
- Bash: Run tsc, eslint, prettier, npm audit

### Collaboration with Other Agents
- build-error-resolver handles build errors
- security-reviewer handles security-critical issues

---

## swift-reviewer

### Name and Purpose
Swift code review expert focused on protocol-oriented design, value semantics, ARC memory management, Swift Concurrency, and idiomatic patterns.

### Capabilities
- Swift Concurrency safety checking
- Memory management checking (strong reference cycles, weak references)
- Protocol-oriented design validation
- Error handling best practices
- Performance pattern recognition

### Applicable Scenarios
- All code changes in Swift projects
- iOS/macOS app development

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find Swift files
- Bash: Run swift build, swiftlint, swift test

### Collaboration with Other Agents
- swift-build-resolver handles build issues
- security-reviewer handles security-critical issues

---

## csharp-reviewer

### Name and Purpose
C# code review expert focused on .NET conventions, async patterns, security, nullable reference types, and performance.

### Capabilities
- .NET idiom checking
- Async pattern validation
- Nullable type safety
- Security vulnerability detection
- EF Core specific checks

### Applicable Scenarios
- All code changes in C# projects
- .NET/.NET Core app development

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find C# files
- Bash: Run dotnet build, dotnet format, dotnet test

### Collaboration with Other Agents
- build-error-resolver handles .NET build issues
- security-reviewer handles security-critical issues

---

## java-reviewer

### Name and Purpose
Java code review expert for Spring Boot and Quarkus projects. Automatically detects framework and applies appropriate review rules.

### Capabilities
- Framework auto-detection (Spring Boot/Quarkus)
- Layered architecture checking
- JPA/Panache/MongoDB checking
- Concurrency safety checking
- Security vulnerability detection

### Applicable Scenarios
- Spring Boot projects
- Quarkus projects
- Java 11+ projects

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find Java files
- Bash: Run Maven/Gradle checks

### Collaboration with Other Agents
- java-build-resolver handles build issues
- security-reviewer handles security-critical issues

---

## fsharp-reviewer

### Name and Purpose
F# code review expert focused on functional idioms, type safety, pattern matching, computation expressions, and performance.

### Capabilities
- Functional idiom checking
- Pattern matching completeness validation
- Type safety checking
- Computation expression best practices
- Performance pattern recognition

### Applicable Scenarios
- All code changes in F# projects
- Functional programming projects

### Tools Used
- Read: Read file contents
- Grep: Search code patterns
- Glob: Find F# files
- Bash: Run dotnet build, fantomas, dotnet test

### Collaboration with Other Agents
- Shares security review rules with security-reviewer
- Uses tdd-guide to ensure test coverage

[Return to Agent Index](../README.md)