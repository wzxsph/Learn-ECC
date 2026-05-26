# Best Practices

This section covers coding standards, error handling, and general development best practices.

---

## Coding Standards

### Purpose
Baseline coding conventions applicable across projects, including naming, readability, immutability, and code quality review.

### When to Use
- Starting new projects or modules
- Reviewing code quality and maintainability
- Refactoring existing code to follow conventions
- Enforcing naming, formatting, or structure consistency
- Setting up linting, formatting, or type checking rules
- Onboarding new contributor coding conventions

### Core Principles

**1. Readability First**
- Code is read more often than written
- Clear variable and function names
- Prefer self-documenting code over comments
- Consistent formatting

**2. KISS Principle**
- Keep it simple, don't over-engineer
- Avoid premature optimization
- Understandable > clever code

**3. DRY Principle**
- Extract common logic into functions
- Create reusable components
- Share utilities across modules
- Avoid copy-paste programming

**4. YAGNI Principle**
- Don't build features not yet needed
- Avoid speculative generality
- Add complexity only when necessary
- Start simple, refactor when needed

### TypeScript/JavaScript Standards

```typescript
// Good: descriptive names
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000

// Bad: unclear names
const q = 'election'
const flag = true
const x = 1000

// Good: verb-noun pattern
async function fetchMarketData(marketId: string) { }
function calculateSimilarity(a: number[], b: number[]) { }
function isValidEmail(email: string): boolean { }

// Good: immutability with spread
const updatedUser = { ...user, name: 'New Name' }
const updatedArray = [...items, newItem]

// Bad: never modify directly
user.name = 'New Name'  // Bad
items.push(newItem)     // Bad

// Good: comprehensive error handling
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

// Good: parallel execution when possible
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats()
])
```

### React Best Practices

```typescript
// Good: typed function components
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

// Good: custom hooks
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

// Good: appropriate state updates
const [count, setCount] = useState(0)
// Use functional update when state depends on previous state
setCount(prev => prev + 1)
```

### API Design Standards

```typescript
// REST API conventions
GET    /api/markets              // List all markets
GET    /api/markets/:id          // Get specific market
POST   /api/markets              // Create new market
PUT    /api/markets/:id          // Update market (full)
PATCH  /api/markets/:id          // Update market (partial)
DELETE /api/markets/:id          // Delete market

// Good: consistent response structure
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: { total: number; page: number; limit: number }
}

// Good: schema validation
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
    // Continue with validated data
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({ success: false, error: 'Validation failed', details: error.errors }, { status: 400 })
    }
  }
}
```

### Code Smell Detection

```typescript
// Bad: long functions (>50 lines)
function processMarketData() {
  // 100 lines of code
}

// Good: split into smaller functions
function processMarketData() {
  const validated = validateData()
  const transformed = transformData(validated)
  return saveData(transformed)
}

// Bad: deep nesting (5+ levels)
if (user) {
  if (user.isAdmin) {
    if (market) {
      if (market.isActive) {
        if (hasPermission) {
          // do something
        }
      }
    }
  }
}

// Good: early returns
if (!user) return
if (!user.isAdmin) return
if (!market) return
if (!market.isActive) return
if (!hasPermission) return
// do something

// Bad: magic numbers
if (retryCount > 3) { }
setTimeout(callback, 500)

// Good: named constants
const MAX_RETRIES = 3
const DEBOUNCE_DELAY_MS = 500
```

---

## Error Handling

### Purpose
Robust error handling patterns across TypeScript, Python, and Go, including typed errors, error boundaries, retries, circuit breakers, and user-friendly error messages.

### When to Use
- Designing error types or exception hierarchies for new modules or services
- Adding retry logic or circuit breakers for unreliable external dependencies
- Reviewing API endpoints for missing error handling
- Implementing user-friendly error messages and feedback
- Debugging cascading failures or silent error swallowing

### Core Principles

1. **Fail Fast and Loud** - Expose errors at boundaries
2. **Typed Errors Over String Messages** - Errors are first-class citizens with structure
3. **User Message ≠ Developer Message** - Show friendly text to users, log full context server-side
4. **Never Silently Swallow Errors** - Every `catch` block must handle, rethrow, or log
5. **Errors Are Part of the API Contract** - Document every error code clients may receive

