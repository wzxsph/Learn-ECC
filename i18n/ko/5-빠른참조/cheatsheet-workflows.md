# 워크플로우 치트 시트

## 일반적인 워크플로우 템플릿

### 1. TDD 개발 프로세스

```
요구사항 → 테스트 작성(RED) → 구현(GREEN) → 리팩토링(REFACTOR)
```

**사용**: `/tdd`

---

### 2. 코드 검토 프로세스

```
코드 제출 → 검토 시작 → 문제 분류 → 수정 → 재검토 → 병합
```

**사용**: `/code-review`

---

### 3. 오류 수정 프로세스

```
오류 발견 → 원인 분석 → 코드 위치 파악 → 수정 → 검증 → 테스트
```

**사용**: `/build-fix`

---

### 4. Skill 생성 프로세스

```
요구사항 분석 → 기존 패턴 참고 → 템플릿 작성 → 테스트 검증
```

**사용**: `/skill-create`

---

## 사용자 정의 워크플로우

워크플로우 정의 형식：

```yaml
name: my-workflow
description: 내 워크플로우
steps:
  - name: step1
    command: do-something
  - name: step2
    command: do-other
    requires: [step1]
```

---

[빠른 참조 디렉토리로 돌아가기](./README.md)