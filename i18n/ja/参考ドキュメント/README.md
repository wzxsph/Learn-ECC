# ECC ドキュメント体系

ECC (Everything Claude Code) は一个 Claude Code プラグイン，提供 75 个コマンド、60 个 Agent、16 个 Skills、7 个ルールドキュメント和完整的 Hook/MCP 設定体系。

---

## 1. ECC ドキュメント体系概要

ECC 为 Claude Code 提供生产级開発ワークフローサポート，涵盖多个 AI Agent フレームワーク（Claude Code、Codex、OpenCode、Cursor、Gemini）。

### テクスタック

| レイヤー | 技術 | バージョン |
|------|------|------|
| 主要言語 | JavaScript/Node.js | >=18 |
| 次要言語 | Python | >=3.11 |
| パッケージマネージャー | Yarn | 4.9.2 |
| テストランナー | 自定义 Node.js テストランナー | - |
| カバレッジ | c8 | 11.x |
| コード検査 | ESLint + markdownlint-cli | - |

### ディレクトリ構造

```
ECC/
├── agents/        # 60 个专业 Agent
├── commands/      # 75 个コマンドファイル
├── skills/        # 16 个 Skills ディレクトリ
├── rules/         # 7 个ルールドキュメント（common + 言語特定）
├── hooks/         # 3 个 Hook ドキュメント
├── mcp/           # 6 个 MCP サーバー設定
├── scripts/       # 54 个ツールスクリプト
├── README.md
```

---

## 2. クイックナビゲーション

| 分類 | ドキュメント | 説明 |
|------|------|------|
| [コマンド总览](./commands/) | [commands/](commands/) | 75 个コマンド的完全リスト |
| [Agent インデックス](./agents/) | [agents/](agents/) | 60 个专业 Agent |
| [Skills インデックス](./skills/) | [skills/](skills/) | 16 个 Skills 按领域分類 |
| [ルールインデックス](./rules/) | [rules/](rules/) | 7 个ルールドキュメント (common + 言語特定) |
| [Hooks インデックス](./hooks/) | [hooks/](hooks/) | Hook システム設定和開発 |
| [MCP インデックス](./mcp/) | [mcp/](mcp/) | MCP サーバー設定和開発 |
| [Scripts インデックス](./scripts/) | [scripts/](scripts/) | 54 个ツールスクリプト |

---

## 3. コマンドインデックス

共 **75 个コマンド**，按クラス别分组：

### 3.1 コアワークフロー (6)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/plan` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 需求分析+风险评估+分ステップ計画 |
| `/code-review` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | コード品質/セキュリティ/保守性レビュー |
| `/build-fix` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 自動检测言語+增量修正ビルドエラー |
| `/verify` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 完整検証ループ：ビルド→lint→テスト→タイプ検査 |
| `/quality-gate` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 按需実行 ECC 品質流水线 |
| `/tdd` | [01-コアワークフロー.md](commands/01-コアワークフロー.md) | 通用 TDD ワークフロー |

### 3.2 テスト関連 (8)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/go-test` | [02-テスト関連.md](commands/02-テスト関連.md) | Go TDD (表格驱动，80%+ カバレッジ) |
| `/kotlin-test` | [02-テスト関連.md](commands/02-テスト関連.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-テスト関連.md](commands/02-テスト関連.md) | Rust TDD (cargo test + 統合テスト) |
| `/cpp-test` | [02-テスト関連.md](commands/02-テスト関連.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-テスト関連.md](commands/02-テスト関連.md) | Flutter TDD |
| `/e2e` | [02-テスト関連.md](commands/02-テスト関連.md) | 生成 + 実行 Playwright e2e テスト |
| `/test-coverage` | [02-テスト関連.md](commands/02-テスト関連.md) | テストカバレッジレポート+差距分析 |
| `/python-testing` | [02-テスト関連.md](commands/02-テスト関連.md) | Python テストベストプラクティス |

