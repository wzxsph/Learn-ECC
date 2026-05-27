# Frameworks

Esta seção abrange padrões de desenvolvimento, Melhores-Práticas e conceitos principais para principais frameworks web.

---

## Django

### Propósito
Padrões de desenvolvimento Django fornecem padrões de arquitetura para aplicações Django escaláveis e mantíveis em produção, incluindo design de REST API, ORM Melhores-Práticas, cache, signals e middleware.

### Quando Usar
- Construir aplicações web Django
- Design de Django REST Framework API
- Usar Django ORM e modelos
- Configurar estrutura de projetos Django
- Implementar cache, signals, middleware

### Conceitos Centrais

**1. Estrutura de Projeto**
Recomendado usar diretório `config/` para separação de configurações.

**2. Design de Modelos**
Usar QuerySet e Manager customizados para lógica de queries reutilizável.

**3. Padrões Django REST Framework**
Usar serializadores, viewsets e rotas para organizar API.

**4. Camada de Serviço**
Separar lógica de negócio das views para camada de serviço.

**5. Estratégias de Cache**
Usar cache de nível de view, cache de template fragments e cache de baixo nível.

### Exemplo de Uso

```python
# Model Melhores-Práticas
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

# QuerySet Customizado
class ProductQuerySet(models.QuerySet):
    def active(self):
        return self.filter(is_active=True)

    def in_stock(self):
        return self.filter(stock__gt=0)

# Camada de Serviço
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

### Propósito
Padrões de desenvolvimento Laravel fornecem padrões de arquitetura para aplicações Laravel escaláveis e mantíveis em produção, incluindo rotas, controladores, camada de serviço, filas, eventos, cache e recursos de API.

### Quando Usar
- Construir aplicações web ou API Laravel
- Organizar controladores, serviços e lógica de domínio
- Usar modelos Eloquent e relações
- Usar recursos e paginação para design de API
- Adicionar filas, eventos, cache e jobs em background

### Conceitos Centrais

**1. Controlador -> Serviço -> Ações**
Manter controladores finos, colocar lógica de coordenação em serviços, ações de propósito único em actions.

**2. Route Model Binding**
Usar binding de modelo com escopo para manter rotas previsíveis.

**3. Modelos Eloquent**
Usar type hints, casts e scopes para manter lógica de domínio consistente.

**4. Validação de Form Request**
Colocar lógica de validação em form requests, converter para DTO.

**5. API Resources**
Usar API resources para manter respostas consistentes.

### Exemplo de Uso

```php
// Action mantendo responsabilidade única
final class CreateOrderAction
{
    public function __construct(private OrderRepository $orders) {}

    public function handle(CreateOrderData $data): Order
    {
        return $this->orders->create($data);
    }
}

// Controlador usando Action
final class OrdersController extends Controller
{
    public function __construct(private CreateOrderAction $createOrder) {}

    public function store(StoreOrderRequest $request): JsonResponse
    {
        $order = $this->createOrder->handle($request->toDto());
        return response()->json(['success' => true, 'data' => OrderResource::make($order)], 201);
    }
}

// Modelo Eloquent
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

### Propósito
Padrões de desenvolvimento NestJS fornecem padrões production-ready para backend TypeScript modular, incluindo módulos, controladores, provedores, validação de DTO, guards, interceptadores e configuração.

### Quando Usar
- Construir API ou serviços NestJS
- Organizar módulos, controladores e provedores
- Adicionar validação de DTO, guards, interceptadores ou filtros de exceção
- Configurar configurações sensíveis ao ambiente e integração de banco de dados
- Testar unidades ou endpoints HTTP NestJS

### Conceitos Centrais

**1. Estrutura Modular**
Colocar código de domínio em módulos funcionais, cruzar preocupações de domínio em `common/`.

**2. Validação de DTO**
Usar `class-validator` para validar cada DTO de request.

**3. Injeção de Dependência**
NestJS usa injeção de dependência para gerenciar dependências.

**4. Guards e Interceptadores**
Usados para autenticação, autorização e logging.

**5. Filtros de Exceção**
Tratamento centralizado de erros e manter formato de resposta consistente.

### Exemplo de Uso

