# Best Practices

Dieser Abschnitt behandelt Coding-Standards, Fehlerbehandlung und allgemeine Entwicklung-Best-Practices.

---

## Coding-Standards

### Verwendungszweck
Projektuebergreifend anwendbare Baseline-Coding-Konventionen, einschliesslich Benennung, Lesbarkeit, Immutabilitaet und Codequalitaets-Reviews.

### Verwendung
- Start eines neuen Projekts oder Moduls
- Codequalitaets- und Wartbarkeits-Review
- Refactoring bestehenden Codes zur Einhaltung von Konventionen
- Durchsetzung von Konsistenz bei Benennung, Format oder Struktur
- Einrichtung von Linting, Formatierung oder Typpruefungsregeln
- Einarbeitung neuer Mitwirkender in Coding-Konventionen

### Kernprinzipien

**1. Lesbarkeit zuerst**
- Code wird haeufiger gelesen als geschrieben
- Klare Variablen- und Funktionsnamen
- Selbstdokumentierenden Code gegenueber Kommentaren bevorzugen
- Konsistente Formatierung

**2. KISS-Prinzip**
- Einfach halten, nicht ueberdesignen
- Fruehe Optimierung vermeiden
- Verstaendlichkeit > cleverer Code

**3. DRY-Prinzip**
- Gemeinsame Logik in Funktionen extrahieren
- Wiederverwendbare Komponenten erstellen
- Tools ueber Module teilen
- Copy-Paste-Programmierung vermeiden

**4. YAGNI-Prinzip**
- Keine Features bauen die noch nicht gebraucht werden
- Spekulative Generalitaet vermeiden
- Komplexitaet nur bei Bedarf hinzufuegen
- Erst einfach, bei Bedarf refaktorieren

### TypeScript/JavaScript-Standards

```typescript
// Gut: Beschreibende Namen
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000

// Schlecht: Unklare Namen
const q = 'election'
const flag = true
const x = 1000

// Gut: Verb-Nomen-Muster
async function fetchMarketData(marketId: string) { }
function calculateSimilarity(a: number[], b: number[]) { }
function isValidEmail(email: string): boolean { }

// Gut: Immutabilitaet mit Spread-Operator
const updatedUser = { ...user, name: 'New Name' }
const updatedArray = [...items, newItem]

// Schlecht: Niemals direkt modifizieren
user.name = 'New Name'  // Schlecht
items.push(newItem)     // Schlecht

// Gut: Umfassende Fehlerbehandlung
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

// Gut: Parallel wo moeglich
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats()
])
```

### React Best Practices

```typescript
// Gut: Typisierte Funktionskomponenten
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

// Gut: Custom Hook
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

// Gut: Angemessene Zustandsaktualisierung
const [count, setCount] = useState(0)
// Funktionale Aktualisierung verwenden wenn Zustand auf vorherigem basiert
setCount(prev => prev + 1)
```

### API-Design-Standards

```typescript
// REST API-Konventionen
GET    /api/markets              // Alle Maerkte auflisten
GET    /api/markets/:id          // Bestimmten Markt abrufen
POST   /api/markets              // Neuen Markt erstellen
PUT    /api/markets/:id          // Markt aktualisieren (vollstaendig)
PATCH  /api/markets/:id          // Markt aktualisieren (teilweise)
DELETE /api/markets/:id          // Markt loeschen

// Gut: Konsistente Antwortstruktur
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: { total: number; page: number; limit: number }
}

// Gut: Schema-Validierung
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
    // Mit validierten Daten fortfahren
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({ success: false, error: 'Validation failed', details: error.errors }, { status: 400 })
    }
  }
}
```

### Code-Smell-Erkennung

