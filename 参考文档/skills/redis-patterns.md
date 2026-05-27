---
name: redis-patterns
description: Redis 数据结构模式、缓存策略、分布式锁、限流、发布/订阅以及连接管理，适用于生产环境应用。
origin: ECC
---

# Redis 模式

Redis 最佳实践快速参考，涵盖常见的后端用例。

## 工作原理

Redis 是一个内存数据结构存储，支持字符串、哈希、列表、集合、有序集合、流等。单个 Redis 命令在单机上是原子的；多步骤工作流需要 Lua 脚本、MULTI/EXEC 事务或显式同步来保持原子性。数据可通过 RDB 快照或 AOF 日志选择性持久化。客户端通过 TCP 使用 RESP 协议通信；连接池是必需的，以避免每次请求的握手开销。

## 激活时机

- 为应用程序添加缓存
- 实现限流或节流
- 构建分布式锁或协调机制
- 设置会话或令牌存储
- 使用 Pub/Sub 或 Redis Streams 进行消息传递
- 配置生产环境 Redis（连接池、驱逐、集群）

## 数据结构速查表

| 用例 | 数据结构 | 示例键 |
|----------|-----------|-------------|
| 简单缓存 | String | `product:123` |
| 用户会话 | Hash | `session:abc` |
| 排行榜 | Sorted Set | `scores:weekly` |
| 独立访客 | Set | `visitors:2024-01-01` |
| 动态消息流 | List | `feed:user:456` |
| 事件流 | Stream | `events:orders` |
| 计数器/限流 | String (INCR) | `ratelimit:user:123` |
| 布隆过滤器/HLL | HyperLogLog | `hll:pageviews` |

## 核心模式

### 缓存旁路（延迟加载）

```python
import redis
import json

r = redis.Redis(host='localhost', port=6379, decode_responses=True)

def get_product(product_id: int):
    cache_key = f"product:{product_id}"
    cached = r.get(cache_key)

    if cached:
        return json.loads(cached)

    product = db.query("SELECT * FROM products WHERE id = %s", product_id)
    r.setex(cache_key, 3600, json.dumps(product))  # TTL: 1 小时
    return product
```

### 写穿透缓存

```python
def update_product(product_id: int, data: dict):
    # 先写入数据库
    db.execute("UPDATE products SET ... WHERE id = %s", product_id)

    # 立即更新缓存
    cache_key = f"product:{product_id}"
    r.setex(cache_key, 3600, json.dumps(data))
```

### 缓存失效

```python
# 基于标签的失效 — 将相关键分组到集合下
def cache_product(product_id: int, category_id: int, data: dict):
    key = f"product:{product_id}"
    tag = f"tag:category:{category_id}"
    pipe = r.pipeline(transaction=True)
    pipe.setex(key, 3600, json.dumps(data))
    pipe.sadd(tag, key)
    pipe.expire(tag, 3600)
    pipe.execute()

def invalidate_category(category_id: int):
    tag = f"tag:category:{category_id}"
    keys = r.smembers(tag)
    if keys:
        r.delete(*keys)
    r.delete(tag)
```

### 会话存储

```python
import time
import uuid

def create_session(user_id: int, ttl: int = 86400) -> str:
    session_id = str(uuid.uuid4())
    key = f"session:{session_id}"
    pipe = r.pipeline(transaction=True)
    pipe.hset(key, mapping={
        "user_id": user_id,
        "created_at": int(time.time()),
    })
    pipe.expire(key, ttl)
    pipe.execute()
    return session_id

def get_session(session_id: str) -> dict | None:
    data = r.hgetall(f"session:{session_id}")
    return data if data else None

def delete_session(session_id: str):
    r.delete(f"session:{session_id}")
```

## 限流

### 固定窗口（简单）

```python
def is_rate_limited(user_id: int, limit: int = 100, window: int = 60) -> bool:
    key = f"ratelimit:{user_id}:{int(time.time()) // window}"
    pipe = r.pipeline(transaction=True)
    pipe.incr(key)
    pipe.expire(key, window)
    count, _ = pipe.execute()
    return count > limit
```

### 滑动窗口（Lua — 原子操作）

