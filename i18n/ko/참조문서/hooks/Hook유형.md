# Hook 유형

## 개요

ECC Hooks 시스템은 이벤트 기반 자동화 메커니즘으로, Claude Code 도구 실행 전후에 트리거되어 코드 품질 강제, 오류 조기 발견 및 자동화된 반복 검사를 수행합니다.

```
사용자 요청 → Claude가 도구 선택 → PreToolUse 훅 실행 → 도구 실행 → PostToolUse 훅 실행
```

## 훅 유형 상세

### 1. PreToolUse 훅

#### 유형 설명
PreToolUse 훅은 도구 실행 **전에** 실행되며, 차단 (exit code 2) 또는 경고 (stderr 출력 but don't block)都可能.

#### 트리거时机
- 임의 도구 (Edit, Write, Bash, Read 등) 실행 이전
- 모든 도구 작업의 선행 검사에 적용

#### 매개변수 설명
```typescript
interface HookInput {
  tool_name: string;          // 도구 이름: "Bash", "Edit", "Write", "Read" 등
  tool_input: {
    command?: string;         // Bash: 실행할 명령
    file_path?: string;        // Edit/Write/Read: 대상 파일 경로
    old_string?: string;       // Edit: 교체될 텍스트
    new_string?: string;       // Edit: 교체 텍스트
    content?: string;          // Write: 파일 내용
  };
}
```

#### 종료码
| 종료码 | 의미 | 동작 |
|--------|------|------|
| 0 | 성공 | 계속 실행, 경고 출력 없음 |
| 2 | 차단 | 도구 실행 차단 |
| 기타非零 | 오류 | 로그 기록 but don't block |

#### 사용 예제

**개발 서버 차단기 (tmux 밖에서 npm run dev 실행 차단):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "tmux 밖에서 개발 서버 실행 차단"
}
```

**문서 파일 경고 (비표준 .md 파일 차단):**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "비표준 문서 파일 생성 경고"
}
```

#### 주의사항
- PreToolUse는 **블로킹형** 훅으로, 빠르게 실행해야 함 (<200ms)
- PreToolUse 훅에서 네트워크 호출 금지
- 차단 작업 (exit 2)은 핵심 보안 검사에만 신중하게 사용

---

### 2. PostToolUse 훅

#### 유형 설명
PostToolUse 훅은 도구 실행 **후에** 실행되며, 출력을 분석 가능 but 도구 실행 차단 불가.

#### 트리거时机
- 임의 도구 완료 이후 실행
- 도구의 출력 결과에 접근 가능

#### 매개변수 설명
```typescript
interface HookInput {
  tool_name: string;          // 도구 이름
  tool_input: {               // PreToolUse와 동일
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // PostToolUse에서만 사용 가능
    output?: string;          // 명령/도구의 출력
  };
}
```

#### 종료码
| 종료码 | 의미 |
|--------|------|
| 0 | 성공, 계속 실행 |
| 非零 | 오류, 로그 기록 but don't block 도구 실행 |

#### 사용 예제

**PR 로그 기록 (gh pr create 후 PR URL 기록):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "PR 생성 후 URL 및 리뷰 명령 기록"
}
```

**품질 Gate 검사 (편집 후 품질 검사 실행):**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "파일 편집 후 품질 Gate 검사 실행"
}
```

#### 주의사항
- PostToolUse 훅은 도구 실행을 **차단 불가**
- 비동기 (async: true)로 설정하여 블로킹 회피 가능
- 비동기 훅은 30초 내에 완료되어야 함

---

### 3. Stop 훅

#### 유형 설명
Stop 훅은 각 Claude 응답 후에 실행되며, 배치 처리, 최종 검사 및 세션 상태 영속화에 사용됩니다.

#### 트리거时机
- 각 사용자 요청 처리 완료 및 Claude가 응답 생성 후
- 세션의 마지막 응답 이후에 발생

#### 매개변수 설명
Stop 훅은 PostToolUse와 동일한 입력을 수신하며, 완전한 도구 호출 히스토리를 포함합니다.

#### 사용 예제

**콘솔 로그 검사 (응답 후 수정된 파일의 console.log 검사):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "수정된 파일에 console.log是否存在 检查"
}
```

**배치 포맷팅 및 유형 검사:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "모든 편집된 JS/TS 파일 배치 포맷팅 및 유형 검사"
}
```

