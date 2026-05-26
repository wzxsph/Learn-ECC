# Test

本部分涵盖各Programlama Dilleri的Test策略、TDD工作流和E2ETest模式。

---

## TDD工作流

### KullanımAçıklama
TDD工作流确保所有代码开发遵循Test Güdümlü Geliştirme原则，具有全面的Test覆盖率。

### Kullanım Zamanı
- 编写新功能或功能
- 修复bug或问题
- 重构现有代码
- 添加API端点
- 创建新组件

### Temel Kavramlar

**1. Test先行**
总是先写Test，然后Gerçekleştir代码使TestVasıtasıyla。

**2. 覆盖率要求**
最低80%覆盖率（单元Test + 集成Test + E2ETest）

**3. Test型**
- 单元Test：独立函数和组件
- 集成Test：API端点和数据库操作
- E2ETest：关键用户流和浏览器自动化

**4. RED-GREEN-REFACTOR循环**
- RED: 先写失败的Test
- GREEN: 写最简代码使TestVasıtasıyla
- REFACTOR: 保持Test绿色的情况下改进代码

### TDD流程Adımlar

```bash
# 1. 写Test（RED）
npm test  # TestOlmalı失败

# 2. Gerçekleştir代码（GREEN）
# 写最简代码使TestVasıtasıyla

# 3. 重构（REFACTOR）
# 改进Kod Kalitesi，保持Test绿色

# 4. Doğrulama覆盖率
npm run test:coverage  # Doğrulama80%+覆盖率
```

### Test隔离原则

```typescript
// 错误：Test相互依赖
test('creates user', () => { /* ... */ })
test('updates same user', () => { /* 依赖前一个Test */ })

// 正确：每个Test独立设置
test('creates user', () => {
  const user = createTestUser()
  // Test逻辑
})

test('updates user', () => {
  const user = createTestUser()
  // 更新逻辑
})
```

---

## PythonTest

### KullanımAçıklama
PythonTest策略使用pytest、TDD方法论、fixtures、mocking、参数化和覆盖率要求。

### Kullanım Zamanı
- 遵循TDD方法论编写新Python代码
- TasarımPython项目Test套件
- 审查PythonTest覆盖率
- 设置Test基础设施

### Temel Kavramlar

**1. pytest Fixtures**
使用`@pytest.fixture`创建可复用Test数据。

**2. 参数化Test**
使用`@pytest.mark.parametrize`用不同输入运行Test。

**3. Mocking**
使用`unittest.mock`的`@patch`模拟外部依赖。

**4. 覆盖率**
使用`--cov`测量覆盖率，Hedef80%+。

### 使用Örnek

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

# 参数化Test
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

# 异常Test
with pytest.raises(ValueError, match="invalid input"):
    raise ValueError("invalid input provided")

# 运行命令
pytest --cov=mypackage --cov-report=html
```

---

## GoTest

### KullanımAçıklama
GoTest模式İçerir表驱动Test、子Test、基准Test、模糊Test和Test覆盖率，遵循TDD方法和惯用Go实践。

### Kullanım Zamanı
- 编写新的Go函数或方法
- 为现有代码添加Test覆盖率
- 为性能关键代码创建基准Test
- Gerçekleştir模糊Test进行输入Doğrulama
- 在Go项目中遵循TDD工作流

### Temel Kavramlar

**1. 表驱动Test**
使用表驱动模式Gerçekleştir全面覆盖。

**2. 子Test**
使用`t.Run`组织相关Test。

**3. Test辅助函数**
使用`t.Helper()`标记辅助函数。

**4. 接口Mocking**
使用接口进行依赖解耦和Test。

**5. 基准Test和模糊Test**
性能Test和随机输入Test。

### 使用Örnek

```go
// 表驱动Test
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

// Test辅助函数
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

// 基准Test
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

## RustTest

### KullanımAçıklama
RustTest模式İçerir单元Test、集成Test、异步Test、属性Test、mocking和覆盖率，遵循TDD方法论。

### Kullanım Zamanı
- 编写新的Rust函数、方法或trait
- 为现有代码添加Test覆盖率
- 为性能关键代码创建基准Test
- Gerçekleştir属性Test进行输入Doğrulama
- 在Rust项目中遵循TDD工作流

### Temel Kavramlar

**1. 模块级Test**
在`#[cfg(test)]`模块中编写单元Test。

**2. 集成Test**
在`tests/`Dizin中编写集成Test。

**3. 异步Test**
使用`#[tokio::test]`进行异步Test。

**4. 属性Test**
使用`proptest`进行基于属性的Test。

**5. Mocking**
使用`mockall`模拟trait。

### 使用Örnek

```rust
// 模块级Test
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

// 异步Test
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// 属性Test
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

## E2ETest

### KullanımAçıklama
Playwright E2ETest模式Sağla了构建稳定、快速、可维护E2ETest套件的全面模式，İçerir页面对象模型、配置、CI/CD集成和防抖Test策略。

### Kullanım Zamanı
- 创建Uçtan Uca TestDoğrulama完整用户流
- 使用Playwright进行浏览器自动化
- 设置CI/CD中的E2ETest
- 处理不稳定Test

### Temel Kavramlar

**1. 页面对象模型(POM)**
将页面交互封装在页面对象类中。

**2. Test隔离**
每个Test独立设置数据。

**3. 定位器策略**
使用语义定位器而非CSS类名。

**4. 防抖Test**
识别和处理不稳定Test。

**5. ArtifactsYönet**
截图、视频、跟踪记录Kullanılan调试。

### 使用Örnek

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

// Test结构
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

// 防抖Test
test('conditional skip', async ({ page }) => {
  test.skip(process.env.CI, 'Flaky in CI - Issue #123')
})

test('flaky: complex search', async ({ page }) => {
  test.fixme(true, 'Known flaky test - Issue #123')
})
```

---

## 相关技能

| 技能 | Kullanım |
|------|------|
| `coding-standards` | 编码标准和En İyi Uygulamalar |
| `error-handling` | Hata Yönetimi模式 |
| `security-review` | Güvenlik审查检查清单 |
