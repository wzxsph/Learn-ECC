# Ajan 速查

## 内置 Agent

### Planlama
| Ajan | Kullanım | Özellikler |
|-------|------|------|
| `planner` | [Uygulama Planlaması](../ReferansBelgeleri/agents/6.%20Mimari.md) | 分解Görev、生成Adımlar |
| `architect` | [Sistem Tasarımı](../ReferansBelgeleri/agents/6.%20Mimari.md) | 架构决策、Teknoloji Seçimi |

### 审查类
| Ajan | Kullanım | Özellikler |
|-------|------|------|
| `code-reviewer` | Kod İnceleme | 质量把关、模式检查 |
| `security-reviewer` | Güvenlik审查 | 漏洞检测、OWASP |
| `rust-reviewer` | Rust 审查 | 生命周期、借用检查 |

### 开发类
| Ajan | Kullanım | Özellikler |
|-------|------|------|
| `tdd-guide` | [TDD指导](../ReferansBelgeleri/agents/1.%20Kod İnceleme.md) | Test先行、增量重构 |
| `build-error-resolver` | [Yapı Onarımı](../ReferansBelgeleri/agents/2.%20Yapı Onarımı.md) | 错误定位、修复建议 |

### 运维类
| Ajan | Kullanım | Özellikler |
|-------|------|------|
| `e2e-runner` | [E2ETest](../ReferansBelgeleri/agents/1.%20Kod İnceleme.md) | 关键路径、自动化 |
| `refactor-cleaner` | [重构清理](../ReferansBelgeleri/agents/1.%20Kod İnceleme.md) | 死代码移除 |

## Ajan 调用方式

```
/[agent-name]                    # Doğrudan Çağırma
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

[Geri DönHızlı ReferansDizin](./README.md)
