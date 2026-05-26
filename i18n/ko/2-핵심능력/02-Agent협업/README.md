# 02 Agent 협력

> 6류 Agent의 책임을 이해하고, 조합하여 복잡한 개발 임무를 완료

## 모듈 목표

본 모듈을 완료하면, 다음을 할 수 있습니다:

- Claude Code의 6류 Agent 및 각각의 책임을 식별
- 태스크 타입에 따라 적절한 Agent 선택
- 여러 Agent를 조합하여 복잡한 태스크를 완료
- Agent 협력 모드와 베스트 프랙티스를 이해

## Agent 분류 체계

Claude Code의 Agent는 6대류로 분류:

| Agent 타입 | 핵심 책임 | 적용 시나리오 |
|------------|----------|----------|
| **리뷰類** | 코드 품질, 패턴規範, 보안 취약성 | 코드 리뷰, 기술 부채 정리 |
| **빌드類** | 컴파일 빌드, 에러 수리, 의존성 관리 | 빌드 실패, 컴파일 에러 |
| **계획類** | 태스크 분해, 아키텍처 설계, 기술 방안 | 새 기능, 아키텍처 변경 |
| **테스트類** | 테스트 작성, 커버리지 분석, 테스트 전략 | TDD, 테스트 보완 |
| **보안類** | 보안 감사, 취약성 스캔, 규정 준수 검사 | 보안 민감 코드, 결제 모듈 |
| **아키텍처類** | 시스템 설계, 패턴 선택, 기술 선정 | 대규모 리팩토링, 아키텍처 진화 |

## 6류 Agent 상세

### 1. 리뷰類 Agent

**책임**: 코드 품질 리뷰, 패턴規範 검사

사용 가능한 Agent：
- `code-reviewer` - 범용 코드 리뷰
- `python-reviewer` - Python 전문 리뷰
- `go-reviewer` - Go 언어 리뷰
- `rust-reviewer` - Rust 보안 리뷰
- `security-reviewer` - 보안 취약성 리뷰

**사용 시나리오**:
- 코드 제출 전 품질 검사
- 잠재적인 코드 역취 발견
- 서드파티 코드 리뷰

### 2. 빌드類 Agent

**책임**: 컴파일 빌드, 에러 수리, 의존성 관리

사용 가능한 Agent：
- `build-error-resolver` - 빌드 에러 해석
- `go-build` - Go 빌드
- `rust-build` - Rust 빌드
- `kotlin-build` - Kotlin 빌드
- `flutter-build` - Flutter 빌드

**사용 시나리오**:
- 빌드 실패 시 에러 수리
- 의존성 충돌 해결
- 크로스 플랫폼 빌드 문제

### 3. 계획類 Agent

**책임**: 태스크 분해, 아키텍처 설계, 실시 계획

사용 가능한 Agent：
- `planner` - 실시 계획 생성
- `architect` - 아키텍처 설계
- `tdd-guide` - TDD 워크플로 가이드

**사용 시나리오**:
- 새 기능 개발 계획
- 기술 방안 평가
- 복잡한 태스크 분해

### 4. 테스트類 Agent

**책임**: 테스트 작성, 커버리지 향상, 테스트 전략

사용 가능한 Agent：
- `tdd-guide` - 테스트 주도 개발
- `e2e-runner` - 엔드 투 엔드 테스트
- `test-coverage` - 커버리지 분석

**사용 시나리오**:
- 새 기능 테스트 작성
- 테스트 커버리지 향상
- E2E 테스트 시나리오 설계

### 5. 보안類 Agent

**책임**: 보안 감사, 취약성 스캔, 규정 준수 검사

사용 가능한 Agent：
- `security-reviewer` - 보안 취약성 리뷰
- `security-scan` -自動化 보안 스캔

**사용 시나리오**:
- 결제와 인증 코드 리뷰
- 외부 데이터 처리 검사
- 규정 준수 검증

### 6. 아키텍처類 Agent

**책임**: 시스템 설계, 패턴 선택, 기술 선정

사용 가능한 Agent：
- `architect` - 아키텍처 설계
- `refactor-cleaner` - 리팩토링 가이드
- `api-design-reviewer` - API 디자인 리뷰

**사용 시나리오**:
- 마이크로서비스 분할
- 데이터베이스 설계
- API 아키텍처 진화

## Agent 협력 모드

### 모드 1: 직렬 협력

```
태스크 → Planner → 실시 → Reviewer → 테스트 → 완료
```

적용: 선형 플로, 스텝 사이에 의존 관계가 있음

### 모드 2: 병렬 협력

```
            → Reviewer A ─→
태스크 → 분해 ─→ Reviewer B ─→ 통합 → 완료
            → Reviewer C ─→
```

적용: 다각도 리뷰, 독립 모듈 리뷰

### 모드 3: 캐스케이드 협력

```
중대 태스크 → Architect → Planner → 실시 → 각급 Reviewer →合併
```

적용: 복잡한 프로젝트, 대형 기능 개발

## Agent 조합 예

### 시나리오: 새 기능 개발

```
1. /plan을 사용하여 기능 계획
2. tdd-guide를 사용하여 TDD 개발 수행
3. code-reviewer를 사용하여 코드 리뷰
4. security-reviewer를 사용하여 보안 검사
5. verify를 사용하여 기능 검증
```

### 시나리오: 보안 취약성 수리

```
1. security-reviewer를 사용하여 취약성 스캔
2. planner를 사용하여 수리 계획 수립
3. build-fix를 사용하여 수리 실시
4. security-reviewer를 사용하여 재검증
5. verify를 사용하여 수리 확인
```

## Agent 선택 가이드

| 태스크 타입 | 권장 Agent | 이유 |
|----------|------------|------|
| 새 기능 개발 | planner + tdd-guide | 계획과 TDD 필요 |
| 코드 리뷰 | code-reviewer + security-reviewer | 품질과 보안 이중 검사 |
| 빌드 실패 | build-error-resolver | 에러 수리에 집중 |
| 아키텍처 리팩토링 | architect + refactor-cleaner | 아키텍처 레벨 가이드 |
| 보안 민감 | security-reviewer (우선) | 보안 관문 필수 통과 |
| 테스트 작성 | tdd-guide + test-coverage | 테스트 전략과 커버리지 |

##配套 교재

완전한 Agent 문서: `../../참조문서/agents/1. 代码审查类.md`

## 다음으로

- [연습 문제](./exercises/연습.md) 학습
- [Agent 완전한 문서](../../참조문서/agents/1. 代码审查类.md) 阅读