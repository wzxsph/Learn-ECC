# Cloud Native и инфраструктура

,  orchestration, . 

---

## 

### docker-patterns

**Назначение**: Docker Лучшие практики

**Основные концепции**:
- 
- 
- проверка
- Управлять
-  (CPU/память)

**Пример**:
```dockerfile
# 
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

**Назначение**: Безопасность

**проверка**:
- 
-  root 
- Управлять
- 
- 

---

## Kubernetes

### kubernetes-ai

**Назначение**: Kubernetes AI 

**Основные концепции**:
- GPU 
- 
- 
- 

****:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**Назначение**: Kubernetes Безопасность

**Безопасность**:
- RBAC 
- 
- Pod Безопасность
- Управлять
- 

### kube-bench

**Назначение**: Kubernetes БезопасностьТестирование

**проверка**:
- CIS Kubernetes Benchmark
- Проверка соответствия
- 

---

## 

### terraform

**Назначение**: Terraform 

**Основные концепции**:
- HCL 
- Provider 
- управление состоянием
- Спроектировать
- 

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

**Назначение**: Ansible 

**функция**:
- 
- Роль playbook
- Jinja2 Шаблон
- Управлять

### helm

**Назначение**: Helm Kubernetes Управлять

**Основные концепции**:
- Chart 
- Шаблон
- Управлять
- Управлять

### infrastructure-as-code

**Назначение**: IaC Лучшие практики

****:
- 
- 
- 
- 

---

## 

### aws-solution-architect

**Назначение**: AWS Спроектировать

****:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**Назначение**: GCP Спроектировать

****:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**Назначение**: Azure Спроектировать

****:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## Мониторинг и наблюдаемость

### prometheus

**Назначение**: Prometheus 

**Основные концепции**:
- 
- PromQL 
- 
- 

**Пример**:
```yaml
- alert: HighErrorRate
  expr: rate(http_errors_total[5m]) > 0.05
  for: 5m
  labels:
    severity: critical
```

### observability

**Назначение**: 

****:
-  (Metrics)
-  (Logs)
-  (Traces)

### tracing

**Назначение**: 

****:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**Назначение**: Лучшие практики

****:
- 
- 
- 
- 

---

## 

### nginx

**Назначение**: Nginx Управлять

**функция**:
- 
- Балансировка нагрузки
- SSL/TLS 
- 

### network-analysis

**Назначение**: 

****:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**Назначение**: SSH 

****:
- 
- 
- VPN 

---

## 

|  | Назначение |
|------|------|
| `docker-patterns` | Docker  |
| `kubernetes-ai` | K8s AI  |
| `terraform` | Terraform IaC |
| `prometheus` | Prometheus  |
| `observability` |  |