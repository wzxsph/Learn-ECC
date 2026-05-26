# プロジェクト 3: 自動化ワークフロー

エンドツーエンドの自動化ワークフローシステムを構築します。

## プロジェクト目標

**予想時間**: 2-3 時間

常用開発タスクを処理する自動化ワークフローエンジンを設計・実装します。

## 機能要件

### 1. ワークフロー定義
- YAML 形式定義
- ステップ直列・並列実行
- 条件分岐
- ループ処理

### 2. 内蔵ワークフロー
- **コードチェック** - lint + type-check + test
- **ビルド＆リリース** - build + test + deploy
- **イシュー修正** - 分析 + 修正 + 検証

### 3. 監視とアラート
- 実行状態追跡
- 失敗アラート通知
- 消費時間統計

## 技術ソリューション

### ワークフロー定義例

```yaml
name: code-check
steps:
  - name: lint
    command: npm run lint
  - name: type-check
    command: npm run type-check
  - name: test
    command: npm test
    requires: [lint, type-check]
```

### Hook 統合
- PreToolUse - パラメータ検証
- PostToolUse - 出力検証
> 参照: [Hook Types](../参考ドキュメント/hooks/Hook类型.md) | [Automation & Scripts](../参考ドキュメント/skills/自动化とスクリプト.md)

## 完了基準

- [ ] YAML 定義をサポート
- [ ] ステップ依存解決が正しい
- [ ] 並列実行最適化
- [ ] 失敗リトライ機構
- [ ] 完全な実行ログ

---

[実践プロジェクトディレクトリに戻る](./README.md)