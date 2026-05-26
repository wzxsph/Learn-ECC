# Build Fix Commands

This document introduces specialized commands for fixing build errors across various languages in ECC.

---

## /go-build

**Purpose**: Incrementally fix Go build errors, go vet warnings, and linter issues.

**Usage**:
```
/go-build
```

**Use Cases**:
- `go build ./...` fails
- `go vet ./...` reports issues
- `golangci-lint run` shows warnings
- Module dependencies broken
- Build fails after pulling changes

**Workflow**:
1. **Run diagnostics** - Execute `go build`, `go vet`, `staticcheck`
2. **Parse errors** - Group by file and sort by severity
3. **Fix incrementally** - One error at a time
4. **Verify fix** - Re-run build after each change
5. **Report summary** - Show fixed and remaining issues

**Common Error Fixes**:

| Error | Typical Fix |
|-------|-----------|
| `undefined: X` | Add import or fix spelling |
| `cannot use X as Y` | Type conversion or fix assignment |
| `missing return` | Add return statement |
| `X does not implement Y` | Add missing method |
| `import cycle` | Refactor package structure |
| `declared but not used` | Remove or use variable |
| `cannot find package` | `go get` or `go mod tidy` |

**Diagnostic Commands**:
```bash
go build ./...                # Main build check
go vet ./...                  # Static analysis
staticcheck ./...             # Advanced lint (if available)
golangci-lint run             # Linting
go mod verify                # Module verification
go mod tidy -v               # Tidy dependencies
```

---

## /kotlin-build

**Purpose**: Incrementally fix Kotlin/Gradle build errors, compiler warnings, and dependency issues.

**Usage**:
```
/kotlin-build
```

**Use Cases**:
- `./gradlew build` fails
- Kotlin compiler reports errors
- `./gradlew detekt` reports violations
- Gradle dependency resolution fails
- Build fails after pulling changes

**Common Error Fixes**:

| Error | Typical Fix |
|-------|-----------|
| `Unresolved reference: X` | Add import or dependency |
| `Type mismatch` | Fix type conversion or assignment |
| `'when' must be exhaustive` | Add missing sealed class branch |
| `Suspend function can only be called from coroutine` | Add `suspend` modifier |
| `Smart cast impossible` | Use local `val` or `let` |
| `Could not resolve dependency` | Fix version or add repository |

**Diagnostic Commands**:
```bash
./gradlew build 2>&1                      # Main build check
./gradlew detekt 2>&1                      # Static analysis (if configured)
./gradlew ktlintCheck 2>&1                # Format check (if configured)
./gradlew dependencies --configuration runtimeClasspath | head -100  # Dependency issues
./gradlew build --refresh-dependencies     # Deep refresh (when cache or dependency metadata is suspect)
```

---

## /rust-build

**Purpose**: Incrementally fix Rust build errors, borrow checker issues, and dependency problems.

**Usage**:
```
/rust-build
```

**Use Cases**:
- `cargo build` or `cargo check` fails
- `cargo clippy` reports warnings
- Borrow checker or lifetime errors block compilation
- Cargo dependency resolution fails
- Build fails after pulling changes

**Common Error Fixes**:

| Error | Typical Fix |
|-------|-----------|
| `cannot borrow as mutable` | Refactor so immutable borrow ends before mutable use |
| `does not live long enough` | Use owned type or add lifetime annotations |
| `cannot move out of` | Refactor to take ownership; clone as last resort |
| `mismatched types` | Add `.into()`, `as`, or explicit conversion |
| `trait X not implemented` | Add `#[derive(Trait)]` or implement manually |
| `unresolved import` | Add to Cargo.toml or fix `use` path |
| `cannot find value` | Add import or fix path |

**Diagnostic Commands**:
```bash
cargo check 2>&1                               # Main build check
cargo clippy -- -D warnings 2>&1               # Lints
cargo fmt --check 2>&1                         # Format check
cargo tree --duplicates                        # Duplicate dependencies
cargo audit                                   # Security audit (if available)
```

---

## /cpp-build

**Purpose**: Incrementally fix C++ build errors, CMake issues, and linker problems.

**Usage**:
```
/cpp-build
```

**Use Cases**:
- `cmake --build build` fails
- Linker errors (undefined reference, multiple definitions)
- Template instantiation failures
- Include/dependency issues
- Build fails after pulling changes

**Common Error Fixes**:

| Error | Typical Fix |
|-------|-----------|
| `undeclared identifier` | Add `#include` or fix spelling |
| `no matching function` | Fix parameter types or add overload |
| `undefined reference` | Link library or add implementation |
| `multiple definition` | Use `inline` or move to .cpp |
| `incomplete type` | Replace forward declaration with `#include` |
| `no member named X` | Fix member name or add include |
| `cannot convert X to Y` | Add appropriate conversion |
| `CMake Error` | Fix CMakeLists.txt configuration |

**Diagnostic Commands**:
```bash
cmake -B build -S .                        # CMake configuration
cmake --build build 2>&1 | head -100       # Build
clang-tidy src/*.cpp -- -std=c++17         # Static analysis (if available)
cppcheck --enable=all src/                 # Additional analysis (if available)
```

---

## /gradle-build

**Purpose**: Fix Gradle build errors for Android and Kotlin Multiplatform (KMP) projects.

**Usage**:
```
/gradle-build
```

**Use Cases**:
- Android project build fails
- KMP project compilation errors
- Gradle sync fails
- Dependency conflicts

**Project Type Detection**:

| Indicator | Build Command |
|-----------|---------------|
| `build.gradle.kts` + `composeApp/` (KMP) | `./gradlew composeApp:compileKotlinMetadata` |
| `build.gradle.kts` + `app/` (Android) | `./gradlew app:compileDebugKotlin` |
| `settings.gradle.kts` with modules | `./gradlew assemble` |
| detekt configured | `./gradlew detekt` |

**Common Error Fixes**:

| Error | Fix |
|-------|-----|
| Unresolved reference in `commonMain` | Check if dependency is in `commonMain.dependencies {}` |
| Expect declaration without actual | Add `actual` implementation in each platform source set |
| Compose compiler version mismatch | Align Kotlin and Compose compiler versions in `libs.versions.toml` |
| Duplicate classes | Check conflicting dependencies with `./gradlew dependencies` |
| KSP errors | Run `./gradlew kspCommonMainKotlinMetadata` to regenerate |
| Configuration cache issues | Check for non-serializable task inputs |

---

## /flutter-build

**Purpose**: Incrementally fix Dart analyzer errors and Flutter build failures.

**Usage**:
```
/flutter-build
```

**Use Cases**:
- `flutter analyze` reports errors
- `flutter build` fails for any platform
- `flutter pub get` version conflicts
- `build_runner` code generation fails
- Build fails after pulling changes

**Common Error Fixes**:

| Error | Typical Fix |
|-------|-----------|
| `A value of type 'X?' can't be assigned to 'X'` | Add `?? default` or null guard |
| `The name 'X' isn't defined` | Add import or fix spelling |
| `Non-nullable instance field must be initialized` | Add initializer or `late` |
| `Version solving failed` | Adjust version constraints in pubspec.yaml |
| `Missing concrete implementation of 'X'` | Implement missing interface methods |
| `build_runner: Part of X expected` | Delete outdated `.g.dart` and rebuild |

**Diagnostic Commands**:
```bash
flutter analyze 2>&1                  # Analysis
flutter pub get 2>&1                  # Dependencies
dart run build_runner build --delete-conflicting-outputs 2>&1  # Code generation (if using build_runner)
flutter build apk 2>&1                # Platform build
flutter build web 2>&1                # Web build
```

---

## Build Fix Commands Comparison

| Command | Language/Platform | Main Tools | Common Issues |
|---------|------------------|------------|---------------|
| `/go-build` | Go | go build, go vet | Type errors, import cycles |
| `/kotlin-build` | Kotlin | Gradle, detekt | Type mismatch, when non-exhaustive |
| `/rust-build` | Rust | cargo check, clippy | Borrow errors, lifetimes |
| `/cpp-build` | C++ | CMake, clang-tidy | Linker errors, template issues |
| `/gradle-build` | Android/KMP | Gradle | Dependency conflicts, configuration errors |
| `/flutter-build` | Flutter/Dart | Flutter analyze | Null safety, analysis errors |

---

## Universal Fix Strategy

All build fix commands follow the same basic strategy:

1. **Fix build errors first** - Code must compile
2. **Fix lint warnings second** - Fix suspicious constructs
3. **Then fix format warnings** - Style and best practices
4. **Fix one at a time** - Verify each change
5. **Minimal changes** - Don't refactor, just fix

**Stop Conditions**:
The agent will stop and report if:
- Same error persists after 3 attempts
- Fix introduces more errors
- Architecture changes are needed
- External dependencies are missing