# 框架

本部分涵盖主要Web框架的开发模式、Melhores-Práticas和核心概念。

---

## Django

### 用途说明
Django开发模式提供了可扩展、可维护的生产级Django应用程序架构模式，包括REST API设计、ORMMelhores-Práticas、缓存、信号和中间件。

### 使用时机
- 构建Django Web应用程序
- 设计Django REST Framework API
- 使用Django ORM和模型
- 设置Django项目结构
- 实现缓存、信号、中间件

### 核心概念

**1. 项目结构**
推荐使用`config/`目录进行设置分离。

**2. 模型设计**
使用自定义QuerySet和Manager实现可复用查询逻辑。

**3. Django REST Framework模式**
使用序列化器、视图集和路由组织API。

**4. 服务层**
将业务逻辑从视图中分离到服务层。

**5. 缓存策略**
使用视图级缓存、模板片段缓存和低级缓存。

### 使用示例

```python
# 模型Melhores-Práticas
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

# 自定义QuerySet
class ProductQuerySet(models.QuerySet):
    def active(self):
        return self.filter(is_active=True)

    def in_stock(self):
        return self.filter(stock__gt=0)

# 服务层
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

### 用途说明
Laravel开发模式提供了可扩展、可维护的生产级Laravel应用程序架构模式，包括路由、控制器、服务层、队列、事件、缓存和API资源。

### 使用时机
- 构建Laravel Web应用程序或API
- 组织控制器、服务和领域逻辑
- 使用Eloquent模型和关系
- 使用资源和分页设计API
- 添加队列、事件、缓存和后台作业

### 核心概念

**1. 控制器 -> 服务 -> Actions**
保持控制器薄，将协调逻辑放在服务中，单一用途逻辑放在actions中。

**2. 路由模型绑定**
使用作用域绑定保持路由可预测。

**3. Eloquent模型**
使用类型提示、casts和scopes保持领域逻辑一致。

**4. 表单请求验证**
将验证逻辑放在表单请求中，转换为DTO。

**5. API资源**
使用API资源保持响应一致。

### 使用示例

```php
// Action保持单一职责
final class CreateOrderAction
{
    public function __construct(private OrderRepository $orders) {}

    public function handle(CreateOrderData $data): Order
    {
        return $this->orders->create($data);
    }
}

// 控制器使用Action
final class OrdersController extends Controller
{
    public function __construct(private CreateOrderAction $createOrder) {}

    public function store(StoreOrderRequest $request): JsonResponse
    {
        $order = $this->createOrder->handle($request->toDto());
        return response()->json(['success' => true, 'data' => OrderResource::make($order)], 201);
    }
}

// Eloquent模型
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

### 用途说明
NestJS开发模式提供了模块化TypeScript后端的生产级模式，包括模块、控制器、提供者、DTO验证、守卫、拦截器和配置。

### 使用时机
- 构建NestJS API或服务
- 组织模块、控制器和提供者
- 添加DTO验证、守卫、拦截器或异常过滤器
- 配置环境感知设置和数据库集成
- 测试NestJS单元或HTTP端点

### 核心概念

**1. 模块化结构**
将领域代码放在功能模块中，跨领域关注点放在`common/`中。

**2. DTO验证**
使用`class-validator`验证每个请求DTO。

**3. 依赖注入**
NestJS使用依赖注入管理依赖。

**4. 守卫和拦截器**
用于认证、授权和日志记录。

**5. 异常过滤器**
集中处理错误并保持响应格式一致。

### 使用示例

```typescript
// 模块结构
@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}

// DTO验证
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

// 控制器
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get(':id')
  getById(@Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.getById(id);
  }
}

// 全局验证管道
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

### 用途说明
FastAPI模式提供了生产级异步API模式，包括依赖注入、Pydantic请求/响应模型、OpenAPI文档、测试、安全性和生产准备。

### 使用时机
- 构建或审查FastAPI应用
- 拆分路由、schema、依赖和数据库访问
- 编写调用数据库或外部服务的异步端点
- 添加认证、授权、OpenAPI文档、测试或部署设置

### 核心概念

**1. 应用工厂**
使用工厂模式使测试和worker可以使用受控设置构建应用。

**2. Pydantic Schema**
保持请求、更新和响应模型分离。

**3. 依赖注入**
使用依赖注入管理请求作用域资源。

**4. 异步端点**
异步路由处理器中进行I/O操作时使用异步库。

**5. 错误处理**
集中化领域异常，保持响应形状稳定。

### 使用示例

```python
# 应用工厂
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

# 依赖注入
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

### 用途说明
Spring Boot开发模式提供了可扩展、生产级服务的架构和API模式，包括REST API设计、分层服务、数据访问、缓存和异步处理。

### 使用时机
- 使用Spring MVC或WebFlux构建REST API
- 组织控制器 -> 服务 -> 仓库层
- 配置Spring Data JPA、缓存或异步处理
- 添加验证、异常处理或分页
- 设置dev/staging/production环境配置文件

### 核心概念

**1. REST API结构**
使用`@RestController`和`@RequestMapping`组织端点。

**2. 仓储模式**
使用Spring Data JPA定义数据访问接口。

**3. 服务层事务**
使用`@Transactional`管理事务边界。

**4. DTO和验证**
使用记录类型和Bean验证注解。

**5. 异常处理**
使用`@ControllerAdvice`集中处理异常。

### 使用示例

```java
// REST控制器
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

// 服务层事务
@Service
public class MarketService {
  private final MarketRepository repo;

  @Transactional
  public Market create(CreateMarketRequest request) {
    MarketEntity entity = MarketEntity.from(request);
    return Market.from(repo.save(entity));
  }
}

// DTO和验证
public record CreateMarketRequest(
    @NotBlank @Size(max = 200) String name,
    @NotBlank @Size(max = 2000) String description,
    @NotNull @FutureOrPresent Instant endDate,
    @NotEmpty List<@NotBlank String> categories) {}

// 全局异常处理
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

## 相关技能

| 技能 | 用途 |
|------|------|
| `python-patterns` | Python惯用模式和Melhores-Práticas |
| `django-security` | Django安全Melhores-Práticas |
| `fastapi-patterns` | FastAPI生产级模式 |
| `nestjs-patterns` | NestJS模块化TypeScript后端 |
| `springboot-patterns` | Spring Boot架构模式 |
