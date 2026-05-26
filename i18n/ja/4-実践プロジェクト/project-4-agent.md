# プロジェクト 4: Agent システム

マルチエージェントコラボレーションプラットフォームを構築します。

## プロジェクト目標

**予想時間**: 2-3 時間

複数の専門 Agent が協力して複雑なタスクを完了するマルチ Agent コラボレーションシステムを作成します。

## 機能要件

### 1. Agent 役割
- **architect** - システム設計
- **developer** - コード実装
- **code-reviewer** - コードレビュー
- **tester** - テストエンジニア
> 参照: [Code Review Agents](../参考ドキュメント/agents/01-Code-Review.md) | [Architecture Agents](../参考ドキュメント/agents/06-Architecture.md)

### 2. コラボレーション機構
- タスク分配
- 結果集約
- 競合解決
- 状態同期

### 3. 管理機能
- Agent ライフサイクル管理
- タスクキュー
- パフォーマンス監視
- リソーススケジューリング

## 技術ソリューション

### メッセージプロトコル

```json
{
  "type": "task",
  "from": "architect",
  "to": "developer",
  "payload": { "task": "implement-auth", "context": {} }
}
```

### タスクステートマシン
- `pending` - 割り当て待ち
- `running` - 実行中
- `completed` - 完了
- `failed` - 失敗
- `blocked` - ブロック

## 完了基準

- [ ] 4+ Agent 役割をサポート
- [ ] 自動タスク分配
- [ ] 正しい結果集約
- [ ] 失敗回復機構
- [ ] リアルタイム状態監視

---

[実践プロジェクトディレクトリに戻る](./README.md)