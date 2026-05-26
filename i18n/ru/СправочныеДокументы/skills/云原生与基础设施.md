# Cloud Native и инфраструктура技能

本部分涵盖容器化、 orchestration、基础设施即代码和云原生架构。

---

## 容器化

### docker-patterns

**Назначение**: Docker 容器Лучшие практики

**Основные концепции**:
- 多阶段构建
- 最小化镜像大小
- 健康检查配置
- 日志Управлять
- 资源限制 (CPU/内存)

**Пример**:
```dockerfile
# 多阶段构建
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
CMD ["node", "dist/index.js"]
```

### container-security

**Назначение**: 容器Безопасность

**检查项**:
- 最小化基础镜像
- 不使用 root 用户运行
- 密钥Управлять
- 镜像扫描
- 网络隔离

---

## Kubernetes

### kubernetes-ai

**Назначение**: Kubernetes AI 工作负载

**Основные концепции**:
- GPU 调度
- 分布式训练
- 模型服务
- 自动扩缩容

**工具**:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**Назначение**: Kubernetes Безопасность

**Безопасность措施**:
- RBAC 配置
- 网络策略
- Pod Безопасность策略
- 密钥Управлять
- 审计日志

### kube-bench

**Назначение**: Kubernetes Безопасность基准Тестирование

**检查项**:
- CIS Kubernetes Benchmark
- 自动化Проверка соответствия
- 修复建议

---

## 基础设施即代码

### terraform

**Назначение**: Terraform 基础设施编排

**Основные концепции**:
- HCL 配置语言
- Provider 开发
- 状态Управлять
- 模块化Спроектировать
- 远程后端

**Пример**:
```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"
  subnet_id     = aws_subnet.public.id
  
  tags = {
    Name = "web-server"
  }
}
```

### ansible

**Назначение**: Ansible 自动化配置

**核心功能**:
- 幂等性配置
- Роль与 playbook
- Jinja2 Шаблон
- 库存Управлять

### helm

**Назначение**: Helm Kubernetes 包Управлять

**Основные концепции**:
- Chart 开发
- Шаблон渲染
- 版本Управлять
- 仓库Управлять

### infrastructure-as-code

**Назначение**: IaC Лучшие практики

**核心原则**:
- 版本控制
- 自动化部署
- 幂等性
- 文档化

---

## 云平台

### aws-solution-architect

**Назначение**: AWS 架构Спроектировать

**核心服务**:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**Назначение**: GCP 架构Спроектировать

**核心服务**:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**Назначение**: Azure 架构Спроектировать

**核心服务**:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## Мониторинг и наблюдаемость

### prometheus

**Назначение**: Prometheus 监控

**Основные концепции**:
- 指标采集
- PromQL 查询
- 告警规则
- 服务发现

**Пример**:
```yaml
- alert: HighErrorRate
  expr: rate(http_errors_total[5m]) > 0.05
  for: 5m
  labels:
    severity: critical
```

### observability

**Назначение**: 可观测性架构

**三大支柱**:
- 指标 (Metrics)
- 日志 (Logs)
- 追踪 (Traces)

### tracing

**Назначение**: 分布式追踪

**工具**:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**Назначение**: 日志Лучшие практики

**核心原则**:
- 结构化日志
- 日志级别
- 采样策略
- 集中存储

---

## 网络

### nginx

**Назначение**: Nginx 配置与Управлять

**核心功能**:
- 反向代理
- Балансировка нагрузки
- SSL/TLS 终止
- 缓存配置

### network-analysis

**Назначение**: 网络分析

**工具**:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**Назначение**: SSH 隧道

**用例**:
- 端口转发
- 跳板机
- VPN 替代

---

## 相关技能

| 技能 | Назначение |
|------|------|
| `docker-patterns` | Docker 容器化 |
| `kubernetes-ai` | K8s AI 工作负载 |
| `terraform` | Terraform IaC |
| `prometheus` | Prometheus 监控 |
| `observability` | 可观测性 |