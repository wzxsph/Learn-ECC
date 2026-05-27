---
name: deployment-patterns
description: 部署工作流、CI/CD流水线模式、Docker容器化、健康检查、回滚策略，以及Web应用的生产就绪检查清单。
origin: ECC
---

# 部署模式

生产部署工作流与CI/CD最佳实践。

## 激活时机

- 设置CI/CD流水线
- 将应用Docker化
- 规划部署策略（蓝绿、金丝雀、滚动）
- 实现健康检查和就绪探针
- 准备生产发布
- 配置环境特定设置

## 部署策略

### 滚动部署（默认）

逐步替换实例——在新版本推出期间，旧版本和新版本同时运行。

```
实例 1: v1 → v2  (首先更新)
实例 2: v1        (仍在运行v1)
实例 3: v1        (仍在运行v1)

实例 1: v2
实例 2: v1 → v2  (其次更新)
实例 3: v1

实例 1: v2
实例 2: v2
实例 3: v1 → v2  (最后更新)
```

**优点：** 零停机、渐进式推出
**缺点：** 两个版本同时运行——需要向后兼容的变更
**适用场景：** 标准部署、向后兼容的变更

### 蓝绿部署

运行两个相同的环境。原子性地切换流量。

```
蓝色  (v1) ← 流量
绿色  (v2)   空闲，运行新版本

# 验证后：
蓝色  (v1)   空闲（成为备用）
绿色  (v2) ← 流量
```

**优点：** 即时回滚（切回蓝色）、干净切换
**缺点：** 部署期间需要2倍基础设施
**适用场景：** 关键服务、零问题容忍

### 金丝雀部署

首先将一小部分流量路由到新版本。

```
v1: 95% 的流量
v2:  5% 的流量  (金丝雀)

# 如果指标良好：
v1: 50% 的流量
v2: 50% 的流量

# 最终：
v2: 100% 的流量
```

**优点：** 在完全推出前用真实流量捕获问题
**缺点：** 需要流量分割基础设施、监控
**适用场景：** 高流量服务、风险变更、功能开关

## Docker

### 多阶段Dockerfile（Node.js）

```dockerfile
# 阶段1：安装依赖
FROM node:22-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --production=false

# 阶段2：构建
FROM node:22-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build
RUN npm prune --production

# 阶段3：生产镜像
FROM node:22-alpine AS runner
WORKDIR /app

RUN addgroup -g 1001 -S appgroup && adduser -S appuser -u 1001
USER appuser

COPY --from=builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /app/dist ./dist
COPY --from=builder --chown=appuser:appgroup /app/package.json ./

ENV NODE_ENV=production
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

CMD ["node", "dist/server.js"]
```

### 多阶段Dockerfile（Go）

```dockerfile
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /server ./cmd/server

FROM alpine:3.19 AS runner
RUN apk --no-cache add ca-certificates
RUN adduser -D -u 1001 appuser
USER appuser

COPY --from=builder /server /server

EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:8080/health || exit 1
CMD ["/server"]
```

### 多阶段Dockerfile（Python/Django）

```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /app
RUN pip install --no-cache-dir uv
COPY requirements.txt .
RUN uv pip install --system --no-cache -r requirements.txt

FROM python:3.12-slim AS runner
WORKDIR /app

RUN useradd -r -u 1001 appuser
USER appuser

COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin
COPY . .

ENV PYTHONUNBUFFERED=1
EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=3s CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health/')" || exit 1
CMD ["gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "4"]
```

### Docker 最佳实践

```
# 好的实践
- 使用特定版本标签（node:22-alpine，而非node:latest）
- 多阶段构建以减小镜像大小
- 以非root用户运行
- 先复制依赖文件（层缓存）
- 使用.dockerignore排除node_modules、.git、测试文件
- 添加HEALTHCHECK指令
- 在docker-compose或k8s中设置资源限制

# 坏的实践
- 以root运行
- 使用:latest标签
- 在一个COPY层中复制整个仓库
- 在生产镜像中安装开发依赖
- 将 Secrets 存储在镜像中（使用环境变量或secrets管理器）
```

## CI/CD流水线

### GitHub Actions（标准流水线）

```yaml
name: CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
      - run: npm test -- --coverage
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: coverage
          path: coverage/

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - uses: docker/setup-buildx-action@v3
      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - uses: docker/build-push-action@v5
        with:
          push: true
          tags: ghcr.io/${{ github.repository }}:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
      - name: 部署到生产环境
        run: |
          # 平台特定的部署命令
          # Railway: railway up
          # Vercel: vercel --prod
          # K8s: kubectl set image deployment/app app=ghcr.io/${{ github.repository }}:${{ github.sha }}
          echo "部署 ${{ github.sha }}"
```

### 流水线阶段

