# En İyi Uygulamalar

Bu bölüm kodlama standartları, Hata Yönetimi ve genel geliştirme En İyi Uygulamalarını kapsar.

---

## Kodlama Standartları

### Kullanım Açıklaması
Projeler arası uygulanabilir temel kodlama kuralları, Adlandırma, okunabilirlik, değişmezlik ve Kod Kalitesi incelemesini İçerir.

### Kullanım Zamanı
- Yeni proje veya modül başlatırken
- Kod Kalitesi ve bakımlılığı incelerken
- Mevcut kodu yeniden düzenleyip kurallara uydururken
- Adlandırma, biçimlendirme veya yapı tutarlılığını zorunlu kılarken
- Linting, biçimlendirme veya Tür kontrol kuralları ayarlarken
- Yeni katkıda bulunanların kodlama kurallarını dahil ederken

### Temel İlkeler

**1. Okunabilirlik Öncelikli**
- Kod yazmaktan daha çok okunur
- Açık değişken ve fonksiyon Adları
- Yorum yerine kendini belgeleyen kodu tercih edin
- Tutarlı biçimlendirme

**2. KISS İlkesi**
- Basit tutun, aşırı Tasarım yapmayın
- Erken optimizasyondan kaçının
- Anlaşılması kolay > zeki kod

**3. DRY İlkesi**
- Ortak mantığı fonksiyonlara çıkarın
- Yeniden kullanılabilir bileşenler oluşturun
- Modüller arasında araçları paylaşın
- Copy-paste programlamadan kaçının

**4. YAGNI İlkesi**
- Gerekli olmayan özellikler oluşturmayın
- Spekülatif genellikten kaçının
- Yalnızca Gerekli olduğunda karmaşıklık ekleyin
- Basit başlayın, gerekirse yeniden düzenleyin

### TypeScript/JavaScript Standartları

```typescript
// İyi: Tanımlayıcı Ad
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000

// Kötü: Belirsiz Ad
const q = 'election'
const flag = true
const x = 1000

// İyi: Fiil-isim kalıbı
async function fetchMarketData(marketId: string) { }
function calculateSimilarity(a: number[], b: number[]) { }
function isValidEmail(email: string): boolean { }

// İyi: Yayılma operatörü ile değişmezlik
const updatedUser = { ...user, name: 'New Name' }
const updatedArray = [...items, newItem]

// Kötü: Asla doğrudan değiştirmeyin
user.name = 'New Name'  // Kötü
items.push(newItem)     // Kötü

// İyi: Kapsamlı Hata Yönetimi
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

// İyi: Mümkün olduğunca Paralel Yürütme
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats()
])
```

### React En İyi Uygulamaları

```typescript
// İyi: Tür ile fonksiyon bileşeni
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

// İyi: Özel Hook
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

// İyi: Uygun durum güncelleme
const [count, setCount] = useState(0)
// Durum önceki duruma dayandığında fonksiyonel güncelleme kullanın
setCount(prev => prev + 1)
```

### API Tasarım Standartları

```typescript
// REST API kuralları
GET    /api/markets              // Tüm pazarları listele
GET    /api/markets/:id          // Belirli pazarı al
POST   /api/markets              // Yeni pazar oluştur
PUT    /api/markets/:id          // Pazaryı güncelle (tam)
PATCH  /api/markets/:id          // Pazaryı güncelle (kısmi)
DELETE /api/markets/:id          // Pazarı sil

// İyi: Tutarlı yanıt yapısı
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: { total: number; page: number; limit: number }
}

// İyi: Schema Doğrulama
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
    // Doğrulama sonrası verilerle devam et
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({ success: false, error: 'Validation failed', details: error.errors }, { status: 400 })
    }
  }
}
```

### Kod Kokusu Tespiti

