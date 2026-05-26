# Build & Fix Agents

Build & Fix agents specialize in diagnosing and fixing build errors, compilation errors, and dependency issues across various programming languages.

## Agent List

| Agent Name | Purpose | Model Used | Core Tools |
|------------|---------|------------|------------|
| build-error-resolver | TypeScript/general build error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| go-build-resolver | Go build/compilation error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| kotlin-build-resolver | Kotlin/Gradle build error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| rust-build-resolver | Rust build/borrow checker error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| cpp-build-resolver | C++/CMake build error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| java-build-resolver | Java/Maven/Gradle build error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| swift-build-resolver | Swift/Xcode build error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| dart-build-resolver | Dart/Flutter build error fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| django-build-resolver | Django/Python build issue fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| pytorch-build-resolver | PyTorch/CUDA build issue fixing | sonnet | Read, Write, Edit, Bash, Grep, Glob |

---

## build-error-resolver

### Name and Purpose
TypeScript build and type error resolution expert. Fixes build failures or type errors, focusing on getting builds green, without architecture changes.

### Capabilities
- TypeScript error resolution
- Build error fixing
- Dependency issue fixing
- Configuration error resolution
- Minimal diff fixes

### Applicable Scenarios
- When build fails
- When TypeScript type errors occur
- When module resolution issues occur

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run tsc, npm build, eslint
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- No refactoring → Use refactor-cleaner
- No architecture changes → Use architect
- No new features → Use planner
- No test fixes → Use tdd-guide
- No security issues → Use security-reviewer

### Core Principles
- Only fix errors, don't refactor
- Minimal changes
- Verify build passes after each fix

---

## go-build-resolver

### Name and Purpose
Go build, vet, and compilation error resolution expert. Fixes Go build errors, go vet issues, and linter warnings.

### Capabilities
- Go compilation error diagnosis
- go vet warning fixing
- staticcheck/golangci-lint issue fixing
- Module dependency issue resolution
- Type errors and interface mismatch handling

### Applicable Scenarios
- When Go build fails
- When go vet reports errors
- When module dependency conflicts occur

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run go build, go vet, staticcheck, golangci-lint
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- go-reviewer reviews code quality
- security-reviewer handles security-related issues

### Common Error Fix Patterns

| Error Type | Cause | Fix Method |
|------------|-------|------------|
| undefined: X | Missing import, typo | Add import or fix casing |
| cannot use X as type Y | Type mismatch | Type cast or dereference |
| X does not implement Y | Missing method | Implement method with correct receiver |
| import cycle not allowed | Circular dependency | Extract shared types to new package |
| cannot find package | Missing dependency | go get pkg@version or go mod tidy |

---

## kotlin-build-resolver

### Name and Purpose
Kotlin/Gradle build, compilation, and dependency error resolution expert. Fixes Kotlin build errors, Kotlin compiler errors, and Gradle issues.

### Capabilities
- Kotlin compilation error diagnosis
- Gradle build configuration issue fixing
- Dependency conflicts and version mismatch resolution
- Kotlin compiler error handling
- detekt and ktlint violation fixing

### Applicable Scenarios
- When Kotlin build fails
- When Gradle dependency conflicts occur
- When Kotlin compiler reports errors

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run ./gradlew build, detekt, ktlintCheck
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- kotlin-reviewer reviews code quality
- security-reviewer handles security-related issues

### Common Error Fix Patterns

| Error Type | Cause | Fix Method |
|------------|-------|------------|
| Unresolved reference: X | Missing import, typo | Add import or dependency |
| Type mismatch | Type error | Add conversion or fix type |
| Smart cast impossible | Mutable property or concurrent access | Use local val copy |
| when expression must be exhaustive | Sealed class when missing branch | Add missing branch or else |
| Suspend function can only be called from coroutine | Missing suspend or coroutine scope | Add suspend modifier |

---

## rust-build-resolver

### Name and Purpose
Rust build, compilation, and dependency error resolution expert. Fixes cargo build errors, borrow checker issues, and Cargo.toml problems.

### Capabilities
- cargo build/check error diagnosis
- Borrow checker and lifetime error fixing
- Trait implementation mismatch resolution
- Cargo dependency and feature issue handling
- cargo clippy warning fixing

### Applicable Scenarios
- When Rust build fails
- When borrow checker reports errors
- When Cargo dependency conflicts occur

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run cargo check, clippy, fmt, tree
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- rust-reviewer reviews code quality
- security-reviewer handles security-related issues

### Common Error Fix Patterns

| Error Type | Cause | Fix Method |
|------------|-------|------------|
| cannot borrow as mutable | Immutable borrow is active | Refactor to end immutable borrow first |
| does not live long enough | Value dropped while still borrowed | Extend lifetime scope |
| cannot move out of | Moving after reference | Use .clone() or refactor |
| mismatched types | Type error | Add .into(), as, or explicit conversion |
| trait X is not implemented for Y | Missing impl or derive | Add #[derive(Trait)] |

---

## cpp-build-resolver

### Name and Purpose
C++ build, CMake, and compilation error resolution expert. Fixes build errors, linker issues, and template errors.

### Capabilities
- C++ compilation error diagnosis
- CMake configuration issue fixing
- Linker error resolution (undefined references, duplicate definitions)
- Template instantiation error handling
- Include and dependency issue fixing

### Applicable Scenarios
- When C++ build fails
- When CMake configuration errors occur
- When linker reports errors

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run cmake --build, clang-tidy, cppcheck
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- cpp-reviewer reviews code quality
- security-reviewer handles security-related issues

### Common Error Fix Patterns

