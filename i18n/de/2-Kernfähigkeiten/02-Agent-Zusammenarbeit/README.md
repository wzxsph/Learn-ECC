# 02 Agent 协作

> 理解 6 类 Agent 的职责，组合使用完成复杂开发任务

## 模块目标

完成本模块后，你将能够：

- 识别 Claude Code 的 6 类 Agent 及各自职责
- 根据任务类型选择合适的 Agent
- 组合多个 Agent 完成复杂任务
- 理解 Agent 协作模式和最佳实践

## Agent 分类体系

Claude Code 的 Agent 分为 6 大类：

| Agent 类型 | 核心职责 | 适用场景 |
|------------|----------|----------|
| **审查类** | 代码质量、模式规范、安全漏洞 | 代码审查、技术债清理 |
| **构建类** | 编译构建、错误修复、依赖管理 | 构建失败、编译错误 |
| **规划类** | 任务分解、架构设计、技术方案 | 新功能、架构变更 |
| **测试类** | 测试编写、覆盖率分析、测试策略 | TDD、测试补充 |
| **安全类** | 安全审计、漏洞扫描、合规检查 | 安全敏感代码、支付模块 |
| **架构类** | 系统设计、模式选择、技术选型 | 大型重构、架构演进 |

## 6 类 Agent 详解

### 1. 审查类 Agent

**职责**: 代码质量审查、模式规范检查

可用 Agent：
- `code-reviewer` - 通用代码审查
- `python-reviewer` - Python 专项审查
- `go-reviewer` - Go 语言审查
- `rust-reviewer` - Rust 安全审查
- `security-reviewer` - 安全漏洞审查

**使用场景**:
- 提交代码前进行质量检查
- 发现潜在的代码异味
- 审查第三方代码

### 2. 构建类 Agent

**职责**: 编译构建、错误修复、依赖管理

可用 Agent：
- `build-error-resolver` - 构建错误解析
- `go-build` - Go 构建
- `rust-build` - Rust 构建
- `kotlin-build` - Kotlin 构建
- `flutter-build` - Flutter 构建

**使用场景**:
- 构建失败时的错误修复
- 依赖冲突解决
- 跨平台构建问题

### 3. 规划类 Agent

**职责**: 任务分解、架构设计、实施计划

可用 Agent：
- `planner` - 实施计划生成
- `architect` - 架构设计
- `tdd-guide` - TDD 工作流指导

**使用场景**:
- 新功能开发规划
- 技术方案评估
- 复杂任务分解

### 4. 测试类 Agent

**职责**: 测试编写、覆盖率提升、测试策略

可用 Agent：
- `tdd-guide` - 测试驱动开发
- `e2e-runner` - 端到端测试
- `test-coverage` - 覆盖率分析

**使用场景**:
- 编写新功能测试
- 提升测试覆盖率
- E2E 测试场景设计

### 5. 安全类 Agent

**职责**: 安全审计、漏洞扫描、合规检查

可用 Agent：
- `security-reviewer` - 安全漏洞审查
- `security-scan` - 自动化安全扫描

**使用场景**:
- 支付和认证代码审查
- 外部数据处理检查
- 合规性验证

### 6. 架构类 Agent

**职责**: 系统设计、模式选择、技术选型

可用 Agent：
- `architect` - 架构设计
- `refactor-cleaner` - 重构指导
- `api-design-reviewer` - API 设计审查

**使用场景**:
- 微服务拆分
- 数据库设计
- API 架构演进

## Agent 协作模式

### 模式 1: 串行协作

```
任务 → Planner → 实施 → Reviewer → 测试 → 完成
```

适用于：线性流程，步骤之间有依赖

### 模式 2: 并行协作

```
            → Reviewer A ─→
任务 → 分解 ─→ Reviewer B ─→ 汇总 → 完成
            → Reviewer C ─→
```

适用于：多角度审查、独立模块审查

### 模式 3: 级联协作

```
重大任务 → Architect → Planner → 实施 → 各级 Reviewer → 合并
```

适用于：复杂项目，大型功能开发

## Agent 组合示例

### 场景: 开发新功能

```
1. 使用 /plan 规划功能
2. 使用 tdd-guide 进行 TDD 开发
3. 使用 code-reviewer 审查代码
4. 使用 security-reviewer 检查安全
5. 使用 verify 验证功能
```

### 场景: 修复安全漏洞

```
1. 使用 security-reviewer 扫描漏洞
2. 使用 planner 制定修复计划
3. 使用 build-fix 实施修复
4. 使用 security-reviewer 重新验证
5. 使用 verify 确认修复
```

## Agent 选择指南

| 任务类型 | 推荐 Agent | 理由 |
|----------|------------|------|
| 新功能开发 | planner + tdd-guide | 需要规划和 TDD |
| 代码审查 | code-reviewer + security-reviewer | 质量和安全双重检查 |
| 构建失败 | build-error-resolver | 专注错误修复 |
| 架构重构 | architect + refactor-cleaner | 架构层面指导 |
| 安全敏感 | security-reviewer (优先) | 必须先过安全关 |
| 测试编写 | tdd-guide + test-coverage | 测试策略和覆盖率 |

## 配套教材

完整的 Agent 文档位于：`../../Referenzdokumente/agents/1. 代码审查类.md`

## 下一步

- 学习[练习题目](./exercises/练习.md)
- 阅读 [Agent 完整文档](../../Referenzdokumente/agents/1. 代码审查类.md)