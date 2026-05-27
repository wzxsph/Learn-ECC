---
name: mysql-patterns
description: MySQL 和 MariaDB schema、查询、索引、事务、复制和连接池模式，适用于生产后端。
origin: ECC
---

# MySQL 模式

在使用 MySQL 或 MariaDB schema 设计、迁移、慢查询调查、队列风格事务、连接池或生产数据库配置时使用此技能。在应用特定于功能的模式前请进行精确版本检查，因为 MySQL 和 MariaDB 在多个 SQL 细节上已经分化。

## 激活条件

- 设计 MySQL 或 MariaDB 表、索引和约束
- 在大型生产表上运行迁移前审查迁移
- 调试慢查询、锁等待、死锁或连接耗尽
- 添加游标分页、upserts、全文搜索、JSON 列或队列
- 配置应用程序连接池、只读副本、TLS 或慢日志

## 版本检查

首先识别引擎和版本：

```sql
SELECT VERSION();
SHOW VARIABLES LIKE 'version_comment';
```

在语法不同时将 MySQL 和 MariaDB 指导分开：

- MySQL 将行别名文档作为 `ON DUPLICATE KEY UPDATE` 中 `VALUES(col)` 的替代品；`VALUES(col)` 在那里已弃用。
- MariaDB 将 `VALUES(col)` 文档化为在 `ON DUPLICATE KEY UPDATE` 中引用插入值的支持方式；在跨引擎兼容性时使用它。
- `SKIP LOCKED` 仅适用于队列风格的工作。它跳过锁定的行，可能返回不一致的视图，因此不要将其用于一般会计或完整性敏感的读取。

## Schema 默认值

```sql
CREATE TABLE orders (
    id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
    account_id BIGINT UNSIGNED NOT NULL,
    status VARCHAR(32) NOT NULL,
    total DECIMAL(15, 2) NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deleted_at DATETIME NULL,
    PRIMARY KEY (id),
    KEY idx_orders_account_status_created (account_id, status, created_at),
    KEY idx_orders_active (account_id, deleted_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

默认选择：

| 使用场景 | 推荐 | 避免 |
| --- | --- | --- |
| 代理主键 | `BIGINT UNSIGNED AUTO_INCREMENT` | `INT` 用于可能增长超过 20 亿行的表 |
| UUID 查找键 | `BINARY(16)` 配合转换辅助函数 | 热表上使用 `VARCHAR(36)` 主键 |
| 金钱和精确数量 | `DECIMAL(p, s)` | `FLOAT` 或 `DOUBLE` |
| 用户面向文本 | `utf8mb4` 表和索引 | MySQL `utf8` / `utf8mb3` 默认值 |
| 应用程序时间戳 | `DATETIME` 配合应用程序管理的 UTC | 假设 `DATETIME` 存储时区元数据 |
| 软删除 | `deleted_at DATETIME NULL` 配合范围索引 | 无索引过滤软删除行 |
| 可扩展状态值 | 查找表或约束 `VARCHAR` | 值经常变化时使用 `ENUM` |

## 索引

复合索引顺序通常遵循等式谓词在前，范围或排序列在后：

```sql
CREATE INDEX idx_orders_account_status_created
    ON orders (account_id, status, created_at);

SELECT id, total
FROM orders
WHERE account_id = ?
  AND status = 'pending'
  AND created_at >= ?
ORDER BY created_at DESC
LIMIT 50;
```

添加或更改索引前使用 `EXPLAIN`：

```sql
EXPLAIN
SELECT id, total
FROM orders
WHERE account_id = 123 AND status = 'pending'
ORDER BY created_at DESC
LIMIT 50;
```

需要调查的信号：

| 字段 | 风险信号 |
| --- | --- |
| `type` | 大表上为 `ALL` |
| `key` | 存在选择性谓词时为 `NULL` |
| `rows` | 交互路径的非常高行估计 |
| `Extra` | `Using temporary`、`Using filesort` 或广泛的 `Using where` |

不要盲目添加索引。每个索引增加写入成本、迁移时间、备份大小和缓冲池压力。

## 查询模式

### Upsert

跨引擎兼容形式：

```sql
INSERT INTO user_settings (user_id, setting_key, setting_value)
VALUES (?, ?, ?)
ON DUPLICATE KEY UPDATE
    setting_value = VALUES(setting_value),
    updated_at = CURRENT_TIMESTAMP;
