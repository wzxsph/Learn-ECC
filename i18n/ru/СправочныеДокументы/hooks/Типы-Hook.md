# HookТип

## Обзор

ECC Система Hooks, Claude Code, Используется длякачество кода, ошибкапроверка. 

```
 → Claude → PreToolUse →  → PostToolUse
```

## Тип

### 1. PreToolUse 

#### ТипОписание
PreToolUse****, Можно (exit code 2)Предупреждение (stderr). 

#### 
-  (Edit, Write, Bash, Read)
- Используется дляпроверка

#### Описание
```typescript
interface HookInput {
  tool_name: string;          // Название: "Bash", "Edit", "Write", "Read"
  tool_input: {
    command?: string;         // Bash: 
    file_path?: string;        // Edit/Write/Read: Цель
    old_string?: string;       // Edit: 
    new_string?: string;       // Edit: 
    content?: string;          // Write: 
  };
}
```

#### 
|  |  |  |
|--------|------|------|
| 0 | успех | , Предупреждение |
| 2 |  |  |
|  | ошибка |  |

#### Пример

** (tmuxnpm run dev)：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "tmux"
}
```

**Предупреждение (.md)：**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Предупреждение"
}
```

#### Примечание
- PreToolUse****, Обязательно (<200ms)
- PreToolUse
-  (exit 2), Используется дляБезопасностьпроверка

---

### 2. PostToolUse 

#### ТипОписание
PostToolUse****, Можно. 

#### 
- Завершить
- Можно

#### Описание
```typescript
interface HookInput {
  tool_name: string;          // Название
  tool_input: {               // PreToolUse
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // PostToolUse
    output?: string;          // /
  };
}
```

#### 
|  |  |
|--------|------|
| 0 | успех,  |
|  | ошибка,  |

#### Пример

**PR (gh pr createPR URL)：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "PRURL"
}
```

**проверка (проверка)：**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "проверка"
}
```

#### Примечание
- PostToolUse****
- Можно (async: true)
- 30Завершить

---

### 3. Stop 

#### ТипОписание
StopClaude, Используется для, проверка. 

#### 
- Завершить, Claude
- 

#### Описание
StopPostToolUse, . 

#### Пример

**проверка (проверкаconsole.log)：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "проверкаconsole.log"
}
```

**Типпроверка：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "JS/TSТиппроверка"
}
```

**：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": ""
}
```

#### Примечание
- Stop, 
- Можноasync
- /ТиппроверкаStop

---

### 4. SessionStart 

#### ТипОписание
SessionStartНачать, Используется дляобнаружение. 

#### 
- Начать
- Claude Code

#### Пример

**обнаружениеУправлять：**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "Начатьобнаружение",
  "blocking": false
}
```

#### Примечание
- , ОбязательноЗавершить (<30)
- МожноЧерез`ECC_SESSION_START_MAX_CHARS`
- Можно`ECC_SESSION_START_CONTEXT=off`

---

### 5. SessionEnd 

#### ТипОписание
SessionEnd, Используется для. 

#### 
- 
-  transcript 

#### Пример

**：**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "transcript",
  "blocking": false
}
```

#### Примечание
- , 
- Используется для

---

### 6. PreCompact 

#### ТипОписание
PreCompact, Используется для. 

#### 
- /
- 

#### Пример

**：**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "",
  "blocking": false
}
```

#### Примечание
- Задача
- , 

---

### 7. PostToolUseFailure 

#### ТипОписание
PostToolUseFailureнеудача. 

#### 
- неудача

#### Пример

**MCPпроверка (неудачаMCP)：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "неудачаMCP, "
}
```

#### Примечание
- Используется для
- неудача

---

## Тип

| Тип |  |  |  | Назначение |
|------|----------|----------|----------|----------|
| PreToolUse |  |  (exit 2) |  () | проверка, БезопасностьПроверка |
| PostToolUse |  |  |  (async) | ,  |
| Stop |  |  |  (async) | ,  |
| SessionStart | Начать |  |  | , обнаружение |
| SessionEnd |  |  |  | ,  |
| PreCompact |  |  |  |  |
| PostToolUseFailure | неудача |  |  | проверка,  |

---

## 

МожноЧерез：

```bash
# minimal | standard | strict (: standard)
export ECC_HOOK_PROFILE=standard

# ID ()
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# GateGuard
export ECC_GATEGUARD=off

# SessionStart (: 8000)
export ECC_SESSION_START_MAX_CHARS=4000

# SessionStart
export ECC_SESSION_START_CONTEXT=off
```