```typescript
// Kötü: Uzun fonksiyonlar (>50 satır)
function processMarketData() {
  // 100 satır kod
}

// İyi: Daha küçük Bölünmüş fonksiyonlara ayırın
function processMarketData() {
  const validated = validateData()
  const transformed = transformData(validated)
  return saveData(transformed)
}

// Kötü: Derin iç içe geçme (5+ seviye)
if (user) {
  if (user.isAdmin) {
    if (market) {
      if (market.isActive) {
        if (hasPermission) {
          // Bir şey yap
        }
      }
    }
  }
}

// İyi: Erken Geri Dön
if (!user) return
if (!user.isAdmin) return
if (!market) return
if (!market.isActive) return
if (!hasPermission) return
// Bir şey yap

// Kötü: Sihirli sayılar
if (retryCount > 3) { }
setTimeout(callback, 500)

// İyi: Adlandırılmış sabitler
const MAX_RETRIES = 3
const DEBOUNCE_DELAY_MS = 500
```

---

## Hata Yönetimi

### Kullanım Açıklaması
TypeScript, Python ve Go'da sağlam Hata Yönetimi kalıpları, Tür hataları, hata sınırları, yeniden deneme, devre kesiciler ve kullanıcı dostu hata mesajlarını İçerir.

### Kullanım Zamanı
- Yeni modül veya hizmet için hata Türü veya istisna hiyerarşisi Tasarım ederken
- Güvenilmez harici bağımlılıklar için yeniden deneme mantığı veya devre kesici eklerken
- API uç noktalarında eksik Hata Yönetimi olup olmadığını incelerken
- Kullanıcı dostu hata mesajları ve geri bildirim Gerçekleştirirken
- Basamaklı hataları veya sessiz hata yutmayı ayıklarken

### Temel İlkeler

1. **Hızlı başarısız ve açık** - Sınırlarda hataları ortaya çıkarın
2. **Tür hataları dize mesajlarından iyidir** - Hatalar birinci sınıf vatandaştır, yapıya sahiptir
3. **Kullanıcı mesajı ≠ Geliştirici mesajı** - Kullanıcıya dost metin gösterin, sunucu tarafında tam bağlamı günlüğe kaydedin
4. **Asla sessizce hata yutmayın** - Her `catch` bloğu Zorunlu işlemeli, yeniden fırlatmalı veya günlüğe kaydetmeli
5. **Hatalar API sözleşmesinin bir parçasıdır** - İstemcinin alabileceği her hata kodunu belgeleyin

### TypeScript Hata Yönetimi

```typescript
// Tür hata sınıfları
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

// Result kalıbı
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

// API Hata Yönetimcisi
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

### Python Hata Yönetimi

```python
# Özel istisna hiyerarşisi
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

# FastAPI genel istisna işleyicisi
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

### Go Hata Yönetimi

