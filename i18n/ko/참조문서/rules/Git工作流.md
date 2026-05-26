# Git 워크플로우

## Commit 消息형식

```
<type>: <description>

<optional body>
```

**유형 (type)**:
- `feat`: 新기능
- `fix`: 수정 bug
- `refactor`: 리팩토링
- `docs`: 문서
- `test`: 테스트
- `chore`: 杂项
- `perf`: 성능 최적화
- `ci`: CI/CD

> 注意：통과 `~/.claude/settings.json` 全局비활성화归属（attribution）

## Pull Request 워크플로우

생성 PR 时：
1. 분석完整커밋历史（不仅是最新커밋）
2. 사용 `git diff [base-branch]...HEAD` 查看모든变更
3. 起草全面的 PR 总结
4. 패키지含테스트계획的 TODOs
5. 如果是新브랜치，사용 `-u` 标志푸시


## 브랜치命名

사용표준的 git flow 브랜치命名：
- `feature/`: 新기능
- `fix/`: 수정
- `refactor/`: 리팩토링
- `docs/`: 문서

## 개발流程

完整개발流程：
1. **研究 & 重用** - GitHub 搜索、库문서、패키지등록表
2. **계획** - 사용 planner agent 생성实施계획
3. **TDD 方式** - 先写테스트，再구현
4. **코드 리뷰** - 사용 code-reviewer agent
5. **커밋 & 푸시** - 详细커밋消息，遵循 conventional commits
