# 构建修复命令

本文档介绍 ECC 中用于修复各语言构建错误的专门命令。

---

## /go-build

**用途说明**: 逐步修复 Go 构建错误、go vet 警告和 linter 问题。

**使用方法**:
```
/go-build
```

**使用场景**:
- `go build ./...` 失败
- `go vet ./...` 报告问题
- `golangci-lint run` 显示警告
- 模块依赖损坏
- 拉取更改后构建失败

**工作流程**:
1. **运行诊断** - 执行 `go build`、`go vet`、`staticcheck`
2. **解析错误** - 按文件分组并按严重程度排序
3. **逐步修复** - 一次一个错误
4. **验证修复** - 每次更改后重新运行构建
5. **报告总结** - 显示已修复和剩余问题

**常见错误修复**:

| 错误 | 典型修复 |
|------|----------|
| `undefined: X` | 添加导入或修复拼写 |
| `cannot use X as Y` | 类型转换或修复赋值 |
| `missing return` | 添加 return 语句 |
| `X does not implement Y` | 添加缺失方法 |
| `import cycle` | 重构包结构 |
| `declared but not used` | 移除或使用变量 |
| `cannot find package` | `go get` 或 `go mod tidy` |

**诊断命令**:
```bash
go build ./...                # 主构建检查
go vet ./...                  # 静态分析
staticcheck ./...             # 高级 lint（如果可用）
golangci-lint run             # Linting
go mod verify                # 模块验证
go mod tidy -v               # 整理依赖
```

---

## /kotlin-build

**用途说明**: 逐步修复 Kotlin/Gradle 构建错误、编译器警告和依赖问题。

**使用方法**:
```
/kotlin-build
```

**使用场景**:
- `./gradlew build` 失败
- Kotlin 编译器报告错误
- `./gradlew detekt` 报告违规
- Gradle 依赖解析失败
- 拉取更改后构建失败

**常见错误修复**:

| 错误 | 典型修复 |
|------|----------|
| `Unresolved reference: X` | 添加导入或依赖 |
| `Type mismatch` | 修复类型转换或赋值 |
| `'when' must be exhaustive` | 添加缺失的密封类分支 |
| `Suspend function can only be called from coroutine` | 添加 `suspend` 修饰符 |
| `Smart cast impossible` | 使用局部 `val` 或 `let` |
| `Could not resolve dependency` | 修复版本或添加仓库 |

**诊断命令**:
```bash
./gradlew build 2>&1                      # 主构建检查
./gradlew detekt 2>&1                      # 静态分析（如果配置）
./gradlew ktlintCheck 2>&1                # 格式检查（如果配置）
./gradlew dependencies --configuration runtimeClasspath | head -100  # 依赖问题
./gradlew build --refresh-dependencies     # 深度刷新（缓存或依赖元数据可疑时）
```

---

## /rust-build

**用途说明**: 逐步修复 Rust 构建错误、借用检查器问题和依赖问题。

**使用方法**:
```
/rust-build
```

**使用场景**:
- `cargo build` 或 `cargo check` 失败
- `cargo clippy` 报告警告
- 借用检查器或生命周期错误阻止编译
- Cargo 依赖解析失败
- 拉取更改后构建失败

**常见错误修复**:

| 错误 | 典型修复 |
|------|----------|
| `cannot borrow as mutable` | 重构以在使用可变访问之前结束不可变借用 |
| `does not live long enough` | 使用拥有类型或添加生命周期标注 |
| `cannot move out of` | 重构以获取所有权；仅作为最后手段克隆 |
| `mismatched types` | 添加 `.into()`、`as` 或显式转换 |
| `trait X not implemented` | 添加 `#[derive(Trait)]` 或手动实现 |
| `unresolved import` | 添加到 Cargo.toml 或修复 `use` 路径 |
| `cannot find value` | 添加导入或修复路径 |

**诊断命令**:
```bash
cargo check 2>&1                               # 主构建检查
cargo clippy -- -D warnings 2>&1               # Lints
cargo fmt --check 2>&1                         # 格式检查
cargo tree --duplicates                        # 重复依赖
cargo audit                                   # 安全审计（如果可用）
```

---

## /cpp-build

**用途说明**: 逐步修复 C++ 构建错误、CMake 问题和链接器问题。

**使用方法**:
```
/cpp-build
```

**使用场景**:
- `cmake --build build` 失败
- 链接器错误（未定义引用、多次定义）
- 模板实例化失败
- 包含/依赖问题
- 拉取更改后构建失败

**常见错误修复**:

| 错误 | 典型修复 |
|------|----------|
| `undeclared identifier` | 添加 `#include` 或修复拼写 |
| `no matching function` | 修复参数类型或添加重载 |
| `undefined reference` | 链接库或添加实现 |
| `multiple definition` | 使用 `inline` 或移到 .cpp |
| `incomplete type` | 用 `#include` 替换前向声明 |
| `no member named X` | 修复成员名称或添加 include |
| `cannot convert X to Y` | 添加适当转换 |
| `CMake Error` | 修复 CMakeLists.txt 配置 |

**诊断命令**:
```bash
cmake -B build -S .                        # CMake 配置
cmake --build build 2>&1 | head -100       # 构建
clang-tidy src/*.cpp -- -std=c++17         # 静态分析（如果可用）
cppcheck --enable=all src/                 # 额外分析（如果可用）
```

---

## /gradle-build

**用途说明**: 修复 Android 和 Kotlin Multiplatform (KMP) 项目的 Gradle 构建错误。

**使用方法**:
```
/gradle-build
```

**使用场景**:
- Android 项目构建失败
- KMP 项目编译错误
- Gradle 同步失败
- 依赖冲突

**项目类型检测**:

| 指示器 | 构建命令 |
|--------|----------|
| `build.gradle.kts` + `composeApp/` (KMP) | `./gradlew composeApp:compileKotlinMetadata` |
| `build.gradle.kts` + `app/` (Android) | `./gradlew app:compileDebugKotlin` |
| `settings.gradle.kts` 带模块 | `./gradlew assemble` |
| 配置了 detekt | `./gradlew detekt` |

**常见错误修复**:

| 错误 | 修复 |
|------|------|
| `commonMain` 中未解析的引用 | 检查依赖是否在 `commonMain.dependencies {}` 中 |
| Expect 声明无 actual | 在每个平台源集中添加 `actual` 实现 |
| Compose 编译器版本不匹配 | 在 `libs.versions.toml` 中对齐 Kotlin 和 Compose 编译器版本 |
| 重复类 | 用 `./gradlew dependencies` 检查冲突依赖 |
| KSP 错误 | 运行 `./gradlew kspCommonMainKotlinMetadata` 重新生成 |
| 配置缓存问题 | 检查非可序列化的任务输入 |

---

## /flutter-build

**用途说明**: 逐步修复 Dart 分析器错误和 Flutter 构建失败。

**使用方法**:
```
/flutter-build
```

**使用场景**:
- `flutter analyze` 报告错误
- `flutter build` 任何平台失败
- `flutter pub get` 版本冲突
- `build_runner` 生成代码失败
- 拉取更改后构建失败

**常见错误修复**:

| 错误 | 典型修复 |
|------|----------|
| `A value of type 'X?' can't be assigned to 'X'` | 添加 `?? default` 或空值保护 |
| `The name 'X' isn't defined` | 添加导入或修复拼写 |
| `Non-nullable instance field must be initialized` | 添加初始化器或 `late` |
| `Version solving failed` | 调整 pubspec.yaml 中的版本约束 |
| `Missing concrete implementation of 'X'` | 实现缺失的接口方法 |
| `build_runner: Part of X expected` | 删除过时的 `.g.dart` 并重新构建 |

**诊断命令**:
```bash
flutter analyze 2>&1                  # 分析
flutter pub get 2>&1                  # 依赖
dart run build_runner build --delete-conflicting-outputs 2>&1  # 代码生成（如果使用 build_runner）
flutter build apk 2>&1                # 平台构建
flutter build web 2>&1                # Web 构建
```

---

## 构建修复命令对比表

| 命令 | 语言/平台 | 主要工具 | 常见问题 |
|------|----------|----------|----------|
| `/go-build` | Go | go build, go vet | 类型错误、导入循环 |
| `/kotlin-build` | Kotlin | Gradle, detekt | 类型不匹配、when 非穷举 |
| `/rust-build` | Rust | cargo check, clippy | 借用错误、生命周期 |
| `/cpp-build` | C++ | CMake, clang-tidy | 链接器错误、模板问题 |
| `/gradle-build` | Android/KMP | Gradle | 依赖冲突、配置错误 |
| `/flutter-build` | Flutter/Dart | Flutter analyze | 空安全、分析错误 |

---

## 通用修复策略

所有构建修复命令遵循相同的基本策略：

1. **先修复构建错误** - 代码必须编译
2. **其次修复 lint 警告** - 修复可疑构造
3. **然后修复格式警告** - 样式和Melhores-Práticas
4. **一次修复一个** - 验证每次更改
5. **最小更改** - 不要重构，只是修复

**停止条件**:
如果发生以下情况，代理将停止并报告：
- 相同错误在 3 次尝试后仍然存在
- 修复引入了更多错误
- 需要架构更改
- 缺少外部依赖
