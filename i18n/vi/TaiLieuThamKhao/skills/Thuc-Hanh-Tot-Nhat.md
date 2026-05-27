# Thực hành tốt nhất

Phần này bao gồm các tiêu chuẩn mã hóa, xử lý lỗi và thực hành phát triển chung.

---

## Tiêu chuẩn mã hóa

### Mô tả sử dụng
Các quy ước mã hóa cơ sở áp dụng cross-project, bao gồm đặt tên, khả năng đọc, tính bất biến và kiểm tra chất lượng mã.

### Thời điểm sử dụng
- Khởi tạo dự án hoặc module mới
- Kiểm tra chất lượng mã và khả năng bảo trì
- Tái cấu trúc mã hiện có theo quy ước
- Ép buộc tính nhất quán về đặt tên, format hoặc cấu trúc
- Thiết lập quy tắc linting, formatting hoặc kiểm tra loại
- Đưa vào contributor mới các quy ước mã hóa

### Nguyên tắc cốt lõi

**1. Khả năng đọc là ưu tiên hàng đầu**
- Mã được đọc nhiều hơn được viết
- Tên biến và hàm rõ ràng
- Ưu tiên self-documenting code hơn comments
- Format nhất quán

**2. Nguyên tắc KISS**
- Giữ đơn giản, không over-engineering
- Tránh tối ưu hóa sớm
- Dễ hiểu > mã thông minh

**3. Nguyên tắc DRY**
- Trích xuất logic chung thành hàm
- Tạo component có thể tái sử dụng
- Chia sẻ utility giữa các module
- Tránh copy-paste programming

**4. Nguyên tắc YAGNI**
- Không xây dựng tính năng chưa cần
- Tránh speculative generality
- Chỉ thêm độ phức tạp khi cần
- Bắt đầu đơn giản, tái cấu trúc khi cần

### Tiêu chuẩn TypeScript/JavaScript

```typescript
// Tốt: Tên có mô tả
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000

// Không tốt: Tên không rõ ràng
const q = 'election'
const flag = true
const x = 1000

// Tốt: Mẫu động từ-danh từ
async function fetchMarketData(marketId: string) { }
function calculateSimilarity(a: number[], b: number[]) { }
function isValidEmail(email: string): boolean { }

// Tốt: Tính bất biến với spread operator
const updatedUser = { ...user, name: 'New Name' }
const updatedArray = [...items, newItem]

// Không tốt: Không bao giờ sửa đổi trực tiếp
user.name = 'New Name'  // Không tốt
items.push(newItem)     // Không tốt

// Tốt: Xử lý lỗi toàn diện
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

// Tốt: Thực thi song song khi có thể
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats()
])
```

### Thực hành tốt nhất React

```typescript
// Tốt: Function component có loại
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

// Tốt: Custom Hook
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

// Tốt: Cập nhật state phù hợp
const [count, setCount] = useState(0)
// Sử dụng functional update khi state phụ thuộc vào state trước đó
setCount(prev => prev + 1)
```

### Tiêu chuẩn thiết kế API

```typescript
// Quy ước REST API
GET    /api/markets              // Liệt kê tất cả thị trường
GET    /api/markets/:id          // Lấy thị trường cụ thể
POST   /api/markets              // Tạo thị trường mới
PUT    /api/markets/:id          // Cập nhật thị trường (toàn bộ)
PATCH  /api/markets/:id          // Cập nhật thị trường (một phần)
DELETE /api/markets/:id          // Xóa thị trường

// Tốt: Cấu trúc response nhất quán
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: { total: number; page: number; limit: number }
}

// Tốt: Schema validation
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
    // Tiếp tục với dữ liệu đã validated
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({ success: false, error: 'Validation failed', details: error.errors }, { status: 400 })
    }
  }
}
```

### Phát hiện mùi mã

