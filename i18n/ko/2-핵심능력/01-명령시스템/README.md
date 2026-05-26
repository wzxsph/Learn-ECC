# 01 명령 시스템 마스터

> Claude Code의 75+ 명령을 마스터하고,灵活组合으로高效 개발을 실현

## 모듈 목표

본 모듈을 완료하면, 다음을 할 수 있습니다:

- 모든 명령 분류 아래 명령을 식별하고 사용할 수 있음
- 시나리오에 따라 가장 적절한 명령을 선택할 수 있음
- 여러 명령을 조합하여 커스텀 워크플로를 생성할 수 있음
- 명령 문서를 사용하여 명령 사용법을 빠르게 검색할 수 있음

## 명령 개요

Claude Code는 75+ 명령을 제공하며,以下の分類을カバー：

| 분류 | 명령수 | 예명령 |
|------|--------|----------|
| 개발 플로 | 15+ | `/plan`, `/tdd`, `/build-fix`, `/verify` |
| 코드 리뷰 | 10+ | `/code-review`, `/security-review`, `/python-review` |
| 빌드 테스트 | 15+ | `/go-build`, `/rust-build`, `/flutter-build` |
| 프로젝트 관리 | 10+ | `/project-init`, `/projects`, `/santa-loop` |
| 스킬 관리 | 10+ | `/skill-create`, `/skill-stocktake`, `/skill-scout` |
| 학습 연구 | 10+ | `/learn`, `/learn-eval`, `/deep-research` |
| 보조 도구 | 15+ | `/config`, `/jira`, `/pr`, `/e2e` |

## 명령 문서 구조

각 명령은 표준 문서 구조를 가집니다:

```
명령 이름
├── 명령 설명
├── 사용シーン
├── 파라미터 설명
├── 사용 예
└── 주의 사항
```

## 자주 사용하는 명령 빠른 참조

### 개발 플로
- `/plan` - 실시 계획 작성
- `/tdd` - 테스트 주도 개발
- `/build-fix` - 빌드 에러 수리
- `/verify` - 코드 변경 검증
- `/review` - 코드 리뷰

### 빌드 관련
- `/go-build`, `/rust-build`, `/kotlin-build` - 각종 언어 빌드
- `/cpp-build`, `/flutter-build` - 컴파일형 언어 빌드
- `/docker-build` - Docker 이미지 빌드

### 프로젝트 관리
- `/project-init` - 새 프로젝트 초기화
- `/projects` - 모든 프로젝트 나열
- `/santa-loop` - 자동화 태스크 루프

### 스킬 개발
- `/skill-create` - git 히스토리에서 Skill 생성
- `/skill-scout` - 사용 가능한 Skills 검색
- `/learn` - 세션에서 패턴 추출

## 명령 문서 사용 방법

配套の 완전한 命令 문서: `../../참조문서/commands/01-핵심워크플로우.md`

문서가 포함하는 것:
- 각 명령의 상세 설명
- 파라미터와 옵션 설명
- 사용 예와 출력
- 베스트 프랙티스 제안

## 명령 조합 사용

명령은 조합하여 사용할 수 있으며, 복잡한 워크플로를 실현:

```
/plan → /tdd → /build-fix → /code-review → /verify
```

각 명령의 출력은 다음 명령에 전달되어 자동화 파이프라인을 형성.

## 다음으로

- [연습 문제](./exercises/연습.md) 학습
- [명령 완전한 문서](../../참조문서/commands/01-핵심워크플로우.md) 阅读