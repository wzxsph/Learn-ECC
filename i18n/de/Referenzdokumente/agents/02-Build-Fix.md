# 构建修复类 Agent

构建修复类 Agent 专门负责诊断和修复各种编程语言的构建错误、编译错误和依赖问题。

## Agent 列表

| Agent 名称 | 用途 | 使用模型 | 核心工具 |
|------------|------|----------|----------|
| build-error-resolver | TypeScript/通用构建错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| go-build-resolver | Go 构建/编译错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| kotlin-build-resolver | Kotlin/Gradle 构建错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| rust-build-resolver | Rust 构建/借用检查器错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| cpp-build-resolver | C++/CMake 构建错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| java-build-resolver | Java/Maven/Gradle 构建错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| swift-build-resolver | Swift/Xcode 构建错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| dart-build-resolver | Dart/Flutter 构建错误修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| django-build-resolver | Django/Python 构建问题修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| pytorch-build-resolver | PyTorch/CUDA 构建问题修复 | sonnet | Read, Write, Edit, Bash, Grep, Glob |

---

## build-error-resolver

### 名称和用途
TypeScript 构建和类型错误解决专家。修复构建失败或类型错误，专注于让构建变绿，不做架构修改。

### 能力说明
- TypeScript 错误解决
- 构建错误修复
- 依赖问题修复
- 配置错误解决
- 最小化 diff 修复

### 适用场景
- 构建失败时
- TypeScript 类型错误发生时
- 模块解析问题时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 tsc, npm build, eslint
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- 不做重构 → 使用 refactor-cleaner
- 不做架构变更 → 使用 architect
- 不添加新功能 → 使用 planner
- 不修复测试 → 使用 tdd-guide
- 不处理安全问题 → 使用 security-reviewer

### 核心原则
- 只修复错误，不要重构
- 最小化变更
- 验证每次修复后构建通过

---

## go-build-resolver

### 名称和用途
Go 构建、vet 和编译错误解决专家。修复 Go 构建错误、go vet 问题和 linter 警告。

### 能力说明
- Go 编译错误诊断
- go vet 警告修复
- staticcheck/golangci-lint 问题修复
- 模块依赖问题解决
- 类型错误和接口不匹配处理

### 适用场景
- Go 构建失败时
- go vet 报错时
- 模块依赖冲突时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 go build, go vet, staticcheck, golangci-lint
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- go-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

### 常见错误修复模式

| 错误类型 | 原因 | 修复方法 |
|----------|------|----------|
| undefined: X | 缺少导入、拼写错误 | 添加导入或修复大小写 |
| cannot use X as type Y | 类型不匹配 | 类型转换或解引用 |
| X does not implement Y | 缺少方法 | 使用正确 receiver 实现方法 |
| import cycle not allowed | 循环依赖 | 提取共享类型到新包 |
| cannot find package | 缺少依赖 | go get pkg@version 或 go mod tidy |

---

## kotlin-build-resolver

### 名称和用途
Kotlin/Gradle 构建、编译和依赖错误解决专家。修复 Kotlin 构建错误、Kotlin 编译器错误和 Gradle 问题。

### 能力说明
- Kotlin 编译错误诊断
- Gradle 构建配置问题修复
- 依赖冲突和版本不匹配解决
- Kotlin 编译器错误处理
- detekt 和 ktlint 违规修复

### 适用场景
- Kotlin 构建失败时
- Gradle 依赖冲突时
- Kotlin 编译器报错时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 ./gradlew build, detekt, ktlintCheck
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- kotlin-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

### 常见错误修复模式

| 错误类型 | 原因 | 修复方法 |
|----------|------|----------|
| Unresolved reference: X | 缺少导入、拼写错误 | 添加导入或依赖 |
| Type mismatch | 类型错误 | 添加转换或修复类型 |
| Smart cast impossible | 可变属性或并发访问 | 使用局部 val 副本 |
| when expression must be exhaustive | sealed class when 缺少分支 | 添加缺失分支或 else |
| Suspend function can only be called from coroutine | 缺少 suspend 或协程作用域 | 添加 suspend 修饰符 |

