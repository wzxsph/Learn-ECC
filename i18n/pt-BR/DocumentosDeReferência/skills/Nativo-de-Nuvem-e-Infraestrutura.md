# Nativo-de-Nuvem-e-Infraestrutura技能

本部分涵盖容器化、 orchestration、基础设施即代码和云原生架构。

---

## 容器化

### docker-patterns

**用途**: Docker 容器Melhores-Práticas

**核心概念**:
- 多阶段构建
- 最小化镜像大小
- 健康检查配置
- 日志管理
- 资源限制 (CPU/内存)

**示例**:
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

**用途**: 容器Segurança

**检查项**:
- 最小化基础镜像
- 不使用 root 用户运行
- 密钥管理
- 镜像扫描
- 网络隔离

---

## Kubernetes

### kubernetes-ai

**用途**: Kubernetes AI 工作负载

**核心概念**:
- GPU 调度
- 分布式训练
- 模型服务
- 自动扩缩容

**工具**:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**用途**: Kubernetes Segurança

**安全措施**:
- RBAC 配置
- 网络策略
- Pod 安全策略
- 密钥管理
- 审计日志

### kube-bench

**用途**: Kubernetes 安全基准测试

**检查项**:
- CIS Kubernetes Benchmark
- 自动化合规检查
- 修复建议

---

## 基础设施即代码

### terraform

**用途**: Terraform 基础设施编排

**核心概念**:
- HCL 配置语言
- Provider 开发
- 状态管理
- 模块化设计
- 远程后端

**示例**:
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

**用途**: Ansible 自动化配置

**核心功能**:
- 幂等性配置
- 角色与 playbook
- Jinja2 模板
- 库存管理

### helm

**用途**: Helm Kubernetes 包管理

**核心概念**:
- Chart 开发
- 模板渲染
- 版本管理
- 仓库管理

### infrastructure-as-code

**用途**: IaC Melhores-Práticas

**核心原则**:
- 版本控制
- 自动化部署
- 幂等性
- 文档化

---

## 云平台

### aws-solution-architect

**用途**: AWS 架构设计

**核心服务**:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**用途**: GCP 架构设计

**核心服务**:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**用途**: Azure 架构设计

**核心服务**:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## Monitoramento-e-Observabilidade

### prometheus

**用途**: Prometheus 监控

**核心概念**:
- 指标采集
- PromQL 查询
- 告警规则
- 服务发现

**示例**:
```yaml
- alert: HighErrorRate
  expr: rate(http_errors_total[5m]) > 0.05
  for: 5m
  labels:
    severity: critical
```

### observability

**用途**: 可观测性架构

**三大支柱**:
- 指标 (Metrics)
- 日志 (Logs)
- 追踪 (Traces)

### tracing

**用途**: 分布式追踪

**工具**:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**用途**: 日志Melhores-Práticas

**核心原则**:
- 结构化日志
- 日志级别
- 采样策略
- 集中存储

---

## 网络

### nginx

**用途**: Nginx 配置与管理

**核心功能**:
- 反向代理
- 负载均衡
- SSL/TLS 终止
- 缓存配置

### network-analysis

**用途**: 网络分析

**工具**:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**用途**: SSH 隧道

**用例**:
- 端口转发
- 跳板机
- VPN 替代

---

## 相关技能

| 技能 | 用途 |
|------|------|
| `docker-patterns` | Docker 容器化 |
| `kubernetes-ai` | K8s AI 工作负载 |
| `terraform` | Terraform IaC |
| `prometheus` | Prometheus 监控 |
| `observability` | 可观测性 |