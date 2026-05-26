# 05 Rules 作成

> プロジェクトレベルの規則を作成して、コードレビューと品質基準をカスタマイズ

## モジュール目標

本モジュールを完了すると、以下ができる：

- Claude Code Rules の形式と分類を理解
- セキュリティ規則、コードスタイル規則、テスト規則を作成
- プロジェクト固有の開発規範を作成
- 規則の優先度と実行策略を構成

## Rules 分類体系

| 分類 | 用途 | 例 |
|------|------|------|
| **セキュリティ規則** | セキュリティチェック、脆弱性防護 | ハードコードパスワード不可、SQL 注入防護 |
| **コードスタイル** | コーディング規範、フォーマット | インデント、命名規則、コメント規範 |
| **テスト規則** | テストカバレッジ、テスト品質 | 80% カバレッジ要件、AAA モード |
| **Git 規則** | 提出規範、分岐戦略 | 提出情報形式、PR フロー |
| **性能規則** | 性能最適化、リソース管理 | N+1 クエリ検出、キャッシュ戦略 |

## Rules 形式

Rules ファイルは Markdown 形式を使用し、YAML frontmatter をサポート：

```markdown
---
name: rule-name
severity: high
trigger: tool-name
---

# 規則名称

## 規則説明

規則の詳細な説明...

## チェックロジック

違反を検出する方法...

## 修復提案

問題を修復する方法...
```

### Frontmatter フィールド

| フィールド | 説明 | 必須 |
|------|------|------|
| name | 規則名称 | はい |
| severity | 重大度 (critical/high/medium/low) | はい |
| trigger | トリガーツール | いいえ |
| enabled | 有効かどうか | いいえ |

## セキュリティ規則

### ハードコード凭证の禁止

```markdown
---
name: no-hardcoded-credentials
severity: critical
trigger: Bash
---

# ハードコード凭证の禁止

## 規則説明

コードでの API keys、パスワード、tokens などの機密情報のハードコードを禁止。

## チェックロジック

正規表現を使用して一般的な凭证パターンを検出：
- `api[_-]?key\s*=\s*["'][A-Za-z0-9]{20,}`
- `password\s*=\s*["'][^"']+`
- `token\s*=\s*["'][A-Za-z0-9]{32,}`

## 修復提案

環境変数を使用：
```javascript
const apiKey = process.env.API_KEY;
```
```

### SQL 注入防護

```markdown
---
name: no-sql-injection
severity: critical
trigger: Read
---

# SQL 注入防護

## 規則説明

文字列連結で SQL クエリを構築することを禁止。

## チェックロジック

文字列連結の SQL 文を検出：
- `"SELECT * FROM users WHERE id = " + userId`
- `query("SELECT * FROM " + tableName)`

## 修復提案

パラメータ化クエリを使用：
```javascript
db.query("SELECT * FROM users WHERE id = ?", [userId]);
```
```

## コードスタイル規則

### 関数長の制限

```markdown
---
name: max-function-length
severity: medium
---

# 関数長制限

## 規則説明

関数は短く保つべきで、的理想的には50行を超えない。

## チェックロジック

関数の行数をカウントし、50行を超えると警告。

## 修復提案

長い関数を複数の小関数に分割し、それぞれが単一責務を担当。

### 変数命名規範

```markdown
---
name: consistent-naming
severity: low
---

# 変数命名規範

## 規則説明

一貫した命名規則を使用。

## チェックロジック

- 変数と関数は camelCase を使用
- 定数は UPPER_SNAKE_CASE を使用
- クラス名は PascalCase を使用

## 修復提案

プロジェクトの naming-conventions.md を参照。

## テスト規則

### カバレッジ要件

```markdown
---
name: minimum-test-coverage
severity: high
---

# 最低テストカバレッジ

## 規則説明

すべての新しいコードは80%以上的テストカバレッジが必要。

## チェックロジック

テストカバレッジツールを実行し、80%に到達するかどうかをチェック。

## 修復提案

カバーされていないコードのテストケースを作成。

### AAA テストモード

```markdown
---
name: aaa-test-pattern
severity: medium
---

# AAA テストモード

## 規則説明

テストは Arrange-Act-Assert 構造に従うべき。

## チェックロジック

テストが明確に3つの段階を含むかどうかをチェック。

## 修復提案

テストを書き直し、明確に分離：
1. Arrange - テストデータを準備
2. Act - テスト対象の操作を実行
3. Assert - 結果を検証

## Git 規則

### 提出情報形式

```markdown
---
name: conventional-commits
severity: medium
trigger: Bash
condition: command.includes('git commit')
---

# 提出情報形式

## 規則説明

Conventional Commits 規範に従う。

## チェックロジック

提出情報形式を検証：
```
<type>: <description>

[optional body]
```

タイプ：feat, fix, docs, test, refactor, perf, ci, chore

## 修復提案

正しい形式で提出情報を書き直し。

## 規則優先度

| レベル | 説明 | 動作 |
|------|------|------|
| critical | 重大なセキュリティリスク | 即時阻止 |
| high | 重大な品質問題 | 警告して阻止 |
| medium | 改善提案 | 警告するが阻止しない |
| low | スタイル提案 | のみ提示 |

## プロジェクト規則の作成

### ステップ 1: プロジェクト需要を分析

```markdown
# プロジェクト規則需要分析

## 必須規範
1. セキュリティ：ハードコード凭证禁止
2. テスト：新コード 80% カバレッジ
3. 提出：Conventional Commits に準拠

## 推奨規範
1. 関数は50行以下
2. ファイルは800行以下
3. console.log 禁止
```

### ステップ 2: 規則ファイルを作成

プロジェクトで `.claude/rules/` ディレクトリを作成：

```
project/
└── .claude/
    └── rules/
        ├── security.md
        ├── testing.md
        ├── coding-style.md
        └── git.md
```

### ステップ 3: 規則優先度を構成

`rules.json` 設定を作成して実行順序を構成：

```json
{
  "order": [
    "security/no-hardcoded-credentials",
    "security/no-sql-injection",
    "testing/minimum-test-coverage",
    "coding-style/max-function-length"
  ]
}
```

## 配套教材

完全な Rules ドキュメント：`../../参考ドキュメント/rules/Git工作流.md`

## 次へ

- [演習题目](./exercises/演習.md)を学習
- [Rules 完全ドキュメント](../../参考ドキュメント/rules/Git工作流.md)を阅读