# 02 - Agent 협력

다중 Agent 협력 시스템의 설계, 구현, 관리 방법을 학습합니다.

## 개요

ECC는 **66개의 전문 [Agent](../../참조문서/agents/1.%20代码审查类.md)**를 제공하며, 6대 카테고리로 분류됩니다. 이러한 Agent들은 표준화된 워크플로를 통해 협동动作하며, 코드 리뷰부터 시스템 설계까지 각종 복잡한 태스크를 완료합니다.

---

## 1. Agent 아키텍처

### 1.1 단일 Agent vs 다중 Agent

| 아키텍처 | 적용 시나리오 | 특징 |
|------|----------|------|
| **단일 Agent** | 단순한 태스크, 독립 작업 | 직접 호출, 빠른 응답 |
| **다중 Agent** | 복잡한 태스크, 협조 필요 | 태스크 분해, 결과 집계 |

### 1.2 사용 가능한 Agent 목록

| Agent | 용도 | 사용 타이밍 |
|-------|------|----------|
| **planner** | 실시 계획 | 복잡한 기능, 리팩토링 |
| **architect** | 시스템 설계 | 아키텍처 결정 |
| **tdd-guide** | 테스트 주도 개발 | 새 기능, 버그 수리 |
| **code-reviewer** | 코드 리뷰 | 코드 작성 후 |
| **security-reviewer** | 보안 분석 | 제출 전 |
| **build-error-resolver** | 빌드 에러 수리 | 빌드 실패 시 |
| **e2e-runner** | 엔드 투 엔드 테스트 | 중요한 사용자 플로 |
| **refactor-cleaner** | 죽은 코드 정리 | 코드 유지보수 |
| **doc-updater** | 문서 업데이트 | 문서 업데이트 시 |
| **rust-reviewer** | Rust 코드 리뷰 | Rust 프로젝트 |
| **harmonyos-app-resolver** | HarmonyOS 앱 개발 | HarmonyOS/ArkTS 프로젝트 |

### 1.3 Agent 파일 형식

```yaml
---
name: agent-name
description: Agent 용도
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

---

## 2. Agent 카테고리 상세

### 2.1 코드 리뷰類（16개）

코드 품질, 보안, 유지보수성 리뷰를 담당.

| Agent | 전담 |
|-------|------|
| code-reviewer | 범용 코드 리뷰 |
| security-reviewer | 보안 취약성 분석 |
| python-reviewer | Python PEP 8, 타입 힌트 |
| go-reviewer | Go 관용구, 병행 처리 |
| kotlin-reviewer | Kotlin null 안전, 코루틴 |
| rust-reviewer | Rust 소유권, 라이프사이클 |
| cpp-reviewer | C++ 메모리 안전 |
| flutter-reviewer | Flutter/Dart 모드 |
| fastapi-reviewer | FastAPI 아키텍처, 비동기 |
| typescript-reviewer | TypeScript 타입 안전 |
| java-reviewer | Java 코딩 표준 |
| csharp-reviewer | C# 베스트 프랙티스 |
| ruby-reviewer | Ruby 관용구 |
| php-reviewer | PHP 보안과 성능 |
| swift-reviewer | Swift 병행 처리, 메모리 관리 |
| go-security-reviewer | Go 보안 리뷰 |

### 2.2 빌드 수리類（10개）

각 언어의 빌드 에러 수리에 전담.

| Agent | 전담 |
|-------|------|
| build-error-resolver | 범용 빌드 에러 수리 |
| go-build | Go 빌드 + go vet |
| kotlin-build | Kotlin/Gradle 컴파일 |
| rust-build | Rust 빌rowing 체크러 |
| cpp-build | C++ CMake + 링커 |
| gradle-build | Android/KMP Gradle |
| flutter-build | Flutter 빌드 |
| maven-build | Maven 빌드 |
| cmake-build | CMake 설정 |
| ninja-build | Ninja 빌드 시스템 |

### 2.3 계획類（5개）

요구 분석과 시스템 계획을 담당.

| Agent | 전담 |
|-------|------|
| planner | 실시 계획 |
| architect | 시스템 아키텍처 설계 |
| prp-planner | PRP 워크플로 계획 |
| multi-plan | 멀티모델 협조 계획 |
| product-manager | 제품 요구 분석 |

### 2.4 테스트類（2개）

테스트 주도 개발과 커버리지에 전담.

| Agent | 전담 |
|-------|------|
| tdd-guide | TDD 워크플로 가이드 |
| e2e-runner | Playwright E2E 테스트 |

### 2.5 보안類（5개）

보안 리뷰와 취약성 스캔을 담당.

| Agent | 전담 |
|-------|------|
| security-reviewer | 범용 보안 리뷰 |
| owasp-reviewer | OWASP Top 10 |
| pentest-reviewer | 침투 테스트 |
| vulnerability-scanner | 취약성 스캔 |
| secrets-detector | 키와 자격 증명 탐지 |

### 2.6 아키텍처類（5개）

시스템 아키텍처와 기술 결정을 담당.

| Agent | 전담 |
|-------|------|
| architect | 시스템 아키텍처 설계 |
| microservices-architect | 마이크로서비스 아키텍처 |
| frontend-architect | 프론트엔드 아키텍처 |
| backend-architect | 백엔드 아키텍처 |
| devops-architect | DevOps 아키텍처 |

---

## 3. 협력 모드

### 3.1 마스터 슬레이브 모드

하나의 마스터 Agent가 여러 슬레이브 Agent를 협조:

```
사용자 요청 → 마스터 Agent（planner）
           ├── 슬레이브 Agent 1（code-reviewer）
           ├── 슬레이브 Agent 2（tdd-guide）
           └── 슬레이브 Agent 3（build-error-resolver）
