# 내장 Hooks

## 개요

ECC 플러그인은 `ECC/scripts/hooks/` 디렉토리에 풍부한 내장 Hook 구현을 제공합니다.

## PreToolUse 내장 Hook

### pre:bash:dispatcher

**유형**: PreToolUse
**matcher**: Bash
**설명**: Bash 사전 검사 디스패처로, 품질 검사, tmux 알림, git 푸시 알림 및 GateGuard 검사를 통합합니다.

**기능**:
- 개발서버 차단기 - tmux 외부에서 `npm run dev` 등의 명령 실행 차단
- Tmux 알림 - 장시간 실행 명령(npm test, cargo build, docker)에 tmux 사용 권장
- Git 푸시 알림 - `git push` 이전에 변경 사항 검토 권장
- Pre-commit 품질 검사 - 품질 검사 실행: 스테이징 파일 lint, 커밋 정보 형식 검증, console.log/debugger/밀열쇠 감지

**종료 코드**:
- 0: 경고하지만 계속
- 2: 실행 차단

---

### pre:write:doc-file-warning

**유형**: PreToolUse
**matcher**: Write
**설명**: 비표준 문서 파일 생성 경고

**기능**:
- 허용 표준 문서 파일: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- 허용 디렉토리: docs/, skills/
- 기타 .md/.txt 파일 경고
- 크로스 플랫폼 경로 처리

**종료 코드**: 0 (경고만)

---

### pre:edit-write:suggest-compact

**유형**: PreToolUse
**matcher**: Edit|Write
**설명**: 논리적 간격(약 50회 도구 호출마다) 수동 `/compact` 권장

**기능**:
- 도구 호출 횟수 추적
- 적절한 간격에서컨텍스트 압축 권장
- 논블로킹, 권장만 제공

**종료 코드**: 0 (권장)

---

### pre:observe:continuous-learning

**유형**: PreToolUse
**matcher**: *
**설명**: 지속 학습 신호를 지원하기 위해 도구 의도를 기록

**기능**:
- 도구 사용 관찰 캡처
- 패턴 추출 및 분석용
- 비동기 실행, 논블로킹

**종료 코드**: 0

---

### pre:governance-capture

**유형**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**설명**:治理 이벤트 캡처 (밀열쇠, 전략 위반, 승인 요청)

**기능**:
-秘밀/밀열쇠 유출 감지
- 전략 위반 캡처
- 승인 요청 기록

**활성화 조건**: `ECC_GOVERNANCE_CAPTURE=1`

**종료 코드**: 0

---

### pre:config-protection

**유형**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**설명**: linter/formatter 설정 파일 수정 차단

**기능**:
- .eslintrc, .prettierrc 등 수정 차단
- 설정 약화보다 코드 수정 안내

**종료 코드**: 0

---

### pre:mcp-health-check

**유형**: PreToolUse
**matcher**: *
**설명**: MCP 도구 실행 전 MCP 서버 건강 상태 검사

**기능**:
- MCP 서버 상태 검사
- 비정상 MCP 서버의 호출 차단

**종료 코드**: 0

---

### pre:edit-write:gateguard-fact-force

**유형**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**설명**: 사실 강제 GateGuard: 각 파일의 첫 번째 편집/쓰기를 차단하고, 허용 전 조사 요구

**기능**:
- 첫 번째 편집 차단
- 확인 요구: 가져오기, 데이터 패턴, 사용자 명령
- 충분한 이유 없이 수정 금지

**종료 코드**: 0

---

## PostToolUse 내장 Hook

### post:bash:dispatcher

**유형**: PostToolUse
**matcher**: Bash
**설명**: Bash 사후 검사 디스패처로, 로그 기록, PR 및 빌드 알림용

**기능**:
- PR 로그 - `gh pr create` 후 PR URL 및 리뷰 명령 기록
- 빌드 분석 - 빌드 명령 후 백그라운드 분석 (비동기, 논블로킹)

**종료 코드**: 0

---

### post:quality-gate

**유형**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**설명**: 파일 편집 후 빠른 품질 검사 실행

**기능**:
- 코드 품질 검사
- 비동기 실행
- 편집 작업 논블로킹

**종료 코드**: 0

---

### post:edit:design-quality-check

