# 02 - Agent 协作

学习如何设计、实现和管理多 Agent 协作系统。

## 概述

ECC 提供 **66 个专业[Agent](../../DocumentosDeReferência/agents/1.%20代码审查类.md)**，分为 6 大类别。这些 Agent 通过标准化的工作流协同工作，完成从代码审查到系统设计的各类复杂任务。

---

## 1. Agent 架构

### 1.1 单 Agent vs 多 Agent

| 架构 | 适用场景 | 特点 |
|------|----------|------|
| **单 Agent** | 简单任务、独立操作 | 直接调用、快速响应 |
| **多 Agent** | 复杂任务、需要协作 | 任务分解、结果聚合 |

### 1.2 可用 Agent 列表

| Agent | 用途 | 使用时机 |
|-------|------|----------|
| **planner** | 实施规划 | 复杂功能、重构 |
| **architect** | 系统设计 | 架构决策 |
| **tdd-guide** | 测试驱动开发 | 新功能、bug 修复 |
| **code-reviewer** | 代码审查 | 编写代码后 |
| **security-reviewer** | 安全分析 | 提交前 |
| **build-error-resolver** | 修复构建错误 | 构建失败时 |
| **e2e-runner** | 端到端测试 | 关键用户流程 |
| **refactor-cleaner** | 死代码清理 | 代码维护 |
| **doc-updater** | 文档更新 | 更新文档时 |
| **rust-reviewer** | Rust 代码审查 | Rust 项目 |
| **harmonyos-app-resolver** | HarmonyOS 应用开发 | HarmonyOS/ArkTS 项目 |

### 1.3 Agent 文件格式

```yaml
---
name: agent-name
description: Agent 用途
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

---

## 2. Agent 类别详解

### 2.1 代码审查类（16个）

负责代码质量、安全和可维护性审查。

| Agent | 专长 |
|-------|------|
| code-reviewer | 通用代码审查 |
| security-reviewer | 安全漏洞分析 |
| python-reviewer | Python PEP 8、类型提示 |
| go-reviewer | Go 惯用法、并发 |
| kotlin-reviewer | Kotlin 空安全、协程 |
| rust-reviewer | Rust 所有权、生命周期 |
| cpp-reviewer | C++ 内存安全 |
| flutter-reviewer | Flutter/Dart 模式 |
| fastapi-reviewer | FastAPI 架构、异步 |
| typescript-reviewer | TypeScript 类型安全 |
| java-reviewer | Java 编码标准 |
| csharp-reviewer | C# Melhores-Práticas |
| ruby-reviewer | Ruby 惯用法 |
| php-reviewer | PHP 安全和性能 |
| swift-reviewer | Swift 并发、内存管理 |
| go-security-reviewer | Go 安全审查 |

### 2.2 构建修复类（10个）

专注于修复各语言的构建错误。

| Agent | 专长 |
|-------|------|
| build-error-resolver | 通用构建错误修复 |
| go-build | Go 构建 + go vet |
| kotlin-build | Kotlin/Gradle 编译 |
| rust-build | Rust 借用检查器 |
| cpp-build | C++ CMake + 链接器 |
| gradle-build | Android/KMP Gradle |
| flutter-build | Flutter 构建 |
| maven-build | Maven 构建 |
| cmake-build | CMake 配置 |
| ninja-build | Ninja 构建系统 |

### 2.3 规划类（5个）

负责需求分析和系统规划。

| Agent | 专长 |
|-------|------|
| planner | 实施规划 |
| architect | 系统架构设计 |
| prp-planner | PRP 工作流规划 |
| multi-plan | 多模型协作规划 |
| product-manager | 产品需求分析 |

### 2.4 测试类（2个）

专注于测试驱动开发和覆盖率。

| Agent | 专长 |
|-------|------|
| tdd-guide | TDD 工作流指导 |
| e2e-runner | Playwright E2E 测试 |

### 2.5 安全类（5个）

负责安全审查和漏洞扫描。

| Agent | 专长 |
|-------|------|
| security-reviewer | 通用安全审查 |
| owasp-reviewer | OWASP Top 10 |
| pentest-reviewer | 渗透测试 |
| vulnerability-scanner | 漏洞扫描 |
| secrets-detector | 密钥和凭证检测 |

### 2.6 架构类（5个）

负责系统架构和技术决策。

| Agent | 专长 |
|-------|------|
| architect | 系统架构设计 |
| microservices-architect | 微服务架构 |
| frontend-architect | 前端架构 |
| backend-architect | 后端架构 |
| devops-architect | DevOps 架构 |

---

## 3. 协作模式

### 3.1 主从模式

一个主 Agent 协调多个从 Agent：

```
用户请求 → 主 Agent（planner）
           ├── 子 Agent 1（code-reviewer）
           ├── 子 Agent 2（tdd-guide）
           └── 子 Agent 3（build-error-resolver）
