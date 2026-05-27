# Entwicklung-Toolchain-Faehigkeiten

Dieser Abschnitt behandelt Entwicklungstools, Testing, Monitoring, Logging und andere Engineering-Praxis-Faehigkeiten.

---

## Test-Tools

### pytest

**Zweck**: Python pytest Test-Framework

**Kernfunktionen**:
- Fixture-System
- Parametrisierte Tests
- Markierungen und Ueberspringen
- Abdeckungsintegration

**Beispiel**:
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

**Zweck**: JavaScript Jest Test-Framework

**Kernfunktionen**:
- Snapshot-Tests
- Mock-Funktionen
- Asynchrone Tests
- Abdeckung

### rspec

**Zweck**: Ruby RSpec Test-Framework

**Kernkonzepte**:
- Describe-Context
- Matcher
- Mocks und Stubs
- Shared Examples

### minitest

**Zweck**: Ruby Minitest

**Eigenschaften**:
- Leichtgewichtig
- Mehrere Stilunterstuetzung
- Benchmarking

---

## Browser-Automatisierung

### playwright

**Zweck**: Playwright E2E-Tests

**Kernfunktionen**:
- Multi-Browser-Unterstuetzung
- Automatisches Warten
- Netzwerk-Intercept
- Trace-Aufzeichnung

**Beispiel**:
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

**Zweck**: Puppeteer Browser-Automatisierung

**Verwendung**:
- Web-Screenshots
- PDF-Generierung
- Web-Scraping
- UI-Tests

### selenium

**Zweck**: Selenium WebDriver

**Eigenschaften**:
- Multi-Sprache-Unterstuetzung
- Multi-Browser-Unterstuetzung
- Verteilte Ausfuehrung

### monkey-testing

**Zweck**: Monkey-Testing

**Tools**:
- Firebase Test Lab
- Monkey (Android)
- UI AutoMonkey (iOS)

---

## Code-Qualitaet

### eslint

**Zweck**: ESLint Code-Linting

**Kernkonzepte**:
- Regelkonfiguration
- Plugin-Entwicklung
- Automatische Korrektur
- Konfigurationsvererbung

**Beispiel**:
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

**Zweck**: Prettier Code-Formatierung

**Eigenschaften**:
- Opinionated Formatierung
- Multi-Sprache-Unterstuetzung
- ESLint-Integration

### semgrep

**Zweck**: Semgrep Statische Analyse

**Verwendung**:
- Sicherheitsluecken-Erkennung
- Code-Smell-Erkennung
- Benutzerdefinierte Regeln

### code-review

**Zweck**: Code-Review-Prozess

**Pruefpunkte**:
- Code-Lesbarkeit
- Logik-Korrektheit
- Sicherheit
- Performance
- Testabdeckung

---

## Debugging und Tracing

### debugging

**Zweck**: Debugging-Techniken

**Tools**:
- Chrome DevTools
- VS Code Debugger
- GDB
- LLDB

### troubleshooting

**Zweck**: Problemloesung

**Methodik**:
- Binaere Suche
- Log-Analyse
- Metrik-Monitoring
- Distributed Tracing

### profiling

**Zweck**: Performance-Analyse

**Tools**:
- Chrome Performance
- Node Clinic
- Python cProfile
- Go pprof

### benchmarking

**Zweck**: Benchmarking

**Tools**:
- k6
- Apache Bench
- JMeter
- Gatling

---

## CI/CD

### github-actions

**Zweck**: GitHub Actions CI/CD

**Kernkonzepte**:
- Workflow-Konfiguration
- Action-Marktplatz
- Matrix-Builds
- Caching

**Beispiel**:
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

**Zweck**: GitLab CI/CD

**Kernkonzepte**:
- Pipeline-Konfiguration
- Runner
- Caching und Artifacts
- Deployment-Strategien

### jenkins

**Zweck**: Jenkins Continous Integration

**Eigenschaften**:
- Plugin-Oekosystem
- Verteilte Builds
- Pipeline-as-Code

---

## Log-Verwaltung

### logging-best-practices

**Zweck**: Logging-Best-Practices

**Kernprinzipien**:
- Strukturiertes Logging (JSON)
- Log-Level
- Kontextinformationen
- Sampling-Strategie

**Tools**:
- ELK Stack
- Loki
- Splunk
- Datadog

### centralized-logging

**Zweck**: Zentralisiertes Logging

**Architektur**:
- Log-Sammlung (Fluentd, Filebeat)
- Log-Speicherung (Elasticsearch)
- Log-Visualisierung (Kibana, Grafana)

---

## Monitoring und Alarme

### prometheus

**Zweck**: Prometheus Monitoring

**Kernkonzepte**:
- Pull-Modell
- PromQL
- Alarm-Regeln
- Service-Discovery

### alertmanager

**Zweck**: Alertmanager Alarme

**Funktionen**:
- Alarm-Gruppierung
- Inhibition
- Stille
- Routing

### grafana

**Zweck**: Grafana Visualisierung

**Verwendung**:
- Metrik-Dashboards
- Alarm-Regeln
- Datenquellen-Integration

---

## Dokumentations-Tools

### swagger

**Zweck**: Swagger API-Dokumentation

**Tools**:
- Swagger UI
- OpenAPI Generator
- Swagger Editor

### postman

**Zweck**: Postman API-Tests

**Funktionen**:
- Sammlungsverwaltung
- Umgebungsvariablen
- Automatisierte Tests
- Mock-Server

### redoc

**Zweck**: ReDoc API-Dokumentation

**Eigenschaften**:
- OpenAPI-Unterstuetzung
- Responsive Design
- Theme-Anpassung

---

## Zugehoerige Faehigkeiten

| Faehigkeit | Verwendungszweck |
|------|------|
| `pytest` | Python-Tests |
| `playwright` | E2E-Tests |
| `eslint` | Code-Linting |
| `semgrep` | Statische Analyse |
| `github-actions` | CI/CD |
| `prometheus` | Monitoring |
| `grafana` | Visualisierung |