# Kỹ Năng Cloud-Native và Hạ Tầng

Phần này cover containerization, orchestration, infrastructure as code và cloud-native architecture.

---

## Containerization

### docker-patterns

**Mục đích**: Docker container best practice

**Core concept**:
- Multi-stage build
- Minimize image size
- Health check configuration
- Log management
- Resource limit (CPU/memory)

**Example**:
```dockerfile
# Multi-stage build
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

**Mục đích**: Container security

**Check item**:
- Minimize base image
- Don't run as root user
- Secret management
- Image scanning
- Network isolation

---

## Kubernetes

### kubernetes-ai

**Mục đích**: Kubernetes AI workload

**Core concept**:
- GPU scheduling
- Distributed training
- Model serving
- Auto scaling

**Tool**:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**Mục đích**: Kubernetes security

**Security measure**:
- RBAC configuration
- Network policy
- Pod security policy
- Secret management
- Audit log

### kube-bench

**Mục đích**: Kubernetes security benchmark

**Check item**:
- CIS Kubernetes Benchmark
- Automated compliance check
- Fix suggestion

---

## Infrastructure as Code

### terraform

**Mục đích**: Terraform infrastructure orchestration

**Core concept**:
- HCL configuration language
- Provider development
- State management
- Modular design
- Remote backend

**Example**:
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

**Mục đích**: Ansible automation configuration

**Core function**:
- Idempotent configuration
- Role và playbook
- Jinja2 template
- Inventory management

### helm

**Mục đích**: Helm Kubernetes package management

**Core concept**:
- Chart development
- Template rendering
- Version management
- Repository management

### infrastructure-as-code

**Mục đích**: IaC best practice

**Core principle**:
- Version control
- Automated deployment
- Idempotent
- Documentation

---

## Cloud Platform

### aws-solution-architect

**Mục đích**: AWS architecture design

**Core service**:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**Mục đích**: GCP architecture design

**Core service**:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**Mục đích**: Azure architecture design

**Core service**:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## Monitoring và Observability

### prometheus

**Mục đích**: Prometheus monitoring

**Core concept**:
- Metrics collection
- PromQL query
- Alert rule
- Service discovery

**Example**:
```yaml
- alert: HighErrorRate
  expr: rate(http_errors_total[5m]) > 0.05
  for: 5m
  labels:
    severity: critical
```

### observability

**Mục đích**: Observability architecture

**Three pillar**:
- Metrics
- Logs
- Traces

### tracing

**Mục đích**: Distributed tracing

**Tool**:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**Mục đích**: Log best practice

**Core principle**:
- Structured log
- Log level
- Sampling strategy
- Centralized storage

---

## Network

### nginx

**Mục đích**: Nginx configuration và management

**Core function**:
- Reverse proxy
- Load balancing
- SSL/TLS termination
- Cache configuration

### network-analysis

**Mục đích**: Network analysis

**Tool**:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**Mục đích**: SSH tunneling

**Use case**:
- Port forwarding
- Jump host
- VPN alternative

---

## Related Skill

| Skill | Usage |
|------|------|
| `docker-patterns` | Docker containerization |
| `kubernetes-ai` | K8s AI workload |
| `terraform` | Terraform IaC |
| `prometheus` | Prometheus monitoring |
| `observability` | Observability |