```go
// Sentinel hataları
var (
    ErrNotFound    = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrConflict     = errors.New("conflict")
)

// Hataları bağlamla sarmala
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

// İşleyici seviyesinde yanıtları aç
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

### Üstel Geri Çekilme Yeniden Denemesi

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

### Kullanıcı Dostu Hata Mesajları

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

### Hata Yönetimi Kontrol Listesi

- [ ] Her `catch` bloğu işleme, yeniden fırlatma veya günlüğe kaydetme - sessiz yutma yok
- [ ] API hataları standart zarfı `{ error: { code, message } }`ı izler
- [ ] Kullanıcıya görünür mesajlar yığın izi veya dahili ayrıntıları içermez
- [ ] Tam hata bağlamı sunucu tarafında günlüğe kaydedilir
- [ ] Özel hata sınıfları `code` alanlı temel `AppError`'ı genişletir
- [ ] Async fonksiyonlar hataları çağırana iletir - fire-and-forget yok
- [ ] Yeniden deneme mantığı yalnızca yeniden denenebilir hataları yeniden dener (4xx istemci hataları değil)
- [ ] React bileşenleri oluşturma hatalarını işlemek için `ErrorBoundary` ile sarılır

---

## İlgili Beceriler

| Beceri | Kullanım |
|------|------|
| `python-patterns` | Python usulü kalıpları ve En İyi Uygulamalar |
| `golang-patterns` | Go usulü kalıpları ve En İyi Uygulamalar |
| `rust-patterns` | Rust usulü kalıpları ve En İyi Uygulamalar |
| `kotlin-patterns` | Kotlin usulü kalıpları ve En İyi Uygulamalar |
| `cpp-coding-standards` | C++ kodlama standartları ve En İyi Uygulamalar |
| `tdd-workflow` | Test Güdümlü Geliştirme iş akışı |
| `security-review` | Güvenlik inceleme kontrol listesi |

---

## Otonom Ajanlar ve Döngü Sistemleri

### autonomous-agent-harness

**Kullanım**: Claude Code'u tamamen otonom bir ajan sistemine dönüştürme

**Çekirdek işlevler**:
- Kalıcı bellek depolama
- Zamanlı Görev zamanlama (cron benzeri)
- Uzaktan ajan tetikleme (Dispatch)
- Bilgisayar kontrolü (Computer Use)
- Görev kuyruğu Yönetimi

**Kullanım Zamanı**:
- Kullanıcı sürekli otonom işlem veya zamanlı Görev istediğinde
- Gerekli oturumlar arası belleğe sahip kişisel AI asistanı oluşturmak
- Gerekli Hermes, AutoGPT gibi Otonom Ajanlar Çerçeveler işlevlerini kopyalamak
- Gerekli bilgisayar kontrolü ve zamanlı yürütmeyi birleştirmek

**Mimari bileşenler**:
| Bileşen | Açıklama |
|------|------|
| Crons | Zamanlı Görev zamanlama |
| Dispatch | Uzaktan ajan tetikleme |
| Memory | Kalıcı bellek depolama |
| Computer Use | Tarayıcı ve masaüstü kontrol |

**Hermes'in ECC karşılıkları**:
| Hermes | ECC Eşdeğeri |
|--------|-----------|
| Gateway/Router | Claude Code dispatch + crons |
| Memory System | Claude memory + MCP memory server |
| Tool Registry | MCP servers |
| Orchestration | ECC skills + agents |
| Computer Use | computer-use MCP |

---

### autonomous-loops

**Kullanım**: Otonom döngü kalıpları ve mimarisi - basit sıralı boru hattından RFC güdümlü çoklu ajan DAG sistemine

**Döngü kalıbı soybilimi**:
| Kalıp | Karmaşıklık | Uygulama Senaryosu |
|------|--------|----------|
| Sequential Pipeline | Düşük | Günlük geliştirme Adımları, betikleştirilmiş iş akışları |
| NanoClaw REPL | Düşük | Etkileşimli kalıcı oturum |
| Infinite Agentic Loop | Orta | Paralel içerik üretimi, spesifikasyon güdümlü çalışma |
| Continuous Claude PR Loop | Orta | Çok günlü iterasyon projeleri, CI kapıları |
| De-Sloppify Pattern | Ek | Herhangi bir Uygulayıcı Adımlar sonrası kalite temizliği |
| Ralphinho / RFC-Driven DAG | Yüksek | Büyük özellikler, çoklu birim paralel çalışma ve birleştirme kuyrukları |

**Kullanım Zamanı**:
- Otonom geliştirme iş akışları kurarken
- Uygun döngü mimarisini seçerken
- CI/CD tarzı sürekli geliştirme boru hatları oluştururken
- Birleştirme koordinasyonu ile paralel ajanlar çalıştırırken

**Çekirdek kalıplar**:

1. **Sequential Pipeline** - Basit `claude -p` boru hattı
2. **NanoClaw REPL** - Yerleşik kalıcı döngü, oturum bilgili
3. **Infinite Agentic Loop** - Spesifikasyon güdümlü paralel üretim
4. **Continuous Claude PR Loop** - Üretim düzeyinde shell betikleri, otomatik PR oluşturma, CI bekleyen, otomatik birleştirme
5. **De-Sloppify Pattern** - Özel temizlik/yeniden düzenleme Adımları ekleme
6. **Ralphinho** - RFC güdümlü çoklu ajan DAG orkestrasyonu

