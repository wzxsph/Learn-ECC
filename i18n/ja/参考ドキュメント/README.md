# ECC ドキュメント体系

ECC (Everything Claude Code) は Claude Code 用プラグインで、75個のコマンド、60個のAgent、16個のSkills分野、7つのルールドキュメント、Hook/MCP設定体系を提供します。

---

## 1. ECC ドキュメント体系概要

ECC は Claude Code にプロダクションレベルの開発ワークフローサポートを提供し、複数の AI Agent フレームワーク（Claude Code、Codex、OpenCode、Cursor、Gemini）をカバーします。

### 技術スタック

| レイヤー | 技術 | バージョン |
|------|------|------|
| 主要言語 | JavaScript/Node.js | >=18 |
| 次要言語 | Python | >=3.11 |
| パッケージマネージャー | Yarn | 4.9.2 |
| テストランナー | 自作 Node.js テストランナー | - |
| カバレッジ | c8 | 11.x |
| コード検査 | ESLint + markdownlint-cli | - |

### ディレクトリ構造

```
ECC/
├── agents/        # 60個の専門Agent
├── commands/      # 75個のコマンドファイル
├── skills/        # 16個のSkillsディレクトリ
├── rules/         # 7つのルールドキュメント（common + 言語特定）
├── hooks/         # 3つのHookドキュメント
├── mcp/           # 6つのMCPサーバー設定
├── scripts/       # 54個のツールスクリプト
├── README.md
```

---

## 2. クイックナビゲーション

| 分類 | ドキュメント | 説明 |
|------|------|------|
| [コマンド総覧](./commands/) | [commands/](commands/) | 75個のコマンド完全リスト |
| [Agent インデックス](./agents/) | [agents/](agents/) | 60個の専門Agent |
| [Skills インデックス](./skills/) | [skills/](skills/) | 16のSkillsを分野別に分類 |
| [ルールインデックス](./rules/) | [rules/](rules/) | 7つのルールドキュメント (common + 言語特定) |
| [Hooks インデックス](./hooks/) | [hooks/](hooks/) | Hookシステム設定と開発 |
| [MCP インデックス](./mcp/) | [mcp/](mcp/) | MCPサーバー設定と開発 |
| [Scripts インデックス](./scripts/) | [scripts/](scripts/) | 54個のツールスクリプト |

---

## 3. コマンドインデックス

全 **75個のコマンド**、クラス別に分類：

### 3.1 コアワークフロー (6)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/plan` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 要件分析+リスク評価+段階的計画 |
| `/code-review` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | コード品質/セキュリティ/保守性のレビュー |
| `/build-fix` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 自動検出言語+増分修正ビルドエラー修正 |
| `/verify` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 完全検証ループ：ビルド→lint→テスト→型チェック |
| `/quality-gate` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 必要に応じてECC品質パイプラインを実行 |
| `/tdd` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 汎用 TDD ワークフロー |

### 3.2 テスト関連 (8)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/go-test` | [02-テスト.md](commands/02-テスト.md) | Go TDD (テーブル駆動、80%+ カバレッジ) |
| `/kotlin-test` | [02-テスト.md](commands/02-テスト.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-テスト.md](commands/02-テスト.md) | Rust TDD (cargo test + 統合テスト) |
| `/cpp-test` | [02-テスト.md](commands/02-テスト.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-テスト.md](commands/02-テスト.md) | Flutter TDD |
| `/e2e` | [02-テスト.md](commands/02-テスト.md) | 生成 + 実行 Playwright e2e テスト |
| `/test-coverage` | [02-テスト.md](commands/02-テスト.md) | テストカバレッジレポート+ギャップ分析 |
| `/python-testing` | [02-テスト.md](commands/02-テスト.md) | Python テストベストプラクティス |

