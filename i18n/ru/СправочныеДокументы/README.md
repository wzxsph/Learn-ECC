# ECC 文档体系

ECC (Everything Claude Code) 是一个 Claude Code 插件，提供 75 个命令、60 个 Agent、16 个 Skills、7 个规则文档和完整的 Hook/MCP 配置体系。

---

## 1. ECC 文档体系概述

ECC 为 Claude Code 提供生产级开发工作流支持，涵盖多个 AI Agent 框架（Claude Code、Codex、OpenCode、Cursor、Gemini）。

### 技术栈

| 层级 | 技术 | 版本 |
|------|------|------|
| 主要语言 | JavaScript/Node.js | >=18 |
| 次要语言 | Python | >=3.11 |
| 包管理器 | Yarn | 4.9.2 |
| 测试运行器 | 自定义 Node.js 测试运行器 | - |
| 覆盖率 | c8 | 11.x |
| 代码检查 | ESLint + markdownlint-cli | - |

### 目录结构

```
ECC/
├── agents/        # 60 个专业 Agent
├── commands/      # 75 个命令文件
├── skills/        # 16 个 Skills 目录
├── rules/         # 7 个规则文档（common + 语言特定）
├── hooks/         # 3 个 Hook 文档
├── mcp/           # 6 个 MCP 服务器配置
├── scripts/       # 54 个工具脚本
├── README.md
```

---

## 2. 快速导航

| 分类 | 文档 | 说明 |
|------|------|------|
| [命令总览](./commands/) | [commands/](commands/) | 75 个命令的完整列表 |
| [Agent 索引](./agents/) | [agents/](agents/) | 60 个专业 Agent |
| [Skills 索引](./skills/) | [skills/](skills/) | 16 个 Skills 按领域分类 |
| [规则索引](./rules/) | [rules/](rules/) | 7 个规则文档 (common + 语言特定) |
| [Hooks 索引](./hooks/) | [hooks/](hooks/) | Hook 系统配置和开发 |
| [MCP 索引](./mcp/) | [mcp/](mcp/) | MCP 服务器配置和开发 |
| [Scripts 索引](./scripts/) | [scripts/](scripts/) | 54 个工具脚本 |

---

## 3. 命令索引

共 **75 个命令**，按类别分组：

### 3.1 核心工作流 (6)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/plan` | [01-Ключевой-Workflow.md](commands/01-Ключевой-Workflow.md) | 需求分析+风险评估+分步骤计划 |
| `/code-review` | [01-Ключевой-Workflow.md](commands/01-Ключевой-Workflow.md) | 代码质量/安全/可维护性审查 |
| `/build-fix` | [01-Ключевой-Workflow.md](commands/01-Ключевой-Workflow.md) | 自动检测语言+增量修复构建错误 |
| `/verify` | [01-Ключевой-Workflow.md](commands/01-Ключевой-Workflow.md) | 完整验证循环：构建→lint→测试→类型检查 |
| `/quality-gate` | [01-Ключевой-Workflow.md](commands/01-Ключевой-Workflow.md) | 按需运行 ECC 质量流水线 |
| `/tdd` | [01-Ключевой-Workflow.md](commands/01-Ключевой-Workflow.md) | 通用 TDD 工作流 |

### 3.2 测试相关 (8)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/go-test` | [02-Тестирование.md](commands/02-Тестирование.md) | Go TDD (表格驱动，80%+ 覆盖率) |
| `/kotlin-test` | [02-Тестирование.md](commands/02-Тестирование.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-Тестирование.md](commands/02-Тестирование.md) | Rust TDD (cargo test + 集成测试) |
| `/cpp-test` | [02-Тестирование.md](commands/02-Тестирование.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-Тестирование.md](commands/02-Тестирование.md) | Flutter TDD |
| `/e2e` | [02-Тестирование.md](commands/02-Тестирование.md) | 生成 + 运行 Playwright e2e 测试 |
| `/test-coverage` | [02-Тестирование.md](commands/02-Тестирование.md) | 测试覆盖率报告+差距分析 |
| `/python-testing` | [02-Тестирование.md](commands/02-Тестирование.md) | Python 测试最佳实践 |

