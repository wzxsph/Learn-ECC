# Cadeia-de-Ferramentas-de-Desenvolvimento

Esta seção abrange ferramentas de desenvolvimento, práticas de engenharia como teste, monitoramento, logs, etc.

---

## Ferramentas de Teste

### pytest

**Propósito**: Framework de teste Python pytest

**Funcionalidades Centrais**:
- Sistema de fixtures
- Testes parametrizados
- Marcação e skip
- Integração de cobertura

**Exemplo**:
```python
import pytest

@pytest.fixture
def database():
    db = Database(":memory:")
    yield db
    db.close()

@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("world", "WORLD"),
])
def test_uppercase(input, expected):
    assert input.upper() == expected

def test_with_db(database):
    result = database.query("SELECT 1")
    assert result == 1
```

### jest

**Propósito**: Framework de teste JavaScript Jest

**Funcionalidades Centrais**:
- Testes de snapshots
- Funções de mock
- Testes assíncronos
- Cobertura

### rspec

**Propósito**: Framework de teste Ruby RSpec

**Conceitos Centrais**:
- Describe context
- Matchers
- Mocks e stubs
- Shared examples

### minitest

**Propósito**: Ruby Minitest

**Características**:
- Leve
- Suporte a múltiplos estilos
- Benchmark tests

---

## Automação de Navegador

### playwright

**Propósito**: Testes E2E Playwright

**Funcionalidades Centrais**:
- Suporte multi-navegador
- Espera automática
- Intercepção de rede
- Recording de traces

**Exemplo**:
```typescript
import { test, expect } from '@playwright/test';

test('login flow', async ({ page }) => {
  await page.goto('/login');
  await page.fill('[name="email"]', 'user@example.com');
  await page.fill('[name="password"]', 'password');
  await page.click('button[type="submit"]');
  await expect(page).toHaveURL('/dashboard');
});
```

### puppeteer

**Propósito**: Automação de navegador Puppeteer

**Propósito**:
- Capturas de tela de páginas web
- Geração de PDF
- Scraping
- Testes de UI

### selenium

**Propósito**: Selenium WebDriver

**Características**:
- Suporte multi-linguagem
- Suporte multi-navegador
- Execução distribuída

### monkey-testing

**Propósito**: Testes monkey

**Ferramentas**:
- Firebase Test Lab
- Monkey (Android)
- UI AutoMonkey (iOS)

---

## Qualidade de Código

### eslint

**Propósito**: Linting de código ESLint

**Conceitos Centrais**:
- Configuração de regras
- Desenvolvimento de plugins
- Auto fix
- Herança de configuração

**Exemplo**:
```json
{
  "rules": {
    "no-unused-vars": "error",
    "prefer-const": "error",
    "semi": ["error", "always"]
  }
}
```

### prettier

**Propósito**: Formatação de código Prettier

**Características**:
- Formatação opinionada
- Suporte multi-linguagem
- Integração com ESLint

### semgrep

**Propósito**: Análise estática Semgrep

**Propósito**:
- Detecção de vulnerabilidades de segurança
- Detecção de code smells
- Regras personalizadas

### code-review

**Propósito**: Processo de revisão de código

**Pontos de Verificação**:
- Legibilidade de código
- Correção de lógica
- Segurança
- Performance
- Cobertura de testes

---

## Debug e Tracing

### debugging

**Propósito**: Técnicas de debugging

**Ferramentas**:
- Chrome DevTools
- VS Code Debugger
- GDB
- LLDB

### troubleshooting

**Propósito**: Troubleshooting

**Metodologias**:
- Busca binária
- Análise de logs
- Monitoramento de métricas
- Rastreamento de cadeia

### profiling

**Propósito**: Análise de performance

**Ferramentas**:
- Chrome Performance
- Node Clinic
- Python cProfile
- Go pprof

### benchmarking

**Propósito**: Benchmarking

**Ferramentas**:
- k6
- Apache Bench
- JMeter
- Gatling

---

## CI/CD

### github-actions

**Propósito**: GitHub Actions CI/CD

**Conceitos Centrais**:
- Configuração de workflows
- Marketplace de Actions
- Build em matriz
- Cache

**Exemplo**:
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test
```

### gitlab-ci

**Propósito**: GitHub CI/CD

**Conceitos Centrais**:
- Configuração de pipeline
- Runner
- Cache e artifacts
- Estratégias de deploy

### jenkins

**Propósito**: Integração contínua Jenkins

**Características**:
- Ecossistema de plugins
- Build distribuído
- Pipeline as code

---

## Gerenciamento de Logs

### logging-best-practices

**Propósito**: Melhores-Práticas de logs

**Princípios Centrais**:
- Logs estruturados (JSON)
- Níveis de log
- Informações de contexto
- Estratégias de amostragem

**Ferramentas**:
- ELK Stack
- Loki
- Splunk
- Datadog

### centralized-logging

**Propósito**: Logs centralizados

**Arquitetura**:
- Coleta de logs (Fluentd, Filebeat)
- Armazenamento de logs (Elasticsearch)
- Visualização de logs (Kibana, Grafana)

---

## Monitoramento e Alertas

### prometheus

**Propósito**: Monitoramento Prometheus

**Conceitos Centrais**:
- Modelo pull
- PromQL
- Regras de alertas
- Descoberta de serviços

### alertmanager

**Propósito**: Alertmanager de alertas

**Funcionalidades**:
- Agrupamento de alertas
- Supressão
- Silenciamento
- Roteamento

### grafana

**Propósito**: Visualização Grafana

**Propósito**:
- Dashboards de métricas
- Regras de alertas
- Integração de fontes de dados

---

## Ferramentas de Documentação

### swagger

**Propósito**: Documentação de API Swagger

**Ferramentas**:
- Swagger UI
- OpenAPI Generator
- Swagger Editor

### postman

**Propósito**: Teste de API Postman

**Funcionalidades**:
- Gerenciamento de collections
- Variáveis de ambiente
- Testes automatizados
- Mock Server

### redoc

**Propósito**: Documentação de API ReDoc

**Características**:
- Suporte a OpenAPI
- Design responsivo
- Personalização de temas

---

## Habilidades Relacionadas

| Habilidade | Propósito |
|------|------|
| `pytest` | Testes Python |
| `playwright` | Testes E2E |
| `eslint` | Linting de código |
| `semgrep` | Análise estática |
| `github-actions` | CI/CD |
| `prometheus` | Monitoramento |
| `grafana` | Visualização |