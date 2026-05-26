# 02 - Agent コラボレーション

複数Agentコラボレーションシステムの設計、实现、管理を学習します。

## 概要

ECCは**66個の専門[Agent](../../参考ドキュメント/agents/1.%20代码审查类.md)**を提供し、6大カテゴリーに分類されます。これらのAgentは標準化されたワークフローを通じて协同動作し、コードレビューからシステム設計まで各类の複雑なタスクを完了します。

---

## 1. Agent アーキテクチャ

### 1.1 単一 Agent vs 複数 Agent

| アーキテクチャ | 適用シーン | 特徴 |
|------|----------|------|
| **単一 Agent** | 単純なタスク、独立操作 | 直接呼び出し、快速的응답 |
| **複数 Agent** | 複雑なタスク、協調が必要 | タスク分解、結果統合 |

### 1.2 利用可能な Agent 列表

| Agent | 用途 | 使用タイミング |
|-------|------|----------|
| **planner** | 実施計画 | 複雑な機能、リファクタリング |
| **architect** | システム設計 | アーキテクチャ決定 |
| **tdd-guide** | テスト駆動開発 | 新機能、bug 修復 |
| **code-reviewer** | コードレビュー | コード記述後 |
| **security-reviewer** | セキュリティ分析 | 提交前 |
| **build-error-resolver** | ビルドエラー修復 | ビルド失敗時 |
| **e2e-runner** | エンドツーエンドテスト | 重要なユーザーフロー |
| **refactor-cleaner** | 死コード清理 | コードメンテナンス |
| **doc-updater** | ドキュメント更新 | ドキュメント更新時 |
| **rust-reviewer** | Rust コードレビュー | Rust プロジェクト |
| **harmonyos-app-resolver** | HarmonyOS アプリ開発 | HarmonyOS/ArkTS プロジェクト |

### 1.3 Agent ファイル形式

```yaml
---
name: agent-name
description: Agent 用途
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

---

## 2. Agent カテゴリー详解

### 2.1 コードレビュー類（16個）

コード品質、セキュリティ、メンテナンス性レビューを担当。

| Agent | 專門 |
|-------|------|
| code-reviewer | 通用コードレビュー |
| security-reviewer | セキュリティ脆弱性分析 |
| python-reviewer | Python PEP 8、型ヒント |
| go-reviewer | Go 慣用法、並行処理 |
| kotlin-reviewer | Kotlin null安全、协程 |
| rust-reviewer | Rust 所有権、ライフサイクル |
| cpp-reviewer | C++ メモリ安全 |
| flutter-reviewer | Flutter/Dart モード |
| fastapi-reviewer | FastAPI アーキテクチャ、async |
| typescript-reviewer | TypeScript 型安全 |
| java-reviewer | Java コーディング標準 |
| csharp-reviewer | C# ベストプラクティス |
| ruby-reviewer | Ruby 慣用法 |
| php-reviewer | PHP セキュリティと性能 |
| swift-reviewer | Swift 並行処理、メモリ管理 |
| go-security-reviewer | Go セキュリティレビュー |

### 2.2 ビルドリペア類（10個）

各言語のビルドエラー修復に專門。

| Agent | 專門 |
|-------|------|
| build-error-resolver | 通用ビルドエラー修復 |
| go-build | Go ビルド + go vet |
| kotlin-build | Kotlin/Gradle コンパイル |
| rust-build | Rust借用チェッカー |
| cpp-build | C++ CMake + リンカー |
| gradle-build | Android/KMP Gradle |
| flutter-build | Flutter ビルド |
| maven-build | Maven ビルド |
| cmake-build | CMake 設定 |
| ninja-build | Ninja ビルドシステム |

### 2.3 計画類（5個）

需要分析和システム計画を担当。

| Agent | 專門 |
|-------|------|
| planner | 実施計画 |
| architect | システムアーキテクチャ設計 |
| prp-planner | PRP ワークフロー計画 |
| multi-plan | マルチモデル協調計画 |
| product-manager | 製品需要分析 |

### 2.4 テスト類（2個）

テスト駆動開発とカバー率に專門。

| Agent | 專門 |
|-------|------|
| tdd-guide | TDD ワークフローガイド |
| e2e-runner | Playwright E2E テスト |

### 2.5 セキュリティ類（5個）

セキュリティレビューと脆弱性スキャンを担当。

| Agent | 專門 |
|-------|------|
| security-reviewer | 通用セキュリティレビュー |
| owasp-reviewer | OWASP Top 10 |
| pentest-reviewer | ペネトレーションテスト |
| vulnerability-scanner | 脆弱性スキャン |
| secrets-detector | 鍵と凭证検出 |

### 2.6 アーキテクチャ類（5個）

システムアーキテクチャと技術決定を担当。

| Agent | 專門 |
|-------|------|
| architect | システムアーキテクチャ設計 |
| microservices-architect | マイクロサービスアーキテクチャ |
| frontend-architect | フロントエンドアーキテクチャ |
| backend-architect | バックエンドアーキテクチャ |
| devops-architect | DevOps アーキテクチャ |

---

## 3. コラボレーションモード

### 3.1 マスタースレーブモード

1つのマスタAgentが複数のスレーブAgentを協調：

```
ユーザー要求 → マスタ Agent（planner）
           ├── スレーブ Agent 1（code-reviewer）
           ├── スレーブ Agent 2（tdd-guide）
           └── スレーブ Agent 3（build-error-resolver）
