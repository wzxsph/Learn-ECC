# Cadeia-de-Ferramentas-de-Desenvolvimento技能

本部分涵盖开发工具、测试、监控、日志等工程实践技能。

---

## 测试工具

### pytest

**用途**: Python pytest 测试框架

**核心功能**:
- Fixture 系统
- 参数化测试
- 标记与跳过
- 覆盖率集成

**示例**:
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

**用途**: JavaScript Jest 测试框架

**核心功能**:
- 快照测试
- 模拟函数
- 异步测试
- 覆盖率

### rspec

**用途**: Ruby RSpec 测试框架

**核心概念**:
- 描述上下文
- 匹配器
- 模拟与存根
- 共享示例

### minitest

**用途**: Ruby Minitest

**特点**:
- 轻量级
- 多种风格支持
- 基准测试

---

## 浏览器自动化

### playwright

**用途**: Playwright E2E 测试

**核心功能**:
- 多浏览器支持
- 自动等待
- 网络拦截
- 追踪录制

**示例**:
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

**用途**: Puppeteer 浏览器自动化

**用途**:
- 网页截图
- PDF 生成
- 爬虫
- UI 测试

### selenium

**用途**: Selenium WebDriver

**特点**:
- 多语言支持
- 多浏览器支持
- 分布式执行

### monkey-testing

**用途**: 猴子测试

**工具**:
- Firebase Test Lab
- Monkey (Android)
- UI AutoMonkey (iOS)

---

## 代码质量

### eslint

**用途**: ESLint 代码检查

**核心概念**:
- 规则配置
- 插件开发
- 自动修复
- 配置继承

**示例**:
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

**用途**: Prettier 代码格式化

**特点**:
- opinionated 格式化
- 多语言支持
- 集成 ESLint

### semgrep

**用途**: Semgrep 静态分析

**用途**:
- 安全漏洞检测
- 代码味道检测
- 自定义规则

### code-review

**用途**: 代码审查流程

**审查要点**:
- 代码可读性
- 逻辑正确性
- 安全性
- 性能
- 测试覆盖

---

## 调试与追踪

### debugging

**用途**: 调试技术

**工具**:
- Chrome DevTools
- VS Code Debugger
- GDB
- LLDB

### troubleshooting

**用途**: 问题排查

**方法论**:
- 二分查找
- 日志分析
- 指标监控
- 链路追踪

### profiling

**用途**: 性能分析

**工具**:
- Chrome Performance
- Node Clinic
- Python cProfile
- Go pprof

### benchmarking

**用途**: 基准测试

**工具**:
- k6
- Apache Bench
- JMeter
- Gatling

---

## CI/CD

### github-actions

**用途**: GitHub Actions CI/CD

**核心概念**:
- Workflow 配置
- Action 市场
- 矩阵构建
- 缓存

**示例**:
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

**用途**: GitHub CI/CD

**核心概念**:
- Pipeline 配置
- Runner
- 缓存与 artifacts
- 部署策略

### jenkins

**用途**: Jenkins 持续集成

**特性**:
- 插件生态
- 分布式构建
- 流水线即代码

---

## 日志管理

### logging-best-practices

**用途**: 日志Melhores-Práticas

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

**用途**: 集中式日志

**架构**:
- 日志收集 (Fluentd, Filebeat)
- 日志存储 (Elasticsearch)
- 日志可视化 (Kibana, Grafana)

---

## 监控告警

### prometheus

**用途**: Prometheus 监控

**核心概念**:
- 拉取模型
- PromQL
- 告警规则
- 服务发现

### alertmanager

**用途**: Alertmanager 告警

**功能**:
- 告警分组
- 抑制
- 静默
- 路由

### grafana

**用途**: Grafana 可视化

**用途**:
- 指标仪表板
- 告警规则
- 数据源集成

---

## 文档工具

### swagger

**用途**: Swagger API 文档

**工具**:
- Swagger UI
- OpenAPI Generator
- Swagger Editor

### postman

**用途**: Postman API 测试

**功能**:
- 集合管理
- 环境变量
- 自动化测试
- Mock Server

### redoc

**用途**: ReDoc API 文档

**特点**:
- OpenAPI 支持
- 响应式设计
- 主题定制

---

## 相关技能

| 技能 | 用途 |
|------|------|
| `pytest` | Python 测试 |
| `playwright` | E2E 测试 |
| `eslint` | 代码检查 |
| `semgrep` | 静态分析 |
| `github-actions` | CI/CD |
| `prometheus` | 监控 |
| `grafana` | 可视化 |