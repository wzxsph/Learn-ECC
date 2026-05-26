# Cloud Native and Infrastructure Skills

This section covers containerization, orchestration, infrastructure as code, and cloud-native architecture.

---

## Containerization

### docker-patterns

**Purpose**: Docker container best practices

**Core concepts**:
- Multi-stage builds
- Minimizing image size
- Health check configuration
- Log management
- Resource limits (CPU/memory)

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

**Purpose**: Container security

**Checklist**:
- Minimize base image
- Do not run as root user
- Secret management
- Image scanning
- Network isolation

---

## Kubernetes

### kubernetes-ai

**Purpose**: Kubernetes AI workloads

**Core concepts**:
- GPU scheduling
- Distributed training
- Model serving
- Auto-scaling

**Tools**:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**Purpose**: Kubernetes security

**Security measures**:
- RBAC configuration
- Network policies
- Pod security policies
- Secret management
- Audit logs

### kube-bench

**Purpose**: Kubernetes security benchmark testing

**Checks**:
- CIS Kubernetes Benchmark
- Automated compliance checks
- Remediation suggestions

---

## Infrastructure as Code

### terraform

**Purpose**: Terraform infrastructure orchestration

**Core concepts**:
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

**Purpose**: Ansible automation configuration

**Core features**:
- Idempotent configuration
- Roles and playbooks
- Jinja2 templates
- Inventory management

### helm

**Purpose**: Helm Kubernetes package management

**Core concepts**:
- Chart development
- Template rendering
- Version management
- Repository management

### infrastructure-as-code

**Purpose**: IaC best practices

**Core principles**:
- Version control
- Automated deployment
- Idempotency
- Documentation

---

## Cloud Platforms

### aws-solution-architect

**Purpose**: AWS architecture design

**Core services**:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**Purpose**: GCP architecture design

**Core services**:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**Purpose**: Azure architecture design

**Core services**:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## Monitoring and Observability

### prometheus

**Purpose**: Prometheus monitoring

**Core concepts**:
- Metric collection
- PromQL queries
- Alert rules
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

**Purpose**: Observability architecture

**Three pillars**:
- Metrics
- Logs
- Traces

### tracing

**Purpose**: Distributed tracing

**Tools**:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**Purpose**: Logging best practices

**Core principles**:
- Structured logging
- Log levels
- Sampling strategy
- Centralized storage

---

## Networking

### nginx

**Purpose**: Nginx configuration and management

**Core features**:
- Reverse proxy
- Load balancing
- SSL/TLS termination
- Cache configuration

### network-analysis

**Purpose**: Network analysis

**Tools**:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**Purpose**: SSH tunneling

**Use cases**:
- Port forwarding
- Jump host
- VPN alternative

---

## Related Skills

| Skill | Purpose |
|-------|---------|
| `docker-patterns` | Docker containerization |
| `kubernetes-ai` | K8s AI workloads |
| `terraform` | Terraform IaC |
| `prometheus` | Prometheus monitoring |
| `observability` | Observability |