```

### 3.2 ピアツーピアモード

複数のAgentが並行して動作し、結果が統合：

```
ユーザー要求 → Agent 1（セキュリティ分析）
        → Agent 2（性能分析）
        → Agent 3（コード品質分析）
           ↓ 結果統合
        最終報告
```

### 3.3 パイプラインモード

Agentが順番に処理し、各Agentの出力が次の入力：

```
入力 → Agent 1 → Agent 2 → Agent 3 → 出力
```

---

## 4. タスク配布策略

### 4.1 タスク分解

複雑なタスクを独立したサブタスクに分解：

```markdown
# 複雑なタスク分解例
元のタスク：ユーザー認証システムを実装

分解後：
1. データベース schema 設計 → architect
2. API エンドポイント実装 → backend-developer
3. テスト作成 → tdd-guide
4. セキュリティレビュー → security-reviewer
```

### 4.2 並行実行

獨立したタスクを並行して実行して効率を向上：

```markdown
# 良い：並行実行
3つの Agent を並行起動：
1. Agent 1: 認証モジュールセキュリティ分析
2. Agent 2: キャッシュシステム性能レビュー
3. Agent 3: ユーティリティ型チェック

# 悪い： 불필요한 순차 실행
まず agent 1、その後 agent 2、その後 agent 3
```

### 4.3 ロードバランシング

Agent の负载に応じてタスクを分配：

| 要素 | 説明 |
|------|------|
| タスク複雑度 | 単純なタスク → 軽量 Agent |
| リソース需要 | 重計算 → 专用 Agent |
| 可用性 | 過負荷 Agent を回避 |

---

## 5. マルチパースペクティブ分析

複雑な問題には、役割分担の sub-agents を使用：

| 役割 | 責務 |
|------|------|
| 事実審査員 | 情報とデータの正確性を検証 |
| 上級エンジニア | 技術的実現可能性とベストプラクティスを評価 |
| セキュリティ専門家 | セキュリティリスクと脆弱性を識別 |
| 一貫性審査員 | 解決策の内部一貫性を確保 |
| 重複検査員 | 複製と浪费を識別 |

---

## 6. Agent 通信

### 6.1 直接呼び出し

```markdown
planner agentを使用して実施計画を作成
```

### 6.2 タスク委託

```markdown
コードレビュータスクを code-reviewer agentに委託
```

### 6.3 結果統合

複数の Agent の結果を統合する必要がある：

```markdown
以下から統合：
- security-reviewer: セキュリティ問題列表
- code-reviewer: コード品質問題列表
- tdd-guide: テストカバー率報告

→ 包括的報告を生成
```

---

## 7. 即時使用原則

ユーザーのプロンプトなしで自動的に呼び出す：

| シーン | Agent |
|------|-------|
| 複雑な機能要求 | **planner** |
| コード作成/変更直後 | **code-reviewer** |
| Bug 修復または新機能 | **tdd-guide** |
| アーキテクチャ決定 | **architect** |
| ビルド失敗 | **build-error-resolver** |
| セキュリティ関連 | **security-reviewer** |

---

## 演習

[演習](./exercises/演習.md) のタスクを完了します。

---

[コア能力ディレクトリに戻る](../README.md)