```lua
-- sliding_window.lua
local key = KEYS[1]
local now = tonumber(ARGV[1])
local window = tonumber(ARGV[2])
local limit = tonumber(ARGV[3])

redis.call('ZREMRANGEBYSCORE', key, 0, now - window)
local count = redis.call('ZCARD', key)

if count < limit then
    -- 使用唯一成员 (now + sequence) 避免同一毫秒内的冲突
    local seq_key = key .. ':seq'
    local seq = redis.call('INCR', seq_key)
    redis.call('EXPIRE', seq_key, math.ceil(window / 1000))
    redis.call('ZADD', key, now, now .. '-' .. seq)
    redis.call('EXPIRE', key, math.ceil(window / 1000))
    return 1
end
return 0
```

```python
sliding_window = r.register_script(open('sliding_window.lua').read())

def allow_request(user_id: int) -> bool:
    key = f"ratelimit:sliding:{user_id}"
    now = int(time.time() * 1000)
    return bool(sliding_window(keys=[key], args=[now, 60000, 100]))
```

## 分布式锁

### 分布式锁（单节点 — SET NX PX）

```python
import uuid

def acquire_lock(resource: str, ttl_ms: int = 5000) -> str | None:
    lock_key = f"lock:{resource}"
    token = str(uuid.uuid4())
    acquired = r.set(lock_key, token, px=ttl_ms, nx=True)
    return token if acquired else None

def release_lock(resource: str, token: str) -> bool:
    release_script = """
    if redis.call('get', KEYS[1]) == ARGV[1] then
        return redis.call('del', KEYS[1])
    else
        return 0
    end
    """
    result = r.eval(release_script, 1, f"lock:{resource}", token)
    return bool(result)

# 用法
token = acquire_lock("order:payment:123")
if token:
    try:
        process_payment()
    finally:
        release_lock("order:payment:123", token)
```

> 多节点设置请使用 `redlock-py` 库，该库实现了完整的 Redlock 算法。

## Pub/Sub 与 Streams

### 发布/订阅（即发即忘）

```python
# 发布者
def publish_event(channel: str, payload: dict):
    r.publish(channel, json.dumps(payload))

# 订阅者（阻塞 — 在独立线程/进程中运行）
def subscribe_events(channel: str):
    pubsub = r.pubsub()
    pubsub.subscribe(channel)
    for message in pubsub.listen():
        if message['type'] == 'message':
            handle(json.loads(message['data']))
```

### Redis Streams（持久化队列）

```python
# 生产者
def emit(stream: str, event: dict):
    r.xadd(stream, event, maxlen=10000)  # 限制流长度

# 消费者组 — 保证至少一次投递
try:
    r.xgroup_create('events:orders', 'processor', id='0', mkstream=True)
except Exception:
    pass  # 组已存在

def consume(stream: str, group: str, consumer: str):
    while True:
        messages = r.xreadgroup(group, consumer, {stream: '>'}, count=10, block=2000)
        for _, entries in (messages or []):
            for msg_id, data in entries:
                process(data)
                r.xack(stream, group, msg_id)
```

> 如果需要投递保证、消费者组或重放功能，请优先使用 **Streams** 而非 Pub/Sub。

## 键设计

### 命名规范

```
# 模式: resource:id:field
user:123:profile
order:456:status
cache:product:789

# 模式: namespace:resource:id
myapp:session:abc123
myapp:ratelimit:user:123

# 模式: resource:date（时间绑定键）
stats:pageviews:2024-01-01
```

### TTL 策略

| 数据类型 | 建议 TTL |
|-----------|--------------|
| 用户会话 | 24小时 (`86400`) |
| API 响应缓存 | 5–15 分钟 |
| 限流窗口 | 与窗口大小匹配 |
| 短期令牌 | 5–10 分钟 |
| 排行榜 | 1–24 小时 |
| 静态/参考数据 | 1 小时至 1 周 |

务必设置 TTL。没有 TTL 的键会无限积累，导致内存压力。

## 连接管理

### 连接池

```python
from redis import ConnectionPool, Redis

pool = ConnectionPool(
    host='localhost',
    port=6379,
    db=0,
    max_connections=20,
    decode_responses=True,
    socket_connect_timeout=2,
    socket_timeout=2,
)

r = Redis(connection_pool=pool)
```

### 集群模式

