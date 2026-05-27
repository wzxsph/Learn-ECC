# Frameworks

Dieser Abschnitt behandelt Entwicklungsmodi, Best Practices und Kernkonzepte fuer die wichtigsten Web-Frameworks.

---

## Django

### Verwendungszweck
Django-Entwicklungsmuster bieten skalierbare, wartbare, produktionsreife Django-Anwendungs-Architekturmuster, einschliesslich REST-API-Design, ORM-Best-Practices, Caching, Signals und Middleware.

### Verwendung
- Django-Webanwendungen bauen
- Django REST Framework API entwerfen
- Django ORM und Modelle verwenden
- Django-Projektstruktur einrichten
- Caching, Signals, Middleware implementieren

### Kernkonzepte

**1. Projektstruktur**
Es wird empfohlen, ein `config/`-Verzeichnis fuer die Trennung von Konfiguration zu verwenden.

**2. Modell-Design**
Benutzerdefinierte QuerySets und Manager fuer wiederverwendbare Abfragelogik verwenden.

**3. Django REST Framework-Muster**
Serializer, Viewsets und Router fuer API-Organisation verwenden.

**4. Service-Schicht**
Geschaeftslogik aus Views in eine Service-Schicht auslagern.

**5. Caching-Strategien**
View-Level-Caching, Template-Fragment-Caching und Low-Level-Caching verwenden.

### Verwendungsbeispiel

```python
# Modell-Best-Practices
class Product(models.Model):
    name = models.CharField(max_length=200)
    slug = models.SlugField(unique=True)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    is_active = models.BooleanField(default=True)

    objects = ProductQuerySet.as_manager()

    class Meta:
        indexes = [
            models.Index(fields=['slug']),
            models.Index(fields=['-created_at']),
        ]

# Benutzerdefiniertes QuerySet
class ProductQuerySet(models.QuerySet):
    def active(self):
        return self.filter(is_active=True)

    def in_stock(self):
        return self.filter(stock__gt=0)

# Service-Schicht
class OrderService:
    @staticmethod
    @transaction.atomic
    def create_order(user, cart: Cart) -> Order:
        order = Order.objects.create(user=user, total_price=cart.total_price)
        for item in cart.items.all():
            OrderItem.objects.create(order=order, product=item.product, quantity=item.quantity)
        cart.items.all().delete()
        return order
```

---

## Laravel

### Verwendungszweck
Laravel-Entwicklungsmuster bieten skalierbare, wartbare, produktionsreife Laravel-Anwendungs-Architekturmuster, einschliesslich Routing, Controller, Service-Schicht, Queues, Events, Caching und API-Resources.

### Verwendung
- Laravel-Webanwendungen oder APIs bauen
- Controller, Services und Domain-Logik organisieren
- Eloquent-Modelle und Beziehungen verwenden
- Resources und Pagination fuer API-Design verwenden
- Queues, Events, Caching und Hintergrundjobs hinzufuegen

### Kernkonzepte

**1. Controller -> Service -> Actions**
Controller duenn halten, Koordinationslogik in Services, einzweckige Logik in Actions.

**2. Routing-Modell-Binding**
Scoped Binding verwenden, um Routes vorhersehbar zu halten.

**3. Eloquent-Modelle**
Type-Hints, Casts und Scopes verwenden, um Domain-Logik konsistent zu halten.

**4. Form-Request-Validierung**
Validierungslogik in Form-Requests ablegen und in DTOs konvertieren.

**5. API-Resources**
API-Resources verwenden, um Antworten konsistent zu halten.

### Verwendungsbeispiel

```php
// Action behält eine einzelne Verantwortung
final class CreateOrderAction
{
    public function __construct(private OrderRepository $orders) {}

    public function handle(CreateOrderData $data): Order
    {
        return $this->orders->create($data);
    }
}

// Controller verwendet Action
final class OrdersController extends Controller
{
    public function __construct(private CreateOrderAction $createOrder) {}

    public function store(StoreOrderRequest $request): JsonResponse
    {
        $order = $this->createOrder->handle($request->toDto());
        return response()->json(['success' => true, 'data' => OrderResource::make($order)], 201);
    }
}

// Eloquent-Modell
final class Project extends Model
{
    use HasFactory;

    protected $fillable = ['name', 'owner_id', 'status'];
    protected $casts = ['status' => ProjectStatus::class, 'archived_at' => 'datetime'];

    public function scopeActive(Builder $query): Builder
    {
        return $query->whereNull('archived_at');
    }
}
```

