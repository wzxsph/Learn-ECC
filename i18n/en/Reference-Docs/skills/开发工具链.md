# Development Toolchain Skills

This section covers development tools, testing, monitoring, logging, and other engineering practice skills.

---

## Testing Tools

### pytest

**Purpose**: Python pytest testing framework

**Core features**:
- Fixture system
- Parametrized tests
- Markers and skip
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

**Purpose**: JavaScript Jest testing framework

**Core features**:
- Snapshot testing
- Mock functions
- Async testing
- Coverage

### rspec

**Purpose**: Ruby RSpec testing framework

**Core concepts**:
- Describe context
- Matchers
- Mocks and stubs
- Shared examples

### minitest

**Purpose**: Ruby Minitest

**Features**:
- Lightweight
- Multiple style support
- Benchmarking

---

## Browser Automation

### playwright

**Purpose**: Playwright E2E testing

**Core features**:
- Multi-browser support
- Auto-waiting
- Network interception
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

**Purpose**: Puppeteer browser automation

**Uses**:
- Webpage screenshots
- PDF generation
- Crawling
- UI testing

### selenium

**Purpose**: Selenium WebDriver

**Features**:
- Multi-language support
- Multi-browser support
- Distributed execution

### monkey-testing

**Purpose**: Monkey testing

**Tools**:
- Firebase Test Lab
- Monkey (Android)
- UI AutoMonkey (iOS)

---

## Code Quality

### eslint

**Purpose**: ESLint code checking

**Core concepts**:
- Rule configuration
- Plugin development
- Auto-fix
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

**Purpose**: Prettier code formatting

**Features**:
- Opinionated formatting
- Multi-language support
- ESLint integration

### semgrep

**Purpose**: Semgrep static analysis

**Uses**:
- Security vulnerability detection
- Code smell detection
- Custom rules

### code-review

**Purpose**: Code review process

**Review points**:
- Code readability
- Logic correctness
- Security
- Performance
- Test coverage

---

## Debugging and Tracing

### debugging

**Purpose**: Debugging techniques

**Tools**:
- Chrome DevTools
- VS Code Debugger
- GDB
- LLDB

### troubleshooting

**Purpose**: Problem investigation

**Methods**:
- Binary search
- Log analysis
- Metric monitoring
-链路追踪

### profiling

**Purpose**: Performance analysis

**Tools**:
- Chrome Performance
- Node Clinic
- Python cProfile
- Go pprof

### benchmarking

**Purpose**: Benchmarking

**Tools**:
- k6
- Apache Bench
- JMeter
- Gatling

---

## CI/CD

### github-actions

**Purpose**: GitHub Actions CI/CD

**Core concepts**:
- Workflow configuration
- Action marketplace
- Matrix builds
- Caching

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

**Purpose**: GitLab CI/CD

**Core concepts**:
- Pipeline configuration
- Runner
- Caching and artifacts
- Deployment strategies

### jenkins

**Purpose**: Jenkins continuous integration

**Features**:
- Plugin ecosystem
- Distributed builds
- Pipeline as code

---

## Log Management

### logging-best-practices

**Purpose**: Logging best practices

**Core principles**:
- Structured logging (JSON)
- Log levels
- Contextual information
- Sampling strategies

**Tools**:
- ELK Stack
- Loki
- Splunk
- Datadog

### centralized-logging

**Purpose**: Centralized logging

**Architecture**:
- Log collection (Fluentd, Filebeat)
- Log storage (Elasticsearch)
- Log visualization (Kibana, Grafana)

---

## Monitoring and Alerting

### prometheus

**Purpose**: Prometheus monitoring

**Core concepts**:
- Pull model
- PromQL
- Alert rules
- Service discovery

### alertmanager

**Purpose**: Alertmanager alerting

**Features**:
- Alert grouping
- Inhibition
- Silencing
- Routing

### grafana

**Purpose**: Grafana visualization

**Uses**:
- Metric dashboards
- Alert rules
- Data source integration

---

## Documentation Tools

### swagger

**Purpose**: Swagger API documentation

**Tools**:
- Swagger UI
- OpenAPI Generator
- Swagger Editor

### postman

**Purpose**: Postman API testing

**Features**:
- Collection management
- Environment variables
- Automated testing
- Mock Server

### redoc

**Purpose**: ReDoc API documentation

**Features**:
- OpenAPI support
- Responsive design
- Theme customization

---

## Related Skills

| Skill | Purpose |
|-------|---------|
| `pytest` | Python testing |
| `playwright` | E2E testing |
| `eslint` | Code checking |
| `semgrep` | Static analysis |
| `github-actions` | CI/CD |
| `prometheus` | Monitoring |
| `grafana` | Visualization |