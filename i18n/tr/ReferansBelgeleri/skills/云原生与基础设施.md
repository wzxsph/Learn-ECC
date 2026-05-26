# Cloud Native ve Altyapı技能

本部分涵盖容器化、 orchestration、基础设施即代码和云原生架构。

---

## 容器化

### docker-patterns

**Kullanım**: Docker 容器En İyi Uygulamalar

**Temel Kavramlar**:
- 多阶段构建
- 最小化镜像大小
- 健康检查配置
- 日志Yönet
- 资源限制 (CPU/内存)

**Örnek**:
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

**Kullanım**: 容器Güvenlik

**检查项**:
- 最小化基础镜像
- 不使用 root 用户运行
- 密钥Yönet
- 镜像扫描
- 网络隔离

---

## Kubernetes

### kubernetes-ai

**Kullanım**: Kubernetes AI 工作负载

**Temel Kavramlar**:
- GPU 调度
- 分布式训练
- 模型服务
- 自动扩缩容

**工具**:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**Kullanım**: Kubernetes Güvenlik

**Güvenlik措施**:
- RBAC 配置
- 网络策略
- Pod Güvenlik策略
- 密钥Yönet
- 审计日志

### kube-bench

**Kullanım**: Kubernetes Güvenlik基准Test

**检查项**:
- CIS Kubernetes Benchmark
- 自动化Uyumluluk Kontrolü
- 修复建议

---

## 基础设施即代码

### terraform

**Kullanım**: Terraform 基础设施编排

**Temel Kavramlar**:
- HCL 配置语言
- Provider 开发
- 状态Yönet
- 模块化Tasarım
- 远程后端

**Örnek**:
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

**Kullanım**: Ansible 自动化配置

**核心功能**:
- 幂等性配置
- Rol与 playbook
- Jinja2 Şablon
- 库存Yönet

### helm

**Kullanım**: Helm Kubernetes 包Yönet

**Temel Kavramlar**:
- Chart 开发
- Şablon渲染
- 版本Yönet
- 仓库Yönet

### infrastructure-as-code

**Kullanım**: IaC En İyi Uygulamalar

**核心原则**:
- 版本控制
- 自动化部署
- 幂等性
- 文档化

---

## 云平台

### aws-solution-architect

**Kullanım**: AWS 架构Tasarım

**核心服务**:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**Kullanım**: GCP 架构Tasarım

**核心服务**:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**Kullanım**: Azure 架构Tasarım

**核心服务**:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## İzleme ve Gözlemlenebilirlik

### prometheus

**Kullanım**: Prometheus 监控

**Temel Kavramlar**:
- 指标采集
- PromQL 查询
- 告警规则
- 服务发现

**Örnek**:
```yaml
- alert: HighErrorRate
  expr: rate(http_errors_total[5m]) > 0.05
  for: 5m
  labels:
    severity: critical
```

### observability

**Kullanım**: 可观测性架构

**三大支柱**:
- 指标 (Metrics)
- 日志 (Logs)
- 追踪 (Traces)

### tracing

**Kullanım**: 分布式追踪

**工具**:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**Kullanım**: 日志En İyi Uygulamalar

**核心原则**:
- 结构化日志
- 日志级别
- 采样策略
- 集中存储

---

## 网络

### nginx

**Kullanım**: Nginx 配置与Yönet

**核心功能**:
- 反向代理
- Yük Dengeleme
- SSL/TLS 终止
- 缓存配置

### network-analysis

**Kullanım**: 网络分析

**工具**:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**Kullanım**: SSH 隧道

**用例**:
- 端口转发
- 跳板机
- VPN 替代

---

## 相关技能

| 技能 | Kullanım |
|------|------|
| `docker-patterns` | Docker 容器化 |
| `kubernetes-ai` | K8s AI 工作负载 |
| `terraform` | Terraform IaC |
| `prometheus` | Prometheus 监控 |
| `observability` | 可观测性 |