```typescript
// Không tốt: Hàm dài (>50 dòng)
function processMarketData() {
  // 100 dòng code
}

// Tốt: Tách thành các hàm nhỏ hơn
function processMarketData() {
  const validated = validateData()
  const transformed = transformData(validated)
  return saveData(transformed)
}

// Không tốt: Lồng ghép sâu (5+ tầng)
if (user) {
  if (user.isAdmin) {
    if (market) {
      if (market.isActive) {
        if (hasPermission) {
          // Làm gì đó
        }
      }
    }
  }
}

// Tốt: Early return
if (!user) return
if (!user.isAdmin) return
if (!market) return
if (!market.isActive) return
if (!hasPermission) return
// Làm gì đó

// Không tốt: Số ma thuật
if (retryCount > 3) { }
setTimeout(callback, 500)

// Tốt: Hằng số có tên
const MAX_RETRIES = 3
const DEBOUNCE_DELAY_MS = 500
```

---

## Xử lý lỗi

### Mô tả sử dụng
Các pattern xử lý lỗi mạnh mẽ cross-TypeScript, Python và Go, bao gồm lỗi có loại, error boundaries, retry, circuit breaker và thông báo lỗi thân thiện với người dùng.

### Thời điểm sử dụng
- Thiết kế loại lỗi hoặc exception hierarchy cho module hoặc service mới
- Thêm retry logic hoặc circuit breaker cho external dependencies không đáng tin cậy
- Kiểm tra endpoint API có xử lý lỗi thiếu
- Triển khai thông báo lỗi thân thiện với người dùng và feedback
- Debug cascading failures hoặc lỗi bị nuốt âm thầm

### Nguyên tắc cốt lõi

1. **Fail nhanh và to tiếng** - Phơi lỗi ở ranh giới
2. **Lỗi có loại tốt hơn chuỗi thông báo** - Lỗi là công dân hạng nhất, có cấu trúc
3. **Thông báo người dùng ≠ thông báo developer** - Hiển thị text thân thiện cho người dùng, server-side ghi log context đầy đủ
4. **Không bao giờ nuốt lỗi âm thầm** - Mỗi `catch` block phải xử lý, re-throw hoặc ghi log
5. **Lỗi là một phần của hợp đồng API** - Ghi lại mỗi error code client có thể nhận

### Xử lý lỗi TypeScript

```typescript
// Lớp lỗi có loại
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

// Mẫu Result
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

// API error handler
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

### Xử lý lỗi Python

```python
# Exception hierarchy tùy chỉnh
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

