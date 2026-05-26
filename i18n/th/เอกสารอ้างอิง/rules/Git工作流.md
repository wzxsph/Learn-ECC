# Git 工作流

## Commit 消息格式

```
<type>: <description>

<optional body>
```

**类型 (type)**:
- `feat`: 新功能
- `fix`: 修复 bug
- `refactor`: 重构
- `docs`: 文档
- `test`: 测试
- `chore`: 杂项
- `perf`: 性能优化
- `ci`: CI/CD

> 注意：通过 `~/.claude/settings.json` 全局禁用归属（attribution）

## Pull Request 工作流

创建 PR 时：
1. 分析完整提交历史（不仅是最新提交）
2. 使用 `git diff [base-branch]...HEAD` 查看所有变更
3. 起草全面的 PR 总结
4. 包含测试计划的 TODOs
5. 如果是新分支，使用 `-u` 标志推送


## 分支命名

使用标准的 git flow 分支命名：
- `feature/`: 新功能
- `fix/`: 修复
- `refactor/`: 重构
- `docs/`: 文档

## 开发流程

完整开发流程：
1. **研究 & 重用** - GitHub 搜索、库文档、包注册表
2. **计划** - 使用 planner agent 创建实施计划
3. **TDD 方式** - 先写测试，再实现
4. **代码审查** - 使用 code-reviewer agent
5. **提交 & 推送** - 详细提交消息，遵循 conventional commits
