# Agent 速查

## 内蔵 Agent

### 計画類
| Agent | 用途 | 特徴 |
|-------|------|------|
| `planner` | [実施計画](../参考ドキュメント/agents/06-Architecture.md) | タスク分解、ステップ生成 |
| `architect` | [システム設計](../参考ドキュメント/agents/06-Architecture.md) | アーキテクチャ決定、技術選択 |

### レビュー類
| Agent | 用途 | 特徴 |
|-------|------|------|
| `code-reviewer` | コードレビュー | 品質チェック、モード検査 |
| `security-reviewer` | セキュリティレビュー | 脆弱性検出、OWASP |
| `rust-reviewer` | Rust レビュー | ライフサイクル、借用検査 |

### 開発類
| Agent | 用途 | 特徴 |
|-------|------|------|
| `tdd-guide` | [TDD指導](../参考ドキュメント/agents/01-Code-Review.md) | テスト先行、增量リファクタリング |
| `build-error-resolver` | [ビルド修復](../参考ドキュメント/agents/02-Build-Fix.md) | エラー位置特定、修復提案 |

### 運用類
| Agent | 用途 | 特徴 |
|-------|------|------|
| `e2e-runner` | [E2Eテスト](../参考ドキュメント/agents/01-Code-Review.md) | 重要パス、自動化 |
| `refactor-cleaner` | [リファクタリング清理](../参考ドキュメント/agents/01-Code-Review.md) | 死コード移除 |

## Agent 呼び出し方式

```
/[agent-name]                    # 直接呼び出し
/[agent-name] [parameters]       # パラメータ付き
/[agent-name] --option value    # オプション付き
```

## カスタム Agent

Agent ファイル形式：`agents/*.md` + YAML frontmatter

```yaml
---
name: my-agent
description: 私のカスタム Agent
tools: [Read, Bash, Edit]
model: sonnet
---
```

---

[クイックリファレンスディレクトリに戻る](./README.md)