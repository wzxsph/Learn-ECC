---
name: database-migrations
description: 数据库迁移最佳实践，涵盖Schema变更、数据迁移、回滚及PostgreSQL、MySQL和常见ORM（Prisma、Drizzle、Kysely、Django、TypeORM、golang-migrate）的零停机部署。
origin: ECC
---

# 数据库迁移模式

面向生产系统的安全、可逆的数据库Schema变更。

## 激活条件

- 创建或修改数据库表
- 添加/删除列或索引
- 执行数据迁移（回填、转换）
- 规划零停机Schema变更
- 为新项目设置迁移工具

## 核心原则

1. **每次变更都是迁移** — 永远不要手动修改生产数据库
2. **生产环境中迁移只能向前** — 回滚使用新的前向迁移
3. **Schema和数据迁移分离** — 永远不要在一个迁移中混合DDL和DML
4. **用生产规模数据测试迁移** — 在100行上能用的迁移可能在1000万行上导致锁表
5. **迁移一旦部署就不可变** — 永远不要编辑已在生产环境运行过的迁移

## 迁移安全检查清单

应用任何迁移之前：

- [ ] 迁移有UP和DOWN（或明确标记为不可逆）
- [ ] 大表上没有全表锁（使用并发操作）
- [ ] 新列有默认值或可为NULL（永远不要在无默认值的情况下添加NOT NULL）
- [ ] 索引并发创建（对现有表不要在CREATE TABLE内联创建）
- [ ] 数据回填与Schema变更分离为不同的迁移
- [ ] 已用生产数据副本测试
- [ ] 已记录回滚计划

## PostgreSQL 模式

### 安全添加列

```sql
-- 好：可空列，无锁
ALTER TABLE users ADD COLUMN avatar_url TEXT;

-- 好：有默认值的列（Postgres 11+即时完成，无需重写）
ALTER TABLE users ADD COLUMN is_active BOOLEAN NOT NULL DEFAULT true;

-- 坏：在已有表上无默认值添加NOT NULL（需要全表重写）
ALTER TABLE users ADD COLUMN role TEXT NOT NULL;
-- 这会锁表并重写每一行
```

### 零停机添加索引

```sql
-- 坏：在大型表上阻塞写入
CREATE INDEX idx_users_email ON users (email);

-- 好：非阻塞，允许并发写入
CREATE INDEX CONCURRENTLY idx_users_email ON users (email);

-- 注意：CONCURRENTLY不能在事务块内运行
-- 大多数迁移工具需要特殊处理
```

### 重命名列（零停机）

永远不要在生产环境直接重命名。使用展开-收缩模式：

```sql
-- 步骤1：添加新列（迁移001）
ALTER TABLE users ADD COLUMN display_name TEXT;

-- 步骤2：回填数据（迁移002，数据迁移）
UPDATE users SET display_name = username WHERE display_name IS NULL;

-- 步骤3：更新应用程序代码读写两列
-- 部署应用程序变更

-- 步骤4：停止写入旧列，删除它（迁移003）
ALTER TABLE users DROP COLUMN username;
```

### 安全删除列

```sql
-- 步骤1：移除所有应用程序对该列的引用
-- 步骤2：部署不含该列引用的应用程序
-- 步骤3：在下一个迁移中删除列
ALTER TABLE orders DROP COLUMN legacy_status;

-- Django用法：使用SeparateDatabaseAndState从模型中移除字段
-- 而不生成DROP COLUMN（然后在下一个迁移中删除）
```

### 大型数据迁移

```sql
-- 坏：在一个事务中更新所有行（锁表）
UPDATE users SET normalized_email = LOWER(email);

-- 好：批量更新带进度
DO $$
DECLARE
  batch_size INT := 10000;
  rows_updated INT;
BEGIN
  LOOP
    UPDATE users
    SET normalized_email = LOWER(email)
    WHERE id IN (
      SELECT id FROM users
      WHERE normalized_email IS NULL
      LIMIT batch_size
      FOR UPDATE SKIP LOCKED
    );
    GET DIAGNOSTICS rows_updated = ROW_COUNT;
    RAISE NOTICE 'Updated % rows', rows_updated;
    EXIT WHEN rows_updated = 0;
    COMMIT;
  END LOOP;
END $$;
```

