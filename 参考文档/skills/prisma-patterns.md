---
name: prisma-patterns
description: Prisma ORM 模式 — TypeScript 后端模式、Schema 设计、查询优化、事务、分页，以及关键陷阱（如 updateMany 返回计数而非记录、$transaction 超时、migrate dev 重置数据库、@updatedAt 在批量写入时被忽略、无服务器环境连接耗尽）。
origin: ECC
---

# Prisma 模式

Prisma ORM 在 TypeScript 后端中的生产模式和非显而易见的陷阱。
已在 Prisma 5.x 和 6.x 上测试。部分行为在 Prisma 4 中有所不同。

应用特定版本模式前请先检查 Prisma 版本：

```bash
npx prisma --version
```

Prisma 5 引入了 `relationJoins`，可根据查询策略和配置通过 JOIN 加载关联而非单独查询。`omit` 字段修饰符和 `prisma.$extends` 客户端扩展 API 也已添加。注意：`relationJoins` 在大型 1:N 关系或深层嵌套 `include` 时可能导致行膨胀 — 当关联可能返回多个父行时，请对两种方案进行基准测试。

## 何时激活

- 设计或修改 Prisma schema 模型和关系
- 编写查询、事务或分页逻辑
- 使用 `updateMany`、`deleteMany` 或任何批量操作
- 运行或规划数据库迁移
- 部署到无服务器环境（Vercel、Lambda、Cloudflare Workers）
- 实现软删除或多租户行过滤

## 核心概念

### ID 策略

| 策略 | 使用场景 | 避免场景 |
|---|---|---|
| `@default(cuid())` | 默认选择 — URL 安全、可排序、无冲突 | 外部系统需要序列 ID |
| `@default(uuid())` | 需要与非 Prisma 系统互操作 | 高写入表（随机 UUID 使 B 树索引碎片化） |
| `@default(autoincrement())` | 内部连接表、审计日志 | 面向公众的 ID（暴露记录数量） |

### Schema 默认值

```prisma
model User {
  id        String    @id @default(cuid())
  email     String    @unique  // @unique 已创建索引 — 无需 @@index
  name      String
  role      Role      @default(USER)
  posts     Post[]
  createdAt DateTime  @default(now())
  updatedAt DateTime @updatedAt
  deletedAt DateTime?

  @@index([createdAt])
  @@index([deletedAt, createdAt]) // 复合索引用于软删除 + 排序查询
}
```

- 在每个外键和用于 `WHERE` 或 `ORDER BY` 的列上添加 `@@index`。
- 当软删除是可预见的需要时，提前声明 `deletedAt DateTime?` — 稍后添加需要在线表迁移。
- `updatedAt @updatedAt` 仅在 `update` 和 `upsert` 时由 Prisma 自动设置（参见反模式中的批量更新陷阱）。

### `include` vs `select`

| | `include` | `select` |
|---|---|---|
| 返回 | 所有标量字段 + 指定关联 | 仅指定字段 |
| 使用时机 | 需要大部分字段加关联 | 高负载路径、大表、避免过度获取 |
| 性能 | 在宽表上可能过度获取 | 最小化负载，大数据集上更快 |
| Prisma 5 注意 | 默认使用 JOIN（`relationJoins`） | 相同 |

```ts
// include — 所有列 + 关联
const user = await prisma.user.findUnique({
  where: { id },
  include: { posts: { select: { id: true, title: true } } },
});

// select — 明确的允许列表
const user = await prisma.user.findUnique({
  where: { id },
  select: { id: true, email: true, name: true },
});
```

永远不要从 API 响应中返回原始 Prisma 实体 — 映射到响应 DTO 以控制暴露字段：

```ts
// BAD: 泄漏 passwordHash、deletedAt、内部字段
return await prisma.user.findUniqueOrThrow({ where: { id } });

// GOOD: 明确的 DTO 映射
const user = await prisma.user.findUniqueOrThrow({ where: { id } });
return { id: user.id, name: user.name, email: user.email };
```

### 事务形式选择

| 情况 | 使用 |
|---|---|
| 独立操作，无相互依赖 | 数组形式 |
| 后一步依赖前一步结果 | 交互式形式 |
| 涉及外部调用（邮件、HTTP） | 完全在事务外部 |

```ts
// 数组形式 — 一次往返批量处理
const [user, post] = await prisma.$transaction([
  prisma.user.update({ where: { id }, data: { name } }),
  prisma.post.create({ data: { title, authorId: id } }),
]);

// 交互式形式 — 仅使用 tx 客户端，绝不使用外部 prisma 客户端
const post = await prisma.$transaction(async (tx) => {
  const user = await tx.user.findUniqueOrThrow({ where: { id } });
  if (user.role !== 'ADMIN') throw new Error('Forbidden');
  return tx.post.create({ data: { title, authorId: user.id } });
});
```

