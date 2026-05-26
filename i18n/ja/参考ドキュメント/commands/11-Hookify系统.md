# Hookify システムコマンド

## 概要

Hookify システム用于作成和管理 hooks，防止不良行为和自動化ワークフロー。

## コマンド列表

### /hookify

**用途**: 作成 hooks 防止不良行为

**描述**: 作成自定义 hooks 来防止不希望发生的行为。

**使用场景**:
- 防止意外削除操作
- 阻止危险コマンド
- 自動フォーマット化
- 品質検査

**Hook タイプ**:
- **PreToolUse**: ツール実行前
- **PostToolUse**: ツール実行后
- **Stop**: セッション終了时

---

### /hookify-list

**用途**: 列出すべて設定的 hookify ルール

**描述**: 表示当前すべて已設定的 hookify ルール。

---

### /hookify-configure

**用途**: 交互式あり効化/無効化 hookify ルール

**描述**: 交互式設定 hookify ルール的あり効化和無効化状态。

---

### /hookify-help

**用途**: Hookify システム帮助

**描述**: 取得 Hookify システム的详细帮助情報。

---

## Hook 設定サンプル

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## 相关コマンド

- `/hookify` - 作成 hook
- `/hookify-list` - 列出ルール
- `/hookify-configure` - 設定ルール
- `/hookify-help` - 取得帮助