---

## NestJS

### Verwendungszweck
NestJS-Entwicklungsmuster bieten modulare TypeScript-Backend-Produktionsmuster, einschliesslich Module, Controller, Provider, DTO-Validierung, Guards, Interceptors und Konfiguration.

### Verwendung
- NestJS APIs oder Services bauen
- Module, Controller und Provider organisieren
- DTO-Validierung, Guards, Interceptors oder Exception-Filter hinzufuegen
- Umgebungsbewusste Einstellungen und Datenbankintegration konfigurieren
- NestJS-Unit- oder HTTP-Endpunkte testen

### Kernkonzepte

**1. Modulare Struktur**
Domain-Code in Feature-Modulen ablegen, domingenuebergreifende Anliegen in `common/`.

**2. DTO-Validierung**
`class-validator` verwenden, um jeden Request-DTO zu validieren.

**3. Abhaengigkeitsinjektion**
NestJS verwendet Abhaengigkeitsinjektion zur Verwaltung von Abhaengigkeiten.

**4. Guards und Interceptors**
Verwendet fuer Authentifizierung, Autorisierung und Logging.

**5. Exception-Filter**
Zentralisierte Fehlerbehandlung, um Antwortformat konsistent zu halten.

### Verwendungsbeispiel

```typescript
// Modulstruktur
@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}

// DTO-Validierung
export class CreateUserDto {
  @IsEmail()
  email!: string;

  @IsString()
  @Length(2, 80)
  name!: string;

  @IsOptional()
  @IsEnum(UserRole)
  role?: UserRole;
}

// Controller
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get(':id')
  getById(@Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.getById(id);
  }
}

// Globale Validierungspipeline
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,
    forbidNonWhitelisted: true,
    transform: true,
  }),
);
```

---

## FastAPI

### Verwendungszweck
FastAPI-Muster bieten produktionsreife asynchrone API-Muster, einschliesslich Abhaengigkeitsinjektion, Pydantic Request/Response-Modellen, OpenAPI-Dokumentation, Tests, Sicherheit und Produktionstauglichkeit.

### Verwendung
- FastAPI-Anwendungen bauen oder ueberpruefen
- Routes, Schemas, Abhaengigkeiten und Datenbankzugriff aufteilen
- Asynchrone Endpunkte schreiben, die Datenbanken oder externe Services aufrufen
- Authentifizierung, Autorisierung, OpenAPI-Dokumentation, Tests oder Deployment-Einstellungen hinzufuegen

### Kernkonzepte

**1. App-Factory**
Factory-Muster verwenden, damit Tests und Worker mit kontrollierter Konfiguration Apps bauen koennen.

**2. Pydantic-Schema**
Request-, Update- und Response-Modelle getrennt halten.

**3. Abhaengigkeitsinjektion**
Abhaengigkeitsinjektion verwenden, um Request-Scope-Ressourcen zu verwalten.

**4. Asynchrone Endpunkte**
Asynchrone Bibliotheken verwenden, wenn I/O-Operationen in asynchronen Route-Handlern durchgefuehrt werden.

**5. Fehlerbehandlung**
Domain-Ausnahmen zentralisieren, Antwortform konsistent halten.

### Verwendungsbeispiel

