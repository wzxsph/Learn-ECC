# 测试

本部分涵盖各Linguagens-de-Programação的测试策略、TDD工作流和E2E测试模式。

---

## TDD工作流

### 用途说明
TDD工作流确保所有代码开发遵循测试驱动开发原则，具有全面的测试覆盖率。

### 使用时机
- 编写新功能或功能
- 修复bug或问题
- 重构现有代码
- 添加API端点
- 创建新组件

### 核心概念

**1. 测试先行**
总是先写测试，然后实现代码使测试通过。

**2. 覆盖率要求**
最低80%覆盖率（单元测试 + 集成测试 + E2E测试）

**3. 测试类型**
- 单元测试：独立函数和组件
- 集成测试：API端点和数据库操作
- E2E测试：关键用户流和浏览器自动化

**4. RED-GREEN-REFACTOR循环**
- RED: 先写失败的测试
- GREEN: 写最简代码使测试通过
- REFACTOR: 保持测试绿色的情况下改进代码

### TDD流程步骤

```bash
# 1. 写测试（RED）
npm test  # 测试应该失败

# 2. 实现代码（GREEN）
# 写最简代码使测试通过

# 3. 重构（REFACTOR）
# 改进代码质量，保持测试绿色

# 4. 验证覆盖率
npm run test:coverage  # 验证80%+覆盖率
```

### 测试隔离原则

```typescript
// 错误：测试相互依赖
test('creates user', () => { /* ... */ })
test('updates same user', () => { /* 依赖前一个测试 */ })

// 正确：每个测试独立设置
test('creates user', () => {
  const user = createTestUser()
  // 测试逻辑
})

test('updates user', () => {
  const user = createTestUser()
  // 更新逻辑
})
```

---

## Python测试

### 用途说明
Python测试策略使用pytest、TDD方法论、fixtures、mocking、参数化和覆盖率要求。

### 使用时机
- 遵循TDD方法论编写新Python代码
- 设计Python项目测试套件
- 审查Python测试覆盖率
- 设置测试基础设施

### 核心概念

**1. pytest Fixtures**
使用`@pytest.fixture`创建可复用测试数据。

**2. 参数化测试**
使用`@pytest.mark.parametrize`用不同输入运行测试。

**3. Mocking**
使用`unittest.mock`的`@patch`模拟外部依赖。

**4. 覆盖率**
使用`--cov`测量覆盖率，目标80%+。

### 使用示例

```python
import pytest
from unittest.mock import patch

# Fixtures
@pytest.fixture
def database():
    db = Database(":memory:")
    db.create_tables()
    yield db
    db.close()

# 参数化测试
@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("world", "WORLD"),
    ("PyThOn", "PYTHON"),
])
def test_uppercase(input, expected):
    assert input.upper() == expected

# Mocking
@patch("mypackage.external_api_call")
def test_with_mock(api_call_mock):
    api_call_mock.return_value = {"status": "success"}
    result = my_function()
    assert result["status"] == "success"

# 异常测试
with pytest.raises(ValueError, match="invalid input"):
    raise ValueError("invalid input provided")

# 运行命令
pytest --cov=mypackage --cov-report=html
```

---

## Go测试

### 用途说明
Go测试模式包括表驱动测试、子测试、基准测试、模糊测试和测试覆盖率，遵循TDD方法和惯用Go实践。

### 使用时机
- 编写新的Go函数或方法
- 为现有代码添加测试覆盖率
- 为性能关键代码创建基准测试
- 实现模糊测试进行输入验证
- 在Go项目中遵循TDD工作流

### 核心概念

**1. 表驱动测试**
使用表驱动模式实现全面覆盖。

**2. 子测试**
使用`t.Run`组织相关测试。

**3. 测试辅助函数**
使用`t.Helper()`标记辅助函数。

**4. 接口Mocking**
使用接口进行依赖解耦和测试。

**5. 基准测试和模糊测试**
性能测试和随机输入测试。

### 使用示例

```go
// 表驱动测试
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"negative numbers", -1, -2, -3},
        {"zero values", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.expected)
            }
        })
    }
}

// 测试辅助函数
func setupTestDB(t *testing.T) *sql.DB {
    t.Helper()
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatalf("failed to open database: %v", err)
    }
    t.Cleanup(func() { db.Close() })
    return db
}

// 接口Mocking
type UserRepository interface {
    GetUser(id string) (*User, error)
    SaveUser(user *User) error
}

type MockUserRepository struct {
    GetUserFunc  func(id string) (*User, error)
    SaveUserFunc func(user *User) error
}

func (m *MockUserRepository) GetUser(id string) (*User, error) {
    return m.GetUserFunc(id)
}

// 基准测试
func BenchmarkProcess(b *testing.B) {
    data := generateTestData(1000)
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        Process(data)
    }
}

// 运行命令
go test -cover ./...
go test -bench=. -benchmem
go test -fuzz=FuzzParse -fuzztime=30s
```