### PrismaClient 单例

每个 `PrismaClient` 实例打开自己的连接池。只实例化一次。

```ts
// lib/prisma.ts
import { PrismaClient } from '@prisma/client';

const globalForPrisma = globalThis as unknown as { prisma?: PrismaClient };

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === 'development' ? ['query', 'error'] : ['error'],
  });

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
```

`globalThis` 模式可在热重载（Next.js、nodemon、ts-node-dev）期间防止重复实例。

### N+1 问题

在循环内加载关联会为每一行发出一查询。

```ts
// BAD: N+1 — 每个用户一个额外查询
const users = await prisma.user.findMany();
for (const user of users) {
  const posts = await prisma.post.findMany({ where: { authorId: user.id } });
}

// GOOD: 单个查询
const users = await prisma.user.findMany({ include: { posts: true } });
```

使用 Prisma 5+ `relationJoins` 时，`include` 形式使用单个 JOIN。在大型 1:N 集合上这可能增加结果集大小 — 如果关联可能为每个父返回多行，请对两种方案进行基准测试。

## 代码示例

### 游标分页（适用于 feeds 和大数据集）

```ts
async function getPosts(cursor?: string, limit = 20) {
  const items = await prisma.post.findMany({
    where: { published: true },
    orderBy: [
      { createdAt: 'desc' },
      { id: 'desc' }, // 次要排序防止重复时间戳导致分页不稳定
    ],
    take: limit + 1,
    ...(cursor && { cursor: { id: cursor }, skip: 1 }),
  });

  const hasNextPage = items.length > limit;
  if (hasNextPage) items.pop();

  return { items, nextCursor: hasNextPage ? items[items.length - 1].id : null };
}
```

获取 `limit + 1` 并弹出 — 检测 `hasNextPage` 的经典方式，无需额外 count 查询。始终包含唯一字段（如 `id`）作为次要 `orderBy`，以防止多条记录具有相同时间戳时出现分页不稳定。仅当用户需要跳转到任意页面时使用偏移分页（管理表）。

### 软删除

```ts
// 始终明确过滤 — 不要依赖中间件（隐藏行为，难以调试）
const activeUsers = await prisma.user.findMany({ where: { deletedAt: null } });

await prisma.user.update({ where: { id }, data: { deletedAt: new Date() } });
await prisma.user.update({ where: { id }, data: { deletedAt: null } }); // 恢复
```

### 错误处理

```ts
import { Prisma } from '@prisma/client';

try {
  await prisma.user.create({ data: { email } });
} catch (e) {
  if (e instanceof Prisma.PrismaClientKnownRequestError) {
    if (e.code === 'P2002') throw new ConflictError('Email already exists');
    if (e.code === 'P2025') throw new NotFoundError('Record not found');
    if (e.code === 'P2003') throw new BadRequestError('Referenced record does not exist');
  }
  throw e;
}
```

常见代码：`P2002` 唯一约束冲突 · `P2025` 未找到 · `P2003` 外键冲突。

在服务边界捕获并转换为领域错误。永远不要向 API 消费者暴露原始 Prisma 消息。

### 连接池 — 无服务器

直接将连接参数嵌入 `DATABASE_URL` — 字符串连接在 URL 已包含查询参数时会失效（如 `?schema=public`）：

```bash
# .env — 推荐：将参数嵌入 URL
DATABASE_URL="postgresql://user:pass@host/db?connection_limit=1&pool_timeout=20"

# 使用外部连接池（PgBouncer、Supabase pooler）
DATABASE_URL="postgresql://user:pass@host/db?pgbouncer=true&connection_limit=1"
```

```ts
// Vercel、AWS Lambda 及类似无服务器运行时：每个实例池上限为 1
// connection_limit 和 pool_timeout 通过 DATABASE_URL 控制
const prisma = new PrismaClient();
```

## 反模式

### `updateMany` 返回计数，而非记录

```ts
// BAD: 结果是 { count: 2 } — users[0] 是 undefined
const users = await prisma.user.updateMany({ where: { role: 'GUEST' }, data: { role: 'USER' } });

// GOOD: 先捕获 ID，然后更新，然后仅获取受影响的行
const targets = await prisma.user.findMany({
  where: { role: 'GUEST' },
  select: { id: true },
});
const ids = targets.map((u) => u.id);
await prisma.user.updateMany({ where: { id: { in: ids } }, data: { role: 'USER' } });
const updated = await prisma.user.findMany({ where: { id: { in: ids } } });
```

`deleteMany` 同样适用 — 返回 `{ count: n }`，永远不是删除的行。

### `$transaction` 交互式形式在 5 秒后超时

