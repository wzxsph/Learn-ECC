# コマンド速查

## スラッシュコマンド

| コマンド | 機能 | 使用シーン |
|------|----------|----------|
| `/tdd` | テスト駆動開発ワークフロー | 新機能開発 |
| `/plan` | 実施計画生成 | 複雑なタスク計画 |
| `/code-review` | [コードレビュー](../参考ドキュメント/commands/01-コアワークフロー.md) | コード品質チェック |
| `/build-fix` | [ビルド修正](../参考ドキュメント/commands/04-ビルド修復.md) | ビルド失敗時 |
| `/learn` | セッションからパターンを抽出 | ナレッジ沈殿 |
| `/skill-create` | Git から Skill を生成 | 経験の再利用 |
| `/test` | [テスト関連コマンド](../参考ドキュメント/commands/02-テスト.md) | テスト関連 |

## スクリプトコマンド

| スクリプト | 機能 |
|------|----------|
| `node scripts/hooks/run-with-flags.js` | フラグ付きで hooks を実行 |
| `node tests/run-all.js` | すべてのテストを実行 |
| `node scripts/lib/utils.js` | ユーティリティ関数ライブラリ |

## Agent コマンド

| Agent | 機能 |
|-------|----------|
| `/planner` | タスク計画 |
| `/code-reviewer` | コードレビュー |
| `/tdd-guide` | TDD ガイダンス |
| `/security-reviewer` | セキュリティレビュー |
| `/build-error-resolver` | ビルド修正 |

## グローバルパラメータ

| パラメータ | 説明 |
|------|----------|
| `--model` | モデルを指定 |
| `--no-input` | 非インタラクティブモード |
| `--output` | 出力形式 |

## Hook 環境変数

| 変数 | 説明 |
|------|----------|
| `ECC_HOOK_PROFILE` | Hook 設定 |
| `ECC_DISABLED_HOOKS` | 無効化された hooks リスト |

---

[クイックリファレンスディレクトリに戻る](./README.md)