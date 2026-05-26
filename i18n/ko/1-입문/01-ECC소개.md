# 01 - [ECC](../참조문서/README.md) 소개

## [ECC](../참조문서/README.md)란?

[ECC](../참조문서/README.md)（Everything Claude Code）는 Claude Code용 플러그인 시스템으로, 프로덕션 수준의 [Agent](../참조문서/agents/1.%20코드검토类.md), [Skills](../참조문서/skills/最佳实践.md), hooks, rules, [Commands](../참조문서/commands/01-핵심워크플로우.md), 및 MCP 설정 방안을 제공합니다. Anthropic Hackathon 수상 작품에서 탄생하여 10개월 이상의 일상 사용的打磨를 거쳤습니다.

## [ECC](../참조문서/README.md)는 무엇을 할 수 있나요?

### 1. [Agent](../참조문서/agents/1.%20코드검토类.md) 시스템
[ECC](../참조문서/README.md)는 60+ 전문 [Agent](../참조문서/agents/1.%20코드검토类.md)를 제공하며, 다국어 리뷰, 빌드 수리, 아키텍처 설계 등을 커버:

| [Agent](../참조문서/agents/1.%20코드검토类.md) | 용도 |
|-------|------|
| `planner` | 기능 실시 계획 |
| `code-reviewer` | 코드 품질 및 보안 리뷰 |
| `security-reviewer` | 보안 취약점 분석 |
| `build-error-resolver` | 빌드 에러 수리 |
| `architect` | 시스템 아키텍처 설계 |
| `tdd-guide` | 테스트 주도 개발 가이드 |

### 2. [Skills](../참조문서/skills/最佳实践.md) 워크플로
[Skills](../참조문서/skills/最佳实践.md)는 주요 워크플로 면이며, [ECC](../참조문서/README.md)는 232+의 [Skills](../참조문서/skills/最佳实践.md)를 제공:

- `tdd-workflow` - 테스트 주도 개발
- `e2e-testing` - 엔드 투 엔드 테스트
- `security-review` - 보안 리뷰 체크리스트
- `continuous-learning-v2` - instinct 기반의 지속적인 학습
- `eval-harness` - 검증 루프 평가

### 3. [Commands](../참조문서/commands/01-핵심워크플로우.md) 명령
전통적인 슬래시 명령 호환성을 유지（총 75+ 명령）:

| 명령 | 기능 |
|------|------|
| `/plan` | 실시 계획 생성 |
| `/code-review` | 코드 리뷰 |
| `/build-fix` | 빌드 에러 수리 |
| `/learn` | 세션에서 패턴 추출 |
| `/skill-create` | Git 히스토리에서 Skill 생성 |
| `/sessions` | 세션 히스토리 관리 |

### 4. Hooks 자동화
유연한 훅 시스템（8가지 이벤트 타입）:
- `SessionStart` - 세션 시작 시 컨텍스트 로드
- `SessionEnd` - 세션 종료 시 상태 저장
- `PreToolUse` - 도구 실행 전 검증
- `PostToolUse` - 도구 실행 후 포맷

### 5. Rules 규칙 체계
전면적 개발 규범（common + 언어特定）:
- **common/** - 공통 원칙（보안, 코딩 스타일, 테스트, 성능）
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. MCP 서비스
14+ MCP 서버 설정을 지원: GitHub, Supabase, Vercel, Railway, Context7, Exa, Playwright 등.

## 크로스 플랫폼 지원

[ECC](../참조문서/README.md)는 다양한 AI 프로그래밍 도구를 지원:

| 도구 | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 types |
| Cursor | 48 | 공유 | 공유 | 15 types |
| Codex | 공유 | 명령식 | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 types |
| GitHub Copilot | - | 6 prompts | 명령식 | - |

## 핵심 개념

### [Agent](../참조문서/agents/1.%20코드검토类.md)
재사용 가능한 서브태스크 실행자로, 각 [Agent](../참조문서/agents/1.%20코드검토类.md)에는 명확한 책임이 있다. 여러 [Agent](../참조문서/agents/1.%20코드검토类.md)를 조합하여 복잡한 워크플로를 구축한다.

### [Skill](../참조문서/skills/最佳实践.md)
특정 태스크를 완료하는 워크플로를 정의하며, [ECC](../참조문서/README.md)의 주요 워크플로 면이다.

### Hook
특정 타이밍에 자동으로 트리거되는 스크립트로, 워크플로 자동화를 지원한다.

### Rule
Claude Code가 반드시 준수해야 하는 제약 규칙으로, 출력 품질을 보장한다.


- [설치 설정](./02-설치-설정.md) - [ECC](../참조문서/README.md) 사용 시작
- [입문 디렉토리로 돌아가기](../README.md)