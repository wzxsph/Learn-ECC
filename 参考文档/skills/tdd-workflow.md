---
name: tdd-workflow
description: 在编写新功能、修复 bug 或重构代码时使用此技能。强制执行测试驱动开发，要求 80% 以上的覆盖率，包括单元测试、集成测试和 E2E 测试。
origin: ECC
---

# 测试驱动开发工作流

本技能确保所有代码开发遵循 TDD 原则，并提供全面的测试覆盖率。

## 激活时机

- 编写新功能或功能模块
- 修复 bug 或问题
- 重构现有代码
- 添加 API 端点
- 创建新组件

## 核心原则

### 1. 测试优先于代码
始终先编写测试，然后实现代码使测试通过。

### 2. 覆盖率要求
- 最低 80% 覆盖率（单元测试 + 集成测试 + E2E）
- 所有边界情况都已覆盖
- 错误场景已测试
- 边界条件已验证

### 3. 测试类型

#### 单元测试
- 独立的函数和工具函数
- 组件逻辑
- 纯函数
- 帮助函数和工具函数

#### 集成测试
- API 端点
- 数据库操作
- 服务间交互
- 外部 API 调用

#### E2E 测试（Playwright）
- 关键用户流程
- 完整的业务流程
- 浏览器自动化
- UI 交互

### 4. Git 检查点
- 如果代码库使用 Git 管理，在每个 TDD 阶段后创建检查点提交
- 在工作流完成之前，不要压缩或重写这些检查点提交
- 每个检查点提交消息必须描述阶段和捕获的确切证据
- 仅计算当前活动分支上当前任务的提交
- 不要将其他分支、早期无关工作或遥远的分支历史记录视为有效的检查点证据
- 在将检查点视为满足之前，验证该提交可从活动分支的当前 `HEAD` 访问，并属于当前任务序列
- 首选的紧凑工作流是：
  - 一个提交用于添加失败的测试并验证 RED
  - 一个提交用于应用最小修复并验证 GREEN
  - 一个可选提交用于完成重构
- 如果测试提交明确对应 RED，修复提交明确对应 GREEN，则不需要单独的证据提交

## TDD 工作流步骤

### 步骤 1：编写用户旅程
```
作为 [角色]，我希望 [操作]，以便 [收益]

示例：
作为用户，我希望进行语义搜索市场，
以便即使没有精确关键词也能找到相关市场。
```

### 步骤 2：生成测试用例
为每个用户旅程创建全面的测试用例：

```typescript
describe('语义搜索', () => {
  it('返回查询的相关市场', async () => {
    // 测试实现
  })

  it('优雅处理空查询', async () => {
    // 测试边界情况
  })

  it('当 Redis 不可用时回退到子字符串搜索', async () => {
    // 测试回退行为
  })

  it('按相似度分数排序结果', async () => {
    // 测试排序逻辑
  })
})
```

### 步骤 3：运行测试（应该失败）
```bash
npm test
# 测试应该失败 - 我们还没有实现
```

此步骤是所有生产变更的 RED 门禁，必须执行。

在修改业务逻辑或其他生产代码之前，必须通过以下方式之一验证有效的 RED 状态：
- 运行时 RED：
  - 相关的测试目标成功编译
  - 新的或修改的测试实际被执行
  - 结果为 RED
- 编译时 RED：
  - 新测试新实例化、引用或行使了有 bug 的代码路径
  - 编译失败本身就是预期的 RED 信号
- 无论哪种情况，失败都是由预期的业务逻辑 bug、未定义行为或缺失实现引起的
- 失败不是仅由无关的语法错误、测试设置损坏、缺失依赖或无关的回归引起的

仅编写但未编译和执行的测试不计入 RED。

在确认 RED 状态之前不要编辑生产代码。

如果代码库使用 Git 管理，在此阶段验证后立即创建检查点提交。
建议的提交消息格式：
- `test: add reproducer for <功能或 bug>`
- 如果复现器已编译和执行并因预期原因失败，此提交也可以作为 RED 验证检查点
- 在继续之前验证此检查点提交位于当前活动分支上

### 步骤 4：实现代码
编写最少的代码使测试通过：

```typescript
// 由测试引导的实现
export async function searchMarkets(query: string) {
  // 实现代码
}
```

