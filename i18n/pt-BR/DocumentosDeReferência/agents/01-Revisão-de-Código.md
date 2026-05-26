# 代码审查类 Agent

代码审查类 Agent 专门负责代码质量、安全性和可维护性的审查工作。

## Agent 列表

| Agent 名称 | 用途 | 使用模型 | 核心工具 |
|------------|------|----------|----------|
| code-reviewer | 通用代码审查专家 | sonnet | Read, Grep, Glob, Bash |
| python-reviewer | Python 代码审查（PEP 8、类型提示、安全） | sonnet | Read, Grep, Glob, Bash |
| go-reviewer | Go 代码审查（并发、错误处理、性能） | sonnet | Read, Grep, Glob, Bash |
| kotlin-reviewer | Kotlin/Android 代码审查（协程、Compose） | sonnet | Read, Grep, Glob, Bash |
| rust-reviewer | Rust 代码审查（所有权、生命周期的安全） | sonnet | Read, Grep, Glob, Bash |
| cpp-reviewer | C++ 代码审查（内存安全、现代 C++ 惯用法） | sonnet | Read, Grep, Glob, Bash |
| flutter-reviewer | Flutter/Dart 代码审查（状态管理、性能） | sonnet | Read, Grep, Glob, Bash |
| typescript-reviewer | TypeScript/JavaScript 代码审查（类型安全） | sonnet | Read, Grep, Glob, Bash |
| swift-reviewer | Swift 代码审查（协议、并发、ARC 内存管理） | sonnet | Read, Grep, Glob, Bash |
| csharp-reviewer | C# 代码审查（.NET 约定、异步模式） | sonnet | Read, Grep, Glob, Bash |
| fsharp-reviewer | F# 代码审查（函数式惯用语、模式匹配） | sonnet | Read, Grep, Glob, Bash |
| java-reviewer | Java 代码审查（Spring Boot/Quarkus 框架） | sonnet | Read, Grep, Glob, Bash |

---

## code-reviewer

### 名称和用途
通用代码审查专家，主动审查代码的质量、安全性和可维护性。所有代码变更后必须使用。

### 能力说明
- 代码质量检查（函数大小、嵌套深度、代码重复）
- 安全漏洞检测（SQL 注入、XSS、硬编码凭证）
- React/Next.js 模式检查
- Node.js/后端模式检查
- 性能问题识别
- Melhores-Práticas建议

### 适用场景
- 代码写入或修改后
- Pull Request 审查
- 合并到共享分支前

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找文件
- Bash: 运行 git diff 和 lint 命令

### 与其他 Agent 的协作方式
- 发现安全问题时 Escalation 到 security-reviewer
- 发现构建错误时使用 build-error-resolver
- 发现需要重构时使用 refactor-cleaner

---

## python-reviewer

### 名称和用途
Python 代码审查专家，专注于 PEP 8 合规、Pythonic 惯用语、类型提示、安全和性能。

### 能力说明
- PEP 8 风格检查
- Pythonic 惯用语推广（列表推导、枚举、with 语句）
- 类型提示验证
- SQL 注入检测
- 协程与异步模式检查
- 框架特定检查（Django、FastAPI、Flask）

### 适用场景
- Python 项目中的所有代码变更
- Django/FastAPI/Flask 特定项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 Python 文件
- Bash: 运行 mypy, ruff, black, bandit, pytest

### 与其他 Agent 的协作方式
- 与 security-reviewer 共享安全检查规则
- 使用 tdd-guide 确保测试覆盖率

---

## go-reviewer

### 名称和用途
Go 代码审查专家，专注于 idiomatic Go、并发模式、错误处理和性能。

### 能力说明
- Go 惯用语检查（早期返回、错误包装）
- 并发安全检查（goroutine 泄漏、通道死锁）
- 错误处理Melhores-Práticas
- 性能模式识别
- 安全漏洞检测

### 适用场景
- Go 项目中的所有代码变更
- 需要遵守 Go 代码规范的项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 Go 文件
- Bash: 运行 go vet, staticcheck, golangci-lint

### 与其他 Agent 的协作方式
- go-build-resolver 处理构建错误
- security-reviewer 处理安全相关问题

---

## kotlin-reviewer

### 名称和用途
Kotlin 和 Android/KMP 代码审查专家，专注于惯用语模式、协程安全、Compose Melhores-Práticas和 Clean Architecture 违规。

### 能力说明
- Kotlin 惯用语检查
- 协程和 Flow 反模式检测
- Compose 性能问题识别
- Clean Architecture 模块边界强制
- Android 特定问题检查

### 适用场景
- Android 原生开发
- Kotlin Multiplatform (KMP) 项目
- Jetpack Compose 项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 Kotlin/KTS 文件
- Bash: 运行 Gradle 检查

### 与其他 Agent 的协作方式
- kotlin-build-resolver 处理构建问题
- security-reviewer 处理安全关键问题

---

## rust-reviewer

### 名称和用途
Rust 代码审查专家，专注于所有权、生命周期的安全性、错误处理、unsafe 使用和惯用语模式。

### 能力说明
- 所有权和生命周期检查
- 借用检查器错误诊断
- unsafe 代码安全审查
- 错误处理模式验证
- 并发安全检查

