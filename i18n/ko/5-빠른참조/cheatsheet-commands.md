# 명령어 치트 시트

## 슬래시 명령

| 명령 | 기능 | 사용 시나리오 |
|------|------|----------|
| `/tdd` | 테스트 주도 개발 워크플로 | 새 기능 개발 |
| `/plan` | 구현 계획 생성 | 복잡한 작업 계획 |
| `/code-review` | [코드 검토](../참조문서/commands/01-核心工作流.md) | 코드 품질 검사 |
| `/build-fix` | [빌드 수정](../참조문서/commands/04-构建修复.md) | 빌드 실패 시 |
| `/learn` | 세션에서 패턴 추출 | 지식 축적 |
| `/skill-create` | Git에서 Skill 생성 | 경험 재사용 |
| `/test` | [테스트 관련 명령](../참조문서/commands/02-测试相关.md) | 테스트 관련 |

## 스크립트 명령

| 스크립트 | 기능 |
|------|------|
| `node scripts/hooks/run-with-flags.js` | 플래그와 함께 훅 실행 |
| `node tests/run-all.js` | 전체 테스트 실행 |
| `node scripts/lib/utils.js` | 유틸리티 함수 라이브러리 |

## Agent 명령

| Agent | 기능 |
|-------|------|
| `/planner` | 작업 계획 |
| `/code-reviewer` | 코드 검토 |
| `/tdd-guide` | TDD 가이드 |
| `/security-reviewer` | 보안 검토 |
| `/build-error-resolver` | 빌드 수정 |

## 전역 매개변수

| 매개변수 | 설명 |
|------|------|
| `--model` | 모델 지정 |
| `--no-input` | 非交互模式 |
| `--output` | 출력 형식 |

## Hook 환경 변수

| 변수 | 설명 |
|------|------|
| `ECC_HOOK_PROFILE` | 훅 설정 |
| `ECC_DISABLED_HOOKS` | 비활성화된 훅 목록 |

---

[빠른 참조 디렉토리로 돌아가기](./README.md)