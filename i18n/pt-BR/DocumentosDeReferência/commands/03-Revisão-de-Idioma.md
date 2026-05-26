# 语言审查命令

本文档介绍 ECC 中用于各Linguagens-de-Programação代码审查的专门命令。

---

## /python-review

**用途说明**: Python 代码全面审查。检查 PEP 8 合规性、类型提示、安全性和 Python 习惯用法。

**使用方法**:
```
/python-review
```

**使用场景**:
- 编写或修改 Python 代码之后
- 提交 Python 变更之前
- 审查包含 Python 代码的 PR
- 学习 Python 习惯用法和Melhores-Práticas

**审查类别**:

### CRITICAL (必须修复)
- SQL/命令注入漏洞
- 不安全的 eval/exec 使用
- Pickle 不安全反序列化
- 硬编码凭证
- YAML 不安全加载
- 隐藏错误的裸 except 子句

### HIGH (应该修复)
- 公共函数缺少类型提示
- 可变默认参数
- 静默吞没异常
- 资源管理未使用上下文管理器
- C 风格循环而非推导式
- 使用 type() 而非 isinstance()

### MEDIUM (建议考虑)
- PEP 8 格式违规
- 公共函数缺少文档字符串
- Print 语句而非日志
- 低效的字符串操作
- 魔术数字未使用命名常量
- 未使用 f-strings

**自动检查命令**:
```bash
mypy .                              # 类型检查
ruff check .                        # Linting
black --check .                     # 格式化检查
isort --check-only .                # 导入排序
bandit -r .                         # 安全扫描
pip-audit                           # 依赖审计
pytest --cov=app --cov-report=term-missing  # 测试覆盖
```

**常见问题修复**:

```python
# SQL 注入修复
# 错误
query = f"SELECT * FROM users WHERE id = {user_id}"

# 正确 - 使用参数化查询
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))

# 可变默认参数修复
# 错误
def process_items(items=[]):
    items.append("new")
    return items

# 正确
def process_items(items=None):
    if items is None:
        items = []
    items.append("new")
    return items

# 使用上下文管理器
# 错误
f = open("config.json")
data = f.read()
f.close()

# 正确
with open("config.json") as f:
    data = f.read()
```

---

## /go-review

**用途说明**: Go 代码全面审查。检查惯用模式、并发安全性、错误处理和安全性。

**使用方法**:
```
/go-review
```

**使用场景**:
- 编写或修改 Go 代码之后
- 提交 Go 变更之前
- 审查包含 Go 代码的 PR
- 学习 Go 惯用模式

**审查类别**:

### CRITICAL (必须修复)
- SQL/命令注入漏洞
- 无同步的竞态条件
- Goroutine 泄漏
- 硬编码凭证
- 不安全的指针使用
- 关键路径忽略错误

### HIGH (应该修复)
- 缺少错误的上下文包装
- Panic 而非错误返回
- Context 未传播
- 未缓冲通道导致死锁
- 接口未实现错误
- 缺少互斥锁保护

### MEDIUM (建议考虑)
- 非惯用代码模式
- 导出函数缺少 godoc 注释
- 低效的字符串拼接
- Slice 未预分配
- 未使用表驱动测试

**自动检查命令**:
```bash
go vet ./...                         # 静态分析
staticcheck ./...                    # 高级检查（如果安装）
golangci-lint run                   # Linting
go build -race ./...                # 竞态检测
govulncheck ./...                   # 安全漏洞
```

---

## /kotlin-review

**用途说明**: Kotlin 代码全面审查。检查惯用模式、空安全、协程安全性和安全性。

**使用方法**:
```
/kotlin-review
```

**使用场景**:
- 编写或修改 Kotlin 代码之后
- 提交 Kotlin 变更之前
- 审查 Android/KMP 项目的 PR
- 学习 Kotlin 惯用模式

**审查类别**:

### CRITICAL (必须修复)
- SQL/命令注入漏洞
- 无充分理由的强制解包 `!!`
- 平台类型空安全违规
- GlobalScope 使用（结构化并发违规）
- 硬编码凭证
- 不安全反序列化

### HIGH (应该修复)
- 可变状态（可用不可变时）
- 协程上下文内的阻塞调用
- 长循环中缺少取消检查
- 密封类 `when` 非穷举
- 大函数（>50行）
- 深嵌套（>4层）

### MEDIUM (建议考虑)
- 非惯用 Kotlin（Java 风格模式）
- 缺少尾随逗号
- Scope 函数误用或嵌套
- 大集合链缺少 Sequence
- 冗余显式类型

**自动检查命令**:
```bash
./gradlew build                      # 构建检查
./gradlew detekt                     # 静态分析
./gradlew ktlintCheck                # 格式检查
./gradlew test                       # 测试
```

---

## /rust-review

**用途说明**: Rust 代码全面审查。检查所有权生命周期、错误处理、不安全使用和惯用模式。

**使用方法**:
```
/rust-review
```

**使用场景**:
- 编写或修改 Rust 代码之后
- 提交 Rust 变更之前
- 审查 Rust 项目的 PR
- 学习 Rust 惯用模式

**审查类别**:

