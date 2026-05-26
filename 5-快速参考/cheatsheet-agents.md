# Agent 速查

## 内置 Agent

### 规划类
| Agent | 用途 | 特点 |
|-------|------|------|
| `planner` | [实施规划](../参考文档/agents/6.%20架构类.md) | 分解任务、生成步骤 |
| `architect` | [系统设计](../参考文档/agents/6.%20架构类.md) | 架构决策、技术选型 |

### 审查类
| Agent | 用途 | 特点 |
|-------|------|------|
| `code-reviewer` | 代码审查 | 质量把关、模式检查 |
| `security-reviewer` | 安全审查 | 漏洞检测、OWASP |
| `rust-reviewer` | Rust 审查 | 生命周期、借用检查 |

### 开发类
| Agent | 用途 | 特点 |
|-------|------|------|
| `tdd-guide` | [TDD指导](../参考文档/agents/1.%20代码审查类.md) | 测试先行、增量重构 |
| `build-error-resolver` | [构建修复](../参考文档/agents/2.%20构建修复类.md) | 错误定位、修复建议 |

### 运维类
| Agent | 用途 | 特点 |
|-------|------|------|
| `e2e-runner` | [E2E测试](../参考文档/agents/1.%20代码审查类.md) | 关键路径、自动化 |
| `refactor-cleaner` | [重构清理](../参考文档/agents/1.%20代码审查类.md) | 死代码移除 |

## Agent 调用方式

```
/[agent-name]                    # 直接调用
/[agent-name] [parameters]       # 带参数
/[agent-name] --option value    # 选项参数
```

## 自定义 Agent

Agent 文件格式：`agents/*.md` + YAML frontmatter

```yaml
---
name: my-agent
description: 我的自定义 Agent
tools: [Read, Bash, Edit]
model: sonnet
---
```

---

[返回快速参考目录](./README.md)