```

### 3.2 对等模式

多个 Agent 并行工作，结果聚合：

```
用户请求 → Agent 1（安全分析）
        → Agent 2（性能分析）
        → Agent 3（代码质量分析）
           ↓ 结果聚合
        最终报告
```

### 3.3 管道模式

Agent 按顺序处理，每个 Agent 的输出是下一个的输入：

```
输入 → Agent 1 → Agent 2 → Agent 3 → 输出
```

---

## 4. 任务分发策略

### 4.1 任务分解

将复杂任务分解为独立子任务：

```markdown
# 复杂任务分解示例
原始任务：实现用户认证系统

分解后：
1. 设计数据库 schema → architect
2. 实现 API 端点 → backend-developer
3. 编写测试 → tdd-guide
4. 安全审查 → security-reviewer
```

### 4.2 并行执行

独立任务并行执行以提高效率：

```markdown
# 好：并行执行
启动 3 个 Agent 并行：
1. Agent 1: 认证模块安全分析
2. Agent 2: 缓存系统性能审查
3. Agent 3: 工具类型检查

# 坏：不必要的顺序执行
先 agent 1，然后 agent 2，然后 agent 3
```

### 4.3 负载均衡

根据 Agent 负载分配任务：

| 因素 | 说明 |
|------|------|
| 任务复杂度 | 简单任务 → 轻量 Agent |
| 资源需求 | 重计算 → 专用 Agent |
| 可用性 | 避免过载 Agent |

---

## 5. 多视角分析

对于复杂问题，使用分角色 sub-agents：

| 角色 | 职责 |
|------|------|
| 事实审查员 | 验证信息和数据准确性 |
| 高级工程师 | 评估技术可行性和Melhores-Práticas |
| 安全专家 | 识别安全风险和漏洞 |
| 一致性审查员 | 确保解决方案内部一致 |
| 冗余检查员 | 识别重复和浪费 |

---

## 6. Agent 通信

### 6.1 直接调用

```markdown
使用 planner agent 创建实施计划
```

### 6.2 任务委托

```markdown
将代码审查任务委托给 code-reviewer agent
```

### 6.3 结果聚合

多个 Agent 的结果需要聚合：

```markdown
聚合来自：
- security-reviewer: 安全问题列表
- code-reviewer: 代码质量问题列表
- tdd-guide: 测试覆盖率报告

→ 生成综合报告
```

---

## 7. 立即使用原则

无需用户提示时自动调用：

| 场景 | Agent |
|------|-------|
| 复杂功能请求 | **planner** |
| 代码刚编写/修改 | **code-reviewer** |
| Bug 修复或新功能 | **tdd-guide** |
| 架构决策 | **architect** |
| 构建失败 | **build-error-resolver** |
| 安全相关 | **security-reviewer** |

---

## Exercícios

完成 [练习](./exercises/Exercícios.md) 中的任务。

---

[返回核心能力目录](../README.md)