## Prisma (TypeScript/Node.js)

### 工作流程

```bash
# 从Schema变更创建迁移
npx prisma migrate dev --name add_user_avatar

# 在生产环境应用待定迁移
npx prisma migrate deploy

# 重置数据库（仅限开发）
npx prisma migrate reset

# Schema变更后生成客户端
npx prisma generate
```

### Schema示例

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  avatarUrl String?  @map("avatar_url")
  createdAt DateTime @default(now()) @map("created_at")
  updatedAt DateTime @updatedAt @map("updated_at")
  orders    Order[]

  @@map("users")
  @@index([email])
}
```

### 自定义SQL迁移

用于Prisma无法表达的操作（并发索引、数据回填）：

```bash
# 创建空迁移，然后手动编辑SQL
npx prisma migrate dev --create-only --name add_email_index
```

```sql
-- migrations/20240115_add_email_index/migration.sql
-- Prisma无法生成CONCURRENTLY，所以我们手动编写
CREATE INDEX CONCURRENTLY IF NOT EXISTS idx_users_email ON users (email);
```

## Drizzle (TypeScript/Node.js)

### 工作流程

```bash
# 从Schema变更生成迁移
npx drizzle-kit generate

# 应用迁移
npx drizzle-kit migrate

# 直接推送Schema（仅限开发，不生成迁移文件）
npx drizzle-kit push
```

### Schema示例

```typescript
import { pgTable, text, timestamp, uuid, boolean } from "drizzle-orm/pg-core";

export const users = pgTable("users", {
  id: uuid("id").primaryKey().defaultRandom(),
  email: text("email").notNull().unique(),
  name: text("name"),
  isActive: boolean("is_active").notNull().default(true),
  createdAt: timestamp("created_at").notNull().defaultNow(),
  updatedAt: timestamp("updated_at").notNull().defaultNow(),
});
```

## Kysely (TypeScript/Node.js)

### 工作流程（kysely-ctl）

```bash
# 初始化配置文件（kysely.config.ts）
kysely init

# 创建新的迁移文件
kysely migrate make add_user_avatar

# 应用所有待定迁移
kysely migrate latest

# 回滚上一个迁移
kysely migrate down

# 显示迁移状态
kysely migrate list
```

### 迁移文件

```typescript
// migrations/2024_01_15_001_create_user_profile.ts
import { type Kysely, sql } from 'kysely'

// 重要：始终使用Kysely<any>，不要用你的类型化DB接口。
// 迁移是时间冻结的，不应依赖当前Schema类型。
export async function up(db: Kysely<any>): Promise<void> {
  await db.schema
    .createTable('user_profile')
    .addColumn('id', 'serial', (col) => col.primaryKey())
    .addColumn('email', 'varchar(255)', (col) => col.notNull().unique())
    .addColumn('avatar_url', 'text')
    .addColumn('created_at', 'timestamp', (col) =>
      col.defaultTo(sql`now()`).notNull()
    )
    .execute()

  await db.schema
    .createIndex('idx_user_profile_avatar')
    .on('user_profile')
    .column('avatar_url')
    .execute()
}

export async function down(db: Kysely<any>): Promise<void> {
  await db.schema.dropTable('user_profile').execute()
}
```

### 程序化迁移器

```typescript
import { Migrator, FileMigrationProvider } from 'kysely'
import { promises as fs } from 'fs'
import * as path from 'path'
// 仅ESM — CJS可直接使用__dirname
import { fileURLToPath } from 'url'
const migrationFolder = path.join(
  path.dirname(fileURLToPath(import.meta.url)),
  './migrations',
)

// `db`是你的Kysely<any>数据库实例
const migrator = new Migrator({
  db,
  provider: new FileMigrationProvider({
    fs,
    path,
    migrationFolder,
  }),
  // 警告：仅在开发中启用。禁用时间戳排序验证，
  // 可能导致环境间Schema漂移。
  // allowUnorderedMigrations: true,
})