**세션 요약 영속화:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "세션 상태 저장"
}
```

#### 주의사항
- Stop 훅은 각 응답 후에 실행되어 배치 작업에 적합
- 비블로킹 작업을 위해 async 패턴 사용 가능
- 포맷팅/유형 검사 등은 배칭 시 Stop에서 실행에 적합

---

### 4. SessionStart 훅

#### 유형 설명
SessionStart 훅은 세션 라이프사이클 시작 시 트리거되어, 이전 컨텍스트 로드 및 프로젝트 상태 감지에 사용됩니다.

#### 트리거时机
- 새 세션 시작 시
- Claude Code 시작 또는 새 세션 생성 시

#### 사용 예제

**이전 컨텍스트 로드 및 패키지 관리자 감지:**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "세션 시작 시 제한된 이전 컨텍스트 로드 및 프로젝트 상태 감지",
  "blocking": false
}
```

#### 주의사항
- 라이프사이클 훅으로, 빠르게 완료되어야 함 (<30초)
- `ECC_SESSION_START_MAX_CHARS` 환경 변수로 추가 컨텍스트 크기 제어 가능
- `ECC_SESSION_START_CONTEXT=off`로 완전 비활성화 가능

---

### 5. SessionEnd 훅

#### 유형 설명
SessionEnd 훅은 세션 종료 시 트리거되어, 세션 종료 요약 영속화 및 정리에 사용됩니다.

#### 트리거时机
- 세션 종료 시
- 세션 transcript 메타데이터 사용 가능 시

#### 사용 예제

**세션 종료 표시:**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "transcript 메타데이터 사용 가능 시 세션 종료 요약 영속화",
  "blocking": false
}
```

#### 주의사항
- 비동기 실행이 주이며, 세션 종료를 블로킹하면 안 됨
- 라이프사이클 표시는 지표 수집 및 정리에 사용

---

### 6. PreCompact 훅

#### 유형 설명
PreCompact 훅은 컨텍스트 압축 전에 트리거되어, 상태 저장에 사용됩니다.

#### 트리거时机
- 컨텍스트 압축/압축 이전
- 컨텍스트 창이 거의 찼을 때

#### 사용 예제

**컨텍스트 압축 전 상태 저장:**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "컨텍스트 압축 전 세션 상태 영속화",
  "blocking": false
}
```

#### 주의사항
- 긴 작업의 상태 저장에 적합
- 논블로킹, 압축 흐름에 영향 없음

---

### 7. PostToolUseFailure 훅

#### 유형 설명
PostToolUseFailure 훅은 도구 실행 실패 후 트리거됩니다.

#### 트리거时机
- 임의 도구 호출 실패 후

#### 사용 예제

**MCP 상태 검사 (실패한 MCP 호출 추적):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "실패한 MCP 도구 호출 추적, 비정상 서버 표시 및 재연결 시도"
}
```

#### 주의사항
- 재시도 로직 또는 건강 상태 추적에 사용 가능
- 도구 실패 상태를 변경하지 않음

---

## 훅 유형 비교표

| 유형 | 트리거时机 | 차단 가능 | 블로킹 가능 | 일반 용도 |
|------|----------|----------|----------|----------|
| PreToolUse | 도구 실행 전 | 가능 (exit 2) | 가능 (동기화) | 품질 검사, 보안 검증 |
| PostToolUse | 도구 실행 후 | 아니오 | 선택 (async) | 로그 기록, 분석 |
| Stop | 각 응답 후 | 아니오 | 선택 (async) | 배치 포맷팅, 세션 요약 |
| SessionStart | 세션 시작 | 아니오 | 아니오 | 컨텍스트 로드, 프로젝트 감지 |
| SessionEnd | 세션 종료 | 아니오 | 아니오 | 요약 영속화, 정리 |
| PreCompact | 압축 전 | 아니오 | 아니오 | 상태 저장 |
| PostToolUseFailure | 도구 실패 후 | 아니오 | 아니오 | 건강 검사, 재시도 |

---

## 환경 변수 제어

환경 변수로 훅 동작 제어 가능:

```bash
# minimal | standard | strict (기본: standard)
export ECC_HOOK_PROFILE=standard

# 특정 훅 ID 비활성화 (쉼표分隔)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# GateGuard 비활성화
export ECC_GATEGUARD=off

# SessionStart 추가 컨텍스트 크기 제어 (기본: 8000자)
export ECC_SESSION_START_MAX_CHARS=4000

# SessionStart 추가 컨텍스트 완전 비활성화
export ECC_SESSION_START_CONTEXT=off
```