```

### 3.2 피어 투 피어 모드

여러 Agent가 병행으로 작업하고 결과가 집계:

```
사용자 요청 → Agent 1（보안 분석）
        → Agent 2（성능 분석）
        → Agent 3（코드 품질 분석）
           ↓ 결과 집계
        최종 보고서
```

### 3.3 파이프라인 모드

Agent가 순서대로 처리, 각 Agent의 출력이 다음의 입력:

```
입력 → Agent 1 → Agent 2 → Agent 3 → 출력
```

---

## 4. 태스크 배포 전략

### 4.1 태스크 분해

복잡한 태스크를 독립적인 서브태스크로 분해:

```markdown
# 복잡한 태스크 분해 예
원래 태스크: 사용자 인증 시스템 구현

분해 후:
1. 데이터베이스 schema 설계 → architect
2. API 엔드포인트 구현 → backend-developer
3. 테스트 작성 → tdd-guide
4. 보안 리뷰 → security-reviewer
```

### 4.2 병행 실행

독립적인 태스크를 병행으로 실행하여 효율 향상:

```markdown
# 좋음: 병행 실행
3개의 Agent를 병행起動:
1. Agent 1: 인증 모듈 보안 분석
2. Agent 2: 캐시 시스템 성능 리뷰
3. Agent 3: 유틸리티 타입 체크

# 나쁨: 불필요한 순차 실행
먼저 agent 1, 그 다음 agent 2, 그 다음 agent 3
```

### 4.3 로드 밸런싱

Agent의 로드에 따라 태스크를分配:

| 요소 | 설명 |
|------|------|
| 태스크 복잡도 | 단순한 태스크 → 가벼운 Agent |
| 리소스 요구 | 무거운 계산 → 전용 Agent |
| 가용성 | 과적 Agent 회피 |

---

## 5. 멀티パ스펙티브 분석

복잡한 문제에는 역할 분담 sub-agents 사용:

| 역할 | 책임 |
|------|------|
| 사실審査員 | 정보와 데이터의 정확성 검증 |
| 상급 엔지니어 | 기술적 실현 가능성과 베스트 프랙티스 평가 |
| 보안 전문가 | 보안 위험과 취약성 식별 |
| 일관성審査員 | 해결책의 내부 일관성 확보 |
| 중복 검사원 | 중복과 낭비 식별 |

---

## 6. Agent 통신

### 6.1 직접 호출

```markdown
planner agent를 사용하여 실시 계획 생성
```

### 6.2 태스크 위임

```markdown
코드 리뷰 태스크를 code-reviewer agent에 위임
```

### 6.3 결과 집계

여러 Agent의 결과를 집계해야 함:

```markdown
다음에서 집계:
- security-reviewer: 보안 문제 목록
- code-reviewer: 코드 품질 문제 목록
- tdd-guide: 테스트 커버리지 보고서

→ 종합 보고서 생성
```

---

## 7. 즉시 사용 원칙

사용자의 프롬프트 없이 자동으로 호출:

| 시나리오 | Agent |
|------|-------|
| 복잡한 기능 요청 | **planner** |
| 코드 작성/수정 직후 | **code-reviewer** |
| 버그 수리 또는 새 기능 | **tdd-guide** |
| 아키텍처 결정 | **architect** |
| 빌드 실패 | **build-error-resolver** |
| 보안 관련 | **security-reviewer** |

---

## 연습

[연습](./exercises/연습.md) 의 태스크를 완료합니다.

---

[핵심 능력 디렉토리로 돌아가기](../README.md)