| Error Type | Cause | Fix Method |
|------------|-------|------------|
| undefined reference to X | Missing implementation or library | Add source file or link library |
| no matching function for call | Wrong argument types | Fix types or add overload |
| multiple definition of | Duplicate symbol | Use inline or move to .cpp |
| cannot convert X to Y | Type mismatch | Add conversion or fix type |
| template argument deduction failed | Wrong template arguments | Fix template arguments |

---

## java-build-resolver

### Name and Purpose
Java/Maven/Gradle build, compilation, and dependency error resolution expert. Automatically detects Spring Boot or Quarkus and applies framework-specific fixes.

### Capabilities
- Java compilation error diagnosis
- Maven and Gradle build configuration issue fixing
- Dependency conflicts and version mismatch resolution
- Annotation processor error handling (Lombok, MapStruct, Spring, Quarkus)
- Checkstyle and SpotBugs violation fixing

### Applicable Scenarios
- When Java build fails
- When Maven/Gradle dependency conflicts occur
- When framework-specific build issues occur

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run ./mvnw compile, ./gradlew build, dependency:tree
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- java-reviewer reviews code quality
- security-reviewer handles security-related issues

### Framework Detection
Automatically detects project framework:
- If contains `quarkus` → Apply [QUARKUS] rules
- If contains `spring-boot` → Apply [SPRING] rules
- Both present → Apply both rule sets

### Common Error Fix Patterns

| Error Type | Cause | Fix Method |
|------------|-------|------------|
| cannot find symbol | Missing import, typo | Add import or dependency |
| incompatible types | Type mismatch | Add explicit conversion |
| package X does not exist | Missing dependency | Add to pom.xml/build.gradle |
| No qualifying bean of type X | Missing @Component or component scanning | Add annotation or fix scanning package |

---

## swift-build-resolver

### Name and Purpose
Swift/Xcode build, compilation, and dependency error resolution expert. Fixes swift build errors, Xcode build failures, SPM dependency issues, and code signing problems.

### Capabilities
- swift build/xcodebuild error diagnosis
- Type checker and protocol conformance error fixing
- Swift Concurrency and Sendable issue resolution
- SPM dependency and version resolution failure handling
- Xcode project configuration and code signing issue fixing

### Applicable Scenarios
- When Swift build fails
- When Xcode build fails
- When SPM dependency conflicts occur

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run swift build, xcodebuild, swiftlint
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- swift-reviewer reviews code quality
- security-reviewer handles security-related issues

### Common Error Fix Patterns

| Error Type | Cause | Fix Method |
|------------|-------|------------|
| cannot find type 'X' in scope | Missing import or typo | Add import or fix name |
| type 'X' does not conform to protocol 'Y' | Missing required members | Implement missing protocol requirements |
| non-sendable type passed | Sendable violation | Add Sendable conformance or refactor |
| @MainActor function cannot be called | Main actor isolation | Add await or use MainActor.run |

---

## dart-build-resolver

### Name and Purpose
Dart/Flutter build, analysis, and dependency error resolution expert. Fixes dart analyze errors, Flutter compilation failures, pub dependency conflicts, and build_runner issues.

### Capabilities
- dart analyze/flutter analyze error diagnosis
- Dart type errors, null safety violations, and missing imports fixing
- pubspec.yaml dependency conflicts and version constraint resolution
- build_runner code generation failure fixing
- Flutter-specific build error handling (Android Gradle, iOS CocoaPods, web)

### Applicable Scenarios
- When Dart/Flutter build fails
- When pub dependency conflicts occur
- When build_runner generation fails

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run flutter analyze, dart analyze, flutter pub get, build_runner
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- flutter-reviewer reviews code quality
- security-reviewer handles security-related issues

### Common Error Fix Patterns

| Error Type | Cause | Fix Method |
|------------|-------|------------|
| The name 'X' isn't defined | Missing import or typo | Add correct import |
| A value of type 'X?' can't be assigned to type 'X' | Null safety - nullable type not handled | Add !, ?? default, or null check |
| Because X depends on Y >=A and Z depends on Y <B | Pub version conflict | Adjust version constraints or add dependency_overrides |
| build_runner: No actions were run | Code generation input unchanged | Use --delete-conflicting-outputs to force rebuild |

---

## django-build-resolver

### Name and Purpose
Django/Python build and dependency issue fixing expert. Handles Django-specific build issues and Python dependency conflicts.

### Capabilities
- Django project configuration issue fixing
- Python dependency conflict resolution
- Django migration issue handling
- requirements.txt/pipfile dependency management

### Applicable Scenarios
- When Django project build fails
- When Python dependency conflicts occur
- When Django migration issues occur

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run pip, pipenv, poetry, django-admin
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- python-reviewer reviews code quality
- security-reviewer handles security-related issues

---

## pytorch-build-resolver

### Name and Purpose
PyTorch/CUDA build issue fixing expert. Handles PyTorch-specific build issues, CUDA compatibility issues, and tensor operation errors.

### Capabilities
- PyTorch build error diagnosis
- CUDA compatibility issue fixing
- Tensor shape and device placement error handling
- DataLoader and AMP failure fixing
- PyTorch environment configuration issues

### Applicable Scenarios
- When PyTorch build fails
- When CUDA compatibility issues occur
- When training/inference fails

### Tools Used
- Read: Read file contents
- Write: Write fixes
- Edit: Edit files
- Bash: Run python, pip, nvcc, nvidia-smi
- Grep: Search error patterns
- Glob: Find relevant files

### Collaboration with Other Agents
- mle-reviewer reviews ML code
- security-reviewer handles security-related issues
[Return to Agent Index](../README.md)