```

MySQL 行别名形式：

```sql
INSERT INTO user_settings (user_id, setting_key, setting_value)
VALUES (?, ?, ?) AS new
ON DUPLICATE KEY UPDATE
    setting_value = new.setting_value,
    updated_at = CURRENT_TIMESTAMP;
```

仅在确认目标是 MySQL 后才使用行别名形式。在 MariaDB 或混合 MySQL/MariaDB 集群中使用 `VALUES(col)`。

### 游标分页

```sql
SELECT id, name, created_at
FROM products
WHERE (created_at, id) < (?, ?)
ORDER BY created_at DESC, id DESC
LIMIT 50;
```

用匹配游标的索引支持：

```sql
CREATE INDEX idx_products_created_id ON products (created_at, id);
```

不要在大表上使用深度 `OFFSET` 分页；它使服务器扫描并在返回页面之前丢弃行。

### JSON 字段

将 JSON 列用于扩展数据，而非需要重型关系过滤或约束的字段。

```sql
CREATE TABLE events (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    payload JSON NOT NULL,
    event_type VARCHAR(64)
        GENERATED ALWAYS AS (JSON_UNQUOTE(JSON_EXTRACT(payload, '$.type'))) STORED,
    KEY idx_events_type (event_type)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

对于频繁查询的 JSON 路径，暴露生成的列并对该列建立索引。将外键、所有权、租户和生命周期字段保持为关系型。

### 全文搜索

```sql
ALTER TABLE articles ADD FULLTEXT KEY ft_articles_title_body (title, body);

SELECT id, title, MATCH(title, body) AGAINST (? IN NATURAL LANGUAGE MODE) AS score
FROM articles
WHERE MATCH(title, body) AGAINST (? IN NATURAL LANGUAGE MODE)
ORDER BY score DESC
LIMIT 20;
```

当你需要拼写容错、复杂排名、跨表方面或超出内置全文行为的语言特定分析时，使用外部搜索。

## 事务

保持事务简短并以一致的顺序锁定行：

```sql
START TRANSACTION;

SELECT id, balance
FROM accounts
WHERE id IN (?, ?)
ORDER BY id
FOR UPDATE;

UPDATE accounts SET balance = balance - ? WHERE id = ?;
UPDATE accounts SET balance = balance + ? WHERE id = ?;

COMMIT;
```

死锁和锁等待检查清单：

- 在代码路径中以确定性顺序锁定行。
- 在打开事务之前进行外部 API 调用，而不是在事务内部。
- 为 `UPDATE`、`DELETE` 和锁定读取中使用的谓词添加索引。
- 发生死锁时，回滚并用有界重试预算重试整个事务。
- 死锁后尽快捕获 `SHOW ENGINE INNODB STATUS\G`；它会被后续事件覆盖。

队列风格的工作者声明：

```sql
START TRANSACTION;

SELECT id
FROM jobs
WHERE status = 'pending'
ORDER BY created_at
LIMIT 1
FOR UPDATE SKIP LOCKED;

UPDATE jobs
SET status = 'processing', started_at = CURRENT_TIMESTAMP
WHERE id = ?;

COMMIT;
```

仅将 `SKIP LOCKED` 用于队列风格的工作负载，其中跳过锁定的行是可接受的。它不是正常事务一致性的替代品。

## 连接池

SQLAlchemy 示例：

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+mysqlconnector://app:secret@db.internal/app",
    pool_size=10,
    max_overflow=5,
    pool_timeout=30,
    pool_recycle=240,
    pool_pre_ping=True,
    connect_args={"connect_timeout": 5},
)
```

Node.js `mysql2` 示例：

```javascript
import mysql from 'mysql2/promise';

const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
  enableKeepAlive: true,
  keepAliveInitialDelay: 30000,
});

