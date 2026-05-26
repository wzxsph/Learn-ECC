# Frameworks

This section covers development patterns, best practices, and core concepts for major web frameworks.

---

## Django

### Purpose
Django development patterns provide scalable, maintainable production-grade Django application architecture patterns, including REST API design, ORM best practices, caching, signals, and middleware.

### When to Use
- Building Django web applications
- Designing Django REST Framework APIs
- Using Django ORM and models
- Setting up Django project structure
- Implementing caching, signals, middleware

### Core Concepts

**1. Project Structure**
Recommended to use `config/` directory for settings separation.

**2. Model Design**
Use custom QuerySets and Managers for reusable query logic.

**3. Django REST Framework Patterns**
Use serializers, viewsets, and routers to organize APIs.

**4. Service Layer**
Separate business logic from views into services.

**5. Caching Strategies**
Use view-level caching, template fragment caching, and low-level caching.

### Usage Examples

```python
# Model best practices
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

# Custom QuerySet
class ProductQuerySet(models.QuerySet):
    def active(self):
        return self.filter(is_active=True)

    def in_stock(self):
        return self.filter(stock__gt=0)

# Service layer
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

### Purpose
Laravel development patterns provide scalable, maintainable production-grade Laravel application architecture patterns, including routing, controllers, service layer, queues, events, caching, and API resources.

### When to Use
- Building Laravel web applications or APIs
- Organizing controllers, services, and domain logic
- Using Eloquent models and relationships
- Designing APIs with resources and pagination
- Adding queues, events, caching, and background jobs

### Core Concepts

**1. Controller -> Service -> Actions**
Keep controllers thin, put coordination logic in services, single-purpose logic in actions.

**2. Route Model Binding**
Use scoped binding to keep routes predictable.

**3. Eloquent Models**
Use type hints, casts, and scopes to keep domain logic consistent.

**4. Form Request Validation**
Put validation logic in form requests, transform to DTOs.

**5. API Resources**
Use API resources to keep responses consistent.

### Usage Examples

```php
// Actions keep single responsibility
final class CreateOrderAction
{
    public function __construct(private OrderRepository $orders) {}

    public function handle(CreateOrderData $data): Order
    {
        return $this->orders->create($data);
    }
}

// Controller uses Action
final class OrdersController extends Controller
{
    public function __construct(private CreateOrderAction $createOrder) {}

    public function store(StoreOrderRequest $request): JsonResponse
    {
        $order = $this->createOrder->handle($request->toDto());
        return response()->json(['success' => true, 'data' => OrderResource::make($order)], 201);
    }
}

// Eloquent model
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

### Purpose
NestJS development patterns provide modular TypeScript backend production-grade patterns, including modules, controllers, providers, DTO validation, guards, interceptors, and configuration.

### When to Use
- Building NestJS APIs or services
- Organizing modules, controllers, and providers
- Adding DTO validation, guards, interceptors, or exception filters
- Configuring environment-aware settings and database integration
- Testing NestJS units or HTTP endpoints

### Core Concepts

**1. Modular Structure**
Put domain code in feature modules, cross-cutting concerns in `common/`.

**2. DTO Validation**
Use `class-validator` to validate each request DTO.

**3. Dependency Injection**
NestJS uses dependency injection to manage dependencies.

**4. Guards and Interceptors**
Used for authentication, authorization, and logging.

**5. Exception Filters**
Centrally handle errors and keep response format consistent.

### Usage Examples

```typescript
// Module structure
@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}

// DTO validation
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

// Global validation pipe
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

### Purpose
FastAPI patterns provide production-grade async API patterns, including dependency injection, Pydantic request/response models, OpenAPI documentation, testing, security, and production readiness.

### When to Use
- Building or reviewing FastAPI applications
- Splitting routes, schemas, dependencies, and database access
- Writing async endpoints that call databases or external services
- Adding authentication, authorization, OpenAPI documentation, testing, or deployment setup

### Core Concepts

**1. Application Factory**
Use factory pattern to allow tests and workers to build app with controlled settings.

**2. Pydantic Schema**
Keep request, update, and response models separate.

**3. Dependency Injection**
Use dependency injection to manage request-scoped resources.

**4. Async Endpoints**
Use async libraries for I/O operations in async route handlers.

**5. Error Handling**
Centralize domain exceptions, keep response shape stable.

### Usage Examples

```python
# Application factory
@asynccontextmanager
async def lifespan(app: FastAPI):
    await init_db()
    yield
    await close_db()

def create_app() -> FastAPI:
    app = FastAPI(title=settings.api_title, version=settings.api_version, lifespan=lifespan)
    app.include_router(users.router, prefix="/api/v1/users", tags=["users"])
    return app

# Pydantic Schema
class UserBase(BaseModel):
    email: EmailStr
    full_name: Annotated[str, Field(min_length=1, max_length=100)]

class UserCreate(UserBase):
    password: Annotated[str, Field(min_length=12, max_length=128)]

class UserResponse(UserBase):
    model_config = ConfigDict(from_attributes=True)
    id: UUID
    created_at: datetime

# Dependency injection
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

### Purpose
Spring Boot development patterns provide scalable, production-grade service architecture and API patterns, including REST API design, layered services, data access, caching, and async processing.

### When to Use
- Building REST APIs with Spring MVC or WebFlux
- Organizing controller -> service -> repository layers
- Configuring Spring Data JPA, caching, or async processing
- Adding validation, exception handling, or pagination
- Setting up dev/staging/production environment profiles

### Core Concepts

**1. REST API Structure**
Use `@RestController` and `@RequestMapping` to organize endpoints.

**2. Repository Pattern**
Use Spring Data JPA to define data access interfaces.

**3. Service Layer Transactions**
Use `@Transactional` to manage transaction boundaries.

**4. DTOs and Validation**
Use record types and Bean Validation annotations.

**5. Exception Handling**
Use `@ControllerAdvice` for centralized exception handling.

### Usage Examples

```java
// REST controller
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

// Service layer transaction
@Service
public class MarketService {
  private final MarketRepository repo;

  @Transactional
  public Market create(CreateMarketRequest request) {
    MarketEntity entity = MarketEntity.from(request);
    return Market.from(repo.save(entity));
  }
}

// DTO and validation
public record CreateMarketRequest(
    @NotBlank @Size(max = 200) String name,
    @NotBlank @Size(max = 2000) String description,
    @NotNull @FutureOrPresent Instant endDate,
    @NotEmpty List<@NotBlank String> categories) {}

// Global exception handler
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

## Related Skills

| Skill | Purpose |
|-------|---------|
| `python-patterns` | Python idiomatic patterns and best practices |
| `django-security` | Django security best practices |
| `fastapi-patterns` | FastAPI production-grade patterns |
| `nestjs-patterns` | NestJS modular TypeScript backend |
| `springboot-patterns` | Spring Boot architecture patterns |