### 3.3 言語レビュー (7)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/python-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Python PEP 8、型ヒント、セキュリティ |
| `/go-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Go 慣用法、並行処理、エラー処理 |
| `/kotlin-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Kotlin 空安全、コルーチン、アーキテクチャ |
| `/rust-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Rust 所有権、ライフタイム、unsafe |
| `/cpp-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | C++ メモリ安全、モダン idiom |
| `/flutter-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Flutter/Dart パターン |
| `/fastapi-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | FastAPI アーキテクチャ、非同期、Pydantic |

### 3.4 ビルド修正 (6)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/go-build` | [04-ビルド修復.md](commands/04-ビルド修復.md) | Go ビルドエラー + go vet 警告を修正 |
| `/kotlin-build` | [04-ビルド修復.md](commands/04-ビルド修復.md) | Kotlin/Gradle コンパイラエラー修正 |
| `/rust-build` | [04-ビルド修復.md](commands/04-ビルド修復.md) | Rust ビルド + 借用チェッカー問題修正 |
| `/cpp-build` | [04-ビルド修復.md](commands/04-ビルド修復.md) | C++ CMake + リンカー問題修正 |
| `/gradle-build` | [04-ビルド修復.md](commands/04-ビルド修復.md) | Android/KMP Gradle エラー修正 |
| `/flutter-build` | [04-ビルド修復.md](commands/04-ビルド修復.md) | Flutter ビルド修正 |

### 3.5 計画とアーキテクチャ (7)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/plan-prd` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | インタラクティブ PRD 生成器 |
| `/prp-plan` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | 全面的機能計画 |
| `/prp-prd` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | PRP ワークフロー PRD 生成器 |
| `/prp-implement` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | PRP 計画を実行+検証ループ |
| `/prp-pr` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | PRP ワークフローからPRを作成 |
| `/prp-commit` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | PRP 検証コミット |
| `/multi-plan` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | マルチモデル協力計画 (Codex + Gemini) |

### 3.6 セッション管理 (5)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/save-session` | [06-セッション管理.md](commands/06-セッション管理.md) | セッション状態を ~/.claude/session-data/ に保存 |
| `/resume-session` | [06-セッション管理.md](commands/06-セッション管理.md) | 保存したセッションをロードして再開 |
| `/sessions` | [06-セッション管理.md](commands/06-セッション管理.md) | セッション履歴を閲覧+検索+管理 |
| `/checkpoint` | [06-セッション管理.md](commands/06-セッション管理.md) | ワークフローチェックポイントを作成/検証 |
| `/aside` | [06-セッション管理.md](commands/06-セッション管理.md) | コンテキストを失わずにサイド问答に回答 |

### 3.7 学習と改善 (10)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/learn` | [07-学習と改善.md](commands/07-学習と改善.md) | セッションから再利用可能なパターンを抽出 |
| `/learn-eval` | [07-学習と改善.md](commands/07-学習と改善.md) | パターンを抽出 + 自己評価品質 |
| `/evolve` | [07-学習と改善.md](commands/07-学習と改善.md) | instinct を分析 + 進化構造を提案 |
| `/promote` | [07-学習と改善.md](commands/07-学習と改善.md) | プロジェクトの instinct をグローバル範囲に昇格 |
| `/instinct-status` | [07-学習と改善.md](commands/07-学習と改善.md) | すべての学習した instinct を表示 |
| `/instinct-export` | [07-学習と改善.md](commands/07-学習と改善.md) | instinct をファイルにエクスポート |
| `/instinct-import` | [07-学習と改善.md](commands/07-学習と改善.md) | ファイル/URLから instinct をインポート |
| `/skill-create` | [07-学習と改善.md](commands/07-学習と改善.md) | git 歴史を分析 + skill ファイルを生成 |
| `/skill-health` | [07-学習と改善.md](commands/07-学習と改善.md) | Skill 組み合わせ健康ダッシュボード |
| `/rules-distill` | [07-学習と改善.md](commands/07-学習と改善.md) | skills をスキャン + 分野横断原則を抽出 |

