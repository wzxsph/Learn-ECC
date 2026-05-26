# Git ワークフロー

## Commit 消息フォーマット

```
<type>: <description>

<optional body>
```

**タイプ (type)**:
- `feat`: 新機能
- `fix`: 修正 bug
- `refactor`: リファクタリング
- `docs`: ドキュメント
- `test`: テスト
- `chore`: 杂项
- `perf`: パフォーマンス最適化
- `ci`: CI/CD

> 注意：通過 `~/.claude/settings.json` 全局無効化归属（attribution）

## Pull Request ワークフロー

作成 PR 时：
1. 分析完整コミット历史（不仅は最新コミット）
2. 使用 `git diff [base-branch]...HEAD` 查看すべて变更
3. 起草全面的 PR 总结
4. パッケージ含テスト計画的 TODOs
5. 如果は新ブランチ，使用 `-u` 标志プッシュ


## ブランチ命名

使用標準的 git flow ブランチ命名：
- `feature/`: 新機能
- `fix/`: 修正
- `refactor/`: リファクタリング
- `docs/`: ドキュメント

## 開発流程

完整開発流程：
1. **研究 & 重用** - GitHub 搜索、库ドキュメント、パッケージ登録表
2. **計画** - 使用 planner agent 作成实施計画
3. **TDD 方式** - 先写テスト，再実装
4. **コードレビュー** - 使用 code-reviewer agent
5. **コミット & プッシュ** - 详细コミット消息，遵循 conventional commits
