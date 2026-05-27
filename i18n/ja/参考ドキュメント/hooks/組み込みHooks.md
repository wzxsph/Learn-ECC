# 組み込みHooks

## 概要

ECC プラグインは`、`ECC/scripts/hooks/` ディレクトリにある豊富な組み込み Hook 実装を提供します。

## PreToolUse 組み込みフック

### pre:bash:dispatcher

**タイプ**: PreToolUse
**matcher**: Bash
**説明**: Bash プレ検査ディスパッチャー、品質検査、tmux  reminders、git プッシュ reminders と GateGuard 检查を統合。

**機能**:
- 開発サーバーブロッカー - tmux 外での `npm run dev` などのコマンドの実行を阻止
- Tmux リマインダー - 長時間実行コマンド（npm test、cargo build、docker）に tmux を使用することを推奨
- Git プッシュリマインダー - `git push` 前、変更をレビューすることを提醒
- Pre-commit 品質検査 - 品質検査を実行：リントステージファイル、コミット情報フォーマットの検証、console.log/debugger/ secrets の検出

**終了コード**:
- 0: 警告のみ継続
- 2: 実行を阻止

---

### pre:write:doc-file-warning

**タイプ**: PreToolUse
**matcher**: Write
**説明**: 非標準ドキュメントファイルの作成を警告

**機能**:
- 許可されたファイル: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- 許可されたディレクトリ: docs/, skills/
- その他 .md/.txt ファイルに対して警告
- クロスプラットフォームパス処理

**終了コード**: 0（警告のみ）

---

### pre:edit-write:suggest-compact

**タイプ**: PreToolUse
**matcher**: Edit|Write
**説明**: 論理的間隔（約50回のツール呼び出し每）に手動 `/compact` を推奨

**機能**:
- ツール呼び出し回数をトレース
- コスト閾値をチェック
- ユーザーにコンパクト化を提案

**終了コード**: 0（警告のみ）

---

### pre:edit-write:permission-prompt

**タイプ**: PreToolUse
**matcher**: Edit|Write
**説明**: Allow/disallow ルールに基づく編集/書き込みツールのアクセス制御

**機能**:
- パターンマッチングで allowlist をチェック
- allowlist にない場合、アクセスを拒否
- 信頼された操作の自動許可

**終了コード**:
- 0: 許可
- 1: 拒否（権限なし）

---

## PostToolUse 組み込みフック

### post:edit-write:code-expert

**タイプ**: PostToolUse
**matcher**: Edit|Write
**説明**: コード変更後の品質 gates を実行

**機能**:
- 変更後の quality gate をトリガー
- 安全でないパターンを検出
- コードの品質指標をチェック

**終了コード**: 0（常に続行、警告を発信）

---

## Stop 組み込みフック

### stop:session-summary

**タイプ**: Stop
**matcher**: ALWAYS
**説明**: セッション終了時にサマリーを生成

**機能**:
- 完了した作業の要約
- 検出された問題のリスト
- 次のステップの提案

---

## 設定

組み込みフックは `~/.claude/settings.json` の `hooks` セクションで設定できます：

```json
{
  "hooks": {
    "pre:bash:dispatcher": {
      "enabled": true,
      "blockDevServer": true,
      "tmuxReminder": true
    },
    "pre:write:doc-file-warning": {
      "enabled": true
    },
    "pre:edit-write:suggest-compact": {
      "enabled": true,
      "threshold": 50
    }
  }
}
```

## カスタムフックへの拡張

既存のフックを拡張するには：

```typescript
// .claude/hooks/custom-dispatcher.ts
export async function dispatcher(args: HookArgs) {
  // カスタムロジック
  console.log('Custom pre-hook executed');

  // 組み込みフックの委讓
  const { builtinDispatcher } = await import('./builtin-pre-bash-dispatcher');
  return builtinDispatcher(args);
}
```

---

[戻る Hook インデックス](../README.md)