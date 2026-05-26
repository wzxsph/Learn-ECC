# 내장 Hooks

## 개요

ECC플러그인提供了丰富的内置Hook구현，位于`ECC/scripts/hooks/`디렉토리中。

## PreToolUse 内置钩子

### pre:bash:dispatcher

**유형**: PreToolUse
**matcher**: Bash
**描述**: Bash预检分发器，整合了품질검사、tmux提醒、git푸시提醒和GateGuard검사

**기능**:
- 개발서버阻止器 - 阻止在tmux外실행`npm run dev`等명령
- Tmux提醒 - 권장对长时间실행的명령（npm test、cargo build、docker）사용tmux
- Git푸시提醒 - 提醒在`git push`前리뷰更改
- Pre-commit품질검사 - 실행품질검사：lint暂存파일、검증커밋정보형식、检测console.log/debugger/密钥

**退出码**:
- 0: 경고但계속
- 2: 阻止실행

---

### pre:write:doc-file-warning

**유형**: PreToolUse
**matcher**: Write
**描述**: 경고생성非표준문서파일

**기능**:
- 허용표준파일: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- 허용디렉토리: docs/, skills/
- 경고기타.md/.txt파일
- 跨平台路径处理

**退出码**: 0（仅경고）

---

### pre:edit-write:suggest-compact

**유형**: PreToolUse
**matcher**: Edit|Write
**描述**: 在逻辑间隔（约每50次도구调用）권장手动`/compact`

**기능**:
- 추적도구调用次数
- 在适当间隔提醒上下文압축
- 非阻塞，仅提供권장

**退出码**: 0（권장）

---

### pre:observe:continuous-learning

**유형**: PreToolUse
**matcher**: *
**描述**: 기록도구意图以지원持续학습信号

**기능**:
- 捕获도구사용观察
- 用于패턴提取和분석
- 비동기실행，不阻塞

**退出码**: 0

---

### pre:governance-capture

**유형**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**描述**: 捕获治理事件（密钥、전략违规、审批요청）

**기능**:
- 检测秘密/密钥泄露
- 捕获전략违规
- 기록审批요청

**활성화条件**: `ECC_GOVERNANCE_CAPTURE=1`

**退出码**: 0

---

### pre:config-protection

**유형**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**描述**: 阻止修改linter/formatter설정파일

**기능**:
- 阻止.eslintrc、.prettierrc等修改
- 引导수정코드而非削弱설정

**退出码**: 0

---

### pre:mcp-health-check

**유형**: PreToolUse
**matcher**: *
**描述**: MCP도구실행前검사MCP서버健康状态

**기능**:
- 검사MCP서버状态
- 阻止对不健康MCP서버的调用

**退出码**: 0

---

### pre:edit-write:gateguard-fact-force

**유형**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: 事实强制GateGuard：阻止각파일的首次편집/쓰기，요구在허용前进行调查

**기능**:
- 阻止首次편집
- 요구확인：가져오기、데이터패턴、用户指令
- 确保있음充分的理由再修改

**退出码**: 0

---

## PostToolUse 内置钩子

### post:bash:dispatcher

**유형**: PostToolUse
**matcher**: Bash
**描述**: Bash后检分发器，用于로그기록、PR和빌드알림

**기능**:
- PR로그 - `gh pr create`后기록PR URL和리뷰명령
- 빌드분석 - 빌드명령后的后台분석（비동기，非阻塞）

**退出码**: 0

---

### post:quality-gate

**유형**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: 파일편집后실행快速품질검사

**기능**:
- 코드 품질 검사
- 비동기실행
- 不阻塞편집操作

**退出码**: 0

---

### post:edit:design-quality-check

**유형**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: 경고프론트엔드편집偏离为通用模板外观的UI

**기능**:
- 检测UI설계품질
- 防止过于通用的模板外观

**退出码**: 0

---

### post:edit:accumulator

**유형**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: 기록편집的JS/TS파일路径，以便在Stop时배치형식化和유형검사

**기능**:
- 累积편집的파일列表
- 供stop:format-typecheck사용

**退出码**: 0

---

### post:edit:console-warn

**유형**: PostToolUse
**matcher**: Edit
**描述**: 편집后경고console.log语句

**기능**:
- 检测코드中的console.log
- 경고개발者정리디버깅코드

**退出码**: 0

---

### post:governance-capture

**유형**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**描述**: 从도구输出捕获治理事件

**기능**:
- 분석도구输出
- 检测潜在문제

**활성화条件**: `ECC_GOVERNANCE_CAPTURE=1`

**退出码**: 0

---

### post:session-activity-tracker

**유형**: PostToolUse
**matcher**: *
**描述**: 기록각세션的도구调用和파일活动

**기능**:
- 추적도구사용통계
- 파일活动기록
- 用于ECC2지표

**退出码**: 0

---

### post:observe:continuous-learning

**유형**: PostToolUse
**matcher**: *
**描述**: 기록도구结果以지원持续학습信号

**기능**:
- 捕获도구실행结果
- 지원패턴분석

**退出码**: 0

---

### post:ecc-metrics-bridge

**유형**: PostToolUse
**matcher**: *
**描述**: 维护실행中的세션지표聚合

**기능**:
- 추적token和成本지표
- 供状态栏和上下文监视器사용

**退出码**: 0

---

### post:ecc-context-monitor

**유형**: PostToolUse
**matcher**: *
**描述**: 在上下文耗尽、高成本、范围 creep 或도구루프时注入프록시경고

