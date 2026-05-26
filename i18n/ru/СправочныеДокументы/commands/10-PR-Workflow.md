# Pull Request 工作流命令

## Обзор

PR 工作流命令Используется для创建、Управлять和审查 GitHub Pull Requests。

## 命令列表

### /pr

**Назначение**: 从当前分支创建 GitHub PR，带 unpushed 提交

**Описание**: 发现Шаблон、分析变更、推送分支并创建 PR。分析完整提交历史，使用 `git diff [base-branch]...HEAD` 查看所有变更。

**输入**: `$ARGUMENTS` - 可选，可包含基础分支名和/或标志（如 `--draft`）

**解析规则**:
- 提取任何识别的标志（如 `--draft`）
- 将剩余的非标志文本作为基础分支名
- 如果未指定，默认为 `main`

**工作流**:

| 阶段 | Описание |
|---|---|
| **Phase 1: VALIDATE** | 检查前置条件（不在base分支、无未提交变更、有ahead提交、无已有PR） |
| **Phase 2: DISCOVER** | 搜索PRШаблон、分析提交、分析文件变更、检查相关规划产物 |
| **Phase 3: PUSH** | 推送分支（如果Требуется则先rebase） |
| **Phase 4: CREATE** | 使用Шаблон或默认格式创建PR |
| **Phase 5: VERIFY** | ПроверкаPR创建成功 |
| **Phase 6: OUTPUT** | 报告PR URL和Следующий шаг |

**PR 创建Проверка检查**:

| 检查 | 条件 | 失败时动作 |
|---|---|---|
| 不在base分支 | 当前分支 ≠ base | 停止："先切换到功能分支" |
| 工作区干净 | 无未提交变更 | Предупреждение："先提交或stash" |
| 有ahead提交 | `git log origin/<base>..HEAD` 非空 | 停止："没有ahead提交" |
| 无已有PR | `gh pr list` 为空 | 停止："PR已存在" |

**Шаблон搜索顺序**:
1. `.github/PULL_REQUEST_TEMPLATE/` Содержание（如存在）
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

**默认 PR 格式**（无Шаблон时）:
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

**边缘情况处理**:
- **无 `gh` CLI**: 停止并Подсказка安装
- **未认证**: Подсказка运行 `gh auth login`
- **分支分叉**: 使用 `git fetch && git rebase` 然后推送
- **大型 PR (>20 文件)**: Предупреждение并建议拆分

**与Другие команды集成**:
- 使用 `/code-review <number>` 审查 PR
- 使用 `gh pr merge <number>` 合并就绪的 PR
- 使用 `/plan-prd` 或 `/plan` 创建相关规划产物

---

### /review-pr

**Назначение**: 审查 GitHub PR 或本地未提交变更

**Описание**: 审查 GitHub 上的 Pull Request 或本地未提交变更。PR 审查模式适配自 PRPs-agentic-eng。

**输入**: `$ARGUMENTS` - 可为 PR 编号、PR URL 或空白（本地审查）

---

#### Выбор паттернов

| 输入 | 动作 |
|---|---|
| PR 编号（如 `42`） | 作为 PR 编号使用 |
| URL（如 `github.com/.../pull/42`） | 提取 PR 编号 |
| 分支名 | Через `gh pr list --head <branch>` 查找 PR |
| 空白 | 使用本地审查模式 |

---

#### 本地审查模式

**工作流**:
1. **GATHER** - 收集变更: `git diff --name-only HEAD`
2. **REVIEW** - 审查每个文件（Безопасность问题、质量问题、Лучшие практики）
3. **REPORT** - 生成带严重级别和修复建议的报告

**审查清单**:

| Категория | 检查项 |
|---|---|
| **Безопасность问题 (CRITICAL)** | Жёстко закодированные учётные данные、SQL注入、XSS、缺少输入Проверка、不Безопасность依赖、路径遍历 |
| **Качество кода (HIGH)** | 函数>50行、文件>800行、嵌套>4层、缺少Обработка ошибок、console.log、TODO/FIXME |
| **Лучшие практики (MEDIUM)** | 变更模式、emoji使用、新代码缺少Тестирование、可访问性问题 |

**决策**:

| 条件 | 决策 |
|---|---|
| 存在 CRITICAL 或 HIGH 问题 | **BLOCK** - 阻止提交 |
| 仅 MEDIUM/LOW 问题 | Через但带评论 |
| 无问题且ПроверкаЧерез | **APPROVE** |

---

#### PR 审查模式

**工作流**:
1. **FETCH** - 获取 PR 元数据和 diff
2. **CONTEXT** - 构建审查上下文（项目规则、规划产物、变更文件）
3. **REVIEW** - 读取完整文件，应用 7 类审查清单
4. **VALIDATE** - 运行项目Тип的Проверка命令
5. **DECIDE** - 根据发现形成建议
6. **REPORT** - 创建审查产物
7. **PUBLISH** - 发布审查到 GitHub
8. **OUTPUT** - 报告给用户

**7 类审查清单**:

| Категория | 检查项 |
|---|---|
| **Correctness** | 逻辑错误、边界情况、空处理、竞态条件 |
| **Type Safety** | Тип不匹配、不Безопасность型转换、`any` 使用、缺少泛型 |
| **Pattern Compliance** | 符合项目约定（命名、文件结构、Обработка ошибок、导入） |
| **Security** | 注入、认证缺口、secret暴露、SSRF、路径遍历、XSS |
| **Performance** | N+1查询、缺少索引、无界循环、内存泄漏、大payload |
| **Completeness** | 缺少Тестирование、缺少Обработка ошибок、不完整迁移、缺少文档 |
| **Maintainability** | 死代码、魔法数字、深嵌套、命名不清、缺少Тип |

**Проверка命令检测**:

| 项目Тип | 检测文件 | Проверка命令 |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

**决策级别**:

| 级别 | 含义 | 动作 |
|---|---|---|
| CRITICAL | Уязвимости безопасности或数据丢失风险 | Обязательно修复才能合并 |
| HIGH | 可能导致问题的 bug 或逻辑错误 | 合并前应修复 |
| MEDIUM | Качество кода或Лучшие практики缺失 | 建议修复 |
| LOW | 样式问题或小建议 | 可选 |

**发布审查到 GitHub**:
```bash
# APPROVE
gh pr review <NUMBER> --approve --body "<summary>"

# REQUEST_CHANGES
gh pr review <NUMBER> --request-changes --body "<summary with required fixes>"

# COMMENT only (draft PR 或信息性)
gh pr review <NUMBER> --comment --body "<summary>"
```

---

### /multi-workflow（待完善）

> （待完善）命令详情待补充

**Назначение**: 多模型协作开发

**Описание**: 协调多个 AI 模型进行协作开发。

---

### /multi-backend（待完善）

> （待完善）命令详情待补充

**Назначение**: 后端多模型开发

**Описание**: 以后端开发为中心的多模型协作。

---

### /multi-frontend（待完善）

> （待完善）命令详情待补充

**Назначение**: 前端多模型开发

**Описание**: 以前端开发为中心的多模型协作。

---

### /multi-execute（待完善）

> （待完善）命令详情待补充

**Назначение**: 多模型协作执行

**Описание**: 协调多个模型Параллельное выполнениеЗадача。

---

## 相关命令

- `/pr` - 创建 PR
- `/review-pr` - 审查 PR
- `/multi-workflow` - 多模型协作
