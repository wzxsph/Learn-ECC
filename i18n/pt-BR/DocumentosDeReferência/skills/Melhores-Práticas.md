# Melhores-Práticas

本部分涵盖编码标准、错误处理和通用开发Melhores-Práticas。

---

## 编码标准

### 用途说明
跨项目适用的基线编码约定，包括命名、可读性、不可变性和代码质量审查。

### 使用时机
- 启动新项目或模块
- 审查代码质量和可维护性
- 重构现有代码遵循约定
- 强制命名、格式或结构一致性
- 设置linting、格式化或类型检查规则
- 纳入新贡献者编码约定

### 核心原则

**1. 可读性优先**
- 代码阅读次数多于编写次数
- 清晰的变量和函数名称
- 优先使用自文档化代码而非注释
- 一致的格式化

**2. KISS原则**
- 保持简单，不要过度设计
- 避免过早优化
- 容易理解 > 聪明的代码

**3. DRY原则**
- 提取公共逻辑到函数
- 创建可复用组件
- 跨模块共享工具
- 避免复制粘贴编程

**4. YAGNI原则**
- 不构建尚未需要的特性
- 避免 speculative generality
- 仅在需要时添加复杂性
- 先简单，必要时重构

### TypeScript/JavaScript标准

```typescript
// 好：描述性名称
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000

// 不好：不清晰的名称
const q = 'election'
const flag = true
const x = 1000

// 好：动词-名词模式
async function fetchMarketData(marketId: string) { }
function calculateSimilarity(a: number[], b: number[]) { }
function isValidEmail(email: string): boolean { }

// 好：使用展开运算符的不可变性
const updatedUser = { ...user, name: 'New Name' }
const updatedArray = [...items, newItem]

// 不好：永远不要直接修改
user.name = 'New Name'  // 不好
items.push(newItem)     // 不好

// 好：全面的错误处理
async function fetchData(url: string) {
  try {
    const response = await fetch(url)
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }
    return await response.json()
  } catch (error) {
    console.error('Fetch failed:', error)
    throw new Error('Failed to fetch data')
  }
}

// 好：尽可能并行执行
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats()
])
```

### ReactMelhores-Práticas

```typescript
// 好：带类型的函数组件
interface ButtonProps {
  children: React.ReactNode
  onClick: () => void
  disabled?: boolean
  variant?: 'primary' | 'secondary'
}

export function Button({ children, onClick, disabled = false, variant = 'primary' }: ButtonProps) {
  return (
    <button onClick={onClick} disabled={disabled} className={`btn btn-${variant}`}>
      {children}
    </button>
  )
}

// 好：自定义Hook
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value)
    }, delay)
    return () => clearTimeout(handler)
  }, [value, delay])

  return debouncedValue
}

// 好：适当的状态更新
const [count, setCount] = useState(0)
// 状态基于前一个状态时使用函数式更新
setCount(prev => prev + 1)
```

### API设计标准

```typescript
// REST API约定
GET    /api/markets              // 列出所有市场
GET    /api/markets/:id          // 获取特定市场
POST   /api/markets              // 创建新市场
PUT    /api/markets/:id          // 更新市场（完整）
PATCH  /api/markets/:id          // 更新市场（部分）
DELETE /api/markets/:id          // 删除市场

// 好：一致的响应结构
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: { total: number; page: number; limit: number }
}

// 好：Schema验证
import { z } from 'zod'

const CreateMarketSchema = z.object({
  name: z.string().min(1).max(200),
  description: z.string().min(1).max(2000),
  endDate: z.string().datetime(),
})

export async function POST(request: Request) {
  const body = await request.json()
  try {
    const validated = CreateMarketSchema.parse(body)
    // 使用验证后的数据继续
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({ success: false, error: 'Validation failed', details: error.errors }, { status: 400 })
    }
  }
}
```

### 代码异味检测

```typescript
// 不好：长函数（>50行）
function processMarketData() {
  // 100行代码
}

// 好：拆分为更小的函数
function processMarketData() {
  const validated = validateData()
  const transformed = transformData(validated)
  return saveData(transformed)
}

// 不好：深层嵌套（5+层）
if (user) {
  if (user.isAdmin) {
    if (market) {
      if (market.isActive) {
        if (hasPermission) {
          // 做某事
        }
      }
    }
  }
}

// 好：早期返回
if (!user) return
if (!user.isAdmin) return
if (!market) return
if (!market.isActive) return
if (!hasPermission) return
// 做某事

// 不好：魔术数字
if (retryCount > 3) { }
setTimeout(callback, 500)

// 好：命名常量
const MAX_RETRIES = 3
const DEBOUNCE_DELAY_MS = 500
```

---

## 错误处理

### 用途说明
跨TypeScript、Python和Go的健壮错误处理模式，包括类型化错误、错误边界、重试、断路器和用户友好错误消息。

