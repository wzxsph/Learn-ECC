# 프로젝트 3: 자동화 워크플로

엔드투엔드 자동화 워크플로 시스템을 구축합니다.

## 프로젝트 목표

**예상 시간**: 2-3시간

일반적인 개발 작업을 처리하는 자동화 워크플로 엔진을 설계하고 구현합니다.

## 기능 요구사항

### 1. 워크플로 정의
- YAML 형식 정의
- 단계 직렬화 및 병렬화
- 조건 분기
- 반복 처리

### 2. 내장 워크플로
- **코드 검사** - lint + type-check + test
- **빌드 배포** - build + test + deploy
- **문제 수정** - analyze + fix + verify

### 3. 모니터링 및 경고
- 실행 상태 추적
- 실패 경고 알림
- 소요 시간 통계

## 기술方案

### 워크플로 정의 예시

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

### Hook 통합
- PreToolUse - 매개변수 검증
- PostToolUse - 출력 검증
> 참고: [Hook 유형](../참조문서/hooks/Hook类型.md) | [자동화 및 스크립트](../참조문서/skills/自动化与脚本.md)

## 수락 기준

- [ ] YAML 정의 지원
- [ ] 단계 종속성 해석 올바름
- [ ] 병렬 실행 최적화
- [ ] 실패 재시도 메커니즘
- [ ] 완전한 실행 로그

---

[실전 프로젝트 디렉토리로 돌아가기](./README.md)