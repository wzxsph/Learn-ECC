# 04 Skills 開発

> Skills の作成と使用をマスターし、ワークフローパターンを再利用可能なスキルとして蓄積

## モジュール目標

本モジュールを完了すると、以下ができる：

- Skill の標準形式と構造を理解
- `/learn` を使用してセッションからパターンを抽出
- `/skill-create` を使用して git 歴史から Skill を作成
- カスタム Skills を作成・管理

## Skill 形式

各 Skill は Markdown ファイルで、標準化された部分を含む：

```markdown
# Skill 名称

## When to Use
この Skill をいつ使用するかを説明

## How It Works
Skill の作動原理を詳細に説明

## Examples
使用例

## Related Skills
関連する Skills
```

### 必須部分

| 部分 | 説明 | 必須 |
|------|------|------|
| When to Use | 使用シーン説明 | はい |
| How It Works | 作動原理説明 | はい |
| Examples | 使用例 | はい |

### オプション部分

| 部分 | 説明 |
|------|------|
| Related Skills | 関連する Skills |
| Prerequisites | 前置条件 |
| Tips | 使用テクニック |

## Skill タイプ

### 1. ワークフロー類 Skill

完全なワークフローを記述：

```markdown
# TDD Workflow

## When to Use
新機能の開発や bug 修復時に TDD 方法を使用

## How It Works
1. テストを作成（レッド）
2. 機能を実装（グリーン）
3. リファクタリング最適化（ブルー）

## Examples
/step 1: 失敗するテストを作成
/step 2: 最小コードを実装
/step 3: テストを実行して検証
/step 4: リファクタリング
```

### 2. パターン類 Skill

コードパターンを蓄積：

```markdown
# API Response Pattern

## When to Use
API 返答形式を設計する場合に使用

## How It Works
統一された返答形式を使用：
- success: 成功状態
- data: 返答データ
- error: エラー情報（オプション）

## Examples
{ "success": true, "data": {...}, "error": null }
```

### 3. ベストプラクティス類 Skill

ベストプラクティスを記録：

```markdown
# Git Commit Best Practices

## When to Use
コード提出前に提出情報をチェック

## How It Works
Conventional Commits 形式に従う：
- feat: 新機能
- fix: 修復
- docs: ドキュメント
- test: テスト
- refactor: リファクタリング

## Examples
feat: add user authentication
fix: resolve login bug
docs: update README
```

## Skill 作成の方法

### 方法 1: `/learn` を使用

成功したセッションからパターンを抽出：

```
/learn "このセッションからワークフローパターンを抽出"
```

### 方法 2: `/skill-create` を使用

git 歴史から Skill を作成：

```
/skill-create "ビルドエラー修復のワークフロー"
```

### 方法 3: 手動作成

Skill ファイルを直接記述：

```markdown
# My Custom Skill

## When to Use
...

## How It Works
...
```

## Skill 存储位置

| タイプ | 位置 | 説明 |
|------|------|------|
| 公式 Skills | `~/.claude/skills/` | Claude Code 内蔵 |
| カスタム Skills | プロジェクト `skills/` | プロジェクト専用 |
| ルール Skills | `~/.claude/rules/` | 全般ルール |

## Skill 管理コマンド

| コマンド | 機能 |
|------|------|
| `/skill-scout` | 利用可能な Skills を検索 |
| `/skill-stocktake` | すべての Skills を一覧表示 |
| `/skill-create` | 新規 Skill を作成 |
| `/skill-health` | Skill 健康状態をチェック |

## Skill の使用

### 直接使用

```
/skill-name
```

### タスクでの使用

複雑なタスクで Skill を参照：

```
支払いモジュールを開発する必要があります。TDD ワークフローを使用
```

## Skill ベストプラクティス

1. **単一責務**: 各 Skill は1つのことだけを実行
2. **明確な命名**: 記述的な名前を使用
3. **完全なドキュメント**: すべての必須部分を含む
4. **実際の検証**: 実際のワークフローに基づく
5. **持續的改善**: 使用フィードバックに基づいて改善

## 配套教材

完全な Skills ドキュメント：`../../参考ドキュメント/skills/最佳实践.md`

## 次へ

- [演習题目](./exercises/演習.md)を学習
- [Skills 完全ドキュメント](../../参考ドキュメント/skills/最佳实践.md)を阅读