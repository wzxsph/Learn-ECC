# 01 - [ECC](../参考ドキュメント/README.md) 紹介

## [ECC](../参考ドキュメント/README.md)とは

[ECC](../参考ドキュメント/README.md)（Everything Claude Code）は、Claude Code用プラグインシステムであり、プロダクションレベルの[Agent](../参考ドキュメント/agents/01-Code-Review.md)、[Skills](../参考ドキュメント/skills/最佳实践.md)、hooks、rules、[Commands](../参考ドキュメント/commands/01-コアワークフロー.md)、およびMCP設定方案を提供します。Anthropic Hackathon受賞作品から生まれ、10ヶ月以上の日常使用的的打磨を経てきました。

## [ECC](../参考ドキュメント/README.md)は何ができるか？

### 1. [Agent](../参考ドキュメント/agents/01-Code-Review.md)システム
[ECC](../参考ドキュメント/README.md)は60+の専門[Agent](../参考ドキュメント/agents/01-Code-Review.md)を提供し、多言語レビュー、ビルドリペア、アーキテクチャ設計などをカバー：

| [Agent](../参考ドキュメント/agents/01-Code-Review.md) | 用途 |
|-------|------|
| `planner` | 機能実施計画 |
| `code-reviewer` | コード品質とセキュリティレビュー |
| `security-reviewer` | セキュリティ脆弱性分析 |
| `build-error-resolver` | ビルドエラー修復 |
| `architect` | システムアーキテクチャ設計 |
| `tdd-guide` | テスト駆動開発ガイド |

### 2. [Skills](../参考ドキュメント/skills/最佳实践.md)ワークフロー
[Skills](../参考ドキュメント/skills/最佳实践.md)是主要ワークフロー面で、[ECC](../参考ドキュメント/README.md)は232+の[Skills](../参考ドキュメント/skills/最佳实践.md)を提供：

- `tdd-workflow` - テスト駆動開発
- `e2e-testing` - エンドツーエンドテスト
- `security-review` - セキュリティレビューチェックリスト
- `continuous-learning-v2` - instinctベースの継続的学習
- `eval-harness` - 検証ループ評価

### 3. [Commands](../参考ドキュメント/commands/01-コアワークフロー.md)コマンド
伝統的なスラッシュコマンド互換性を保持（合計75+コマンド）：

| コマンド | 機能 |
|------|------|
| `/plan` | 実施計画を作成 |
| `/code-review` | コードレビュー |
| `/build-fix` | ビルドエラーを修復 |
| `/learn` | セッションからパターンを抽出 |
| `/skill-create` | Git歴史からSkillを生成 |
| `/sessions` | セッション履歴管理 |

### 4. Hooks自動化
柔軟なhooksシステム（8種類のイベントタイプ）：
- `SessionStart` - セッション起動時にコンテキストを読み込む
- `SessionEnd` - セッション終了時に状態を保存
- `PreToolUse` - ツール実行前に検証
- `PostToolUse` - ツール実行後にフォーマット

### 5. Rules規則体系
全面的開発規範（common + 言語特定）：
- **common/** - 共通原則（セキュリティ、コーディングスタイル、テスト、パフォーマンス）
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. MCPサービス
14+ MCPサーバー設定をサポート：GitHub、Supabase、Vercel、Railway、Context7、Exa、Playwrightなど。

## クロスプラットフォームサポート

[ECC](../参考ドキュメント/README.md)は多种多様なAIプログラミングツールをサポート：

| ツール | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 types |
| Cursor | 48 | 共有 | 共有 | 15 types |
| Codex | 共有 | 命令式 | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 types |
| GitHub Copilot | - | 6 prompts | 命令式 | - |

## コアコンセプト

### [Agent](../参考ドキュメント/agents/01-Code-Review.md)
再利用可能なサブタスク実行者で、各[Agent](../参考ドキュメント/agents/01-Code-Review.md)には明確な責務がある。複数の[Agent](../参考ドキュメント/agents/01-Code-Review.md)を組み合わせて复杂ワークフローを構築。

### [Skill](../参考ドキュメント/skills/最佳实践.md)
特定のタスクを完了するワークフローを定義し、[ECC](../参考ドキュメント/README.md)の主要ワークフロー面。

### Hook
特定のタイミングで自動的にトリガーされるスクリプトで、ワークフロー自動化をサポート。

### Rule
Claude Codeが遵守しなければならない制約ルールで、出力品質を保証。


- [インストール設定](./02-インストール設定.md) - [ECC](../参考ドキュメント/README.md)の使用を開始
- [入門ディレクトリに戻る](../README.md)