### 适用场景
- Rust 项目中的所有代码变更
- 需要内存安全保证的项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 Rust 文件
- Bash: 运行 cargo check, clippy, fmt, test, audit

### 与其他 Agent 的协作方式
- rust-build-resolver 处理构建错误
- 与 security-reviewer 共享安全审查规则

---

## cpp-reviewer

### 名称和用途
C++ 代码审查专家，专注于内存安全、现代 C++ 惯用语、并发和性能。

### 能力说明
- 内存安全检查（raw new/delete、缓冲区溢出）
- 现代 C++ 模式推广（智能指针、RAII）
- 并发安全检查
- 性能模式识别
- 安全漏洞检测

### 适用场景
- C++ 项目中的所有代码变更
- 需要现代 C++ Melhores-Práticas的项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 C++ 文件
- Bash: 运行 clang-tidy, cppcheck, cmake

### 与其他 Agent 的协作方式
- cpp-build-resolver 处理构建问题
- security-reviewer 处理安全关键问题

---

## flutter-reviewer

### 名称和用途
Flutter 和 Dart 代码审查专家，审查 Flutter 代码的 widget Melhores-Práticas、状态管理模式、Dart 惯用语、性能陷阱、可访问性和 Clean Architecture 违规。

### 能力说明
- 状态管理反模式检测（无视解决方案）
- Widget 构建Otimização-de-Desempenho
- 架构边界强制
- 资源生命周期管理
- 可访问性检查

### 适用场景
- Flutter 项目中的所有代码变更
- 跨平台移动应用开发

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 Dart 文件
- Bash: 运行 flutter analyze

### 与其他 Agent 的协作方式
- dart-build-resolver 处理构建问题
- security-reviewer 处理安全关键问题
- e2e-runner 进行端到端测试

---

## typescript-reviewer

### 名称和用途
TypeScript/JavaScript 代码审查专家，专注于类型安全、异步正确性、Node/web 安全和惯用语模式。

### 能力说明
- 类型安全检查（any 滥用、非空断言）
- 异步正确性验证
- 安全漏洞检测
- React/Next.js 特定检查
- Node.js 特定检查

### 适用场景
- TypeScript/JavaScript 项目中的所有代码变更
- Next.js 和 React 项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 TS/TSX/JS/JSX 文件
- Bash: 运行 tsc, eslint, prettier, npm audit

### 与其他 Agent 的协作方式
- build-error-resolver 处理构建错误
- security-reviewer 处理安全关键问题

---

## swift-reviewer

### 名称和用途
Swift 代码审查专家，专注于协议导向设计、值语义、ARC 内存管理、Swift Concurrency 和惯用语模式。

### 能力说明
- Swift Concurrency 安全检查
- 内存管理检查（强引用循环、代理引用）
- 协议导向设计验证
- 错误处理Melhores-Práticas
- 性能模式识别

### 适用场景
- Swift 项目中的所有代码变更
- iOS/macOS 应用开发

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 Swift 文件
- Bash: 运行 swift build, swiftlint, swift test

### 与其他 Agent 的协作方式
- swift-build-resolver 处理构建问题
- security-reviewer 处理安全关键问题

---

## csharp-reviewer

### 名称和用途
C# 代码审查专家，专注于 .NET 约定、异步模式、安全、可空引用类型和性能。

### 能力说明
- .NET 惯用语检查
- 异步模式验证
- 可空类型Segurança
- 安全漏洞检测
- EF Core 特定检查

### 适用场景
- C# 项目中的所有代码变更
- .NET/.NET Core 应用开发

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 C# 文件
- Bash: 运行 dotnet build, dotnet format, dotnet test

### 与其他 Agent 的协作方式
- build-error-resolver 处理 .NET 构建问题
- security-reviewer 处理安全关键问题

---

## java-reviewer

### 名称和用途
Java 代码审查专家，用于 Spring Boot 和 Quarkus 项目。自动检测框架并应用适当的审查规则。

### 能力说明
- 框架自动检测（Spring Boot/Quarkus）
- 分层架构检查
- JPA/Panache/MongoDB 检查
- 并发安全检查
- 安全漏洞检测

### 适用场景
- Spring Boot 项目
- Quarkus 项目
- Java 11+ 项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 Java 文件
- Bash: 运行 Maven/Gradle 检查

### 与其他 Agent 的协作方式
- java-build-resolver 处理构建问题
- security-reviewer 处理安全关键问题

---

## fsharp-reviewer

### 名称和用途
F# 代码审查专家，专注于函数式惯用语、类型安全、模式匹配、计算表达式和性能。

### 能力说明
- 函数式惯用语检查
- 模式匹配完整性验证
- 类型安全检查
- 计算表达式Melhores-Práticas
- 性能模式识别

### 适用场景
- F# 项目中的所有代码变更
- 函数式编程项目

### 使用的工具列表
- Read: 读取文件内容
- Grep: 搜索代码模式
- Glob: 查找 F# 文件
- Bash: 运行 dotnet build, fantomas, dotnet test

### 与其他 Agent 的协作方式
- 与 security-reviewer 共享安全审查规则
- 使用 tdd-guide 确保测试覆盖率
[返回 Agent 索引](../README.md)