---

## rust-build-resolver

### 名称和用途
Rust 构建、编译和依赖错误解决专家。修复 cargo 构建错误、借用检查器问题和 Cargo.toml 问题。

### 能力说明
- cargo build/check 错误诊断
- 借用检查器和生命周期错误修复
- 特征实现不匹配解决
- Cargo 依赖和特征问题处理
- cargo clippy 警告修复

### 适用场景
- Rust 构建失败时
- 借用检查器报错时
- Cargo 依赖冲突时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 cargo check, clippy, fmt, tree
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- rust-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

### 常见错误修复模式

| 错误类型 | 原因 | 修复方法 |
|----------|------|----------|
| cannot borrow as mutable | 不可变借用激活 | 重构以先结束不可变借用 |
| does not live long enough | 值在仍被借用时丢弃 | 扩展生命周期作用域 |
| cannot move out of | 从引用后移动 | 使用 .clone() 或重构 |
| mismatched types | 类型错误 | 添加 .into()、as 或显式转换 |
| trait X is not implemented for Y | 缺少 impl 或 derive | 添加 #[derive(Trait)] |

---

## cpp-build-resolver

### 名称和用途
C++ 构建、CMake 和编译错误解决专家。修复构建错误、链接器问题和模板错误。

### 能力说明
- C++ 编译错误诊断
- CMake 配置问题修复
- 链接器错误解决（未定义引用、重复定义）
- 模板实例化错误处理
- 包含和依赖问题修复

### 适用场景
- C++ 构建失败时
- CMake 配置错误时
- 链接器报错时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 cmake --build, clang-tidy, cppcheck
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- cpp-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

### 常见错误修复模式

| 错误类型 | 原因 | 修复方法 |
|----------|------|----------|
| undefined reference to X | 缺少实现或库 | 添加源文件或链接库 |
| no matching function for call | 参数类型错误 | 修复类型或添加重载 |
| multiple definition of | 重复符号 | 使用 inline 或移到 .cpp |
| cannot convert X to Y | 类型不匹配 | 添加转换或修复类型 |
| template argument deduction failed | 模板参数错误 | 修复模板参数 |

---

## java-build-resolver

### 名称和用途
Java/Maven/Gradle 构建、编译和依赖错误解决专家。自动检测 Spring Boot 或 Quarkus 并应用框架特定修复。

### 能力说明
- Java 编译错误诊断
- Maven 和 Gradle 构建配置问题修复
- 依赖冲突和版本不匹配解决
- 注解处理器错误处理（Lombok、MapStruct、Spring、Quarkus）
- Checkstyle 和 SpotBugs 违规修复

### 适用场景
- Java 构建失败时
- Maven/Gradle 依赖冲突时
- 框架特定构建问题时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 ./mvnw compile, ./gradlew build, dependency:tree
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- java-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

### 框架检测
自动检测项目框架：
- 如果包含 `quarkus` → 应用 [QUARKUS] 规则
- 如果包含 `spring-boot` → 应用 [SPRING] 规则
- 两者都有 → 应用两个规则集

### 常见错误修复模式

| 错误类型 | 原因 | 修复方法 |
|----------|------|----------|
| cannot find symbol | 缺少导入、拼写错误 | 添加导入或依赖 |
| incompatible types | 类型不匹配 | 添加显式转换 |
| package X does not exist | 缺少依赖 | 添加到 pom.xml/build.gradle |
| No qualifying bean of type X | 缺少 @Component 或组件扫描 | 添加注解或修复扫描包 |

---

## swift-build-resolver

### 名称和用途
Swift/Xcode 构建、编译和依赖错误解决专家。修复 swift 构建错误、Xcode 构建失败、SPM 依赖问题和代码签名问题。