### 3.8 ループと自動化 (3)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/loop-start` | [08-ループと自動化.md](commands/08-ループと自動化.md) | 管理自主ループパターンを起動 |
| `/loop-status` | [08-ループと自動化.md](commands/08-ループと自動化.md) | 実行中ループの状態を確認 |
| `/santa-loop` | [08-ループと自動化.md](commands/08-ループと自動化.md) | Santa スタイル自主ループ |

### 3.9 プロジェクト管理 (6)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/projects` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | 既存のプロジェクト + instinct 統計を一覧表示 |
| `/harness-audit` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | agent harness 設定を監査 |
| `/model-route` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | タスクを適切なモデルにルーティング |
| `/pm2` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | PM2 プロセス管理器を初期化 |
| `/setup-pm` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | パッケージマネージャーを設定 (npm/pnpm/yarn/bun) |
| `/project-init` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | プロジェクトを初期化 |

### 3.10 PR ワークフロー (6)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/pr` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | 現在のブランチから GitHub PR を作成 |
| `/review-pr` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | GitHub PR をレビュー |
| `/multi-workflow` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | マルチモデル協力開発 |
| `/multi-backend` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | バックエンド多モデル開発 |
| `/multi-frontend` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | フロントエンド多モデル開発 |
| `/multi-execute` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | マルチモデル協力実行 |

### 3.11 Hookify システム (4)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/hookify` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | 望ましくない動作を防止するhooksを作成 |
| `/hookify-list` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | 設定されたすべての hookify ルールを一覧表示 |
| `/hookify-configure` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | インタラクティブに hookify ルールを有効/無効化 |
| `/hookify-help` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | Hookify システムヘルプ |

### 3.12 ドキュメントと研究 (3)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/update-docs` | [12-ドキュメントと研究.md](commands/12-ドキュメントと研究.md) | プロジェクトドキュメントを更新 |
| `/update-codemaps` | [12-ドキュメントと研究.md](commands/12-ドキュメントと研究.md) | codemaps を再生成 |
| `/ecc-guide` | [12-ドキュメントと研究.md](commands/12-ドキュメントと研究.md) | ECC ユーザーガイド |

### 3.13 リファクタリングとクリーンアップ (2)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/refactor-clean` | [13-リファクタリングとクリーンアップ.md](commands/13-リファクタリングとクリーンアップ.md) | 死コード削除+重複をマージ |
| `/auto-update` | [13-リファクタリングとクリーンアップ.md](commands/13-リファクタリングとクリーンアップ.md) | 自動更新能力 |

### 3.14 その他のコマンド (7)

| コマンド | ファイル | 説明 |
|------|------|------|
| `/jira` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | Jira チケットとの対話 |
| `/gan-build` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | GAN ビルド操作 |
| `/gan-design` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | GAN 設計操作 |
| `/prune` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | 古い instinct を削除 (>30日) |
| `/security-scan` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | セキュリティスキャン |
| `/feature-dev` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | 機能開発アシスタント |
| `/cost-report` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | モデルコストレポート |

---

## 4. Agent インデックス

全 **60個のAgent**、クラス別に分類：

| Agent クラス | ファイル | 説明 |
|------------|------|------|
| [コードレビュー類](agents/1-コードレビュー.md) | [1-コードレビュー.md](agents/1-コードレビュー.md) | 14個のレビューAgent |
| [ビルドリペア類](agents/2-ビルド修復.md) | [2-ビルド修復.md](agents/2-ビルド修復.md) | 14個のビルド修正Agent |
| [計画類](agents/3-計画.md) | [3-計画.md](agents/3-計画.md) | 5個の計画Agent |
| [テスト類](agents/4-テスト.md) | [4-テスト.md](agents/4-テスト.md) | 2個のテストAgent |
| [セキュリティ類](agents/5-セキュリティ.md) | [5-セキュリティ.md](agents/5-セキュリティ.md) | 3個のセキュリティAgent |
| [アーキテクチャ類](agents/6-アーキテクチャ.md) | [6-アーキテクチャ.md](agents/6-アーキテクチャ.md) | 3個のアーキテクチャAgent |