### 使用时机
- 为新模块或服务设计错误类型或异常层次结构
- 为不可靠的外部依赖添加重试逻辑或断路器
- 审查API端点是否缺少错误处理
- 实现用户友好错误消息和反馈
- 调试级联失败或静默错误吞并

### 核心原则

1. **快速失败并大声** - 在边界处暴露错误
2. **类型化错误优于字符串消息** - 错误是一等公民，有结构
3. **用户消息 ≠ 开发者消息** - 向用户显示友好文本，服务器端记录完整上下文
4. **永远不静默吞并错误** - 每个`catch`块必须处理、重新抛出或记录
5. **错误是API合约的一部分** - 记录客户端可能收到的每个错误码

### TypeScript错误处理

```typescript
// 类型化错误类
export class AppError extends Error {
  constructor(
    message: string,
    public readonly code: string,
    public readonly statusCode: number = 500,
    public readonly details?: unknown,
  ) {
    super(message)
    this.name = this.constructor.name
    Object.setPrototypeOf(this, new.target.prototype)
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string, id: string) {
    super(`${resource} not found: ${id}`, 'NOT_FOUND', 404)
  }
}

export class ValidationError extends AppError {
  constructor(message: string, details: { field: string; message: string }[]) {
    super(message, 'VALIDATION_ERROR', 422, details)
  }
}

// Result模式
type Result<T, E = AppError> =
  | { ok: true; value: T }
  | { ok: false; error: E }

async function fetchUser(id: string): Promise<Result<User>> {
  try {
    const user = await db.users.findUnique({ where: { id } })
    if (!user) return { ok: false, error: new NotFoundError('User', id) }
    return { ok: true, value: user }
  } catch (e) {
    return { ok: false, error: new AppError('Database error', 'DB_ERROR') }
  }
}

// API错误处理器
function handleApiError(error: unknown): NextResponse {
  if (error instanceof AppError) {
    return NextResponse.json(
      { error: { code: error.code, message: error.message, ...(error.details && { details: error.details }) } },
      { status: error.statusCode }
    )
  }
  console.error('Unexpected error:', error)
  return NextResponse.json(
    { error: { code: 'INTERNAL_ERROR', message: 'An unexpected error occurred' } },
    { status: 500 }
  )
}
```

### Python错误处理

```python
# 自定义异常层次结构
class AppError(Exception):
    def __init__(self, message: str, code: str, status_code: int = 500):
        super().__init__(message)
        self.code = code
        self.status_code = status_code

class NotFoundError(AppError):
    def __init__(self, resource: str, id: str):
        super().__init__(f"{resource} not found: {id}", "NOT_FOUND", 404)

class ValidationError(AppError):
    def __init__(self, message: str, details: list[dict] | None = None):
        super().__init__(message, "VALIDATION_ERROR", 422)
        self.details = details or []

# FastAPI全局异常处理器
@app.exception_handler(AppError)
async def app_error_handler(request: Request, exc: AppError) -> JSONResponse:
    return JSONResponse(
        status_code=exc.status_code,
        content={"error": {"code": exc.code, "message": str(exc)}},
    )

@app.exception_handler(Exception)
async def generic_error_handler(request: Request, exc: Exception) -> JSONResponse:
    logger.exception("Unexpected error")
    return JSONResponse(
        status_code=500,
        content={"error": {"code": "INTERNAL_ERROR", "message": "An unexpected error occurred"}},
    )
```

### Go错误处理

```go
// 哨兵错误
var (
    ErrNotFound    = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrConflict     = errors.New("conflict")
)

// 用上下文包装错误
func (r *UserRepository) FindByID(ctx context.Context, id string) (*User, error) {
    user, err := r.db.QueryRow(ctx, "SELECT * FROM users WHERE id = $1", id)
    if errors.Is(err, sql.ErrNoRows) {
        return nil, fmt.Errorf("user %s: %w", id, ErrNotFound)
    }
    if err != nil {
        return nil, fmt.Errorf("querying user %s: %w", id, err)
    }
    return user, nil
}

// 在处理器级别展开确定响应
func (h *Handler) GetUser(w http.ResponseWriter, r *http.Request) {
    user, err := h.service.GetUser(r.Context(), chi.URLParam(r, "id"))
    if err != nil {
        switch {
        case errors.Is(err, domain.ErrNotFound):
            writeError(w, http.StatusNotFound, "not_found", err.Error())
        case errors.Is(err, domain.ErrUnauthorized):
            writeError(w, http.StatusForbidden, "forbidden", "Access denied")
        default:
            slog.Error("unexpected error", "err", err)
            writeError(w, http.StatusInternalServerError, "internal_error", "An unexpected error occurred")
        }
        return
    }
    writeJSON(w, http.StatusOK, user)
}
```

### 指数退避重试