### 3.3 语言审查 (7)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/python-review` | [03-Проверка-языка.md](commands/03-Проверка-языка.md) | Python PEP 8、类型提示、安全 |
| `/go-review` | [03-Проверка-языка.md](commands/03-Проверка-языка.md) | Go 惯用法、并发、错误处理 |
| `/kotlin-review` | [03-Проверка-языка.md](commands/03-Проверка-языка.md) | Kotlin 空安全、协程、架构 |
| `/rust-review` | [03-Проверка-языка.md](commands/03-Проверка-языка.md) | Rust 所有权、生命周期、unsafe |
| `/cpp-review` | [03-Проверка-языка.md](commands/03-Проверка-языка.md) | C++ 内存安全、现代 idiom |
| `/flutter-review` | [03-Проверка-языка.md](commands/03-Проверка-языка.md) | Flutter/Dart 模式 |
| `/fastapi-review` | [03-Проверка-языка.md](commands/03-Проверка-языка.md) | FastAPI 架构、异步、Pydantic |

### 3.4 构建修复 (6)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/go-build` | [04-Исправление-сборки.md](commands/04-Исправление-сборки.md) | 修复 Go 构建错误 + go vet 警告 |
| `/kotlin-build` | [04-Исправление-сборки.md](commands/04-Исправление-сборки.md) | 修复 Kotlin/Gradle 编译器错误 |
| `/rust-build` | [04-Исправление-сборки.md](commands/04-Исправление-сборки.md) | 修复 Rust 构建 + 借用检查器问题 |
| `/cpp-build` | [04-Исправление-сборки.md](commands/04-Исправление-сборки.md) | 修复 C++ CMake + 链接器问题 |
| `/gradle-build` | [04-Исправление-сборки.md](commands/04-Исправление-сборки.md) | 修复 Android/KMP Gradle 错误 |
| `/flutter-build` | [04-Исправление-сборки.md](commands/04-Исправление-сборки.md) | Flutter 构建修复 |

### 3.5 规划与架构 (7)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/plan-prd` | [05-Планирование-Архитектура.md](commands/05-Планирование-Архитектура.md) | 交互式 PRD 生成器 |
| `/prp-plan` | [05-Планирование-Архитектура.md](commands/05-Планирование-Архитектура.md) | 全面的功能规划 |
| `/prp-prd` | [05-Планирование-Архитектура.md](commands/05-Планирование-Архитектура.md) | PRP 工作流 PRD 生成器 |
| `/prp-implement` | [05-Планирование-Архитектура.md](commands/05-Планирование-Архитектура.md) | 执行 PRP 计划+验证循环 |
| `/prp-pr` | [05-Планирование-Архитектура.md](commands/05-Планирование-Архитектура.md) | 从 PRP 工作流创建 PR |
| `/prp-commit` | [05-Планирование-Архитектура.md](commands/05-Планирование-Архитектура.md) | PRP 验证提交 |
| `/multi-plan` | [05-Планирование-Архитектура.md](commands/05-Планирование-Архитектура.md) | 多模型协作规划 (Codex + Gemini) |

### 3.6 会话管理 (5)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/save-session` | [06-Управление-сессиями.md](commands/06-Управление-сессиями.md) | 保存会话状态到 ~/.claude/session-data/ |
| `/resume-session` | [06-Управление-сессиями.md](commands/06-Управление-сессиями.md) | 加载并恢复保存的会话 |
| `/sessions` | [06-Управление-сессиями.md](commands/06-Управление-сессиями.md) | 浏览+搜索+管理会话历史 |
| `/checkpoint` | [06-Управление-сессиями.md](commands/06-Управление-сессиями.md) | 创建/验证工作流检查点 |
| `/aside` | [06-Управление-сессиями.md](commands/06-Управление-сессиями.md) | 回答侧问而不丢失上下文 |

### 3.7 学习与改进 (10)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/learn` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 从会话中提取可复用模式 |
| `/learn-eval` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 提取模式 + 自我评估质量 |
| `/evolve` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 分析 instinct + 建议进化结构 |
| `/promote` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 将项目 instinct 提升到全局范围 |
| `/instinct-status` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 显示所有学习的 instinct |
| `/instinct-export` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 导出 instinct 到文件 |
| `/instinct-import` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 从文件/URL 导入 instinct |
| `/skill-create` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 分析 git 历史+生成 skill 文件 |
| `/skill-health` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | Skill 组合健康仪表板 |
| `/rules-distill` | [07-Обучение-улучшение.md](commands/07-Обучение-улучшение.md) | 扫描 skills + 提取跨领域原则 |