---

## 5. Skills インデックス

**16個のSkills分野**、分野別に分類：

| 分野 | ファイル | 説明 |
|------|------|------|
| [ベストプラクティス](skills/ベストプラクティス.md) | [ベストプラクティス.md](skills/ベストプラクティス.md) | コーディング標準、エラー処理、自主ループ |
| [プログラミング言語](skills/プログラミング言語.md) | [プログラミング言語.md](skills/プログラミング言語.md) | Python/Go/Rust/Kotlin/C++ 等 |
| [フレームワーク](skills/フレームワーク.md) | [フレームワーク.md](skills/フレームワーク.md) | Django/Laravel/NestJS/Spring Boot 等 |
| [テスト](skills/テスト.md) | [テスト.md](skills/テスト.md) | TDD/ユニットテスト/統合テスト/E2E |
| [セキュリティ](skills/セキュリティ.md) | [セキュリティ.md](skills/セキュリティ.md) | セキュリティレビュー、脆弱性スキャン |
| [フロントエンドとデザイン](skills/フロントエンドとデザイン.md) | [フロントエンドとデザイン.md](skills/フロントエンドとデザイン.md) | フロントエンド開発、設計システム |
| [バックエンドとAPI](skills/バックエンドとAPI.md) | [バックエンドとAPI.md](skills/バックエンドとAPI.md) | バックエンドサービス、API 設計、データベース |
| [DevOpsとデプロイ](skills/DevOpsとデプロイ.md) | [DevOpsとデプロイ.md](skills/DevOpsとデプロイ.md) | Docker/K8s/デプロイ戦略 |
| [モニタリングと可観測性](skills/モニタリングと可観測性.md) | [モニタリングと可観測性.md](skills/モニタリングと可観測性.md) | 可観測性、ネットワーク診断 |
| [自動化とスクリプト](skills/自動化とスクリプト.md) | [自動化とスクリプト.md](skills/自動化とスクリプト.md) | 自主ループ、持續学習、エージェント工学 |
| [検索とデータ取得](skills/検索とデータ取得.md) | [検索とデータ取得.md](skills/検索とデータ取得.md) | Exa 検索、データスクレイピング、MCP |
| [GitHubとコラボレーション](skills/GitHubとコラボレーション.md) | [GitHubとコラボレーション.md](skills/GitHubとコラボレーション.md) | GitHub ワークフロー、コードレビュー |
| [AIと機械学習](skills/AIと機械学習.md) | [AIと機械学習.md](skills/AIと機械学習.md) | ニューラルネットワーク、PyTorch、MLOps |
| [クラウドネイティブとインフラ](skills/クラウドネイティブとインフラ.md) | [クラウドネイティブとインフラ.md](skills/クラウドネイティブとインフラ.md) | Kubernetes、Docker、Terraform |
| [特殊技能](skills/特殊技能.md) | [特殊技能.md](skills/特殊技能.md) | ブロックチェーン、ゲーム開発、音视频、IoT |
| [開発ツールチェーン](skills/開発ツールチェーン.md) | [開発ツールチェーン.md](skills/開発ツールチェーン.md) | テストフレームワーク、CI/CD、コード品質 |
| [先進技術](skills/先進技術.md) | [先進技術.md](skills/先進技術.md) | AI Agent、量子計算、エッジ計算 |

---

## 6. ルールインデックス

全 **7つのルールドキュメント**（common + 言語特定）：

