---
name: docker-patterns
description: Docker 和 Docker Compose 模式，用于本地开发、容器安全、网络、卷策略和多服务编排。
origin: ECC
---

# Docker 模式

容器化开发的 Docker 和 Docker Compose 最佳实践。

## 激活时机

- 为本地开发设置 Docker Compose
- 设计多容器架构
- 排查容器网络或卷问题
- 审查 Dockerfile 的安全性和体积
- 从本地开发迁移到容器化工作流

## 本地开发的 Docker Compose

### 标准 Web 应用栈

```yaml
# docker-compose.yml
services:
  app:
    build:
      context: .
      target: dev                     # 使用多阶段 Dockerfile 的 dev 阶段
    ports:
      - "3000:3000"
    volumes:
      - .:/app                        # 绑定挂载用于热重载
      - /app/node_modules             # 匿名卷 -- 保留容器内的依赖
    environment:
      - DATABASE_URL=postgres://postgres:postgres@db:5432/app_dev
      - REDIS_URL=redis://redis:6379/0
      - NODE_ENV=development
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
    command: npm run dev

  db:
    image: postgres:16-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: app_dev
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./scripts/init-db.sql:/docker-entrypoint-initdb.d/init.sql
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 3s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redisdata:/data

  mailpit:                            # 本地邮件测试
    image: axllent/mailpit
    ports:
      - "8025:8025"                   # Web UI
      - "1025:1025"                   # SMTP

volumes:
  pgdata:
  redisdata:
```

### 开发与生产 Dockerfile

```dockerfile
# 阶段: 依赖
FROM node:22-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

# 阶段: 开发 (热重载, 调试工具)
FROM node:22-alpine AS dev
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]

# 阶段: 构建
FROM node:22-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build && npm prune --production

# 阶段: 生产 (最小镜像)
FROM node:22-alpine AS production
WORKDIR /app
RUN addgroup -g 1001 -S appgroup && adduser -S appuser -u 1001
USER appuser
COPY --from=build --chown=appuser:appgroup /app/dist ./dist
COPY --from=build --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=build --chown=appuser:appgroup /app/package.json ./
ENV NODE_ENV=production
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:3000/health || exit 1
CMD ["node", "dist/server.js"]
```

### 覆盖文件

```yaml
# docker-compose.override.yml (自动加载, 仅开发设置)
services:
  app:
    environment:
      - DEBUG=app:*
      - LOG_LEVEL=debug
    ports:
      - "9229:9229"                   # Node.js 调试器

# docker-compose.prod.yml (生产环境显式使用)
services:
  app:
    build:
      target: production
    restart: always
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
```

```bash
# 开发 (自动加载覆盖)
docker compose up

# 生产
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

## 网络

### 服务发现

同一 Compose 网络中的服务可通过服务名解析:

```
# 从 "app" 容器内:
postgres://postgres:postgres@db:5432/app_dev    # "db" 解析为 db 容器
redis://redis:6379/0                             # "redis" 解析为 redis 容器
```

### 自定义网络

```yaml
services:
  frontend:
    networks:
      - frontend-net

  api:
    networks:
      - frontend-net
      - backend-net

  db:
    networks:
      - backend-net              # 仅可从 api 访问, frontend 不可达

networks:
  frontend-net:
  backend-net:
```

### 仅暴露所需内容

```yaml
services:
  db:
    ports:
      - "127.0.0.1:5432:5432"   # 仅从主机可访问, 网络不可达
    # 生产环境中省略 ports -- 仅在 Docker 网络内可访问
```

## 卷策略

```yaml
volumes:
  # 命名卷: 在容器重启后持久化, 由 Docker 管理
  pgdata:

  # 绑定挂载: 将主机目录映射到容器 (用于开发)
  # - ./src:/app/src

  # 匿名卷: 保护容器生成的内容免受绑定挂载覆盖
  # - /app/node_modules
```

### 常见模式

```yaml
services:
  app:
    volumes:
      - .:/app                   # 源代码 (绑定挂载用于热重载)
      - /app/node_modules        # 保护容器的 node_modules 免受主机影响
      - /app/.next               # 保护构建缓存

  db:
    volumes:
      - pgdata:/var/lib/postgresql/data          # 持久化数据
      - ./scripts/init.sql:/docker-entrypoint-initdb.d/init.sql  # 初始化脚本
```

## 容器安全

### Dockerfile 加固

```dockerfile
# 1. 使用特定标签 (永不使用 :latest)
FROM node:22.12-alpine3.20

# 2. 以非 root 用户运行
RUN addgroup -g 1001 -S app && adduser -S app -u 1001
USER app

# 3. 丢弃 capabilities (在 compose 中)
# 4. 尽可能使用只读根文件系统
# 5. 不在镜像层中存放 secrets
```

### Compose 安全

```yaml
services:
  app:
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /tmp
      - /app/.cache
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE          # 仅在绑定到 < 1024 端口时需要
```

### 密钥管理

```yaml
# 好: 使用环境变量 (运行时注入)
services:
  app:
    env_file:
      - .env                     # 永不将 .env 提交到 git
    environment:
      - API_KEY                  # 从主机环境继承

# 好: Docker secrets (Swarm 模式)
secrets:
  db_password:
    file: ./secrets/db_password.txt

services:
  db:
    secrets:
      - db_password

# 坏: 硬编码在镜像中
# ENV API_KEY=sk-proj-xxxxx      # 切勿如此
```

## .dockerignore

```
node_modules
.git
.env
.env.*
dist
coverage
*.log
.next
.cache
docker-compose*.yml
Dockerfile*
README.md
tests/
```

## 调试

### 常用命令

```bash
# 查看日志
docker compose logs -f app           # 跟踪 app 日志
docker compose logs --tail=50 db     # db 最近 50 行

# 在运行中的容器内执行命令
docker compose exec app sh           # 进入 app shell
docker compose exec db psql -U postgres  # 连接 postgres

# 检查
docker compose ps                     # 运行中的服务
docker compose top                    # 每个容器中的进程
docker stats                          # 资源使用情况

# 重建
docker compose up --build             # 重建镜像
docker compose build --no-cache app   # 强制完全重建

# 清理
docker compose down                   # 停止并移除容器
docker compose down -v                # 同时移除卷 (破坏性)
docker system prune                   # 移除未使用的镜像/容器
```

### 排查网络问题

```bash
# 检查容器内的 DNS 解析
docker compose exec app nslookup db

# 检查连接性
docker compose exec app wget -qO- http://api:3000/health

# 检查网络
docker network ls
docker network inspect <project>_default
```

## 反模式

```
# 坏: 在生产环境中使用 docker compose 而无编排
# 使用 Kubernetes、ECS 或 Docker Swarm 处理生产多容器工作负载

# 坏: 容器中存储数据而无卷
# 容器是临时的 -- 无卷情况下重启后所有数据丢失

# 坏: 以 root 运行
# 始终创建并使用非 root 用户

# 坏: 使用 :latest 标签
# 固定到特定版本以确保可重复构建

# 坏: 一个大容器运行所有服务
# 分离关注点: 每个容器一个进程

# 坏: 将 secrets 放在 docker-compose.yml 中
# 使用 .env 文件 (gitignore) 或 Docker secrets
```