```typescript
// Schlecht: Lange Funktion (>50 Zeilen)
function processMarketData() {
  // 100 Zeilen Code
}

// Gut: In kleinere Funktionen aufgeteilt
function processMarketData() {
  const validated = validateData()
  const transformed = transformData(validated)
  return saveData(transformed)
}

// Schlecht: Tiefe Verschachtelung (5+ Ebenen)
if (user) {
  if (user.isAdmin) {
    if (market) {
      if (market.isActive) {
        if (hasPermission) {
          // Etwas tun
        }
      }
    }
  }
}

// Gut: Fruehe Returns
if (!user) return
if (!user.isAdmin) return
if (!market) return
if (!market.isActive) return
if (!hasPermission) return
// Etwas tun

// Schlecht: Magische Zahlen
if (retryCount > 3) { }
setTimeout(callback, 500)

// Gut: Bennannte Konstanten
const MAX_RETRIES = 3
const DEBOUNCE_DELAY_MS = 500
```

---

## Fehlerbehandlung

### Verwendungszweck
Robuste Fehlerbehandlungsmuster ueber TypeScript, Python und Go, einschliesslich typisierter Fehler, Error Boundaries, Retry, Circuit Breaker und benutzerfreundlicher Fehlermeldungen.

### Verwendung
- Entwerfen von Fehlertypen oder Ausnahmehierarchien fuer neue Module oder Dienste
- Hinzufuegen von Retry-Logik oder Circuit Breaker fuer unzuverlaessige externe Abhaengigkeiten
- Reviewen von API-Endpoints auf fehlende Fehlerbehandlung
- Implementieren von benutzerfreundlichen Fehlermeldungen und Feedback
- Debuggen von Kaskadenfehlern oder stillschweigend geschluckten Fehlern

### Kernprinzipien

1. **Schnell scheitern und laut** - Fehler an Grenzen exponieren
2. **Typisierte Fehler statt String-Meldungen** - Fehler sind erste Klasse, haben Struktur
3. **Benutzermeldung ≠ Entwicklermeldung** - Benutzern freundlichen Text anzeigen, serverseitig vollstaendigen Kontext loggen
4. **Niemals Fehler stillschweigend schlucken** - Jeder `catch`-Block muss behandeln, weiterwerfen oder loggen
5. **Fehler sind Teil des API-Vertrags** - Jeden Fehlercode dokumentieren den der Client erhalten kann

### TypeScript-Fehlerbehandlung

```typescript
// Typisierte Fehlerklasse
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

// Result-Muster
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

// API-Fehlerhandler
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

### Python-Fehlerbehandlung

```python
# Benutzerdefinierte Ausnahmehierarchie
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

# FastAPI globaler Ausnahmehandler
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

### Go-Fehlerbehandlung