**기능**:
- 上下文사용모니터링
- 成本경고
- 范围 creep 检测
- 도구루프检测

**退出码**: 0

---

### post:mcp-health-check

**유형**: PostToolUseFailure
**matcher**: *
**描述**: 추적실패的MCP도구调用，标记不健康的서버并尝试重连

**기능**:
- 실패추적
- 健康状态标记
- 자동重连尝试

**退出码**: 0

---

## Stop 内置钩子

### stop:format-typecheck

**유형**: Stop
**matcher**: *
**描述**: 배치형식化（Biome/Prettier）和유형검사（tsc）本次응답中편집的모든JS/TS파일

**기능**:
- Prettier/Biome형식化
- TypeScript유형검사
- 在Stop时실행一次，而非每次편집后

**退出码**: 0

**타임아웃**: 300秒

---

### stop:check-console-log

**유형**: Stop
**matcher**: *
**描述**: 每次응답后검사修改파일中的console.log

**기능**:
- 扫描修改的파일
- 보고서console.log사용情况

**退出码**: 0

---

### stop:session-end

**유형**: Stop
**matcher**: *
**描述**: 每次응답后持久化세션状态（Stop携带transcript_path）

**기능**:
- 세션状态저장
- 上下文持久化

**退出码**: 0

---

### stop:evaluate-session

**유형**: Stop
**matcher**: *
**描述**: 评估세션以提取可학습的패턴

**기능**:
- 패턴识别
- 持续학습지원
- 비동기실행

**退出码**: 0

---

### stop:cost-tracker

**유형**: Stop
**matcher**: *
**描述**: 추적각세션的token和成本지표

**기능**:
- Token사용통계
- 成本估算
- 轻量级실행成本遥测标记

**退出码**: 0

---

### stop:desktop-notify

**유형**: Stop
**matcher**: *
**描述**: 在Claude응답时发送macOS/WSL桌面알림，패키지含태스크摘要

**기능**:
- 桌面알림
- 태스크摘要표시

**退出码**: 0

---

## SessionStart 内置钩子

### session:start

**유형**: SessionStart
**matcher**: *
**描述**: 加载先前上下文并检测新세션的패키지 관리자

**기능**:
- 加载있음界限的先前上下文
- 프로젝트状态检测
- 패키지 관리자检测（npm/pnpm/yarn/bun）

**环境변수**:
- `ECC_SESSION_START_MAX_CHARS`: 控制额外上下文크기（기본8000字符）
- `ECC_SESSION_START_CONTEXT=off`: 完全비활성화额外上下文

**退出码**: 0

---

## PreCompact 内置钩子

### pre:compact

**유형**: PreCompact
**matcher**: *
**描述**: 在上下文압축前저장状态

**기능**:
- 세션状态持久化
- 为압축准备上下文

**退出码**: 0

---

## SessionEnd 内置钩子

### session:end:marker

**유형**: SessionEnd
**matcher**: *
**描述**: 세션종료라이프사이클标记

**기능**:
- 라이프사이클事件标记
- 정리로그

**退出码**: 0

---

## 内置钩子速查表

### PreToolUse 钩子

| ID | Matcher | 기능 | 阻塞 |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | 품질/tmux/푸시/GateGuard검사 | 可阻塞 |
| pre:write:doc-file-warning | Write | 문서파일경고 | 否 |
| pre:edit-write:suggest-compact | Edit\|Write | 권장압축 | 否 |
| pre:observe:continuous-learning | * | 持续학습观察 | 否 |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit | 治理事件捕获 | 否 |
| pre:config-protection | Write\|Edit\|MultiEdit | 설정保护 | 否 |
| pre:mcp-health-check | * | MCP健康검사 | 可阻塞 |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | 首次편집GateGuard | 可阻塞 |

### PostToolUse 钩子

| ID | Matcher | 기능 | 비동기 |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR로그/빌드알림 | 是 |
| post:quality-gate | Edit\|Write\|MultiEdit | 품질门검사 | 是 |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | 설계품질검사 | 否 |
| post:edit:accumulator | Edit\|Write\|MultiEdit | 편집累积器 | 否 |
| post:edit:console-warn | Edit | console.log경고 | 否 |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit | 治理事件捕获 | 否 |
| post:session-activity-tracker | * | 세션活动추적 | 否 |
| post:observe:continuous-learning | * | 持续학습观察 | 是 |
| post:ecc-metrics-bridge | * | 지표桥接 | 否 |
| post:ecc-context-monitor | * | 上下文모니터링 | 否 |
| post:mcp-health-check | * (PostToolUseFailure) | MCP健康검사 | 否 |

### Stop 钩子

| ID | Matcher | 기능 | 타임아웃 |
|----|---------|------|------|
| stop:format-typecheck | * | 배치형식化和유형검사 | 300s |
| stop:check-console-log | * | console.log검사 | 30s |
| stop:session-end | * | 세션状态持久化 | 10s |
| stop:evaluate-session | * | 세션评估 | 10s |
| stop:cost-tracker | * | 成本추적 | 10s |
| stop:desktop-notify | * | 桌面알림 | 10s |

### 라이프사이클钩子

| ID | Event | 기능 |
|----|-------|------|
| session:start | SessionStart | 加载上下文和检测패키지 관리자 |
| pre:compact | PreCompact | 압축前状态저장 |
| session:end:marker | SessionEnd | 세션종료标记 |
