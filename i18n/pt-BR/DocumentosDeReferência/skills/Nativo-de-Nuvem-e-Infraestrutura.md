# Nativo-de-Nuvem-e-Infraestrutura

Esta seção abrange containerização, orquestração, infraestrutura como código e arquitetura nativo de nuvem.

---

## Containerização

### docker-patterns

**Propósito**: Melhores-Práticas de containers Docker

**Conceitos Centrais**:
- Builds multi-stage
- Minimização de tamanho de imagem
- Configuração de health checks
- Gerenciamento de logs
- Limites de recursos (CPU/memória)

**Exemplo**:
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

**Propósito**: Segurança de containers

**Itens de Verificação**:
- Minimizar imagem base
- Não executar como root
- Gerenciamento de chaves
- Scan de imagens
- Isolamento de rede

---

## Kubernetes

### kubernetes-ai

**Propósito**: Cargas de trabalho AI no Kubernetes

**Conceitos Centrais**:
- Agendamento de GPU
- Treinamento distribuído
- Servindo de modelos
- Auto scaling

**Ferramentas**:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**Propósito**: Segurança Kubernetes

**Medidas de Segurança**:
- Configuração RBAC
- Políticas de rede
- Pod Security Policies
- Gerenciamento de chaves
- Logs de auditoria

### kube-bench

**Propósito**: Benchmark de segurança Kubernetes

**Itens de Verificação**:
- CIS Kubernetes Benchmark
- Verificações de conformidade automatizadas
- Recomendações de correção

---

## Infraestrutura como Código

### terraform

**Propósito**: Orquestração de infraestrutura Terraform

**Conceitos Centrais**:
- Linguagem de configuração HCL
- Desenvolvimento de providers
- Gerenciamento de estado
- Design modular
- Backends remotos

**Exemplo**:
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

**Propósito**: Automação de configuração Ansible

**Funcionalidades Centrais**:
- Configuração idempotente
- Roles e playbooks
- Templates Jinja2
- Gerenciamento de inventário

### helm

**Propósito**: Gerenciamento de pacotes Helm Kubernetes

**Conceitos Centrais**:
- Desenvolvimento de Charts
- Renderização de templates
- Gerenciamento de versões
- Gerenciamento de repositórios

### infrastructure-as-code

**Propósito**: Melhores-Práticas de IaC

**Princípios Centrais**:
- Versionamento
- Deploy automatizado
- Idempotência
- Documentação

---

## Plataformas de Nuvem

### aws-solution-architect

**Propósito**: Design de arquitetura AWS

**Serviços Centrais**:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**Propósito**: Design de arquitetura GCP

**Serviços Centrais**:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**Propósito**: Design de arquitetura Azure

**Serviços Centrais**:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## Monitoramento-e-Observabilidade

### prometheus

**Propósito**: Monitoramento Prometheus

**Conceitos Centrais**:
- Coleta de métricas
- Consultas PromQL
- Regras de alertas
- Descoberta de serviços

**Exemplo**:
```yaml
- alert: HighErrorRate
  expr: rate(http_errors_total[5m]) > 0.05
  for: 5m
  labels:
    severity: critical
```

### observability

**Propósito**: Arquitetura de observabilidade

**Três Pilares**:
- Métricas (Metrics)
- Logs (Logs)
- Rastreamento (Traces)

### tracing

**Propósito**: Rastreamento distribuído

**Ferramentas**:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**Propósito**: Melhores-Práticas de logs

**Princípios Centrais**:
- Logs estruturados (JSON)
- Níveis de log
- Informações de contexto
- Estratégias de amostragem
- Armazenamento centralizado

---

## Rede

### nginx

**Propósito**: Configuração e gerenciamento do Nginx

**Funcionalidades Centrais**:
- Proxy reverso
- Load balancing
- Terminção SSL/TLS
- Configuração de cache

### network-analysis

**Propósito**: Análise de rede

**Ferramentas**:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**Propósito**: Túneis SSH

**Casos de Uso**:
- Port forwarding
- Bastion hosts
- Alternativa VPN

---

## Habilidades Relacionadas

| Habilidade | Propósito |
|------|------|
| `docker-patterns` | Containerização Docker |
| `kubernetes-ai` | Cargas de trabalho K8s AI |
| `terraform` | IaC Terraform |
| `prometheus` | Monitoramento Prometheus |
| `observability` | Observabilidade |