```python
from redis.cluster import RedisCluster

r = RedisCluster(
    startup_nodes=[{"host": "redis-1", "port": 6379}],
    decode_responses=True,
    skip_full_coverage_check=True,
)
```

### 哨兵（高可用）

```python
from redis.sentinel import Sentinel

sentinel = Sentinel(
    [('sentinel-1', 26379), ('sentinel-2', 26379)],
    socket_timeout=0.5,
)
master = sentinel.master_for('mymaster', decode_responses=True)
replica = sentinel.slave_for('mymaster', decode_responses=True)
```

## 驱逐策略

| 策略 | 行为 | 适用场景 |
|--------|----------|----------|
| `noeviction` | 满时写入报错 | 队列/关键数据 |
| `allkeys-lru` | 驱逐最近最少使用 | 通用缓存 |
| `volatile-lru` | 仅对有 TTL 的键进行 LRU | 混合数据存储 |
| `allkeys-lfu` | 驱逐最不常用 | 偏斜访问模式 |
| `volatile-ttl` | 驱逐即将过期 | 优先保留长生命周期数据 |

通过 `redis.conf` 设置：`maxmemory-policy allkeys-lru`

## 反模式

| 反模式 | 问题 | 解决方案 |
|---|---|---|
| 键无 TTL | 内存无限增长 | 始终设置 TTL |
| 生产环境使用 `KEYS *` | 阻塞服务器 O(N) | 使用 `SCAN` 游标 |
| 存储大二进制 (>100KB) | 序列化慢，内存压力大 | 存储引用，从对象存储获取 |
| 所有数据用一个 Redis | 缓存与队列无隔离 | 使用独立的 DB 或实例 |
| 忽略连接池限制 | 负载下连接耗尽 | 根据工作负载调整池大小 |
| 不处理缓存击穿 | 冷启动时惊群效应 | 使用锁或概率性提前过期 |
| 不考虑后果地 `FLUSHALL` | 擦除整个实例 | 按键模式限定删除范围 |

### 缓存击穿防护

```python
import threading

_locks: dict[str, threading.Lock] = {}
_locks_mutex = threading.Lock()

def get_with_lock(key: str, fetch_fn, ttl: int = 300):
    cached = r.get(key)
    if cached:
        return json.loads(cached)

    with _locks_mutex:
        if key not in _locks:
            _locks[key] = threading.Lock()
        lock = _locks[key]
    with lock:
        cached = r.get(key)  # 获取锁后重新检查
        if cached:
            return json.loads(cached)
        value = fetch_fn()
        r.setex(key, ttl, json.dumps(value))
        return value
```

> 注意：多进程部署时，将进程内锁替换为上文分布式锁部分的 `acquire_lock`/`release_lock`。

## 示例

**为 Django/Flask API 端点添加缓存：**
使用缓存旁路模式，配合 `setex` 和 5 分钟 TTL。以请求参数作为键。

**按用户限流 API：**
低流量端点使用固定窗口配合 `pipeline(transaction=True)`；需要精确的按用户节流时使用滑动窗口 Lua 脚本。

**跨 workers 协调后台作业：**
使用带 TTL 的 `acquire_lock`，TTL 应超过预期作业时长。务必在 `finally` 块中释放。

**向多个订阅者推送通知：**
使用 Pub/Sub 进行即发即忘。如果需要可靠投递或为晚到的消费者提供重放功能，切换到 Streams。

## 快速参考

| 模式 | 何时使用 |
|---------|-------------|
| 缓存旁路 | 读密集型，可容忍轻微过期 |
| 写穿透 | 需要强一致性 |
| 分布式锁 | 防止并发访问资源 |
| 滑动窗口限流 | 精确的按用户节流 |
| Redis Streams | 带消费者组的持久化事件队列 |
| Pub/Sub | 无投递保证的广播 |
| 有序集合排行榜 | 排名计分、分页 |
| HyperLogLog | 低内存近似唯一计数 |

## 相关

- 技能: `postgres-patterns` — 关系数据模式
- 技能: `backend-patterns` — API 和服务层模式
- 技能: `database-migrations` — 数据库版本管理
- 技能: `django-patterns` — Django 缓存框架集成
- 智能体: `database-reviewer` — 完整数据库审查工作流

---

*基于 Supabase Agent Skills（鸣谢：Supabase 团队）（MIT License）*