### 3.8 循环与自动化 (3)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/loop-start` | [08-Циклы-автоматизация.md](commands/08-Циклы-автоматизация.md) | 启动托管自主循环模式 |
| `/loop-status` | [08-Циклы-автоматизация.md](commands/08-Циклы-автоматизация.md) | 检查运行中循环的状态 |
| `/santa-loop` | [08-Циклы-автоматизация.md](commands/08-Циклы-автоматизация.md) | Santa 风格自主循环 |

### 3.9 项目管理 (6)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/projects` | [09-Управление-проектами.md](commands/09-Управление-проектами.md) | 列出已知项目 + instinct 统计 |
| `/harness-audit` | [09-Управление-проектами.md](commands/09-Управление-проектами.md) | 审计 agent harness 配置 |
| `/model-route` | [09-Управление-проектами.md](commands/09-Управление-проектами.md) | 路由任务到合适模型 |
| `/pm2` | [09-Управление-проектами.md](commands/09-Управление-проектами.md) | PM2 进程管理器初始化 |
| `/setup-pm` | [09-Управление-проектами.md](commands/09-Управление-проектами.md) | 配置包管理器 (npm/pnpm/yarn/bun) |
| `/project-init` | [09-Управление-проектами.md](commands/09-Управление-проектами.md) | 项目初始化 |

### 3.10 PR 工作流 (6)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/pr` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | 从当前分支创建 GitHub PR |
| `/review-pr` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | 审查 GitHub PR |
| `/multi-workflow` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | 多模型协作开发 |
| `/multi-backend` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | 后端多模型开发 |
| `/multi-frontend` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | 前端多模型开发 |
| `/multi-execute` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | 多模型协作执行 |

### 3.11 Hookify 系统 (4)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/hookify` | [11-Hookify-система.md](commands/11-Hookify-система.md) | 创建 hooks 防止不良行为 |
| `/hookify-list` | [11-Hookify-система.md](commands/11-Hookify-система.md) | 列出所有配置的 hookify 规则 |
| `/hookify-configure` | [11-Hookify-система.md](commands/11-Hookify-система.md) | 交互式启用/禁用 hookify 规则 |
| `/hookify-help` | [11-Hookify-система.md](commands/11-Hookify-система.md) | Hookify 系统帮助 |

### 3.12 文档与研究 (3)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/update-docs` | [12-документация-исследования.md](commands/12-документация-исследования.md) | 更新项目文档 |
| `/update-codemaps` | [12-документация-исследования.md](commands/12-документация-исследования.md) | 重新生成 codemaps |
| `/ecc-guide` | [12-документация-исследования.md](commands/12-документация-исследования.md) | ECC 用户指南 |

### 3.13 重构与清理 (2)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/refactor-clean` | [13-Рефакторинг-очистка.md](commands/13-Рефакторинг-очистка.md) | 删除死代码+合并重复 |
| `/auto-update` | [13-Рефакторинг-очистка.md](commands/13-Рефакторинг-очистка.md) | 自动更新能力 |

### 3.14 其他命令 (7)

| 命令 | 文件 | 用途 |
|------|------|------|
| `/jira` | [14-Другие-команды.md](commands/14-Другие-команды.md) | Jira 工单交互 |
| `/gan-build` | [14-Другие-команды.md](commands/14-Другие-команды.md) | GAN 构建操作 |
| `/gan-design` | [14-Другие-команды.md](commands/14-Другие-команды.md) | GAN 设计操作 |
| `/prune` | [14-Другие-команды.md](commands/14-Другие-команды.md) | 删除陈旧 instinct (>30天) |
| `/security-scan` | [14-Другие-команды.md](commands/14-Другие-команды.md) | 安全扫描 |
| `/feature-dev` | [14-Другие-команды.md](commands/14-Другие-команды.md) | 功能开发助手 |
| `/cost-report` | [14-Другие-команды.md](commands/14-Другие-команды.md) | 模型成本报告 |

---