如果代码库使用 Git 管理，现在暂存最小修复，但推迟到步骤 5 验证 GREEN 后再创建检查点提交。

### 步骤 5：再次运行测试
```bash
npm test
# 测试现在应该通过
```

修复后重新运行相同的相关测试目标，确认之前失败的测试现在为 GREEN。

只有在获得有效的 GREEN 结果后才能继续重构。

如果代码库使用 Git 管理，在 GREEN 验证后立即创建检查点提交。
建议的提交消息格式：
- `fix: <功能或 bug>`
- 如果重新运行相同的相关测试目标并通过，修复提交也可以作为 GREEN 验证检查点
- 在继续之前验证此检查点提交位于当前活动分支上

### 步骤 6：重构
在保持测试绿色的同时提高代码质量：
- 消除重复
- 改进命名
- 优化性能
- 增强可读性

如果代码库使用 Git 管理，重构完成且测试保持绿色后立即创建检查点提交。
建议的提交消息格式：
- `refactor: clean up after <功能或 bug> implementation`
- 在考虑 TDD 周期完成之前，验证此检查点提交位于当前活动分支上

### 步骤 7：验证覆盖率
```bash
npm run test:coverage
# 验证达到 80% 以上覆盖率
```

## 测试模式

### 单元测试模式（Jest/Vitest）
```typescript
import { render, screen, fireEvent } from '@testing-library/react'
import { Button } from './Button'

describe('Button 组件', () => {
  it('正确渲染文本', () => {
    render(<Button>点击我</Button>)
    expect(screen.getByText('点击我')).toBeInTheDocument()
  })

  it('点击时调用 onClick', () => {
    const handleClick = jest.fn()
    render(<Button onClick={handleClick}>点击</Button>)

    fireEvent.click(screen.getByRole('button'))

    expect(handleClick).toHaveBeenCalledTimes(1)
  })

  it('当 disabled prop 为 true 时禁用', () => {
    render(<Button disabled>点击</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
  })
})
```

### API 集成测试模式
```typescript
import { NextRequest } from 'next/server'
import { GET } from './route'

describe('GET /api/markets', () => {
  it('成功返回市场列表', async () => {
    const request = new NextRequest('http://localhost/api/markets')
    const response = await GET(request)
    const data = await response.json()

    expect(response.status).toBe(200)
    expect(data.success).toBe(true)
    expect(Array.isArray(data.data)).toBe(true)
  })

  it('验证查询参数', async () => {
    const request = new NextRequest('http://localhost/api/markets?limit=invalid')
    const response = await GET(request)

    expect(response.status).toBe(400)
  })

  it('优雅处理数据库错误', async () => {
    // 模拟数据库失败
    const request = new NextRequest('http://localhost/api/markets')
    // 测试错误处理
  })
})
```

### E2E 测试模式（Playwright）
```typescript
import { test, expect } from '@playwright/test'

test('用户可以搜索和过滤市场', async ({ page }) => {
  // 导航到市场页面
  await page.goto('/')
  await page.click('a[href="/markets"]')

  // 验证页面加载
  await expect(page.locator('h1')).toContainText('市场')

  // 搜索市场
  await page.fill('input[placeholder="搜索市场"]', '选举')

  // 等待防抖和结果
  await page.waitForTimeout(600)

  // 验证搜索结果显示
  const results = page.locator('[data-testid="market-card"]')
  await expect(results).toHaveCount(5, { timeout: 5000 })

  // 验证结果包含搜索词
  const firstResult = results.first()
  await expect(firstResult).toContainText('选举', { ignoreCase: true })

  // 按状态过滤
  await page.click('button:has-text("活跃")')

  // 验证过滤后的结果
  await expect(results).toHaveCount(3)
})

test('用户可以创建新市场', async ({ page }) => {
  // 首先登录
  await page.goto('/creator-dashboard')

  // 填写市场创建表单
  await page.fill('input[name="name"]', '测试市场')
  await page.fill('textarea[name="description"]', '测试描述')
  await page.fill('input[name="endDate"]', '2025-12-31')

  // 提交表单
  await page.click('button[type="submit"]')

  // 验证成功消息
  await expect(page.locator('text=市场创建成功')).toBeVisible()

  // 验证重定向到市场页面
  await expect(page).toHaveURL(/\/markets\/test-market/)
})
```

