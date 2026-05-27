# Встроенные Hooks

## Обзор

ECCПредоставлятьHookРеализовать, `ECC/scripts/hooks/`Содержание. 

## PreToolUse 

### pre:bash:dispatcher

**Тип**: PreToolUse
**matcher**: Bash
**Описание**: Bash, проверка, tmux, gitGateGuardпроверка

**функция**:
-  - tmux`npm run dev`
- Tmux -  (npm test, cargo build, docker)tmux
- Git - `git push`
- Pre-commitпроверка - проверка：lint, Проверка, обнаружениеconsole.log/debugger/

****:
- 0: Предупреждение
- 2: 

---

### pre:write:doc-file-warning

**Тип**: PreToolUse
**matcher**: Write
**Описание**: Предупреждение

**функция**:
- : README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- Содержание: docs/, skills/
- Предупреждение.md/.txt
- 

****: 0 (Предупреждение)

---

### pre:edit-write:suggest-compact

**Тип**: PreToolUse
**matcher**: Edit|Write
**Описание**:  (50)`/compact`

**функция**:
- 
- 
- , Предоставлять

****: 0 ()

---

### pre:observe:continuous-learning

**Тип**: PreToolUse
**matcher**: *
**Описание**: Поддерживать

**функция**:
- 
- Используется для
- , 

****: 0

---

### pre:governance-capture

**Тип**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Описание**:  (, , )

**функция**:
- обнаружение/
- 
- 

****: `ECC_GOVERNANCE_CAPTURE=1`

****: 0

---

### pre:config-protection

**Тип**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**Описание**: linter/formatter

**функция**:
- .eslintrc, .prettierrc
- 

****: 0

---

### pre:mcp-health-check

**Тип**: PreToolUse
**matcher**: *
**Описание**: MCPпроверкаMCP

**функция**:
- проверкаMCP
- MCP

****: 0

---

### pre:edit-write:gateguard-fact-force

**Тип**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: GateGuard：/, 

**функция**:
- 
- ：, , 
- Обоснование

****: 0

---

## PostToolUse 

### post:bash:dispatcher

**Тип**: PostToolUse
**matcher**: Bash
**Описание**: Bash, Используется для, PR

**функция**:
- PR - `gh pr create`PR URL
-  -  (, )

****: 0

---

### post:quality-gate

**Тип**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: проверка

**функция**:
- качество кодапроверка
- 
- 

****: 0

---

### post:edit:design-quality-check

**Тип**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: Предупреждениеобщий специалистШаблонUI

**функция**:
- обнаружениеUIСпроектировать
- общий специалистШаблон

****: 0

---

### post:edit:accumulator

**Тип**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Описание**: JS/TS, StopТиппроверка

**функция**:
- 
- stop:format-typecheck

****: 0

---

### post:edit:console-warn

**Тип**: PostToolUse
**matcher**: Edit
**Описание**: Предупреждениеconsole.log

**функция**:
- обнаружениеconsole.log
- Предупреждение

****: 0

---

### post:governance-capture

**Тип**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Описание**: 

**функция**:
- 
- обнаружениепроблема

****: `ECC_GOVERNANCE_CAPTURE=1`

****: 0

---

### post:session-activity-tracker

**Тип**: PostToolUse
**matcher**: *
**Описание**: 

**функция**:
- 
- 
- Используется дляECC2

****: 0

---

### post:observe:continuous-learning

**Тип**: PostToolUse
**matcher**: *
**Описание**: Поддерживать

**функция**:
- 
- Поддерживать

****: 0

---

### post:ecc-metrics-bridge

**Тип**: PostToolUse
**matcher**: *
**Описание**: 

**функция**:
- token
- 

****: 0

---

### post:ecc-context-monitor

**Тип**: PostToolUse
**matcher**: *
**Описание**: , ,  creep Предупреждение

**функция**:
- 
- Предупреждение
-  creep обнаружение
- обнаружение

****: 0

---

### post:mcp-health-check

**Тип**: PostToolUseFailure
**matcher**: *
**Описание**: неудачаMCP, 

**функция**:
- неудача
- 
- 

****: 0

---

## Stop 

### stop:format-typecheck

**Тип**: Stop
**matcher**: *
**Описание**:  (Biome/Prettier)Типпроверка (tsc)JS/TS

**функция**:
- Prettier/Biome
- TypeScriptТиппроверка
- Stop, 

****: 0

****: 300

---

### stop:check-console-log

**Тип**: Stop
**matcher**: *
**Описание**: проверкаconsole.log

**функция**:
- 
- console.log

****: 0

---

### stop:session-end

**Тип**: Stop
**matcher**: *
**Описание**:  (Stoptranscript_path)

**функция**:
- 
- 

****: 0

---

### stop:evaluate-session

**Тип**: Stop
**matcher**: *
**Описание**: 

**функция**:
- 
- Поддерживать
- 

****: 0

---

### stop:cost-tracker

**Тип**: Stop
**matcher**: *
**Описание**: token

**функция**:
- Token
- 
- 

****: 0

---

### stop:desktop-notify

**Тип**: Stop
**matcher**: *
**Описание**: ClaudemacOS/WSL, Задача

**функция**:
- 
- Задача

****: 0

---

## SessionStart 

### session:start

**Тип**: SessionStart
**matcher**: *
**Описание**: обнаружениеУправлять

**функция**:
- 
- обнаружение
- Управлятьобнаружение (npm/pnpm/yarn/bun)

****:
- `ECC_SESSION_START_MAX_CHARS`:  (8000)
- `ECC_SESSION_START_CONTEXT=off`: 

****: 0

---

## PreCompact 

### pre:compact

**Тип**: PreCompact
**matcher**: *
**Описание**: 

**функция**:
- 
- 

****: 0

---

## SessionEnd 

### session:end:marker

**Тип**: SessionEnd
**matcher**: *
**Описание**: 

**функция**:
- 
- 

****: 0

---

## 

### PreToolUse 

| ID | Matcher | функция |  |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | /tmux//GateGuardпроверка |  |
| pre:write:doc-file-warning | Write | Предупреждение |  |
| pre:edit-write:suggest-compact | Edit\|Write |  |  |
| pre:observe:continuous-learning | * |  |  |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit |  |  |
| pre:config-protection | Write\|Edit\|MultiEdit |  |  |
| pre:mcp-health-check | * | MCPпроверка |  |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | GateGuard |  |

### PostToolUse 

| ID | Matcher | функция |  |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR/ |  |
| post:quality-gate | Edit\|Write\|MultiEdit | проверка |  |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | Спроектироватьпроверка |  |
| post:edit:accumulator | Edit\|Write\|MultiEdit |  |  |
| post:edit:console-warn | Edit | console.logПредупреждение |  |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit |  |  |
| post:session-activity-tracker | * |  |  |
| post:observe:continuous-learning | * |  |  |
| post:ecc-metrics-bridge | * |  |  |
| post:ecc-context-monitor | * |  |  |
| post:mcp-health-check | * (PostToolUseFailure) | MCPпроверка |  |

### Stop 

| ID | Matcher | функция |  |
|----|---------|------|------|
| stop:format-typecheck | * | Типпроверка | 300s |
| stop:check-console-log | * | console.logпроверка | 30s |
| stop:session-end | * |  | 10s |
| stop:evaluate-session | * |  | 10s |
| stop:cost-tracker | * |  | 10s |
| stop:desktop-notify | * |  | 10s |

### 

| ID | Event | функция |
|----|-------|------|
| session:start | SessionStart | обнаружениеУправлять |
| pre:compact | PreCompact |  |
| session:end:marker | SessionEnd |  |