### 3.3 言語レビュー (7)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/python-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Python PEP 8、型ヒント、セキュリティ |
| `/go-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Go 惯用法、並行処理、エラー処理 |
| `/kotlin-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Kotlin 空セキュリティ、コルーチン、アーキテクチャ |
| `/rust-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Rust すべて権、ライフタイム、unsafe |
| `/cpp-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | C++ メモリセキュリティ、现代 idiom |
| `/flutter-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | Flutter/Dart パターン |
| `/fastapi-review` | [03-言語レビュー.md](commands/03-言語レビュー.md) | FastAPI アーキテクチャ、非同期、Pydantic |

### 3.4 ビルド修正 (6)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/go-build` | [04-ビルド修正.md](commands/04-ビルド修正.md) | 修正 Go ビルドエラー + go vet 警告 |
| `/kotlin-build` | [04-ビルド修正.md](commands/04-ビルド修正.md) | 修正 Kotlin/Gradle コンパイル器エラー |
| `/rust-build` | [04-ビルド修正.md](commands/04-ビルド修正.md) | 修正 Rust ビルド + 借用検査器問題 |
| `/cpp-build` | [04-ビルド修正.md](commands/04-ビルド修正.md) | 修正 C++ CMake + 链接器問題 |
| `/gradle-build` | [04-ビルド修正.md](commands/04-ビルド修正.md) | 修正 Android/KMP Gradle エラー |
| `/flutter-build` | [04-ビルド修正.md](commands/04-ビルド修正.md) | Flutter ビルド修正 |

### 3.5 計画とアーキテクチャ (7)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/plan-prd` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | 交互式 PRD 生成器 |
| `/prp-plan` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | 全面的機能計画 |
| `/prp-prd` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | PRP ワークフロー PRD 生成器 |
| `/prp-implement` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | 実行 PRP 計画+検証ループ |
| `/prp-pr` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | 从 PRP ワークフロー作成 PR |
| `/prp-commit` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | PRP 検証コミット |
| `/multi-plan` | [05-計画とアーキテクチャ.md](commands/05-計画とアーキテクチャ.md) | 多モデル协作計画 (Codex + Gemini) |

### 3.6 セッション管理 (5)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/save-session` | [06-セッション管理.md](commands/06-セッション管理.md) | 保存セッション状态到 ~/.claude/session-data/ |
| `/resume-session` | [06-セッション管理.md](commands/06-セッション管理.md) | 加载并再開保存的セッション |
| `/sessions` | [06-セッション管理.md](commands/06-セッション管理.md) | 浏览+搜索+管理セッション历史 |
| `/checkpoint` | [06-セッション管理.md](commands/06-セッション管理.md) | 作成/検証ワークフロー検査点 |
| `/aside` | [06-セッション管理.md](commands/06-セッション管理.md) | 回答侧问而不丢失上下文 |

### 3.7 学習と改善 (10)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/learn` | [07-学習と改善.md](commands/07-学習と改善.md) | 从セッション中提取可复用パターン |
| `/learn-eval` | [07-学習と改善.md](commands/07-学習と改善.md) | 提取パターン + 自我评估品質 |
| `/evolve` | [07-学習と改善.md](commands/07-学習と改善.md) | 分析 instinct + 推奨进化構造 |
| `/promote` | [07-学習と改善.md](commands/07-学習と改善.md) | 将プロジェクト instinct 提升到全局范围 |
| `/instinct-status` | [07-学習と改善.md](commands/07-学習と改善.md) | 表示すべて学習的 instinct |
| `/instinct-export` | [07-学習と改善.md](commands/07-学習と改善.md) | エクスポート instinct 到ファイル |
| `/instinct-import` | [07-学習と改善.md](commands/07-学習と改善.md) | 从ファイル/URL インポート instinct |
| `/skill-create` | [07-学習と改善.md](commands/07-学習と改善.md) | 分析 git 历史+生成 skill ファイル |
| `/skill-health` | [07-学習と改善.md](commands/07-学習と改善.md) | Skill 组合健康仪表板 |
| `/rules-distill` | [07-学習と改善.md](commands/07-学習と改善.md) | 扫描 skills + 提取跨领域原則 |