### TypeScript Error Handling

```typescript
// Typed error classes
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

// Result pattern
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

### Python Error Handling

```python
# Custom exception hierarchy
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
        content={"error": {"code": "INTERNAL_ERROR", "message": "An unexpected error occurred"}},
    )
```

### Go Error Handling

```go
// Sentinel errors
var (
    ErrNotFound    = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrConflict     = errors.New("conflict")
)

// Wrap errors with context
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

// Unwrap deterministically at handler level
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

### Exponential Backoff Retry

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

### User-Friendly Error Messages

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

### Error Handling Checklist

- [ ] Every `catch` block handles, rethrows, or logs - no silent swallowing
- [ ] API errors follow standard envelope `{ error: { code, message } }`
- [ ] User-visible messages don't include stack traces or internal details
- [ ] Full error context is logged server-side
- [ ] Custom error classes extend base `AppError` with `code` field
- [ ] Async functions propagate errors to caller - no fire-and-forget
- [ ] Retry logic only retries retriable errors (non-4xx client errors)
- [ ] React components wrapped in `ErrorBoundary` to handle render errors

---

## Related Skills

| Skill | Purpose |
|-------|---------|
| `python-patterns` | Python idiomatic patterns and best practices |
| `golang-patterns` | Go idiomatic patterns and best practices |
| `rust-patterns` | Rust idiomatic patterns and best practices |
| `kotlin-patterns` | Kotlin idiomatic patterns and best practices |
| `cpp-coding-standards` | C++ coding standards and best practices |
| `tdd-workflow` | Test-driven development workflow |
| `security-review` | Security review checklist |

---

## Autonomous Agents and Loop Systems

### autonomous-agent-harness

**Purpose**: Transform Claude Code into a fully autonomous agent system

**Core features**:
- Persistent memory storage
- Scheduled task execution (like cron)
- Remote agent triggering (Dispatch)
- Computer control (Computer Use)
- Task queue management

**When to Use**:
- When users want continuous autonomous operation or scheduled tasks
- When building a personal AI assistant with cross-session memory
- When needing to replicate Hermes, AutoGPT and other autonomous agent framework features
- When needing computer control combined with scheduled execution

**Architecture components**:
| Component | Description |
|-----------|-------------|
| Crons | Scheduled task execution |
| Dispatch | Remote agent triggering |
| Memory | Persistent memory storage |
| Computer Use | Browser and desktop control |

**Hermes to ECC mapping**:
| Hermes | ECC Equivalent |
|--------|----------------|
| Gateway/Router | Claude Code dispatch + crons |
| Memory System | Claude memory + MCP memory server |
| Tool Registry | MCP servers |
| Orchestration | ECC skills + agents |
| Computer Use | computer-use MCP |

---

### autonomous-loops

**Purpose**: Autonomous loop patterns and architecture - from simple sequential pipelines to RFC-driven multi-agent DAG systems

**Loop pattern spectrum**:
| Pattern | Complexity | Applicable Scenarios |
|---------|------------|---------------------|
| Sequential Pipeline | Low | Daily development steps, scripted workflows |
| NanoClaw REPL | Low | Interactive persistent sessions |
| Infinite Agentic Loop | Medium | Parallel content generation, spec-driven work |
| Continuous Claude PR Loop | Medium | Multi-day iterative projects, CI gates |
| De-Sloppify Pattern | Additive | Quality cleanup after any Implementer step |
| Ralphinho / RFC-Driven DAG | High | Large features, multi-unit parallel work with merge queues |

**When to Use**:
- Setting up autonomous development workflows
- Selecting appropriate loop architecture
- Building CI/CD-style continuous development pipelines
- Running parallel agents with merge coordination

**Key patterns**:

1. **Sequential Pipeline** - Simple `claude -p` pipeline
2. **NanoClaw REPL** - Built-in persistent loop, session-aware
3. **Infinite Agentic Loop** - Spec-driven parallel generation
4. **Continuous Claude PR Loop** - Production-grade shell scripts, auto-create PRs, wait for CI, auto-merge
5. **De-Sloppify Pattern** - Add dedicated cleanup/refactor steps
6. **Ralphinho** - RFC-driven multi-agent DAG orchestration

