# Cloud-Native- und Infrastruktur-Faehigkeiten

Dieser Abschnitt behandelt Containerisierung, Orchestrierung, Infrastructure-as-Code und Cloud-Native-Architektur.

---

## Containerisierung

### docker-patterns

**Zweck**: Docker-Container-Best-Practices

**Kernkonzepte**:
- Multi-Stage-Builds
- Minimierung der Image-Groesse
- Health-Check-Konfiguration
- Log-Management
- Ressourcenlimits (CPU/Speicher)

**Beispiel**:
```dockerfile
# Multi-Stage-Build
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

**Zweck**: Container-Sicherheit

**Pruefpunkte**:
- Minimiertes Basis-Image
- Nicht als Root-Benutzer ausfuehren
- Schluesselverwaltung
- Image-Scanning
- Netzwerkisolierung

---

## Kubernetes

### kubernetes-ai

**Zweck**: Kubernetes AI-Workloads

**Kernkonzepte**:
- GPU-Scheduling
- Verteiltes Training
- Model-Serving
- Automatische Skalierung

**Werkzeuge**:
- Kubeflow
- KServe
- Argo Workflows

### k8s-security

**Zweck**: Kubernetes-Sicherheit

**Sicherheitsmassnahmen**:
- RBAC-Konfiguration
- Netzwerkrichtlinien
- Pod-Sicherheitsrichtlinien
- Schluesselverwaltung
- Audit-Logs

### kube-bench

**Zweck**: Kubernetes-Sicherheits-Benchmark

**Pruefpunkte**:
- CIS Kubernetes Benchmark
- Automatisierte Compliance-Pruefungen
- Reparaturempfehlungen

---

## Infrastructure-as-Code

### terraform

**Zweck**: Terraform-Infrastruktur-Orchestrierung

**Kernkonzepte**:
- HCL-Konfigurationssprache
- Provider-Entwicklung
- State-Management
- Modulares Design
- Remote-Backend

**Beispiel**:
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

**Zweck**: Ansible-Automatisierungskonfiguration

**Kernfunktionen**:
- Idempotente Konfiguration
- Rollen und Playbooks
- Jinja2-Templates
- Inventory-Management

### helm

**Zweck**: Helm Kubernetes-Paketmanagement

**Kernkonzepte**:
- Chart-Entwicklung
- Template-Rendering
- Versionsverwaltung
- Repository-Management

### infrastructure-as-code

**Zweck**: IaC-Best-Practices

**Kernprinzipien**:
- Versionskontrolle
- Automatisierte Bereitstellung
- Idempotenz
- Dokumentation

---

## Cloud-Plattformen

### aws-solution-architect

**Zweck**: AWS-Architektur-Design

**Kerndienste**:
- EC2, ECS, EKS
- S3, RDS, ElastiCache
- VPC, IAM
- CloudWatch, CloudFormation

### gcp-cloud-architect

**Zweck**: GCP-Architektur-Design

**Kerndienste**:
- Compute Engine
- Kubernetes Engine
- Cloud Storage
- BigQuery

### azure-cloud-architect

**Zweck**: Azure-Architektur-Design

**Kerndienste**:
- Azure VMs
- Azure Kubernetes Service
- Azure Storage
- Azure SQL

---

## Ueberwachung und Observability

### prometheus

**Zweck**: Prometheus-Ueberwachung

**Kernkonzepte**:
- Metriken-Sammlung
- PromQL-Abfragen
- Alert-Regeln
- Service-Discovery

**Beispiel**:
```yaml
- alert: HighErrorRate
  expr: rate(http_errors_total[5m]) > 0.05
  for: 5m
  labels:
    severity: critical
```

### observability

**Zweck**: Observability-Architektur

**Drei Saeulen**:
- Metriken (Metrics)
- Logs (Logs)
- Traces (Traces)

### tracing

**Zweck**: Verteiltes Tracing

**Werkzeuge**:
- Jaeger
- Zipkin
- OpenTelemetry

### logging-best-practices

**Zweck**: Logging-Best-Practices

**Kernprinzipien**:
- Strukturiertes Logging
- Log-Level
- Sampling-Strategie
- Zentralisierter Storage

---

## Netzwerk

### nginx

**Zweck**: Nginx-Konfiguration und -Management

**Kernfunktionen**:
- Reverse Proxy
- Load Balancing
- SSL/TLS-Terminierung
- Cache-Konfiguration

### network-analysis

**Zweck**: Netzwerkanalyse

**Werkzeuge**:
- Wireshark
- tcpdump
- netstat

### ssh-tunneling

**Zweck**: SSH-Tunneling

**Anwendungsfaelle**:
- Port-Forwarding
- Jump-Host
- VPN-Alternative

---

## Zugehoerige Faehigkeiten

| Faehigkeit | Verwendungszweck |
|------|------|
| `docker-patterns` | Docker-Containerisierung |
| `kubernetes-ai` | K8s AI-Workloads |
| `terraform` | Terraform-IaC |
| `prometheus` | Prometheus-Ueberwachung |
| `observability` | Observability |