### 3.8 ループと自動化 (3)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/loop-start` | [08-ループと自動化.md](commands/08-ループと自動化.md) | 起動托管自主ループパターン |
| `/loop-status` | [08-ループと自動化.md](commands/08-ループと自動化.md) | 検査実行中ループ的状态 |
| `/santa-loop` | [08-ループと自動化.md](commands/08-ループと自動化.md) | Santa 风格自主ループ |

### 3.9 プロジェクト管理 (6)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/projects` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | 列出已知プロジェクト + instinct 統計 |
| `/harness-audit` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | 审计 agent harness 設定 |
| `/model-route` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | ルーティングタスク到合适モデル |
| `/pm2` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | PM2 プロセス管理器初期化 |
| `/setup-pm` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | 設定パッケージマネージャー (npm/pnpm/yarn/bun) |
| `/project-init` | [09-プロジェクト管理.md](commands/09-プロジェクト管理.md) | プロジェクト初期化 |

### 3.10 PR ワークフロー (6)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/pr` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | 从当前ブランチ作成 GitHub PR |
| `/review-pr` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | レビュー GitHub PR |
| `/multi-workflow` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | 多モデル协作開発 |
| `/multi-backend` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | バックエンド多モデル開発 |
| `/multi-frontend` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | フロントエンド多モデル開発 |
| `/multi-execute` | [10-PRワークフロー.md](commands/10-PRワークフロー.md) | 多モデル协作実行 |

### 3.11 Hookify システム (4)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/hookify` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | 作成 hooks 防止不良行为 |
| `/hookify-list` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | 列出すべて設定的 hookify ルール |
| `/hookify-configure` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | 交互式あり効化/無効化 hookify ルール |
| `/hookify-help` | [11-Hookifyシステム.md](commands/11-Hookifyシステム.md) | Hookify システム帮助 |

### 3.12 ドキュメントと研究 (3)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/update-docs` | [12-ドキュメントと研究.md](commands/12-ドキュメントと研究.md) | 更新プロジェクトドキュメント |
| `/update-codemaps` | [12-ドキュメントと研究.md](commands/12-ドキュメントと研究.md) | 重新生成 codemaps |
| `/ecc-guide` | [12-ドキュメントと研究.md](commands/12-ドキュメントと研究.md) | ECC 用户ガイド |

### 3.13 リファクタリングとクリーンアップ (2)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/refactor-clean` | [13-リファクタリングとクリーンアップ.md](commands/13-リファクタリングとクリーンアップ.md) | 削除死コード+マージ重复 |
| `/auto-update` | [13-リファクタリングとクリーンアップ.md](commands/13-リファクタリングとクリーンアップ.md) | 自動更新能力 |

### 3.14 その他のコマンド (7)

| コマンド | ファイル | 用途 |
|------|------|------|
| `/jira` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | Jira 工单交互 |
| `/gan-build` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | GAN ビルド操作 |
| `/gan-design` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | GAN 設計操作 |
| `/prune` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | 削除陈旧 instinct (>30天) |
| `/security-scan` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | セキュリティ扫描 |
| `/feature-dev` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | 機能開発助手 |
| `/cost-report` | [14-その他のコマンド.md](commands/14-その他のコマンド.md) | モデル成本レポート |

---

## 4. Agent インデックス

共 **60 个 Agent**，按クラス别分组：

| Agent クラス别 | ファイル | 説明 |
|------------|------|------|
| [コードレビュー担当](agents/1.%20コードレビュー担当.md) | [1. コードレビュー担当.md](agents/1.%20コードレビュー担当.md) | 14 个レビュー Agent |
| [ビルド修正クラス](agents/2.%20ビルド修正クラス.md) | [2. ビルド修正クラス.md](agents/2.%20ビルド修正クラス.md) | 14 个ビルド修正 Agent |
| [計画類](agents/3.%20計画類.md) | [3. 計画類.md](agents/3.%20計画類.md) | 5 个計画 Agent |
| [テスト類](agents/4.%20テスト類.md) | [4. テスト類.md](agents/4.%20テスト類.md) | 2 个テスト Agent |
| [セキュリティ類](agents/5.%20セキュリティ類.md) | [5. セキュリティ類.md](agents/5.%20セキュリティ類.md) | 3 个セキュリティ Agent |
| [アーキテクチャ類](agents/6.%20アーキテクチャ類.md) | [6. アーキテクチャ類.md](agents/6.%20アーキテクチャ類.md) | 3 个アーキテクチャ Agent |

