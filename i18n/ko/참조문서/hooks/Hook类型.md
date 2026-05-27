# Hook 유형

## 개요

ECC Hooks 시스템是事件驱动的자동化机制，在Claude Code도구실행的前后触发，用于强制코드 품질、早期发现오류和자동化重复검사。

```
用户요청 → Claude选择도구 → PreToolUse钩子실행 → 도구실행 → PostToolUse钩子실행
```

## 钩子유형详解

### 1. PreToolUse 钩子

#### 유형설명
PreToolUse钩子在도구실행**之前**실행，가능阻止（exit code 2）或경고（stderr输出但不阻止）。

#### 触发时机
- 在임의도구（Edit、Write、Bash、Read等）실행之前
- 适用于모든도구操作的前置검사

#### 参数설명
```typescript
interface HookInput {
  tool_name: string;          // 도구名称: "Bash", "Edit", "Write", "Read"等
  tool_input: {
    command?: string;         // Bash: 要실행的명령
    file_path?: string;        // Edit/Write/Read: 目标파일路径
    old_string?: string;       // Edit: 被교체的文本
    new_string?: string;       // Edit: 교체文本
    content?: string;          // Write: 파일内容
  };
}
```

#### 退出码
| 退出码 | 含义 | 行为 |
|--------|------|------|
| 0 | 성공 | 계속실행，不输出경고 |
| 2 | 阻止 | 阻止도구실행 |
| 기타非零 | 오류 | 기록로그但不阻止 |

#### 사용예제

**개발서버阻止器（阻止在tmux外실행npm run dev）：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "阻止在tmux外실행개발서버"
}
```

**문서파일경고（阻止非표준.md파일）：**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "경고非표준문서파일的생성"
}
```

#### 注意事项
- PreToolUse是**阻塞型**钩子，필수快速실행（<200ms）
- 不要在PreToolUse钩子中진행网络调用
- 阻止操作（exit 2）应谨慎사용，仅用于핵심보안검사

---

### 2. PostToolUse 钩子

#### 유형설명
PostToolUse钩子在도구실행**之后**실행，가능분석输出但불가능阻止도구실행。

#### 触发时机
- 在임의도구완료之后실행
- 가능访问도구的输出结果

#### 参数설명
```typescript
interface HookInput {
  tool_name: string;          // 도구名称
  tool_input: {               // 与PreToolUse相同
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // 仅在PostToolUse中可用
    output?: string;          // 명령/도구的输出
  };
}
```

#### 退出码
| 退出码 | 含义 |
|--------|------|
| 0 | 성공，계속실행 |
| 非零 | 오류，기록로그但不阻止도구실행 |

#### 사용예제

**PR로그기록（gh pr create后기록PR URL）：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "PR생성后기록URL和리뷰명령"
}
```

**품질门검사（편집后실행품질검사）：**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "파일편집后실행품질门검사"
}
```

#### 注意事项
- PostToolUse钩子**불가능阻止**도구실행
- 가능설정为비동기（async: true）以避免阻塞
- 비동기钩子应在30秒内완료

---

### 3. Stop 钩子

#### 유형설명
Stop钩子在每次Claude응답后실행，用于배치处理、最终검사和세션状态持久化。

#### 触发时机
- 在각用户요청处理완료、Claude生成응답后
- 发生在세션的最后一个응답之后

#### 参数설명
Stop钩子接收与PostToolUse相同的输入，패키지含完整的도구调用历史。

#### 사용예제

**控制台로그검사（응답后검사수정파일中的console.log）：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "검사수정파일中是否있음console.log"
}
```

**배치형식化和유형검사：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "배치형식化모든편집的JS/TS파일并진행유형검사"
}
```

**세션摘要持久化：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "저장세션状态"
}
```

#### 注意事项
- Stop钩子在每次응답后실행，适合做배치操作
- 가능사용async패턴진행非阻塞操作
- 형식化/유형검사等适合在Stop时배치실행

---

### 4. SessionStart 钩子

#### 유형설명
SessionStart钩子在세션라이프사이클시작时触发，用于加载先前上下文和감지프로젝트状态。

#### 触发时机
- 新세션시작时
- Claude Code시작或생성新세션时

#### 사용예제

**加载先前上下文并감지패키지 관리자：**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "在세션시작时加载있음界限的先前上下文并감지프로젝트状态",
  "blocking": false
}
```

#### 注意事项
- 라이프사이클钩子，필수快速완료（<30秒）
- 가능통과`ECC_SESSION_START_MAX_CHARS`环境변수控制额外上下文크기
- 가능用`ECC_SESSION_START_CONTEXT=off`完全비활성화

---

### 5. SessionEnd 钩子

#### 유형설명
SessionEnd钩子在세션종료时触发，用于持久化세션종료摘要和정리。

#### 触发时机
- 세션종료时
- 当세션 transcript 元데이터可用时

#### 사용예제

**세션종료标记：**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "当transcript元데이터可用时持久化세션종료摘要",
  "blocking": false
}
```

#### 注意事项
- 비동기실행为主，不应阻塞세션종료
- 라이프사이클标记用于지표收集和정리

---

### 6. PreCompact 钩子

#### 유형설명
PreCompact钩子在上下文압축前触发，用于저장状态。

#### 触发时机
- 在上下文압축/精简之前
- 当上下文窗口接近满时

#### 사용예제

**上下文압축前저장状态：**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "在上下文압축前持久化세션状态",
  "blocking": false
}
```

#### 注意事项
- 适合저장长태스크的状态
- 非阻塞，不影响압축流程

---

### 7. PostToolUseFailure 钩子

#### 유형설명
PostToolUseFailure钩子在도구실행실패后触发。

#### 触发时机
- 임의도구调用실패后

#### 사용예제

**MCP健康검사（추적실패的MCP调用）：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "추적실패的MCP도구调用，标记不健康的서버并尝试重连"
}
```

#### 注意事项
- 可用于재시도逻辑或健康状态추적
- 不会改变도구실패的状态

---

## 钩子유형对比表

| 유형 | 触发时机 | 能否阻止 | 能否阻塞 | 典型용도 |
|------|----------|----------|----------|----------|
| PreToolUse | 도구실행前 | 是（exit 2） | 是（동기화） | 품질검사、보안검증 |
| PostToolUse | 도구실행后 | 否 | 선택（async） | 로그기록、분석 |
| Stop | 每次응답后 | 否 | 선택（async） | 배치형식化、세션摘要 |
| SessionStart | 세션시작 | 否 | 否 | 加载上下文、감지프로젝트 |
| SessionEnd | 세션종료 | 否 | 否 | 持久化摘要、정리 |
| PreCompact | 압축前 | 否 | 否 | 状态저장 |
| PostToolUseFailure | 도구실패后 | 否 | 否 | 健康검사、재시도 |

---

## 环境변수控制

가능통과环境변수控制钩子行为：

```bash
# minimal | standard | strict (기본: standard)
export ECC_HOOK_PROFILE=standard

# 비활성화特定钩子ID（逗号分隔）
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# 비활성화GateGuard
export ECC_GATEGUARD=off

# 控制SessionStart额外上下文크기（기본: 8000字符）
export ECC_SESSION_START_MAX_CHARS=4000

# 完全비활성화SessionStart额外上下文
export ECC_SESSION_START_CONTEXT=off
```