## 4. Agent 索引

共 **60 个 Agent**，按类别分组：

| Agent 类别 | 文件 | 说明 |
|------------|------|------|
| [代码审查类](agents/1.%20代码审查类.md) | [01-Код-ревью.md](agents/1.%20代码审查类.md) | 14 个审查 Agent |
| [构建修复类](agents/2.%20构建修复类.md) | [02-Исправление-сборки.md](agents/2.%20构建修复类.md) | 14 个构建修复 Agent |
| [规划类](agents/3.%20规划类.md) | [03-Планирование.md](agents/3.%20规划类.md) | 5 个规划 Agent |
| [测试类](agents/4.%20测试类.md) | [04-Тестирование.md](agents/4.%20测试类.md) | 2 个测试 Agent |
| [安全类](agents/5.%20安全类.md) | [05-Безопасность.md](agents/5.%20安全类.md) | 3 个安全 Agent |
| [架构类](agents/6.%20架构类.md) | [06-Архитектура.md](agents/6.%20架构类.md) | 3 个架构 Agent |

---

## 5. Skills 索引

$**16 个 Skills 领域**，按领域分类：

| 领域 | 文件 | 说明 |
|------|------|------|
| [最佳实践](skills/Лучшие-практики.md) | [Лучшие-практики.md](skills/Лучшие-практики.md) | 编码标准、错误处理、自主循环 |
| [编程语言](skills/Языки-программирования.md) | [Языки-программирования.md](skills/Языки-программирования.md) | Python/Go/Rust/Kotlin/C++ 等 |
| [框架](skills/Фреймворки.md) | [Фреймворки.md](skills/Фреймворки.md) | Django/Laravel/NestJS/Spring Boot 等 |
| [测试](skills/Тестирование.md) | [Тестирование.md](skills/Тестирование.md) | TDD/单元测试/集成测试/E2E |
| [安全](skills/Безопасность.md) | [Безопасность.md](skills/Безопасность.md) | 安全审查、漏洞扫描 |
| [前端与设计](skills/前端与设计.md) | [前端与设计.md](skills/前端与设计.md) | 前端开发、设计系统 |
| [后端与API](skills/Бэкенд-API.md) | [Бэкенд-API.md](skills/Бэкенд-API.md) | 后端服务、API 设计、数据库 |
| [部署与DevOps](skills/Развертывание-DevOps.md) | [Развертывание-DevOps.md](skills/Развертывание-DevOps.md) | Docker/K8s/部署策略 |
| [监控与可观测性](skills/Мониторинг-observability.md) | [Мониторинг-observability.md](skills/Мониторинг-observability.md) | 可观测性、网络诊断 |
| [自动化与脚本](skills/Автоматизация-скрипты.md) | [Автоматизация-скрипты.md](skills/Автоматизация-скрипты.md) | 自主循环、持续学习、代理工程 |
| [搜索与数据获取](skills/Поиск-получение-данных.md) | [Поиск-получение-данных.md](skills/Поиск-получение-данных.md) | Exa 搜索、数据抓取、MCP |
| [GitHub与协作](skills/GitHub-коллаборация.md) | [GitHub-коллаборация.md](skills/GitHub-коллаборация.md) | GitHub 工作流、代码审查 |
| [AI与机器学习](skills/AI与机器学习.md) | [AI与机器学习.md](skills/AI与机器学习.md) | 神经网络、PyTorch、MLOps |
| [云原生与基础设施](skills/Cloud-native-инфраструктура.md) | [Cloud-native-инфраструктура.md](skills/Cloud-native-инфраструктура.md) | Kubernetes、Docker、Terraform |
| [特殊领域技能](skills/Специализированные-навыки.md) | [Специализированные-навыки.md](skills/Специализированные-навыки.md) | 区块链、游戏开发、音视频、IoT |
| [开发工具链](skills/Инструменты-разработки.md) | [Инструменты-разработки.md](skills/Инструменты-разработки.md) | 测试框架、CI/CD、代码质量 |
| [前沿技术](skills/Передовые-технологии.md) | [Передовые-технологии.md](skills/Передовые-технологии.md) | AI Agent、量子计算、边缘计算 |

---

## 6. 规则索引

共 **7 个规则文档**（common + 语言特定）：