| ルール | ファイル | 説明 |
|------|------|------|
| [Git ワークフロー](rules/Gitワークフロー.md) | [Gitワークフロー.md](rules/Gitワークフロー.md) | Git コミット仕様と PR ワークフロー |
| [Hooks システム](rules/Hooksシステム.md) | [Hooksシステム.md](rules/Hooksシステム.md) | Hook 設定と使用ガイド |
| [Agentオーケストレーション](rules/エージェントオーケストレーション.md) | [エージェントオーケストレーション.md](rules/エージェントオーケストレーション.md) | Agent 编排パターン |
| [パフォーマンス最適化](rules/パフォーマンス最適化.md) | [パフォーマンス最適化.md](rules/パフォーマンス最適化.md) | パフォーマンス最適化ガイド |
| [コーディングスタイル](rules/コードスタイル.md) | [コードスタイル.md](rules/コードスタイル.md) | コーディングスタイル仕様 |
| [テストルール](rules/テストルール.md) | [テストルール.md](rules/テストルール.md) | テスト要件（80% カバレッジ） |
| [セキュリティルール](rules/セキュリティルール.md) | [セキュリティルール.md](rules/セキュリティルール.md) | セキュリティチェックリスト |

---

## 7. Hooks インデックス

全 **4つのドキュメント**：

| ドキュメント | ファイル | 説明 |
|------|------|------|
| [Hook タイプ](hooks/Hookタイプ.md) | [Hookタイプ.md](hooks/Hookタイプ.md) | PreToolUse、PostToolUse、Stop タイプ |
| [組み込み Hooks](hooks/組み込みHooks.md) | [組み込みHooks.md](hooks/組み込みHooks.md) | 内蔵 Hook リストと使用 |
| [カスタム開発](hooks/カスタム開発.md) | [カスタム開発.md](hooks/カスタム開発.md) | カスタム Hook 開発ガイド |
| [設定フォーマット](hooks/設定フォーマット.md) | [設定フォーマット.md](hooks/設定フォーマット.md) | hooks.json 設定フォーマット |

---

## 8. MCP インデックス

全 **6つの MCP サーバー設定**：

| ドキュメント | ファイル | 説明 |
|------|------|------|
| [MCP 設定フォーマット](mcp/MCP設定フォーマット.md) | [MCP設定フォーマット.md](mcp/MCP設定フォーマット.md) | MCP 設定ファイルフォーマット |
| [組み込みサーバー](mcp/組み込みサーバー.md) | [組み込みサーバー.md](mcp/組み込みサーバー.md) | 内蔵 MCP サーバー |
| [カスタム開発](mcp/カスタム開発.md) | [カスタム開発.md](mcp/カスタム開発.md) | カスタム MCP サーバー開発 |

---

## 9. Scripts インデックス

全 **54個のツールスクリプト**：

| ドキュメント | ファイル | 説明 |
|------|------|------|
| [ツールスクリプト](scripts/ツールスクリプト.md) | [ツールスクリプト.md](scripts/ツールスクリプト.md) | ecc.js、install-apply.js 等 |
| [ユーティリティライブラリ](scripts/ユーティリティライブラリ.md) | [ユーティリティライブラリ.md](scripts/ユーティリティライブラリ.md) | 共有関数ライブラリ |
| [テストランナー](scripts/テストランナー.md) | [テストランナー.md](scripts/テストランナー.md) | テストランナー使用 |
| [ビルドスクリプト](scripts/ビルドスクリプト.md) | [ビルドスクリプト.md](scripts/ビルドスクリプト.md) | ビルドスクリプト |

---

## 10. 寄稿ガイド

### ファイル命名仕様

- **コマンド、Skills、Agents、Hooks**: 小文字 + ハイフン（如 `code-review.md`）
- **スクリプト**: camelCase または kebab-case（如 `session-start.js`）
- **ルール**: 言語/テーマ別に組織

### コマンドファイルフォーマット

```yaml
---
description: "短い説明"
argument-hint: "[オプションパラメータ]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent ファイルフォーマット

```yaml
---
name: agent-name
description: Agent 用途
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill ファイルフォーマット

```markdown
# Skill 名称

## When to Use
## How It Works
## Examples
```

### コミット流程

1. 新規ファイル作成または既存ファイル修改
2. 命名仕様準拠を確認
3. テスト実行: `node tests/run-all.js`
4. Markdown lint 実行: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. PR を作成してレビューをリクエスト

---

*ECC ドキュメント体系 - Claude Code にプロダクションレベルの開発ワークフローを提供*
