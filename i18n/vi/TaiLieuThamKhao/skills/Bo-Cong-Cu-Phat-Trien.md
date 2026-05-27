# Kỹ Năng Bộ Công Cụ Phát Triển

Phần này cover development tool, testing, monitoring, logging và các engineering practice skill khác.

---

## Testing Tool

### pytest

**Mục đích**: Python pytest testing framework

**Core function**:
- Fixture system
- Parametric test
- Marker và skip
- Coverage integration

**Example**:
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

**Mục đích**: JavaScript Jest testing framework

**Core function**:
- Snapshot testing
- Mock function
- Async testing
- Coverage

### rspec

**Mục đích**: Ruby RSpec testing framework

**Core concept**:
- Describe context
- Matcher
- Mock và stub
- Shared example

### minitest

**Mục đích**: Ruby Minitest

**Feature**:
- Lightweight
- Multiple style support
- Benchmark

---

## Browser Automation

### playwright

**Mục đích**: Playwright E2E testing

**Core function**:
- Multi-browser support
- Auto wait
- Network intercept
- Trace recording

**Example**:
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

**Mục đích**: Puppeteer browser automation

**Usage**:
- Webpage screenshot
- PDF generation
- Crawler
- UI testing

### selenium

**Mục đích**: Selenium WebDriver

**Feature**:
- Multi-language support
- Multi-browser support
- Distributed execution

### monkey-testing

**Mục đích**: Monkey testing

**Tool**:
- Firebase Test Lab
- Monkey (Android)
- UI AutoMonkey (iOS)

---

## Code Quality

### eslint

**Mục đích**: ESLint code linting

**Core concept**:
- Rule configuration
- Plugin development
- Auto fix
- Config inheritance

**Example**:
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

**Mục đích**: Prettier code formatting

**Feature**:
- Opinionated formatting
- Multi-language support
- ESLint integration

### semgrep

**Mục đích**: Semgrep static analysis

**Usage**:
- Security vulnerability detection
- Code smell detection
- Custom rule

### code-review

**Mục đích**: Code review process

**Review point**:
- Code readability
- Logic correctness
- Security
- Performance
- Test coverage

---

## Debugging và Tracing

### debugging

**Mục đích**: Debugging technique

**Tool**:
- Chrome DevTools
- VS Code Debugger
- GDB
- LLDB

### troubleshooting

**Mục đích**: Troubleshooting

**Methodology**:
- Binary search
- Log analysis
- Metrics monitoring
- Trace chaining

### profiling

**Mục đích**: Performance analysis

**Tool**:
- Chrome Performance
- Node Clinic
- Python cProfile
- Go pprof

### benchmarking

**Mục đích**: Benchmarking

**Tool**:
- k6
- Apache Bench
- JMeter
- Gatling

---

## CI/CD

### github-actions

**Mục đích**: GitHub Actions CI/CD

**Core concept**:
- Workflow configuration
- Action marketplace
- Matrix build
- Cache

**Example**:
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

**Mục đích**: GitLab CI/CD

**Core concept**:
- Pipeline configuration
- Runner
- Cache và artifact
- Deployment strategy

### jenkins

**Mục đích**: Jenkins continuous integration

**Feature**:
- Plugin ecosystem
- Distributed build
- Pipeline as code

---

## Log Management

### logging-best-practices

**Mục đích**: Log best practice

**Core principle**:
- Structured log (JSON)
- Log level
- Context information
- Sampling strategy

**Tool**:
- ELK Stack
- Loki
- Splunk
- Datadog

### centralized-logging

**Mục đích**: Centralized logging

**Architecture**:
- Log collection (Fluentd, Filebeat)
- Log storage (Elasticsearch)
- Log visualization (Kibana, Grafana)

---

## Monitoring Alert

### prometheus

**Mục đích**: Prometheus monitoring

**Core concept**:
- Pull model
- PromQL
- Alert rule
- Service discovery

### alertmanager

**Mục đích**: Alertmanager alerting

**Function**:
- Alert grouping
- Inhibition
- Silencing
- Routing

### grafana

**Mục đích**: Grafana visualization

**Usage**:
- Metrics dashboard
- Alert rule
- Data source integration

---

## Documentation Tool

### swagger

**Mục đích**: Swagger API documentation

**Tool**:
- Swagger UI
- OpenAPI Generator
- Swagger Editor

### postman

**Mục đích**: Postman API testing

**Function**:
- Collection management
- Environment variable
- Automated testing
- Mock Server

### redoc

**Mục đích**: ReDoc API documentation

**Feature**:
- OpenAPI support
- Responsive design
- Theme customization

---

## Related Skill

| Skill | Usage |
|------|------|
| `pytest` | Python testing |
| `playwright` | E2E testing |
| `eslint` | Code linting |
| `semgrep` | Static analysis |
| `github-actions` | CI/CD |
| `prometheus` | Monitoring |
| `grafana` | Visualization |