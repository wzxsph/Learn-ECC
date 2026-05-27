# Framework

Phần này cover development pattern, best practice và core concept cho các main Web framework.

---

## Django

### Mục đích
Django development pattern cung cấp scalable, maintainable production-grade Django application architecture pattern, bao gồm REST API design, ORM best practice, caching, signal và middleware.

### Khi nào sử dụng
- Build Django Web application
- Design Django REST Framework API
- Sử dụng Django ORM và model
- Setup Django project structure
- Implement caching, signal, middleware

### Core concept

**1. Project Structure**
Recommended dùng `config/` directory cho setup separation.

**2. Model Design**
Sử dụng custom QuerySet và Manager để implement reusable query logic.

**3. Django REST Framework Pattern**
Sử dụng serializer, viewset và router để organize API.

**4. Service Layer**
Tách business logic khỏi view vào service layer.

**5. Caching Strategy**
Sử dụng view-level caching, template fragment caching và low-level caching.

### Usage Example

```python
# Model best practice
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

### Mục đích
Laravel development pattern cung cấp scalable, maintainable production-grade Laravel application architecture pattern, bao gồm route, controller, service layer, queue, event, caching và API resource.

### Khi nào sử dụng
- Build Laravel Web application hoặc API
- Organize controller, service và domain logic
- Sử dụng Eloquent model và relationship
- Design API với resource và pagination
- Thêm queue, event, caching và background job

### Core concept

**1. Controller -> Service -> Actions**
Giữ controller mỏng, đặt coordination logic trong service, single-purpose logic trong actions.

**2. Route Model Binding**
Sử dụng scoped binding để giữ route predictable.

**3. Eloquent Model**
Sử dụng type hint, cast và scope để giữ domain logic consistent.

**4. Form Request Validation**
Đặt validation logic trong form request, convert thành DTO.

**5. API Resource**
Sử dụng API resource để giữ response consistent.

### Usage Example

```php
// Action giữ single responsibility
final class CreateOrderAction
{
    public function __construct(private OrderRepository $orders) {}

    public function handle(CreateOrderData $data): Order
    {
        return $this->orders->create($data);
    }
}

// Controller sử dụng Action
final class OrdersController extends Controller
{
    public function __construct(private CreateOrderAction $createOrder) {}

    public function store(StoreOrderRequest $request): JsonResponse
    {
        $order = $this->createOrder->handle($request->toDto());
        return response()->json(['success' => true, 'data' => OrderResource::make($order)], 201);
    }
}

// Eloquent Model
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

### Mục đích
NestJS development pattern cung cấp production-grade modular TypeScript backend pattern, bao gồm module, controller, provider, DTO validation, guard, interceptor và configuration.

### Khi nào sử dụng
- Build NestJS API hoặc service
- Organize module, controller và provider
- Thêm DTO validation, guard, interceptor hoặc exception filter
- Configure environment-aware setting và database integration
- Test NestJS unit hoặc HTTP endpoint

### Core concept

**1. Modular Structure**
Đặt domain code trong feature module, cross-cutting concern trong `common/`.

**2. DTO Validation**
Sử dụng `class-validator` validate mỗi request DTO.

**3. Dependency Injection**
NestJS sử dụng dependency injection để manage dependency.

**4. Guard và Interceptor**
Dùng cho authentication, authorization và logging.

**5. Exception Filter**
Centralize error handling và giữ response format consistent.

### Usage Example

```typescript
// Module structure
@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}

// DTO Validation
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

// Global Validation Pipe
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

### Mục đích
FastAPI pattern cung cấp production-grade async API pattern, bao gồm dependency injection, Pydantic request/response model, OpenAPI documentation, testing, security và production readiness.

### Khi nào sử dụng
- Build hoặc review FastAPI application
- Tách route, schema, dependency và database access
- Viết async endpoint gọi database hoặc external service
- Thêm authentication, authorization, OpenAPI documentation, testing hoặc deployment setup

### Core concept

**1. App Factory**
Sử dụng factory pattern để test và worker có thể build app với controlled setting.

**2. Pydantic Schema**
Giữ request, update và response model tách biệt.

**3. Dependency Injection**
Sử dụng dependency injection để manage request-scoped resource.

**4. Async Endpoint**
Sử dụng async library khi làm I/O operation trong async route handler.

**5. Error Handling**
Centralize domain exception, giữ response shape stable.

### Usage Example

```python
# App factory
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

# Dependency Injection
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

### Mục đích
Spring Boot development pattern cung cấp scalable, production-grade service architecture và API pattern, bao gồm REST API design, layered service, data access, caching và async processing.

### Khi nào sử dụng
- Build REST API với Spring MVC hoặc WebFlux
- Organize controller -> service -> repository layer
- Configure Spring Data JPA, caching hoặc async processing
- Thêm validation, exception handling hoặc pagination
- Setup dev/staging/production environment profile

### Core concept

**1. REST API Structure**
Sử dụng `@RestController` và `@RequestMapping` để organize endpoint.

**2. Repository Pattern**
Sử dụng Spring Data JPA define data access interface.

**3. Service Layer Transaction**
Sử dụng `@Transactional` manage transaction boundary.

**4. DTO và Validation**
Sử dụng record type và Bean validation annotation.

**5. Exception Handling**
Sử dụng `@ControllerAdvice` centralize exception handling.

### Usage Example

```java
// REST Controller
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

// Service Layer Transaction
@Service
public class MarketService {
  private final MarketRepository repo;

  @Transactional
  public Market create(CreateMarketRequest request) {
    MarketEntity entity = MarketEntity.from(request);
    return Market.from(repo.save(entity));
  }
}

// DTO và Validation
public record CreateMarketRequest(
    @NotBlank @Size(max = 200) String name,
    @NotBlank @Size(max = 2000) String description,
    @NotNull @FutureOrPresent Instant endDate,
    @NotEmpty List<@NotBlank String> categories) {}

// Global Exception Handler
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

## Related Skill

| Skill | Usage |
|------|------|
| `python-patterns` | Python idiomatic pattern và best practice |
| `django-security` | Django security best practice |
| `fastapi-patterns` | FastAPI production-grade pattern |
| `nestjs-patterns` | NestJS modular TypeScript backend |
| `springboot-patterns` | Spring Boot architecture pattern |