**유형**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**설명**: 일반 템플릿 외관으로의 프론트엔드 편집 이탈 경고

**기능**:
- UI 설계 품질 감지
- 너무 일반적인 템플릿 외관 방지

**종료 코드**: 0

---

### post:edit:accumulator

**유형**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**설명**: Stop 시 배치 형식화 및 유형 검사를 위해 편집된 JS/TS 파일 경로 기록

**기능**:
- 편집된 파일 목록 누적
- stop:format-typecheck용

**종료 코드**: 0

---

### post:edit:console-warn

**유형**: PostToolUse
**matcher**: Edit
**설명**: 편집 후 console.log 문 경고

**기능**:
- 코드 내 console.log 감지
- 개발자に対しデバッグコード 정리 경고

**종료 코드**: 0

---

### post:governance-capture

**유형**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**설명**: 도구 출력에서治理 이벤트 캡처

**기능**:
- 도구 출력 분석
- 잠재적 문제 감지

**활성화 조건**: `ECC_GOVERNANCE_CAPTURE=1`

**종료 코드**: 0

---

### post:session-activity-tracker

**유형**: PostToolUse
**matcher**: *
**설명**: 각 세션의 도구 호출 및 파일 활동 기록

**기능**:
- 도구 사용 통계 추적
- 파일 활동 기록
- ECC2 지표용

**종료 코드**: 0

---

### post:observe:continuous-learning

**유형**: PostToolUse
**matcher**: *
**설명**: 지속 학습 신호를 지원하기 위해 도구 결과 기록

**기능**:
- 도구 실행 결과 캡처
- 패턴 분석 지원

**종료 코드**: 0

---

### post:ecc-metrics-bridge

**유형**: PostToolUse
**matcher**: *
**설명**: 실행 중 세션 지표 집계 유지

**기능**:
- 토큰 및 비용 지표 추적
- 상태 표시줄 및 컨텍스트 모니터용

**종료 코드**: 0

---

### post:ecc-context-monitor

**유형**: PostToolUse
**matcher**: *
**설명**: 컨텍스트 고갈, 높은 비용, 범위 크리프 또는 도구 루프 시 프록시 경고 주입

**기능**:
- 컨텍스트 사용 모니터링
- 비용 경고
- 범위 크리프 감지
- 도구 루프 감지

**종료 코드**: 0

---

### post:mcp-health-check

**유형**: PostToolUseFailure
**matcher**: *
**설명**: 실패한 MCP 도구 호출 추적, 비정상 서버 표시 및 재연결 시도

**기능**:
- 실패 추적
- 건강 상태 표시
- 자동 재연결 시도

**종료 코드**: 0

---

## Stop 내장 Hook

### stop:format-typecheck

**유형**: Stop
**matcher**: *
**설명**: 배치 형식화(Biome/Prettier) 및 유형 검사(tsc) - 이번 응답에서 편집된 모든 JS/TS 파일

**기능**:
- Prettier/Biome 형식화
- TypeScript 유형 검사
- Stop 시 한 번만 실행 (편집마다 아님)

**종료 코드**: 0

**타임아웃**: 300초

---

### stop:check-console-log

**유형**: Stop
**matcher**: *
**설명**: 모든 응답 후 수정된 파일 내 console.log 검사

**기능**:
- 수정된 파일 스캔
- console.log 사용 보고

**종료 코드**: 0

---

### stop:session-end

**유형**: Stop
**matcher**: *
**설명**: 모든 응답 후 세션 상태 영속화 (Stop은 transcript_path 운반)

**기능**:
- 세션 상태 저장
- 컨텍스트 영속화

**종료 코드**: 0

---

### stop:evaluate-session

**유형**: Stop
**matcher**: *
**설명**: 학습 가능한 패턴을 추출하기 위해 세션 평가

**기능**:
- 패턴 인식
- 지속 학습 지원
- 비동기 실행

**종료 코드**: 0

---

### stop:cost-tracker

**유형**: Stop
**matcher**: *
**설명**: 각 세션의 토큰 및 비용 지표 추적

**기능**:
- 토큰 사용 통계
- 비용 추정
- 경량 실행 비용 텔레메트리 표시

**종료 코드**: 0

---

### stop:desktop-notify

