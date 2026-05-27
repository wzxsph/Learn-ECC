# Git 워크플로우

## Commit 메시지 형식

```
<type>: <description>

<optional body>
```

**유형 (type)**:
- `feat`: 새 기능
- `fix`: 버그 수정
- `refactor`: 리팩토링
- `docs`: 문서
- `test`: 테스트
- `chore`: 잡다한 것
- `perf`: 성능 최적화
- `ci`: CI/CD

> 참고: `~/.claude/settings.json` 전역 비활성화됨 (attribution)

## Pull Request 워크플로우

PR 생성 시:
1. 전체 커밋 히스토리 분석 (가장 최근 커밋만 아닌)
2. `git diff [base-branch]...HEAD` 모든 변경 확인
3. 포괄적인 PR 요약 작성
4. TODO가 포함된 테스트 계획 포함
5. 새 브랜치인 경우 `-u` 플래그로 푸시

## 브랜치 명명

표준 git flow 브랜치 명명 사용:
- `feature/`: 새 기능
- `fix/`: 수정
- `refactor/`: 리팩토링
- `docs/`: 문서

## 개발 흐름

전체 개발 흐름:
1. **연구 & 재사용** - GitHub 검색, 라이브러리 문서, 패키지 레지스트리
2. **계획** - planner agent 사용해서 구현 계획 생성
3. **TDD 방식** - 먼저 테스트 작성, 그 다음 구현
4. **코드 리뷰** - code-reviewer agent 사용
5. **커밋 & 푸시** - 상세 커밋 메시지, conventional commits 따르기