const { error, results } = await migrator.migrateToLatest()

results?.forEach((it) => {
  if (it.status === 'Success') {
    console.log(`migration "${it.migrationName}" executed successfully`)
  } else if (it.status === 'Error') {
    console.error(`failed to execute migration "${it.migrationName}"`)
  }
})

if (error) {
  console.error('migration failed', error)
  process.exit(1)
}
```

## Django (Python)

### 工作流程

```bash
# 从模型变更生成迁移
python manage.py makemigrations

# 应用迁移
python manage.py migrate

# 显示迁移状态
python manage.py showmigrations

# 为自定义SQL生成空迁移
python manage.py makemigrations --empty app_name -n description
```

### 数据迁移

```python
from django.db import migrations

def backfill_display_names(apps, schema_editor):
    User = apps.get_model("accounts", "User")
    batch_size = 5000
    users = User.objects.filter(display_name="")
    while users.exists():
        batch = list(users[:batch_size])
        for user in batch:
            user.display_name = user.username
        User.objects.bulk_update(batch, ["display_name"], batch_size=batch_size)

def reverse_backfill(apps, schema_editor):
    pass  # 数据迁移，无需反向

class Migration(migrations.Migration):
    dependencies = [("accounts", "0015_add_display_name")]

    operations = [
        migrations.RunPython(backfill_display_names, reverse_backfill),
    ]
```

### SeparateDatabaseAndState

从Django模型中移除列但不立即从数据库删除：

```python
class Migration(migrations.Migration):
    operations = [
        migrations.SeparateDatabaseAndState(
            state_operations=[
                migrations.RemoveField(model_name="user", name="legacy_field"),
            ],
            database_operations=[],  # 暂时不碰DB
        ),
    ]
```

## golang-migrate (Go)

### 工作流程

```bash
# 创建迁移对
migrate create -ext sql -dir migrations -seq add_user_avatar

# 应用所有待定迁移
migrate -path migrations -database "$DATABASE_URL" up

# 回滚上一个迁移
migrate -path migrations -database "$DATABASE_URL" down 1

# 强制版本（修复脏状态）
migrate -path migrations -database "$DATABASE_URL" force VERSION
```

### 迁移文件

```sql
-- migrations/000003_add_user_avatar.up.sql
ALTER TABLE users ADD COLUMN avatar_url TEXT;
CREATE INDEX CONCURRENTLY idx_users_avatar ON users (avatar_url) WHERE avatar_url IS NOT NULL;

-- migrations/000003_add_user_avatar.down.sql
DROP INDEX IF EXISTS idx_users_avatar;
ALTER TABLE users DROP COLUMN IF EXISTS avatar_url;
```

## 零停机迁移策略

对于关键的生产变更，遵循展开-收缩模式：

```
第一阶段：展开
  - 添加新列/表（可空或带默认值）
  - 部署：应用同时写入新旧
  - 回填现有数据

第二阶段：迁移
  - 部署：应用从新读取，同时写入新旧
  - 验证数据一致性

第三阶段：收缩
  - 部署：应用仅使用新的
  - 在单独的迁移中删除旧列/表
```

### 时间线示例

```
第一天：迁移添加新status列（可空）
第一天：部署app v2 — 同时写入status和new_status
第二天：运行现有行的回填迁移
第三天：部署app v3 — 仅从new_status读取
第七天：迁移删除旧的status列
```

## 反模式

| 反模式 | 失败原因 | 更好的方法 |
|-------------|-------------|-----------------|
| 生产环境手动SQL | 无审计跟踪，不可重复 | 始终使用迁移文件 |
| 编辑已部署的迁移 | 导致环境间漂移 | 创建新迁移 |
| 无默认值的NOT NULL | 锁表，重写所有行 | 添加可空，回填，然后添加约束 |
| 大表内联索引 | 构建期间阻塞写入 | CREATE INDEX CONCURRENTLY |
| 一个迁移中混合Schema+数据 | 回滚困难，长事务 | 分离迁移 |
| 在移除代码前删除列 | 应用程序在列缺失时出错 | 先移除代码，下个部署再删除列