## 测试文件组织

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx          # 单元测试
│   │   └── Button.stories.tsx       # Storybook
│   └── MarketCard/
│       ├── MarketCard.tsx
│       └── MarketCard.test.tsx
├── app/
│   └── api/
│       └── markets/
│           ├── route.ts
│           └── route.test.ts         # 集成测试
└── e2e/
    ├── markets.spec.ts               # E2E 测试
    ├── trading.spec.ts
    └── auth.spec.ts
```

## 模拟外部服务

### Supabase 模拟
```typescript
jest.mock('@/lib/supabase', () => ({
  supabase: {
    from: jest.fn(() => ({
      select: jest.fn(() => ({
        eq: jest.fn(() => Promise.resolve({
          data: [{ id: 1, name: '测试市场' }],
          error: null
        }))
      }))
    }))
  }
}))
```

### Redis 模拟
```typescript
jest.mock('@/lib/redis', () => ({
  searchMarketsByVector: jest.fn(() => Promise.resolve([
    { slug: 'test-market', similarity_score: 0.95 }
  ])),
  checkRedisHealth: jest.fn(() => Promise.resolve({ connected: true }))
}))
```

### OpenAI 模拟
```typescript
jest.mock('@/lib/openai', () => ({
  generateEmbedding: jest.fn(() => Promise.resolve(
    new Array(1536).fill(0.1) // 模拟 1536 维嵌入
  ))
}))
```

## 测试覆盖率验证

### 运行覆盖率报告
```bash
npm run test:coverage
```

### 覆盖率阈值
```json
{
  "jest": {
    "coverageThresholds": {
      "global": {
        "branches": 80,
        "functions": 80,
        "lines": 80,
        "statements": 80
      }
    }
  }
}
```

## 应避免的常见测试错误

### 错误：测试实现细节
```typescript
// 不要测试内部状态
expect(component.state.count).toBe(5)
```

### 正确：测试用户可见的行为
```typescript
// 测试用户看到的内容
expect(screen.getByText('计数：5')).toBeInTheDocument()
```

### 错误：脆弱的选择器
```typescript
// 容易破坏
await page.click('.css-class-xyz')
```

### 正确：语义选择器
```typescript
// 对变化有弹性
await page.click('button:has-text("提交")')
await page.click('[data-testid="submit-button"]')
```

### 错误：无测试隔离
```typescript
// 测试相互依赖
test('创建用户', () => { /* ... */ })
test('更新同一用户', () => { /* 依赖上一个测试 */ })
```

### 正确：独立测试
```typescript
// 每个测试设置自己的数据
test('创建用户', () => {
  const user = createTestUser()
  // 测试逻辑
})

test('更新用户', () => {
  const user = createTestUser()
  // 更新逻辑
})
```

## 持续测试

### 开发期间的监视模式
```bash
npm test -- --watch
# 文件更改时自动运行测试
```

### 预提交钩子
```bash
# 每次提交前运行
npm test && npm run lint
```

### CI/CD 集成
```yaml
# GitHub Actions
- name: 运行测试
  run: npm test -- --coverage
- name: 上传覆盖率
  uses: codecov/codecov-action@v3
```

## 最佳实践

1. **先写测试** - 始终 TDD
2. **每个测试一个断言** - 聚焦单一行为
3. **描述性测试名称** - 解释测试内容
4. ** Arrange-Act-Assert** - 清晰的测试结构
5. **模拟外部依赖** - 隔离单元测试
6. **测试边界情况** - null、undefined、空、大数据
7. **测试错误路径** - 不仅是 happy path
8. **保持测试快速** - 单元测试 < 50ms 每个
9. **测试后清理** - 无副作用
10. **审查覆盖率报告** - 识别缺口

## 成功指标

- 达到 80% 以上代码覆盖率
- 所有测试通过（绿色）
- 无跳过或禁用的测试
- 测试执行快速（单元测试 < 30 秒）
- E2E 测试覆盖关键用户流程
- 测试在生产前捕获 bug

---

**记住**：测试不是可选的。它们是使自信重构、快速开发和生产可靠性成为可能的安全网。