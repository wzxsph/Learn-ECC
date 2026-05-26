# Geliştirme Araç Zinciri技能

本部分涵盖开发工具、Test、监控、日志等工程实践技能。

---

## Test工具

### pytest

**Kullanım**: Python pytest TestÇerçeveler

**核心功能**:
- Fixture 系统
- 参数化Test
- 标记与跳过
- 覆盖率集成

**Örnek**:
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

**Kullanım**: JavaScript Jest TestÇerçeveler

**核心功能**:
- 快照Test
- 模拟函数
- 异步Test
- 覆盖率

### rspec

**Kullanım**: Ruby RSpec TestÇerçeveler

**Temel Kavramlar**:
- Tanım上下文
- 匹配器
- 模拟与存根
- 共享Örnek

### minitest

**Kullanım**: Ruby Minitest

**Özellikler**:
- 轻量级
- 多种风格Destekle
- 基准Test

---

## 浏览器自动化

### playwright

**Kullanım**: Playwright E2E Test

**核心功能**:
- 多浏览器Destekle
- 自动等待
- 网络拦截
- 追踪录制

**Örnek**:
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

**Kullanım**: Puppeteer 浏览器自动化

**Kullanım**:
- 网页截图
- PDF 生成
- 爬虫
- UI Test

### selenium

**Kullanım**: Selenium WebDriver

**Özellikler**:
- 多语言Destekle
- 多浏览器Destekle
- 分布式执行

### monkey-testing

**Kullanım**: 猴子Test

**工具**:
- Firebase Test Lab
- Monkey (Android)
- UI AutoMonkey (iOS)

---

## Kod Kalitesi

### eslint

**Kullanım**: ESLint 代码检查

**Temel Kavramlar**:
- 规则配置
- 插件开发
- 自动修复
- 配置继承

**Örnek**:
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

**Kullanım**: Prettier 代码格式化

**Özellikler**:
- opinionated 格式化
- 多语言Destekle
- 集成 ESLint

### semgrep

**Kullanım**: Semgrep 静态分析

**Kullanım**:
- Güvenlik Açıkları检测
- 代码味道检测
- 自定义规则

### code-review

**Kullanım**: Kod İnceleme流程

**审查要点**:
- 代码可读性
- 逻辑正确性
- Güvenlik性
- 性能
- Test覆盖

---

## 调试与追踪

### debugging

**Kullanım**: 调试技术

**工具**:
- Chrome DevTools
- VS Code Debugger
- GDB
- LLDB

### troubleshooting

**Kullanım**: 问题排查

**方法论**:
- 二分查找
- 日志分析
- 指标监控
- 链路追踪

### profiling

**Kullanım**: 性能分析

**工具**:
- Chrome Performance
- Node Clinic
- Python cProfile
- Go pprof

### benchmarking

**Kullanım**: 基准Test

**工具**:
- k6
- Apache Bench
- JMeter
- Gatling

---

## CI/CD

### github-actions

**Kullanım**: GitHub Actions CI/CD

**Temel Kavramlar**:
- Workflow 配置
- Action 市场
- 矩阵构建
- 缓存

**Örnek**:
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

**Kullanım**: GitHub CI/CD

**Temel Kavramlar**:
- Pipeline 配置
- Runner
- 缓存与 artifacts
- 部署策略

### jenkins

**Kullanım**: Jenkins 持续集成

**特性**:
- 插件生态
- 分布式构建
- 流水线即代码

---

## 日志Yönet

### logging-best-practices

**Kullanım**: 日志En İyi Uygulamalar

**核心原则**:
- 结构化日志 (JSON)
- 日志级别
- 上下文信息
- 采样策略

**工具**:
- ELK Stack
- Loki
- Splunk
- Datadog

### centralized-logging

**Kullanım**: 集中式日志

**架构**:
- 日志收集 (Fluentd, Filebeat)
- 日志存储 (Elasticsearch)
- 日志可视化 (Kibana, Grafana)

---

## 监控告警

### prometheus

**Kullanım**: Prometheus 监控

**Temel Kavramlar**:
- 拉取模型
- PromQL
- 告警规则
- 服务发现

### alertmanager

**Kullanım**: Alertmanager 告警

**功能**:
- 告警分组
- 抑制
- 静默
- 路由

### grafana

**Kullanım**: Grafana 可视化

**Kullanım**:
- 指标仪表板
- 告警规则
- 数据源集成

---

## 文档工具

### swagger

**Kullanım**: Swagger API 文档

**工具**:
- Swagger UI
- OpenAPI Generator
- Swagger Editor

### postman

**Kullanım**: Postman API Test

**功能**:
- 集合Yönet
- 环境变量
- 自动化Test
- Mock Server

### redoc

**Kullanım**: ReDoc API 文档

**Özellikler**:
- OpenAPI Destekle
- 响应式Tasarım
- 主题定制

---

## 相关技能

| 技能 | Kullanım |
|------|------|
| `pytest` | Python Test |
| `playwright` | E2E Test |
| `eslint` | 代码检查 |
| `semgrep` | 静态分析 |
| `github-actions` | CI/CD |
| `prometheus` | 监控 |
| `grafana` | 可视化 |