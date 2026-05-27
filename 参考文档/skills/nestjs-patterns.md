---
name: nestjs-patterns
description: NestJS 架构模式 — 模块、控制器、提供者、DTO 验证、守卫、拦截器、配置和生产级 TypeScript 后端。
origin: ECC
---

# NestJS 开发模式

适用于模块化 TypeScript 后端的生产级 NestJS 模式。

## 何时激活

- 构建 NestJS API 或服务
- 构建模块、控制器和提供者
- 添加 DTO 验证、守卫、拦截器或异常过滤器
- 配置环境感知设置和数据库集成
- 测试 NestJS 单元或 HTTP 端点

## 项目结构

```text
src/
├── app.module.ts
├── main.ts
├── common/
│   ├── filters/
│   ├── guards/
│   ├── interceptors/
│   └── pipes/
├── config/
│   ├── configuration.ts
│   └── validation.ts
├── modules/
│   ├── auth/
│   │   ├── auth.controller.ts
│   │   ├── auth.module.ts
│   │   ├── auth.service.ts
│   │   ├── dto/
│   │   ├── guards/
│   │   └── strategies/
│   └── users/
│       ├── dto/
│       ├── entities/
│       ├── users.controller.ts
│       ├── users.module.ts
│       └── users.service.ts
└── prisma/ or database/
```

- 将领域代码保持在功能模块内部。
- 将横切过滤器、装饰器、守卫和拦截器放在 `common/` 中。
- 将 DTO 保持在拥有它们的模块附近。

## 引导和全局验证

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule, { bufferLogs: true });

  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      forbidNonWhitelisted: true,
      transform: true,
      transformOptions: { enableImplicitConversion: true },
    }),
  );

  app.useGlobalInterceptors(new ClassSerializerInterceptor(app.get(Reflector)));
  app.useGlobalFilters(new HttpExceptionFilter());

  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

- 在公共 API 上始终启用 `whitelist` 和 `forbidNonWhitelisted`。
- 优先使用一个全局验证管道，而不是在每个路由上重复验证配置。

## 模块、控制器和提供者

```ts
@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get(':id')
  getById(@Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.getById(id);
  }

  @Post()
  create(@Body() dto: CreateUserDto) {
    return this.usersService.create(dto);
  }
}

@Injectable()
export class UsersService {
  constructor(private readonly usersRepo: UsersRepository) {}

  async create(dto: CreateUserDto) {
    return this.usersRepo.create(dto);
  }
}
```

- 控制器应保持精简：解析 HTTP 输入、调用提供者、返回响应 DTO。
- 将业务逻辑放在可注入服务中，而非控制器中。
- 仅导出其他模块真正需要的提供者。

## DTO 和验证

```ts
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
```

- 使用 `class-validator` 验证每个请求 DTO。
- 使用专门的响应 DTO 或序列化器，而不是直接返回 ORM 实体。
- 避免泄漏密码哈希、令牌或审计列等内部字段。

## 身份验证、守卫和请求上下文

```ts
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('admin')
@Get('admin/report')
getAdminReport(@Req() req: AuthenticatedRequest) {
  return this.reportService.getForUser(req.user.id);
}
```

- 除非真正共享，否则将身份验证策略和守卫保持在模块本地。
- 在守卫中编码粗粒度访问规则，然后在服务中进行资源特定授权。
- 对经过身份验证的请求对象使用显式请求类型。

## 异常过滤器和错误形状

```ts
@Catch()
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const response = host.switchToHttp().getResponse<Response>();
    const request = host.switchToHttp().getRequest<Request>();

    if (exception instanceof HttpException) {
      return response.status(exception.getStatus()).json({
        path: request.url,
        error: exception.getResponse(),
      });
    }

    return response.status(500).json({
      path: request.url,
      error: 'Internal server error',
    });
  }
}
```

- 在整个 API 中保持一种一致的错误信封。
- 对预期的客户端错误抛出框架异常；对意外失败进行日志记录并集中包装。

## 配置和环境验证

```ts
ConfigModule.forRoot({
  isGlobal: true,
  load: [configuration],
  validate: validateEnv,
});
```

- 在启动时验证环境，而不是在第一次请求时延迟验证。
- 通过类型化辅助函数或配置服务访问配置。
- 在配置工厂中分隔开发/暂存/生产关注点，而不是在整个功能代码中分支。

## 持久化和事务

- 将仓库/ORM 代码保持在说领域语言的可注入提供者后面。
- 对于 Prisma 或 TypeORM，将事务工作流隔离在拥有工作单元的服务中。
- 不要让控制器直接协调多步骤写入。

## 测试

```ts
describe('UsersController', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleRef = await Test.createTestingModule({
      imports: [UsersModule],
    }).compile();

    app = moduleRef.createNestApplication();
    app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
    await app.init();
  });
});
```

- 使用模拟依赖项隔离测试提供者。
- 为守卫、验证管道和异常过滤器添加请求级测试。
- 在测试中重用与生产相同的中全局管道/过滤器。

## 生产默认值

- 启用结构化日志和请求相关性 ID。
- 在无效环境/配置时终止，而不是部分启动。
- 优先使用 DB/缓存客户端的异步提供者初始化，并进行显式健康检查。
- 将后台作业和事件消费者保留在自己的模块中，而不是在 HTTP 控制器内部。
- 对公共端点明确设置速率限制、身份验证和审计日志。