**유형**: Stop
**matcher**: *
**설명**: Claude 응답 시 macOS/WSL 데스크톱 알림 전송, 작업 요약 포함

**기능**:
- 데스크톱 알림
- 작업 요약 표시

**종료 코드**: 0

---

## SessionStart 내장 Hook

### session:start

**유형**: SessionStart
**matcher**: *
**설명**: 이전 컨텍스트 불러오기 및 새 세션의 패키지 관리자 감지

**기능**:
- 제한 있는 이전 컨텍스트 불러오기
- 프로젝트 상태 감지
- 패키지 관리자 감지 (npm/pnpm/yarn/bun)

**환경변수**:
- `ECC_SESSION_START_MAX_CHARS`: 추가 컨텍스트 크기 제어 (기본 8000자)
- `ECC_SESSION_START_CONTEXT=off`: 추가 컨텍스트 완전 비활성화

**종료 코드**: 0

---

## PreCompact 내장 Hook

### pre:compact

**유형**: PreCompact
**matcher**: *
**설명**: 컨텍스트 압축 전 상태 저장

**기능**:
- 세션 상태 영속화
- 압축용 컨텍스트 준비

**종료 코드**: 0

---

## SessionEnd 내장 Hook

### session:end:marker

**유형**: SessionEnd
**matcher**: *
**설명**: 세션 종료 라이프사이클 표시

**기능**:
- 라이프사이클 이벤트 표시
- 로그 정리

**종료 코드**: 0

---

## 내장 Hook 참고表

### PreToolUse Hook

| ID | Matcher | 기능 | 블록 |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | 품질/tmux/푸시/GateGuard 검사 | 블록 가능 |
| pre:write:doc-file-warning | Write | 문서 파일 경고 | 아니오 |
| pre:edit-write:suggest-compact | Edit|Write | 압축 권장 | 아니오 |
| pre:observe:continuous-learning | * | 지속 학습 관찰 | 아니오 |
| pre:governance-capture | Bash|Write|Edit|MultiEdit |治理 이벤트 캡처 | 아니오 |
| pre:config-protection | Write|Edit|MultiEdit | 설정 보호 | 아니오 |
| pre:mcp-health-check | * | MCP 건강 검사 | 블록 가능 |
| pre:edit-write:gateguard-fact-force | Edit|Write|MultiEdit | 첫 번째 편집 GateGuard | 블록 가능 |

### PostToolUse Hook

| ID | Matcher | 기능 | 비동기 |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR 로그/빌드 알림 | 예 |
| post:quality-gate | Edit|Write|MultiEdit | 품질 게이트 검사 | 예 |
| post:edit:design-quality-check | Edit|Write|MultiEdit | 설계 품질 검사 | 아니오 |
| post:edit:accumulator | Edit|Write|MultiEdit | 편집 누적기 | 아니오 |
| post:edit:console-warn | Edit | console.log 경고 | 아니오 |
| post:governance-capture | Bash|Write|Edit|MultiEdit |治理 이벤트 캡처 | 아니오 |
| post:session-activity-tracker | * | 세션 활동 추적 | 아니오 |
| post:observe:continuous-learning | * | 지속 학습 관찰 | 예 |
| post:ecc-metrics-bridge | * | 지표 브릿지 | 아니오 |
| post:ecc-context-monitor | * | 컨텍스트 모니터 | 아니오 |
| post:mcp-health-check | * (PostToolUseFailure) | MCP 건강 검사 | 아니오 |

### Stop Hook

| ID | Matcher | 기능 | 타임아웃 |
|----|---------|------|------|
| stop:format-typecheck | * | 배치 형식화 및 유형 검사 | 300s |
| stop:check-console-log | * | console.log 검사 | 30s |
| stop:session-end | * | 세션 상태 영속화 | 10s |
| stop:evaluate-session | * | 세션 평가 | 10s |
| stop:cost-tracker | * | 비용 추적 | 10s |
| stop:desktop-notify | * | 데스크톱 알림 | 10s |

### 라이프사이클 Hook

| ID | Event | 기능 |
|----|-------|------|
| session:start | SessionStart | 컨텍스트 불러오기 및 패키지 관리자 감지 |
| pre:compact | PreCompact | 압축 전 상태 저장 |
| session:end:marker | SessionEnd | 세션 종료 표시 |