---

## 5. Skills インデックス

$**16 个 Skills 领域**，按领域分類：

| 领域 | ファイル | 説明 |
|------|------|------|
| [ベストプラクティス](skills/ベストプラクティス.md) | [ベストプラクティス.md](skills/ベストプラクティス.md) | エンコード標準、エラー処理、自主ループ |
| [プログラミング言語](skills/プログラミング言語.md) | [プログラミング言語.md](skills/プログラミング言語.md) | Python/Go/Rust/Kotlin/C++ 等 |
| [フレームワーク](skills/フレームワーク.md) | [フレームワーク.md](skills/フレームワーク.md) | Django/Laravel/NestJS/Spring Boot 等 |
| [テスト](skills/テスト.md) | [テスト.md](skills/テスト.md) | TDD/ユニットテスト/統合テスト/E2E |
| [セキュリティ](skills/セキュリティ.md) | [セキュリティ.md](skills/セキュリティ.md) | セキュリティレビュー、漏洞扫描 |
| [フロントエンドとデザイン](skills/フロントエンドとデザイン.md) | [フロントエンドとデザイン.md](skills/フロントエンドとデザイン.md) | フロントエンド開発、設計システム |
| [バックエンドとAPI](skills/バックエンドとAPI.md) | [バックエンドとAPI.md](skills/バックエンドとAPI.md) | バックエンドサービス、API 設計、データベース |
| [デプロイとDevOps](skills/デプロイとDevOps.md) | [デプロイとDevOps.md](skills/デプロイとDevOps.md) | Docker/K8s/デプロイ戦略 |
| [監視とオブザーバビリティ](skills/監視とオブザーバビリティ.md) | [監視とオブザーバビリティ.md](skills/監視とオブザーバビリティ.md) | 可観測性、网络診断 |
| [自動化とスクリプト](skills/自動化とスクリプト.md) | [自動化とスクリプト.md](skills/自動化とスクリプト.md) | 自主ループ、持续学習、プロキシ工程 |
| [検索とデータ取得](skills/検索とデータ取得.md) | [検索とデータ取得.md](skills/検索とデータ取得.md) | Exa 搜索、データ抓取、MCP |
| [GitHubと協業](skills/GitHubと協業.md) | [GitHubと協業.md](skills/GitHubと協業.md) | GitHub ワークフロー、コードレビュー |
| [AIと機械学習](skills/AIと機械学習.md) | [AIと機械学習.md](skills/AIと機械学習.md) | 神经网络、PyTorch、MLOps |
| [クラウドネイティブとインフラ](skills/クラウドネイティブとインフラ.md) | [クラウドネイティブとインフラ.md](skills/クラウドネイティブとインフラ.md) | Kubernetes、Docker、Terraform |
| [特殊分野スキル](skills/特殊分野スキル.md) | [特殊分野スキル.md](skills/特殊分野スキル.md) | 区块链、游戏開発、音视频、IoT |
| [開発ツールチェーン](skills/開発ツールチェーン.md) | [開発ツールチェーン.md](skills/開発ツールチェーン.md) | テストフレームワーク、CI/CD、コード品質 |
| [最先端技術](skills/最先端技術.md) | [最先端技術.md](skills/最先端技術.md) | AI Agent、量子计算、边缘计算 |

---

## 6. ルールインデックス

共 **7 个ルールドキュメント**（common + 言語特定）：