| 规则 | 文件 | 说明 |
|------|------|------|
| [Git 工作流](rules/Git工作流.md) | [Git工作流.md](rules/Git工作流.md) | Git 提交规范和 PR 工作流 |
| [Hooks 系统](rules/Hooks系统.md) | [Hooks系统.md](rules/Hooks系统.md) | Hook 配置和使用指南 |
| [代理编排](rules/代理编排.md) | [代理编排.md](rules/代理编排.md) | Agent 编排模式 |
| [性能优化](rules/性能优化.md) | [性能优化.md](rules/性能优化.md) | 性能优化指南 |
| [代码风格](rules/代码风格.md) | [代码风格.md](rules/代码风格.md) | 编码风格规范 |
| [测试规则](rules/Правила-тестирования.md) | [Правила-тестирования.md](rules/Правила-тестирования.md) | 测试要求（80% 覆盖率） |
| [安全规则](rules/Правила-безопасности.md) | [Правила-безопасности.md](rules/Правила-безопасности.md) | 安全检查清单 |

---

## 7. Hooks 索引

共 **4 个文档**：

| 文档 | 文件 | 说明 |
|------|------|------|
| [Hook 类型](hooks/Типы-Hook.md) | [Типы-Hook.md](hooks/Типы-Hook.md) | PreToolUse、PostToolUse、Stop 类型 |
| [内置 Hooks](hooks/Встроенные-Hooks.md) | [Встроенные-Hooks.md](hooks/Встроенные-Hooks.md) | 内置 Hook 列表和使用 |
| [自定义开发](hooks/Пользовательская-разработка.md) | [Пользовательская-разработка.md](hooks/Пользовательская-разработка.md) | 自定义 Hook 开发指南 |
| [配置格式](hooks/Формат-конфигурации.md) | [Формат-конфигурации.md](hooks/Формат-конфигурации.md) | hooks.json 配置格式 |

---

## 8. MCP 索引

共 **6 个 MCP 服务器配置**：

| 文档 | 文件 | 说明 |
|------|------|------|
| [MCP 配置格式](mcp/MCPФормат-конфигурации.md) | [MCPФормат-конфигурации.md](mcp/MCPФормат-конфигурации.md) | MCP 配置文件格式 |
| [内置服务器](mcp/Встроенные-серверы.md) | [Встроенные-серверы.md](mcp/Встроенные-серверы.md) | 内置 MCP 服务器 |
| [自定义开发](mcp/Пользовательская-разработка.md) | [Пользовательская-разработка.md](mcp/Пользовательская-разработка.md) | 自定义 MCP 服务器开发 |

---

## 9. Scripts 索引

共 **54 个工具脚本**：

| 文档 | 文件 | 说明 |
|------|------|------|
| [工具脚本](scripts/Утилиты-скрипты.md) | [Утилиты-скрипты.md](scripts/Утилиты-скрипты.md) | ecc.js、install-apply.js 等 |
| [工具函数库](scripts/Библиотека-функций.md) | [Библиотека-функций.md](scripts/Библиотека-функций.md) | 共享函数库 |
| [测试运行器](scripts/Тест-раннер.md) | [Тест-раннер.md](scripts/Тест-раннер.md) | 测试运行器使用 |
| [构建脚本](scripts/Скрипты-сборки.md) | [Скрипты-сборки.md](scripts/Скрипты-сборки.md) | 构建脚本 |

---

## 10. 贡献指南

### 文件命名规范

- **命令、Skills、Agents、Hooks**: 小写 + 连字符（如 `code-review.md`）
- **脚本**: camelCase 或 kebab-case（如 `session-start.js`）
- **规则**: 按语言/主题组织

### 命令文件格式

```yaml
---
description: "简短描述"
argument-hint: "[可选参数]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent 文件格式

```yaml
---
name: agent-name
description: Agent 用途
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill 文件格式

```markdown
# Skill 名称

## When to Use
## How It Works
## Examples
```

### 提交流程

1. 创建新文件或修改现有文件
2. 确保遵循命名规范
3. 运行测试: `node tests/run-all.js`
4. 运行 markdown lint: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. 创建 PR 请求审查

---

*ECC 文档体系 - 为 Claude Code 提供生产级开发工作流*
