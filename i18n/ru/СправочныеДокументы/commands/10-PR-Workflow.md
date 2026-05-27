# Pull Request 

## Обзор

PR Используется для, Управлять GitHub Pull Requests. 

## 

### /pr

**Назначение**:  GitHub PR,  unpushed 

**Описание**: Шаблон, ,  PR. ,  `git diff [base-branch]...HEAD` . 

****: `$ARGUMENTS` - , / ( `--draft`)

****:
-  ( `--draft`)
- 
- ,  `main`

****:

|  | Описание |
|---|---|
| **Phase 1: VALIDATE** | проверка (base, , ahead, PR) |
| **Phase 2: DISCOVER** | PRШаблон, , , проверка |
| **Phase 3: PUSH** |  (Требуетсяrebase) |
| **Phase 4: CREATE** | ШаблонPR |
| **Phase 5: VERIFY** | ПроверкаPRуспех |
| **Phase 6: OUTPUT** | PR URLСледующий шаг |

**PR Проверкапроверка**:

| проверка |  | неудача |
|---|---|---|
| base |  ≠ base | ："функция" |
|  |  | Предупреждение："stash" |
| ahead | `git log origin/<base>..HEAD`  | ："ahead" |
| PR | `gh pr list`  | ："PR" |

**Шаблон**:
1. `.github/PULL_REQUEST_TEMPLATE/` Содержание ()
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

** PR ** (Шаблон):
```markdown
## Summary
<1-2 sentence description>

## Changes
<bulleted list grouped by area>

## Files Changed
<table of changed files>

## Testing
<how changes were tested, or "Needs testing">

## Related Issues
<linked issues or "None">
```

****:
- ** `gh` CLI**: Подсказка
- ****: Подсказка `gh auth login`
- ****:  `git fetch && git rebase` 
- ** PR (>20 )**: Предупреждение

**Другие команды**:
-  `/code-review <number>`  PR
-  `gh pr merge <number>`  PR
-  `/plan-prd`  `/plan` 

---

### /review-pr

**Назначение**:  GitHub PR 

**Описание**:  GitHub  Pull Request . PR  PRPs-agentic-eng. 

****: `$ARGUMENTS` -  PR , PR URL  ()

---

#### Выбор паттернов

|  |  |
|---|---|
| PR  ( `42`) |  PR  |
| URL ( `github.com/.../pull/42`) |  PR  |
|  | Через `gh pr list --head <branch>`  PR |
|  |  |

---

#### 

****:
1. **GATHER** - : `git diff --name-only HEAD`
2. **REVIEW** -  (Безопасностьпроблема, проблема, Лучшие практики)
3. **REPORT** - 

****:

| Категория | проверка |
|---|---|
| **Безопасностьпроблема (CRITICAL)** | Жёстко закодированные учётные данные, SQL-инъекция, XSS, Проверка, Безопасность,  |
| **качество кода (HIGH)** | >50, >800, >4, обработка ошибок, console.log, TODO/FIXME |
| **Лучшие практики (MEDIUM)** | , emoji, Тестирование, проблема |

****:

|  |  |
|---|---|
|  CRITICAL  HIGH проблема | **BLOCK** -  |
|  MEDIUM/LOW проблема | Через |
| проблемаПроверкаЧерез | **APPROVE** |

---

#### PR 

****:
1. **FETCH** -  PR  diff
2. **CONTEXT** -  (, , )
3. **REVIEW** - ,  7 
4. **VALIDATE** - ТипПроверка
5. **DECIDE** - 
6. **REPORT** - 
7. **PUBLISH** -  GitHub
8. **OUTPUT** - 

**7 **:

| Категория | проверка |
|---|---|
| **Correctness** | ошибка, , ,  |
| **Type Safety** | Тип, Безопасность, `any` ,  |
| **Pattern Compliance** | соглашения (, , обработка ошибок, ) |
| **Security** | , , secret, SSRF, , XSS |
| **Performance** | N+1, , , память, payload |
| **Completeness** | Тестирование, обработка ошибок, ,  |
| **Maintainability** | , , , , Тип |

**Проверкаобнаружение**:

| Тип | обнаружение | Проверка |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

****:

|  |  |  |
|---|---|---|
| CRITICAL | Уязвимости безопасности | Обязательно |
| HIGH | проблема bug ошибка |  |
| MEDIUM | качество кодаЛучшие практики |  |
| LOW | проблема |  |

** GitHub**:
```bash
# APPROVE
gh pr review <NUMBER> --approve --body "<summary>"

# REQUEST_CHANGES
gh pr review <NUMBER> --request-changes --body "<summary with required fixes>"

# COMMENT only (draft PR )
gh pr review <NUMBER> --comment --body "<summary>"
```

---

### /multi-workflow ()

>  ()

**Назначение**: 

**Описание**:  AI . 

---

### /multi-backend ()

>  ()

**Назначение**: 

**Описание**: . 

---

### /multi-frontend ()

>  ()

**Назначение**: 

**Описание**: . 

---

### /multi-execute ()

>  ()

**Назначение**: 

**Описание**: Параллельное выполнениеЗадача. 

---

## 

- `/pr` -  PR
- `/review-pr` -  PR
- `/multi-workflow` - 