---

## Rust测试

### 用途说明
Rust测试模式包括单元测试、集成测试、异步测试、属性测试、mocking和覆盖率，遵循TDD方法论。

### 使用时机
- 编写新的Rust函数、方法或trait
- 为现有代码添加测试覆盖率
- 为性能关键代码创建基准测试
- 实现属性测试进行输入验证
- 在Rust项目中遵循TDD工作流

### 核心概念

**1. 模块级测试**
在`#[cfg(test)]`模块中编写单元测试。

**2. 集成测试**
在`tests/`目录中编写集成测试。

**3. 异步测试**
使用`#[tokio::test]`进行异步测试。

**4. 属性测试**
使用`proptest`进行基于属性的测试。

**5. Mocking**
使用`mockall`模拟trait。

### 使用示例

```rust
// 模块级测试
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn creates_user_with_valid_email() {
        let user = User::new("Alice", "alice@example.com").unwrap();
        assert_eq!(user.display_name(), "Alice");
    }

    #[test]
    fn rejects_invalid_email() {
        let result = User::new("Bob", "not-an-email");
        assert!(result.is_err());
    }
}

// 异步测试
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// 属性测试
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        prop_assert_eq!(input, decoded);
    }
}

// Mocking with mockall
use mockall::{automock, predicate::eq};

#[automock]
trait UserRepository {
    fn find_by_id(&self, id: u64) -> Option<User>;
    fn save(&self, user: &User) -> Result<(), StorageError>;
}

#[test]
fn service_returns_user_when_found() {
    let mut mock = MockUserRepository::new();
    mock.expect_find_by_id()
        .with(eq(42))
        .times(1)
        .returning(|_| Some(User { id: 42, name: "Alice".into() }));

    let service = UserService::new(Box::new(mock));
    let user = service.get_user(42).unwrap();
    assert_eq!(user.name, "Alice");
}

// 运行命令
cargo test
cargo llvm-cov --fail-under-lines 80
cargo bench
```

---

## E2E测试

### 用途说明
Playwright E2E测试模式提供了构建稳定、快速、可维护E2E测试套件的全面模式，包括页面对象模型、配置、CI/CD集成和防抖测试策略。

### 使用时机
- 创建端到端测试验证完整用户流
- 使用Playwright进行浏览器自动化
- 设置CI/CD中的E2E测试
- 处理不稳定测试

### 核心概念

**1. 页面对象模型(POM)**
将页面交互封装在页面对象类中。

**2. 测试隔离**
每个测试独立设置数据。

**3. 定位器策略**
使用语义定位器而非CSS类名。

**4. 防抖测试**
识别和处理不稳定测试。

**5. Artifacts管理**
截图、视频、跟踪记录用于调试。

### 使用示例

```typescript
import { test, expect } from '@playwright/test'

// 页面对象模型
export class ItemsPage {
  readonly page: Page
  readonly searchInput: Locator
  readonly itemCards: Locator

  constructor(page: Page) {
    this.page = page
    this.searchInput = page.locator('[data-testid="search-input"]')
    this.itemCards = page.locator('[data-testid="item-card"]')
  }

  async goto() {
    await this.page.goto('/items')
    await this.page.waitForLoadState('networkidle')
  }

  async search(query: string) {
    await this.searchInput.fill(query)
    await this.page.waitForResponse(resp => resp.url().includes('/api/search'))
  }
}

// 测试结构
test.describe('Item Search', () => {
  let itemsPage: ItemsPage

  test.beforeEach(async ({ page }) => {
    itemsPage = new ItemsPage(page)
    await itemsPage.goto()
  })

  test('should search by keyword', async ({ page }) => {
    await itemsPage.search('test')
    const count = await itemsPage.getItemCount()
    expect(count).toBeGreaterThan(0)
    await expect(itemsPage.itemCards.first()).toContainText(/test/i)
  })
})

// Playwright配置
export default defineConfig({
  testDir: './tests/e2e',
  retries: process.env.CI ? 2 : 0,
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
  ],
})

// 防抖测试
test('conditional skip', async ({ page }) => {
  test.skip(process.env.CI, 'Flaky in CI - Issue #123')
})

test('flaky: complex search', async ({ page }) => {
  test.fixme(true, 'Known flaky test - Issue #123')
})
```

---

## 相关技能

| 技能 | 用途 |
|------|------|
| `coding-standards` | 编码标准和Melhores-Práticas |
| `error-handling` | 错误处理模式 |
| `security-review` | 安全审查检查清单 |