```typescript
// Estrutura de Módulo
@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}

// Validação de DTO
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

// Controlador
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get(':id')
  getById(@Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.getById(id);
  }
}

// Pipe de Validação Global
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

### Propósito
Padrões FastAPI fornecem padrões de API assíncronos production-ready, incluindo injeção de dependência, modelos Pydantic request/response, documentação OpenAPI, testes, segurança e preparação para produção.

### Quando Usar
- Construir ou revisar aplicações FastAPI
- Dividir rotas, schema, dependências e acesso a banco de dados
- Escrever endpoints assíncronos que chamam banco de dados ou serviços externos
- Adicionar autenticação, autorização, documentação OpenAPI, testes ou configurações de deploy

### Conceitos Centrais

**1. Factory de Aplicação**
Usar padrão factory para permitir que testes e workers construam aplicação com configurações controladas.

**2. Schema Pydantic**
Manter request, update e response models separados.

**3. Injeção de Dependência**
Usar injeção de dependência para gerenciar recursos com escopo de request.

**4. Endpoints Assíncronos**
Usar bibliotecas assíncronas para operações I/O em handlers de rota assíncrona.

**5. Tratamento de Erros**
Centralizar exceções de domínio, manter forma de resposta estável.

### Exemplo de Uso

```python
# Factory de Aplicação
@asynccontextmanager
async def lifespan(app: FastAPI):
    await init_db()
    yield
    await close_db()

def create_app() -> FastAPI:
    app = FastAPI(title=settings.api_title, version=settings.api_version, lifespan=lifespan)
    app.include_router(users.router, prefix="/api/v1/users", tags=["users"])
    return app

# Schema Pydantic
class UserBase(BaseModel):
    email: EmailStr
    full_name: Annotated[str, Field(min_length=1, max_length=100)]

class UserCreate(UserBase):
    password: Annotated[str, Field(min_length=12, max_length=128)]

class UserResponse(UserBase):
    model_config = ConfigDict(from_attributes=True)
    id: UUID
    created_at: datetime

# Injeção de Dependência
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

### Propósito
Padrões de desenvolvimento Spring Boot fornecem padrões de arquitetura e API para serviços escaláveis e em produção, incluindo design de REST API, serviços em camadas, acesso a dados, cache e processamento assíncrono.

### Quando Usar
- Usar Spring MVC ou WebFlux para construir REST API
- Organizar camadas controlador -> serviço -> repositório
- Configurar Spring Data JPA, cache ou processamento assíncrono
- Adicionar validação, tratamento de exceções ou paginação
- Configurar arquivos de ambiente para dev/staging/production

### Conceitos Centrais

**1. Estrutura REST API**
Usar `@RestController` e `@RequestMapping` para organizar endpoints.

**2. Padrão Repository**
Usar Spring Data JPA para definir interfaces de acesso a dados.

**3. Transações de Camada de Serviço**
Usar `@Transactional` para gerenciar limites de transação.

**4. DTOs e Validação**
Usar tipos record e anotações de validação Bean.

**5. Tratamento de Exceções**
Usar `@ControllerAdvice` para tratamento centralizado de exceções.

### Exemplo de Uso

```java
// REST Controlador
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

// Transação de Camada de Serviço
@Service
public class MarketService {
  private final MarketRepository repo;

  @Transactional
  public Market create(CreateMarketRequest request) {
    MarketEntity entity = MarketEntity.from(request);
    return Market.from(repo.save(entity));
  }
}

// DTOs e Validação
public record CreateMarketRequest(
    @NotBlank @Size(max = 200) String name,
    @NotBlank @Size(max = 2000) String description,
    @NotNull @FutureOrPresent Instant endDate,
    @NotEmpty List<@NotBlank String> categories) {}

// Tratamento Global de Exceções
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

## Habilidades Relacionadas

| Habilidade | Propósito |
|------|------|
| `python-patterns` | Padrões Python idiomáticos e Melhores-Práticas |
| `django-security` | Melhores-Práticas de segurança Django |
| `fastapi-patterns` | Padrões FastAPI production-ready |
| `nestjs-patterns` | Backend TypeScript modular NestJS |
| `springboot-patterns` | Padrões de arquitetura Spring Boot |