```ts
// BAD: 事务内外部调用超过默认 5s → "Transaction already closed"
await prisma.$transaction(async (tx) => {
  const user = await tx.user.findUniqueOrThrow({ where: { id } });
  await sendWelcomeEmail(user.email); // 外部调用
  await tx.user.update({ where: { id }, data: { emailSent: true } });
});

// GOOD: 外部调用在事务外部
const user = await prisma.user.findUniqueOrThrow({ where: { id } });
await sendWelcomeEmail(user.email);
await prisma.user.update({ where: { id }, data: { emailSent: true } });

// 仅在批量处理确实需要时才提高超时
await prisma.$transaction(async (tx) => { ... }, { timeout: 30_000 });
```

### `migrate dev` 可能重置数据库

`migrate dev` 检测 schema 漂移，可能提示重置 DB，丢弃所有数据。

```bash
# 共享开发、暂存或生产环境上绝对不要使用
npx prisma migrate dev --name add_column

# 除本地单机开发外 everywhere 安全
npx prisma migrate deploy

# 检查漂移但不应用
npx prisma migrate diff \
  --from-migrations ./prisma/migrations \
  --to-schema-datamodel ./prisma/schema.prisma \
  --shadow-database-url "$SHADOW_DATABASE_URL"
```

### 手动编辑迁移文件会破坏未来部署

Prisma 对每个迁移文件进行校验和检查。应用后编辑会导致 `P3006 checksum mismatch`，在任何已运行原始迁移的环境中。创建新迁移代替。

### 破坏性 schema 更改需要多步骤迁移

在一迁移中向现有列添加 `NOT NULL` 或重命名列会锁定表或丢失数据。使用扩展-收缩策略：

```bash
# 步骤 1：本地创建迁移，然后部署
npx prisma migrate dev --name add_new_column   # 仅本地
npx prisma migrate deploy                       # 暂存 / 生产
```

```ts
// 步骤 2：回填数据（在脚本或迁移作业中运行，不在 shell 中）
await prisma.user.updateMany({ data: { newColumn: derivedValue } });
```

```bash
# 步骤 3：创建 NOT NULL 约束迁移（本地，然后部署）
npx prisma migrate dev --name make_new_column_required  # 仅本地
npx prisma migrate deploy                               # 暂存 / 生产
```

### `@updatedAt` 在 `updateMany` 上不触发

`@updatedAt` 仅在 `update` 和 `upsert` 上自动设置。批量写入会使其保持陈旧。

```ts
// BAD: updatedAt 保持旧值
await prisma.post.updateMany({ where: { authorId }, data: { published: true } });

// GOOD
await prisma.post.updateMany({
  where: { authorId },
  data: { published: true, updatedAt: new Date() },
});
```

### 软删除 + `findUniqueOrThrow` 泄漏已删除记录

`findUniqueOrThrow` 仅在 DB 中不存在该行时抛出 `P2025`。软删除的行仍然存在并被返回，无错误。

`findUniqueOrThrow` 要求 where 条件中有唯一约束字段 — 在 `id` 旁添加 `deletedAt: null` 会破坏类型，因为 `{ id, deletedAt }` 不是复合唯一约束。使用 `findFirstOrThrow` 代替。

```ts
// BAD: 返回软删除的用户
const user = await prisma.user.findUniqueOrThrow({ where: { id } });

// BAD: Prisma 类型错误 — { id, deletedAt } 不是唯一约束
const user = await prisma.user.findUniqueOrThrow({ where: { id, deletedAt: null } });

// GOOD: findFirstOrThrow 支持任意 where 条件
const user = await prisma.user.findFirstOrThrow({ where: { id, deletedAt: null } });
```

### `deleteMany` 没有 `where` 会删除每一行

```ts
// BAD: 静默擦除整个表
await prisma.post.deleteMany();

// GOOD
await prisma.post.deleteMany({ where: { authorId: userId } });
```

## 最佳实践

| 规则 | 原因 |
|---|---|
| CI/CD 中使用 `migrate deploy`，仅本地使用 `migrate dev` | `migrate dev` 在漂移时可能重置 DB |
| 将实体映射到响应 DTO | 防止泄漏内部字段 |
| 在服务边界捕获 `PrismaClientKnownRequestError` | 转换为领域错误 |
| 优先使用 `*OrThrow` 方法而非手动 null 检查 | 自动抛出 P2025；过滤非唯一字段时使用 `findFirstOrThrow` |
| 无服务器环境中 `connection_limit=1` + 外部连接池 | 防止连接耗尽 |
| `deleteMany` 始终提供 `where` | 防止意外清空表 |
| 在 `updateMany` 中手动设置 `updatedAt: new Date()` | `@updatedAt` 跳过批量写入 |

## 相关技能

- `nestjs-patterns` — 集成 Prisma 的 NestJS 服务层
- `postgres-patterns` — PostgreSQL 级别的索引和连接调优
- `database-migrations` — 生产环境多步骤迁移规划
- `backend-patterns` — 通用 API 和服务层设计