```go
// Sentinel-Fehler
var (
    ErrNotFound    = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrConflict     = errors.New("conflict")
)

// Fehler mit Kontext wrap
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

// Auf Handler-Ebene bestimmte Antworten generieren
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

### Exponentielles Backoff-Retry

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

### Benutzerfreundliche Fehlermeldungen

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

### Fehlerbehandlung-Checkliste

- [ ] Jeder `catch`-Block behandelt, weiterwirft oder loggt - kein stillschweigendes Schluucken
- [ ] API-Fehler folgen Standard-Envelope `{ error: { code, message } }`
- [ ] Benutzer-sichtbare Meldungen enthalten keinen Stack-Trace oder interne Details
- [ ] Vollstaendiger Fehlerkontext wird serverseitig geloggt
- [ ] Benutzerdefinierte Fehlerklassen erweitern Basis-`AppError` mit `code`-Feld
- [ ] Asynchrone Funktionen leiten Fehler an Aufrufer weiter - kein Fire-and-forget
- [ ] Retry-Logik wendet nur auf retryfaehige Fehler (keine 4xx-Client-Fehler)
- [ ] React-Komponenten sind in `ErrorBoundary` gewrappt um Renderfehler zu behandeln

---

## Zugehoerige Faehigkeiten

| Faehigkeit | Verwendungszweck |
|------|------|
| `python-patterns` | Python-idiomatische Muster und Best Practices |
| `golang-patterns` | Go-idiomatische Muster und Best Practices |
| `rust-patterns` | Rust-idiomatische Muster und Best Practices |
| `kotlin-patterns` | Kotlin-idiomatische Muster und Best Practices |
| `cpp-coding-standards` | C++-Coding-Standards und Best Practices |
| `tdd-workflow` | Testgetriebene Entwicklungsworkflows |
| `security-review` | Sicherheitsreview-Checkliste |

---

## Autonome Agenten und Loop-Systeme

### autonomous-agent-harness

**Zweck**: Claude Code in ein vollstaendig autonomes Agentensystem verwandeln

**Kernfunktionen**:
- Persistenter Memory-Speicher
- Zeitgesteuerte Aufgabenplanung (aehnlich wie cron)
- Remote-Agent-Ausloesung (Dispatch)
- Computer-Nutzung (Computer Use)
- Aufgaben-Warteschlangenverwaltung

**Verwendung**:
- Wenn Benutzer kontinuierliche autonome Operationen oder zeitgesteuerte Aufgaben moechte
- Wenn ein persoenlicher AI-Assistent ueber Sitzungen hinweg aufgebaut werden muss
- Wenn Hermes, AutoGPT und aehnliche autonome Agent-Framework-Funktionen repliziert werden muessen
- Wenn Computer-Nutzung mit zeitgesteuerter Ausfuehrung kombiniert werden muss

**Architekturkomponenten**:
| Komponente | Beschreibung |
|------|------|
| Crons | Zeitgesteuerte Aufgabenplanung |
| Dispatch | Remote-Agent-Ausloesung |
| Memory | Persistenter Memory-Speicher |
| Computer Use | Browser- und Desktop-Kontrolle |

**Entsprechungen zu Hermes ersetzen**:
| Hermes | ECC-Aequivalent |
|--------|-----------|
| Gateway/Router | Claude Code dispatch + crons |
| Memory System | Claude memory + MCP memory server |
| Tool Registry | MCP servers |
| Orchestration | ECC skills + agents |
| Computer Use | computer-use MCP |

---

### autonomous-loops

**Zweck**: Autonome Loop-Modi und -Architektur - von einfacher sequenzieller Pipeline bis zu RFC-gesteuertem Multi-Agent-DAG-System

**Loop-Modus-Spektrum**:
| Modus | Komplexitaet | Anwendungsfall |
|------|--------|----------|
| Sequential Pipeline | Niedrig | Taegliche Entwicklungsschritte, skriptbasierte Workflows |
| NanoClaw REPL | Niedrig | Interaktive persistente Sitzungen |
| Infinite Agentic Loop | Mittel | Parallele Inhaltsgenerierung, spezifikationsgetriebene Arbeit |
| Continuous Claude PR Loop | Mittel | Multi-Tage-Iterierungsprojekte, CI-Gates |
| De-Sloppify Pattern | Zusatz | Qualitaetsbereinigung nach jedem Implementierungsschritt |
| Ralphinho / RFC-Driven DAG | Hoch | Grosse Features, Multi-Unit-Parallelarbeit mit Merge-Warteschlange |

**Verwendung**:
- Autonome Entwicklungsworkflows einrichten
- Angemessene Loop-Architektur auswaehlen
- CI/CD-aehnliche kontinuierliche Entwicklungspipeline bauen
- Parallele Agenten mit Merge-Koordination ausfuehren

**Schluesselmodi**:

1. **Sequential Pipeline** - Einfache `claude -p` Pipeline
2. **NanoClaw REPL** - Integrierte persistente Schleifen, sitzungsbewusst
3. **Infinite Agentic Loop** - Spezifikationsgetriebene parallele Generierung
4. **Continuous Claude PR Loop** - Produktionsreifes Shell-Skript, automatisch PRs erstellen, auf CI warten, automatisch mergen
5. **De-Sloppify Pattern** - Spezialisierte Bereinigungs/Refactoring-Schritte hinzufuegen
6. **Ralphinho** - RFC-gesteuerte Multi-Agent-DAG-Orchestrierung