const [rows] = await pool.execute(
  'SELECT id, total FROM orders WHERE account_id = ? LIMIT 50',
  [accountId],
);
```

保持应用程序池回收低于服务器 `wait_timeout`。如果服务器使用 `wait_timeout = 300`，大约 240 秒的 `pool_recycle` 是连贯的；`pool_pre_ping` 仍有助于从网络和故障转移事件中恢复。

## 诊断

有用的第一轮命令：

```sql
SHOW FULL PROCESSLIST;
SHOW ENGINE INNODB STATUS\G;
SHOW VARIABLES LIKE 'slow_query_log';
SHOW VARIABLES LIKE 'long_query_time';
```

在受控环境中启用慢日志：

```sql
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 1;
SET GLOBAL log_queries_not_using_indexes = 'ON';
```

仅在安全执行查询时使用 `EXPLAIN ANALYZE`。它运行语句，在生产规模数据上可能很昂贵。

## 复制

只读副本可能滞后。不要将读后写路径、结账流程、权限检查或幂等键读取路由到副本在写入后立即读取。

```sql
-- MySQL 遗留术语，在现有集群中仍然常见
SHOW SLAVE STATUS\G;

-- 较新的术语（在支持的版本中）
SHOW REPLICA STATUS\G;
```

在标准化一个命令之前检查引擎/版本。监控副本 SQL 线程健康、IO 线程健康和滞后，而不仅仅是 TCP 连接是否存活。

## 安全

```sql
CREATE USER 'app'@'%' IDENTIFIED BY 'use-a-secret-manager';
GRANT SELECT, INSERT, UPDATE, DELETE ON appdb.* TO 'app'@'%';

ALTER USER 'app'@'%' REQUIRE SSL;

SELECT user, host
FROM mysql.user
WHERE user = '';

DROP USER IF EXISTS ''@'localhost';
DROP USER IF EXISTS ''@'%';
```

安全审查要点：

- 不要授予 `ALL PRIVILEGES` 或 `*.*` 给应用程序用户。
- 当流量跨主机或网络时，要求应用程序用户使用 TLS。
- 在平台密钥管理器中存储凭证，而非在示例、脚本或仓库文件中。
- 将迁移/管理员用户与运行时应用程序用户分开。
- 在调整性能之前审计公共网络暴露和绑定地址。

## 配置

专用数据库主机的示例起点：

```ini
[mysqld]
innodb_buffer_pool_size = 4G
innodb_flush_log_at_trx_commit = 1
sync_binlog = 1

max_connections = 300
thread_cache_size = 50

wait_timeout = 300
interactive_timeout = 300
innodb_lock_wait_timeout = 10

slow_query_log = ON
long_query_time = 1
log_queries_not_using_indexes = ON

log_bin = mysql-bin
binlog_format = ROW
binlog_expire_logs_seconds = 604800
```

将配置值视为审查的提示，而非通用预设。从工作负载、硬件、备份策略和恢复目标调整内存、连接、日志保留和持久性设置。

## 反模式

| 反模式 | 风险 | 更好的模式 |
| --- | --- | --- |
| 热路径中 `SELECT *` | 过度获取和脆弱客户端 | 选择明确的列 |
| 深度 `OFFSET` 分页 | 线性扫描和慢页面 | 游标分页 |
| 外键连接无索引 | 慢连接和锁密集删除 | 有意索引 FK 列 |
| 长事务 | 锁等待和大 undo 历史 | 提交小单元工作 |
| 直接对 `mysql.user` 进行 DML | 授权表损坏风险 | 使用 `CREATE USER`、`ALTER USER`、`DROP USER` |
| 具有管理员权限的应用程序用户 | 高爆炸半径 | 最小权限运行时用户 |
| 池回收高于 `wait_timeout` | 陈旧池连接 | 在超时以下回收和预检 |
| 写入后读取副本 | 陈旧用户面向状态 | 将写后读流程固定到主节点 |

## 输出期望

当此技能用于审查时，返回：

1. 引擎/版本假设。
2. 最高风险正确性、锁、安全和迁移问题。
3. 安全路径的精确 SQL 或代码更改。
4. 验证计划：`EXPLAIN`、迁移试运行、锁/死锁检查和回滚标准。
5. 影响建议的任何 MySQL/MariaDB 语法差异。

## 相关

- 技能：`postgres-patterns` — PostgreSQL 特定的 schema 和查询模式
- 技能：`database-migrations` — 迁移规划和推出安全性
- 技能：`backend-patterns` — API 和服务层模式
- 技能：`security-review` — 密钥处理、认证和最小权限
- 代理：`database-reviewer` — 更广泛的数据库审查工作流程