| ルール | ファイル | 説明 |
|------|------|------|
| [Git ワークフロー](rules/Gitワークフロー.md) | [Gitワークフロー.md](rules/Gitワークフロー.md) | Git コミット仕様和 PR ワークフロー |
| [Hooks システム](rules/Hooksシステム.md) | [Hooksシステム.md](rules/Hooksシステム.md) | Hook 設定和使用ガイド |
| [Agentオーケストレーション](rules/Agentオーケストレーション.md) | [Agentオーケストレーション.md](rules/Agentオーケストレーション.md) | Agent 编排パターン |
| [パフォーマンス最適化](rules/パフォーマンス最適化.md) | [パフォーマンス最適化.md](rules/パフォーマンス最適化.md) | パフォーマンス最適化ガイド |
| [コーディングスタイル](rules/コーディングスタイル.md) | [コーディングスタイル.md](rules/コーディングスタイル.md) | エンコード风格仕様 |
| [テストルール](rules/テストルール.md) | [テストルール.md](rules/テストルール.md) | テスト要件（80% カバレッジ） |
| [セキュリティルール](rules/セキュリティルール.md) | [セキュリティルール.md](rules/セキュリティルール.md) | セキュリティ検査清单 |

---

## 7. Hooks インデックス

共 **4 个ドキュメント**：

| ドキュメント | ファイル | 説明 |
|------|------|------|
| [Hook タイプ](hooks/Hookタイプ.md) | [Hookタイプ.md](hooks/Hookタイプ.md) | PreToolUse、PostToolUse、Stop タイプ |
| [内置 Hooks](hooks/内置Hooks.md) | [内置Hooks.md](hooks/内置Hooks.md) | 内置 Hook 列表和使用 |
| [カスタム開発](hooks/カスタム開発.md) | [カスタム開発.md](hooks/カスタム開発.md) | 自定义 Hook 開発ガイド |
| [設定フォーマット](hooks/設定フォーマット.md) | [設定フォーマット.md](hooks/設定フォーマット.md) | hooks.json 設定フォーマット |

---

## 8. MCP インデックス

共 **6 个 MCP サーバー設定**：

| ドキュメント | ファイル | 説明 |
|------|------|------|
| [MCP 設定フォーマット](mcp/MCP設定フォーマット.md) | [MCP設定フォーマット.md](mcp/MCP設定フォーマット.md) | MCP 設定ファイルフォーマット |
| [内置サーバー](mcp/内置サーバー.md) | [内置サーバー.md](mcp/内置サーバー.md) | 内置 MCP サーバー |
| [カスタム開発](mcp/カスタム開発.md) | [カスタム開発.md](mcp/カスタム開発.md) | 自定义 MCP サーバー開発 |

---

## 9. Scripts インデックス

共 **54 个ツールスクリプト**：

| ドキュメント | ファイル | 説明 |
|------|------|------|
| [ツールスクリプト](scripts/ツールスクリプト.md) | [ツールスクリプト.md](scripts/ツールスクリプト.md) | ecc.js、install-apply.js 等 |
| [ツール関数ライブラリ](scripts/ツール関数ライブラリ.md) | [ツール関数ライブラリ.md](scripts/ツール関数ライブラリ.md) | 共享関数库 |
| [テストランナー](scripts/テストランナー.md) | [テストランナー.md](scripts/テストランナー.md) | テストランナー使用 |
| [ビルドスクリプト](scripts/ビルドスクリプト.md) | [ビルドスクリプト.md](scripts/ビルドスクリプト.md) | ビルドスクリプト |

---

## 10. 贡献ガイド

### ファイル命名仕様

- **コマンド、Skills、Agents、Hooks**: 小写 + 连字符（如 `code-review.md`）
- **スクリプト**: camelCase 或 kebab-case（如 `session-start.js`）
- **ルール**: 按言語/主题组织

### コマンドファイルフォーマット

```yaml
---
description: "简短描述"
argument-hint: "[オプション参数]"
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

1. 作成新ファイル或修改现ありファイル
2. 确保遵循命名仕様
3. 実行テスト: `node tests/run-all.js`
4. 実行 markdown lint: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. 作成 PR リクエストレビュー

---

*ECC ドキュメント体系 - 为 Claude Code 提供生产级開発ワークフロー*