```
PR打开时：
  lint → typecheck → 单元测试 → 集成测试 → 预览部署

合并到main后：
  lint → typecheck → 单元测试 → 集成测试 → 构建镜像 → 部署到预发布 → 冒烟测试 → 部署到生产
```

## 健康检查

### 健康检查端点

```typescript
// 简单健康检查
app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok" });
});

// 详细健康检查（用于内部监控）
app.get("/health/detailed", async (req, res) => {
  const checks = {
    database: await checkDatabase(),
    redis: await checkRedis(),
    externalApi: await checkExternalApi(),
  };

  const allHealthy = Object.values(checks).every(c => c.status === "ok");

  res.status(allHealthy ? 200 : 503).json({
    status: allHealthy ? "ok" : "degraded",
    timestamp: new Date().toISOString(),
    version: process.env.APP_VERSION || "unknown",
    uptime: process.uptime(),
    checks,
  });
});

async function checkDatabase(): Promise<HealthCheck> {
  try {
    await db.query("SELECT 1");
    return { status: "ok", latency_ms: 2 };
  } catch (err) {
    return { status: "error", message: "Database unreachable" };
  }
}
```

### Kubernetes探针

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 3000
  initialDelaySeconds: 10
  periodSeconds: 30
  failureThreshold: 3

readinessProbe:
  httpGet:
    path: /health
    port: 3000
  initialDelaySeconds: 5
  periodSeconds: 10
  failureThreshold: 2

startupProbe:
  httpGet:
    path: /health
    port: 3000
  initialDelaySeconds: 0
  periodSeconds: 5
  failureThreshold: 30    # 30 * 5s = 最多150s启动时间
```

## 环境配置

### 十二要素应用模式

```bash
# 所有配置通过环境变量——永不写在代码中
DATABASE_URL=postgres://user:pass@host:5432/db
REDIS_URL=redis://host:6379/0
API_KEY=${API_KEY}           # 由secrets管理器注入
LOG_LEVEL=info
PORT=3000

# 环境特定行为
NODE_ENV=production          # 或staging、development
APP_ENV=production           # 明确的应用环境
```

### 配置验证

```typescript
import { z } from "zod";

const envSchema = z.object({
  NODE_ENV: z.enum(["development", "staging", "production"]),
  PORT: z.coerce.number().default(3000),
  DATABASE_URL: z.string().url(),
  REDIS_URL: z.string().url(),
  JWT_SECRET: z.string().min(32),
  LOG_LEVEL: z.enum(["debug", "info", "warn", "error"]).default("info"),
});

// 启动时验证——配置错误时快速失败
export const env = envSchema.parse(process.env);
```

## 回滚策略

### 即时回滚

```bash
# Docker/Kubernetes：指向之前的镜像
kubectl rollout undo deployment/app

# Vercel：提升之前的部署
vercel rollback

# Railway：重新部署之前的提交
railway up --commit <previous-sha>

# 数据库：回滚迁移（如果是可逆的）
npx prisma migrate resolve --rolled-back <migration-name>
```

### 回滚检查清单

- [ ] 之前的镜像/制品可用且已标记
- [ ] 数据库迁移向后兼容（无破坏性变更）
- [ ] 功能开关可以在不重新部署的情况下禁用新功能
- [ ] 监控告警配置用于错误率飙升
- [ ] 在生产发布前已在预发布环境测试过回滚

## 生产就绪检查清单

任何生产部署前：

### 应用
- [ ] 所有测试通过（单元、集成、E2E）
- [ ] 代码或配置文件中无硬编码的Secrets
- [ ] 错误处理覆盖所有边缘情况
- [ ] 日志是结构化的（JSON）且不包含PII
- [ ] 健康检查端点返回有意义的的状态

### 基础设施
- [ ] Docker镜像可重现地构建（固定版本）
- [ ] 环境变量已记录并在启动时验证
- [ ] 已设置资源限制（CPU、内存）
- [ ] 已配置水平扩展（最小/最大实例数）
- [ ] 所有端点启用SSL/TLS

### 监控
- [ ] 导出应用指标（请求率、延迟、错误）
- [ ] 为错误率超过阈值配置告警
- [ ] 设置日志聚合（结构化日志、可搜索）
- [ ] 健康端点上的正常运行时间监控

### 安全
- [ ] 依赖项已扫描CVE
- [ ] CORS配置为仅允许的来源
- [ ] 公共端点启用速率限制
- [ ] 已验证身份验证和授权
- [ ] 已设置安全头（CSP、HSTS、X-Frame-Options）

### 运维
- [ ] 回滚计划已记录并测试
- [ ] 已针对生产规模数据测试数据库迁移
- [ ] 常见故障场景的操作手册
- [ ] 已定义值班轮换和升级路径