# FastAPI global exception handler
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
        content={"error": {"code: "INTERNAL_ERROR", "message": "An unexpected error occurred"}},
    )
```

### Xử lý lỗi Go

```go
// Sentinel errors
var (
    ErrNotFound    = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrConflict     = errors.New("conflict")
)

// Wrap error với context
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

// Unwrap ở handler level để xác định response
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

### Retry exponential backoff

```typescript
interface RetryOptions {
  maxAttempts?: number
  baseDelayMs?: number
  maxDelayMs?: number
  retryIf?: (error: unknown) => boolean
}

async function withRetry<T>(fn: () => Promise<T>, options: RetryOptions = {}): Promise<T> {
  const { maxAttempts = 3, baseDelayMs = 500, maxDelayMs = 10_000, retryIf = () => true } = options;

  let lastError: unknown;

  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (error) {
      lastError = error;
      if (attempt === maxAttempts || !retryIf(error)) throw error;

      const jitter = Math.random() * baseDelayMs;
      const delay = Math.min(baseDelayMs * 2 ** (attempt - 1) + jitter, maxDelayMs);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }

  throw lastError;
}
```

### Thông báo lỗi thân thiện với người dùng

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
  return USER_ERROR_MESSAGES[code] ?? USER_ERROR_MESSAGES.INTERNAL_ERROR;
}
```

### Checklist xử lý lỗi

- [ ] Mỗi `catch` block xử lý, re-throw hoặc ghi log - không nuốt âm thầm
- [ ] Lỗi API tuân theo envelope chuẩn `{ error: { code, message } }`
- [ ] Thông báo nhìn thấy với người dùng không chứa stack trace hoặc chi tiết nội bộ
- [ ] Context lỗi đầy đủ được ghi log server-side
- [ ] Lớp lỗi tùy chỉnh mở rộng `AppError` base với trường `code`
- [ ] Hàm async chuyển lỗi cho caller - không fire-and-forget
- [ ] Retry logic chỉ retry các lỗi có thể retry (không phải 4xx client errors)
- [ ] React component bọc trong `ErrorBoundary` để xử lý lỗi render

---

## Skills liên quan

| Skill | Mục đích |
|------|----------|
| `python-patterns` | Python idiomatic patterns và thực hành tốt nhất |
| `golang-patterns` | Go idiomatic patterns và thực hành tốt nhất |
| `rust-patterns` | Rust idiomatic patterns và thực hành tốt nhất |
| `kotlin-patterns` | Kotlin idiomatic patterns và thực hành tốt nhất |
| `cpp-coding-standards` | Tiêu chuẩn mã hóa C++ và thực hành tốt nhất |
| `tdd-workflow` | Quy trình phát triển test-driven |
| `security-review` | Checklist kiểm tra bảo mật |

---

## Hệ thống Agent và Loop tự trị

### autonomous-agent-harness

**Mục đích**: Biến Claude Code thành hệ thống agent hoàn toàn tự trị

**Chức năng chính**:
- Lưu trữ bộ nhớ persistence
- Lập lịch tác vụ định kỳ (giống cron)
- Trigger agent từ xa (Dispatch)
- Điều khiển máy tính (Computer Use)
- Quản lý hàng đợi tác vụ

**Thời điểm sử dụng**:
- Khi người dùng muốn thao tác tự trị liên tục hoặc định kỳ
- Khi cần xây dựng AI assistant cá nhân cross-session memory
- Khi cần tái tạo chức năng autonomous agent framework như Hermes, AutoGPT
- Khi cần kết hợp điều khiển máy tính với thực thi định kỳ

**Các thành phần kiến trúc**:
| Thành phần | Mô tả |
|------|----------|
| Crons | Lập lịch tác vụ định kỳ |
| Dispatch | Trigger agent từ xa |
| Memory | Lưu trữ bộ nhớ persistence |
| Computer Use | Điều khiển trình duyệt và desktop |

**Bảng tương đương thay thế Hermes**:
| Hermes | Tương đương ECC |
|--------|-----------|
| Gateway/Router | Claude Code dispatch + crons |
| Memory System | Claude memory + MCP memory server |
| Tool Registry | MCP servers |
| Orchestration | ECC skills + agents |
| Computer Use | computer-use MCP |

---

### autonomous-loops

**Mục đích**: Mẫu loop tự trị và kiến trúc - từ simple sequential pipeline đến hệ thống DAG đa agent RFC-driven

**Phổ mẫu loop**:
| Mẫu | Độ phức tạp | Kịch bản sử dụng |
|------|--------|----------|
| Sequential Pipeline | Thấp | Các bước phát triển hàng ngày, workflow script |
| NanoClaw REPL | Thấp | Phiên persistence tương tác |
| Infinite Agentic Loop | Trung bình | Tạo nội dung song song, work theo spec |
| Continuous Claude PR Loop | Trung bình | Dự án multi-ngày, CI gates |
| De-Sloppify Pattern | Bổ sung | Cleanup/refactor sau bất kỳ bước Implementer nào |
| Ralphinho / RFC-Driven DAG | Cao | Tính năng lớn, multi-unit parallel work với merge queue |

**Thời điểm sử dụng**:
- Thiết lập workflow phát triển tự trị
- Chọn kiến trúc loop phù hợp
- Xây dựng CI/CD pipeline phát triển liên tục
- Chạy parallel agent với điều phối merge

**Các mẫu chính**:

1. **Sequential Pipeline** - `claude -p` pipeline đơn giản
2. **NanoClaw REPL** - Loop persistence với session-aware tích hợp
3. **Infinite Agentic Loop** - Tạo song song theo spec
4. **Continuous Claude PR Loop** - Script shell production-grade, tự động tạo PR, đợi CI, auto-merge
5. **De-Sloppify Pattern** - Thêm bước cleanup/refactor chuyên dụng sau Implementer
6. **Ralphinho** - RFC-driven multi-agent DAG orchestration