### 能力说明
- swift build/xcodebuild 错误诊断
- 类型检查器和协议一致性错误修复
- Swift Concurrency 和 Sendable 问题解决
- SPM 依赖和版本解析失败处理
- Xcode 项目配置和代码签名问题修复

### 适用场景
- Swift 构建失败时
- Xcode 构建失败时
- SPM 依赖冲突时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 swift build, xcodebuild, swiftlint
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- swift-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

### 常见错误修复模式

| 错误类型 | 原因 | 修复方法 |
|----------|------|----------|
| cannot find type 'X' in scope | 缺少导入或拼写错误 | 添加 import 或修复名称 |
| type 'X' does not conform to protocol 'Y' | 缺少必需成员 | 实现缺失的协议要求 |
| non-sendable type passed | Sendable 违规 | 添加 Sendable 一致性或重构 |
| @MainActor function cannot be called | Main actor 隔离 | 添加 await 或使用 MainActor.run |

---

## dart-build-resolver

### 名称和用途
Dart/Flutter 构建、分析和依赖错误解决专家。修复 dart analyze 错误、Flutter 编译失败、pub 依赖冲突和 build_runner 问题。

### 能力说明
- dart analyze/flutter analyze 错误诊断
- Dart 类型错误、空安全违规和缺少导入修复
- pubspec.yaml 依赖冲突和版本约束解决
- build_runner 代码生成失败修复
- Flutter 特定构建错误处理（Android Gradle、iOS CocoaPods、web）

### 适用场景
- Dart/Flutter 构建失败时
- pub 依赖冲突时
- build_runner 生成失败时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 flutter analyze, dart analyze, flutter pub get, build_runner
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- flutter-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

### 常见错误修复模式

| 错误类型 | 原因 | 修复方法 |
|----------|------|----------|
| The name 'X' isn't defined | 缺少导入或拼写错误 | 添加正确的 import |
| A value of type 'X?' can't be assigned to type 'X' | 空安全 - 可空类型未处理 | 添加 !、?? default 或 null 检查 |
| Because X depends on Y >=A and Z depends on Y <B | Pub 版本冲突 | 调整版本约束或添加 dependency_overrides |
| build_runner: No actions were run | 代码生成输入没有变化 | 使用 --delete-conflicting-outputs 强制重建 |

---

## django-build-resolver

### 名称和用途
Django/Python 构建和依赖问题修复专家。处理 Django 特定构建问题和 Python 依赖冲突。

### 能力说明
- Django 项目配置问题修复
- Python 依赖冲突解决
- Django 迁移问题处理
-requirements.txt/pipfile 依赖管理

### 适用场景
- Django 项目构建失败时
- Python 依赖冲突时
- Django 迁移问题时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 pip, pipenv, poetry, django-admin
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- python-reviewer 审查代码质量
- security-reviewer 处理安全相关问题

---

## pytorch-build-resolver

### 名称和用途
PyTorch/CUDA 构建问题修复专家。处理 PyTorch 特定构建问题、CUDA 兼容性问题和张量操作错误。

### 能力说明
- PyTorch 构建错误诊断
- CUDA 兼容性问题修复
- 张量形状和设备放置错误处理
- DataLoader 和 AMP 失败修复
- PyTorch 环境配置问题

### 适用场景
- PyTorch 构建失败时
- CUDA 兼容性问题时
- 训练/推理失败时

### 使用的工具列表
- Read: 读取文件内容
- Write: 写入修复
- Edit: 编辑文件
- Bash: 运行 python, pip, nvcc, nvidia-smi
- Grep: 搜索错误模式
- Glob: 查找相关文件

### 与其他 Agent 的协作方式
- mle-reviewer 审查 ML 代码
- security-reviewer 处理安全相关问题
[返回 Agent 索引](../README.md)