```python
# App-Factory
@asynccontextmanager
async def lifespan(app: FastAPI):
    await init_db()
    yield
    await close_db()

def create_app() -> FastAPI:
    app = FastAPI(title=settings.api_title, version=settings.api_version, lifespan=lifespan)
    app.include_router(users.router, prefix="/api/v1/users", tags=["users"])
    return app

# Pydantic-Schema
class UserBase(BaseModel):
    email: EmailStr
    full_name: Annotated[str, Field(min_length=1, max_length=100)]

class UserCreate(UserBase):
    password: Annotated[str, Field(min_length=12, max_length=128)]

class UserResponse(UserBase):
    model_config = ConfigDict(from_attributes=True)
    id: UUID
    created_at: datetime

# Abhaengigkeitsinjektion
async def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: AsyncSession = Depends(get_db),
) -> User:
    payload = decode_token(token)
    user = await db.get(User, UUID(payload["sub"]))
    if user is None:
        raise HTTPException(status_code=401, detail="Invalid token")
    return user
```

---

## Spring Boot

### Verwendungszweck
Spring Boot-Entwicklungsmuster bieten skalierbare, produktionsreife Service-Architektur- und API-Muster, einschliesslich REST-API-Design, geschichtete Services, Datenzugriff, Caching und asynchrone Verarbeitung.

### Verwendung
- REST APIs mit Spring MVC oder WebFlux bauen
- Controller -> Service -> Repository-Schichten organisieren
- Spring Data JPA, Caching oder asynchrone Verarbeitung konfigurieren
- Validierung, Exception-Handling oder Pagination hinzufuegen
- dev/staging/production-Umgebungskonfigurationsdateien einrichten

### Kernkonzepte

**1. REST-API-Struktur**
`@RestController` und `@RequestMapping` zur Endpunkt-Organisation verwenden.

**2. Repository-Muster**
Spring Data JPA verwenden, um Datenzugriffsschnittstellen zu definieren.

**3. Service-Schicht-Transaktionen**
`@Transactional` verwenden, um Transaktionsgrenzen zu verwalten.

**4. DTOs und Validierung**
 Record-Typen und Bean-Validierungsannotationen verwenden.

**5. Exception-Handling**
`@ControllerAdvice` verwenden, um Exceptions zentral zu behandeln.

### Verwendungsbeispiel

```java
// REST-Controller
@RestController
@RequestMapping("/api/markets")
@Validated
class MarketController {
  private final MarketService marketService;

  @GetMapping
  ResponseEntity<Page<MarketResponse>> list(
      @RequestParam(defaultValue = "0") int page,
      @RequestParam(defaultValue = "20") int size) {
    Page<Market> markets = marketService.list(PageRequest.of(page, size));
    return ResponseEntity.ok(markets.map(MarketResponse::from));
  }

  @PostMapping
  ResponseEntity<MarketResponse> create(@Valid @RequestBody CreateMarketRequest request) {
    Market market = marketService.create(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(MarketResponse.from(market));
  }
}

// Service-Schicht-Transaktion
@Service
public class MarketService {
  private final MarketRepository repo;

  @Transactional
  public Market create(CreateMarketRequest request) {
    MarketEntity entity = MarketEntity.from(request);
    return Market.from(repo.save(entity));
  }
}

// DTOs und Validierung
public record CreateMarketRequest(
    @NotBlank @Size(max = 200) String name,
    @NotBlank @Size(max = 2000) String description,
    @NotNull @FutureOrPresent Instant endDate,
    @NotEmpty List<@NotBlank String> categories) {}

// Globaler Exception-Handler
@ControllerAdvice
class GlobalExceptionHandler {
  @ExceptionHandler(MethodArgumentNotValidException.class)
  ResponseEntity<ApiError> handleValidation(MethodArgumentNotValidException ex) {
    String message = ex.getBindingResult().getFieldErrors().stream()
        .map(e -> e.getField() + ": " + e.getDefaultMessage())
        .collect(Collectors.joining(", "));
    return ResponseEntity.badRequest().body(ApiError.validation(message));
  }
}
```

---

## Zugehoerige Faehigkeiten

| Faehigkeit | Verwendungszweck |
|------|------|
| `python-patterns` | Python-idiomatische Muster und Best-Practices |
| `django-security` | Django-Sicherheits-Best-Practices |
| `fastapi-patterns` | FastAPI-Produktionsmuster |
| `nestjs-patterns` | NestJS modulares TypeScript-Backend |
| `springboot-patterns` | Spring Boot-Architekturmuster |