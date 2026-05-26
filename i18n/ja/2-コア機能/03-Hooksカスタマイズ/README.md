# 03 Hooks カスタマイズ

> PreToolUse、PostToolUse、Stop Hooksの作成方法を学習し、自动化品質ゲートを実現

## モジュール目標

本モジュールを完了すると、以下ができる：

- Claude CodeのHookタイプとトリガーメカニズムを理解
- PreToolUse Hookを作成してツール呼び出し前検証を実装
- PostToolUse Hookを作成してツール呼び出し後処理を実装
- 品質ゲートHookを構成して自動検査を実装

## Hook タイプ概要

| Hook タイプ | トリガータイミング | 典型的な用途 |
|-----------|----------|----------|
| **PreToolUse** | ツール呼び出し前 | パラメータ検証、権限チェック、セキュリティスキャン |
| **PostToolUse** | ツール呼び出し後 | 結果フォーマット、ログ記録、自動化処理 |
| **Stop** | セッション終了時 | セッションサマリー、状態保存、清理作業 |

## PreToolUse Hook

### 動作原理

```
ユーザー入力 → PreToolUse Hook → ツール実行 → 結果返回
```

### 一般的なシーン

1. **セキュリティ検証**
   - 危険なコマンド（例：`rm -rf`）をチェック
   - ファイルパスの安全性を検証
   - 機密データアクセスをチェック

2. **パラメータ検証**
   - ツールパラメータ形式を検証
   - 必須パラメータ是否存在をチェック
   - タイプチェック

3. **権限制御**
   - 操作の実行が許可されているかチェック
   - プロジェクトアクセス権限を検証

### Hook 設定の例

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('rm -rf')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "警告: 危険なコマンド rm -rf が検出されました。実行前に確認してください！"
    }
  ]
}
```

## PostToolUse Hook

### 動作原理

```
ツール実行 → PostToolUse Hook → 結果返回 → ユーザー
```

### 一般的なシーン

1. **結果フォーマット**
   - 出力を整形
   - 主要情報を抽出
   - 長い出力を簡略化

2. **自動化処理**
   - ビルド成功后自動的にテストを実行
   - テスト失敗後自動的にログを記録
   - 提出前に自動的にコードをフォーマット

3. **品質チェック**
   - 返されたエラーメッセージをチェック
   - 出力セキュリティを検証
   - 性能データを分析

### Hook 設定の例

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm test')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'テスト完了: カバレッジをチェック...'",
      "executeAfter": true
    }
  ]
}
```

## Stop Hook

### 動作原理

```
セッション終了信号 → Stop Hook → 清理を実行 → セッション終了
```

### 一般的なシーン

1. **セッションサマリー**
   - セッションの要約を生成
   - 完了したタスクを記録
   - 保留中の事項を一覧表示

2. **状態保存**
   - 下書きファイルを保存
   - 進捗を永続化
   - 一時ファイルを清理

3. **通知送信**
   - 完了通知を送信
   - 汇总報告
   - 次のステップのプロセスをトリガー

## Hook 設定構造

```json
{
  "matcher": {
    "tool": "ツール名称",
    "operation": "操作タイプ",
    "condition": "条件式"
  },
  "hooks": [
    {
      "type": "notification | command | stop",
      "when": "before | after",
      "message": "通知メッセージ",
      "command": "実行するコマンド",
      "continue": true
    }
  ]
}
```

## 常用 Hook シーン

### シーン 1: Git 提出前チェック

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('git commit')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "npm test",
      "continue": false
    }
  ]
}
```

### シーン 2: ビルド成功通知

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm run build')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "ビルド完了！テストを実行中...",
      "executeAfter": true
    }
  ]
}
```

### シーン 3: セッション終了サマリー

```json
{
  "matcher": {
    "type": "stop"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "セッション終了。完了: {summary}",
      "command": "node scripts/save-session.js"
    }
  ]
}
```

## Hook 開発のベストプラクティス

1. **シンプルに保つ**: Hookロジックは简单直接であるべき
2. **快速実行**: PreToolUse Hookは200ms以内に完了すべき
3. **優雅な低下**: 失敗時は継続実行し、ツールを阻塞しない
4. **明確なログ**: `[HookName]` 接頭辞を使用してログをマーク
5. **テストカバレッジ**: Hookにテストを作成

## 配套教材

完全な Hooks ドキュメント：`../../参考ドキュメント/hooks/Hook类型.md`

## 次へ

- [演習题目](./exercises/演習.md)を学習
- [Hooks 完全ドキュメント](../../参考ドキュメント/hooks/Hook类型.md)を阅读