```typescript
interface RetryOptions {
  maxAttempts?: number
  baseDelayMs?: number
  maxDelayMs?: number
  retryIf?: (error: unknown) => boolean
}

async function withRetry<T>(fn: () => Promise<T>, options: RetryOptions = {}): Promise<T> {
  const { maxAttempts = 3, baseDelayMs = 500, maxDelayMs = 10_000, retryIf = () => true } = options

  let lastError: unknown

  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn()
    } catch (error) {
      lastError = error
      if (attempt === maxAttempts || !retryIf(error)) throw error

      const jitter = Math.random() * baseDelayMs
      const delay = Math.min(baseDelayMs * 2 ** (attempt - 1) + jitter, maxDelayMs)
      await new Promise(resolve => setTimeout(resolve, delay))
    }
  }

  throw lastError
}
```

### 用户友好错误消息

```typescript
const USER_ERROR_MESSAGES: Record<string, string> = {
  NOT_FOUND: 'The requested item could not be found.',
  UNAUTHORIZED: 'Please sign in to continue.',
  FORBIDDEN: "You don't have permission to do that.",
  VALIDATION_ERROR: 'Please check your input and try again.',
  RATE_LIMITED: 'Too many requests. Please wait a moment and try again.',
  INTERNAL_ERROR: 'Something went wrong on our end. Please try again later.',
}

export function getUserMessage(code: string): string {
  return USER_ERROR_MESSAGES[code] ?? USER_ERROR_MESSAGES.INTERNAL_ERROR
}
```

### 错误处理检查清单

- [ ] 每个`catch`块处理、重新抛出或记录 - 无静默吞并
- [ ] API错误遵循标准信封`{ error: { code, message } }`
- [ ] 用户可见消息不包含堆栈跟踪或内部细节
- [ ] 完整错误上下文在服务器端记录
- [ ] 自定义错误类扩展带有`code`字段的基`AppError`
- [ ] 异步函数将错误传递给调用者 - 无fire-and-forget
- [ ] 重试逻辑仅重试可重试错误（非4xx客户端错误）
- [ ] React组件包装在`ErrorBoundary`中处理渲染错误

---

## 相关技能

| 技能 | 用途 |
|------|------|
| `python-patterns` | Python惯用模式和Melhores-Práticas |
| `golang-patterns` | Go惯用模式和Melhores-Práticas |
| `rust-patterns` | Rust惯用模式和Melhores-Práticas |
| `kotlin-patterns` | Kotlin惯用模式和Melhores-Práticas |
| `cpp-coding-standards` | C++编码标准和Melhores-Práticas |
| `tdd-workflow` | 测试驱动开发工作流 |
| `security-review` | 安全审查检查清单 |

---

## 自主代理与循环系统

### autonomous-agent-harness

**用途**: 将 Claude Code 转变为完全自主的代理系统

**核心功能**:
- 持久化内存存储
- 定时任务调度（类似 cron）
- 远程代理触发（Dispatch）
- 计算机控制（Computer Use）
- 任务队列管理

**使用时机**:
- 用户想要连续自主操作或定时任务
- 需要构建跨会话记忆的个人 AI 助手
- 需要复制 Hermes、AutoGPT 等自主代理框架功能
- 需要计算机控制与定时执行结合

**架构组件**:
| 组件 | 说明 |
|------|------|
| Crons | 定时任务调度 |
| Dispatch | 远程触发代理 |
| Memory | 持久化内存存储 |
| Computer Use | 浏览器和桌面控制 |

**替代 Hermes 的对应关系**:
| Hermes | ECC 等价物 |
|--------|-----------|
| Gateway/Router | Claude Code dispatch + crons |
| Memory System | Claude memory + MCP memory server |
| Tool Registry | MCP servers |
| Orchestration | ECC skills + agents |
| Computer Use | computer-use MCP |

---

### autonomous-loops

**用途**: 自主循环模式和架构 - 从简单顺序管道到 RFC 驱动的多代理 DAG 系统

**循环模式谱系**:
| 模式 | 复杂度 | 适用场景 |
|------|--------|----------|
| Sequential Pipeline | 低 | 日常开发步骤、脚本化工作流 |
| NanoClaw REPL | 低 | 交互式持久会话 |
| Infinite Agentic Loop | 中 | 并行内容生成、规范驱动工作 |
| Continuous Claude PR Loop | 中 | 多日迭代项目，CI 门禁 |
| De-Sloppify Pattern | 附加 | 任何 Implementer 步骤后的质量清理 |
| Ralphinho / RFC-Driven DAG | 高 | 大型功能、多单元并行工作与合并队列 |

**使用时机**:
- 设置自主开发工作流
- 选择合适的循环架构
- 构建 CI/CD 风格的持续开发管道
- 运行带合并协调的并行代理

**关键模式**:

1. **Sequential Pipeline** - 简单的 `claude -p` 管道
2. **NanoClaw REPL** - 内置持久化循环，session-aware
3. **Infinite Agentic Loop** - 规范驱动的并行生成
4. **Continuous Claude PR Loop** - 生产级 shell 脚本，自动创建 PR、等待 CI、自动合并
5. **De-Sloppify Pattern** - 添加专门的清理/重构步骤
6. **Ralphinho** - RFC 驱动的多代理 DAG 编排