### CRITICAL (必须修复)
- 生产代码路径中未检查的 `unwrap()`/`expect()`
- 不带 `// SAFETY:` 注释的 `unsafe`
- 字符串插值的 SQL 注入
- 未验证输入的命令注入
- 硬编码凭证
- 原始指针的释放后使用

### HIGH (应该修复)
- 不必要的 `.clone()` 以满足借用检查器
- `String` 参数应用 `&str` 或 `impl AsRef<str>`
- 异步上下文中的阻塞（`std::thread::sleep`）
- 共享类型缺少 `Send`/`Sync` 约束
- 关键枚举上的通配符 `_ =>` 匹配
- 大函数（>50行）

### MEDIUM (建议考虑)
- 热路径中的不必要分配
- 已知大小时缺少 `with_capacity`
- 无充分理由抑制 clippy 警告
- 公共 API 缺少 `///` 文档
- 考虑对可能忽略值的非 `must_use` 返回类型使用 `#[must_use]`

**自动检查命令**:
```bash
cargo check                           # 构建检查
cargo clippy -- -D warnings            # Lints
cargo fmt --check                      # 格式检查
cargo test                            # 测试
cargo audit                           # 安全审计（如果可用）
```

---

## /cpp-review

**用途说明**: C++ 代码全面审查。检查内存安全、现代 C++ 习惯用法、并发和安全性。

**使用方法**:
```
/cpp-review
```

**使用场景**:
- 编写或修改 C++ 代码之后
- 提交 C++ 变更之前
- 审查 C++ 项目的 PR
- 检查内存安全问题

**审查类别**:

### CRITICAL (必须修复)
- 无 RAII 的裸 `new`/`delete`
- 缓冲区溢出和释放后使用
- 无同步的数据竞态
- 通过 `system()` 的命令注入
- 未初始化变量读取
- 空指针解引用

### HIGH (应该修复)
- 五法则违规
- 缺少 `std::lock_guard` / `std::scoped_lock`
- detach 线程的寿命管理不当
- C 风格强制转换而非 `static_cast`/`dynamic_cast`
- 缺少 `const` 正确性

### MEDIUM (建议考虑)
- 不必要的拷贝（传值而非 `const&`）
- 已知大小时缺少 `reserve()`
- 头文件中 `using namespace std;`
- 重要返回值缺少 `[[nodiscard]]`
- 过于复杂的模板元编程

**自动检查命令**:
```bash
clang-tidy --checks='*,-llvmlibc-*' src/*.cpp -- -std=c++17
cppcheck --enable=all --suppress=missingIncludeSystem src/
cmake --build build -- -Wall -Wextra -Wpedantic
```

---

## /flutter-review

**用途说明**: Flutter/Dart 代码审查。检查惯用模式、组件Melhores-Práticas、状态管理、性能、可访问性和安全性。

**使用方法**:
```
/flutter-review
```

**使用场景**:
- 提交包含 Flutter/Dart 变更的 PR 之前（构建和测试通过后）
- 实现新功能后早期发现问题
- 审查他人的 Flutter 代码
- 审核组件、状态管理组件或服务类
- 生产发布之前

**审查区域**:

| 区域 | 严重级别 |
|------|----------|
| 硬编码密钥、明文 HTTP | CRITICAL |
| 架构违规、状态管理反模式 | CRITICAL |
| 组件重建问题、资源泄漏 | HIGH |
| 缺少 `dispose()`、`await` 后的 BuildContext | HIGH |
| Dart 空安全、缺少错误/加载状态 | HIGH |
| Const 传播、组件组合 | HIGH |
| `build()` 中的昂贵工作 | HIGH |
| 可访问性、语义标签 | MEDIUM |
| 状态转换缺少测试 | HIGH |
| 硬编码字符串（l10n） | MEDIUM |

---

## /fastapi-review

**用途说明**: FastAPI 应用审查。检查架构、异步正确性、依赖注入、Pydantic 模式、安全性、性能和可测试性。

**使用方法**:
```
/fastapi-review [文件或目录]
```

**审查区域**:
- App factory、路由边界、中间件和异常处理器
- Pydantic 请求和响应模式分离
- 数据库会话、认证、分页和设置的依赖注入
- 异步数据库和外部 HTTP 模式
- CORS、认证、限流、日志和密钥处理
- OpenAPI 元数据和文档化响应模型
- 测试客户端设置和依赖覆盖

---

## 语言审查命令对比表

| 命令 | 语言 | 关键检查 | 严重级别 |
|------|------|----------|----------|
| `/python-review` | Python | PEP 8、类型提示、SQL注入 | CRITICAL/HIGH/MEDIUM |
| `/go-review` | Go | 并发安全、错误处理、goroutine | CRITICAL/HIGH/MEDIUM |
| `/kotlin-review` | Kotlin | 空安全、协程、GlobalScope | CRITICAL/HIGH/MEDIUM |
| `/rust-review` | Rust | 所有权、生命周期、不安全 | CRITICAL/HIGH/MEDIUM |
| `/cpp-review` | C++ | 内存安全、RAII、并发 | CRITICAL/HIGH/MEDIUM |
| `/flutter-review` | Flutter | 状态管理、可访问性、性能 | CRITICAL/HIGH/MEDIUM |
| `/fastapi-review` | Python | Pydantic、DI、异步、安全 | CRITICAL/HIGH/MEDIUM |
