# Pull Request 工作流命令

## 概述

PR 工作流命令用于创建、管理和审查 GitHub Pull Requests。

## 命令列表

### /pr

**用途**: 从当前分支创建 GitHub PR，带 unpushed 提交

**描述**: 发现模板、分析变更、推送分支并创建 PR。分析完整提交历史，使用 `git diff [base-branch]...HEAD` 查看所有变更。

**输入**: `$ARGUMENTS` - 可选，可包含基础分支名和/或标志（如 `--draft`）

**解析规则**:
- 提取任何识别的标志（如 `--draft`）
- 将剩余的非标志文本作为基础分支名
- 如果未指定，默认为 `main`

**工作流**:

| 阶段 | 说明 |
|---|---|
| **Phase 1: VALIDATE** | 检查前置条件（不在base分支、无未提交变更、有ahead提交、无已有PR） |
| **Phase 2: DISCOVER** | 搜索PR模板、分析提交、分析文件变更、检查相关规划产物 |
| **Phase 3: PUSH** | 推送分支（如果需要则先rebase） |
| **Phase 4: CREATE** | 使用模板或默认格式创建PR |
| **Phase 5: VERIFY** | 验证PR创建成功 |
| **Phase 6: OUTPUT** | 报告PR URL和下一步 |

**PR 创建验证检查**:

| 检查 | 条件 | 失败时动作 |
|---|---|---|
| 不在base分支 | 当前分支 ≠ base | 停止："先切换到功能分支" |
| 工作区干净 | 无未提交变更 | 警告："先提交或stash" |
| 有ahead提交 | `git log origin/<base>..HEAD` 非空 | 停止："没有ahead提交" |
| 无已有PR | `gh pr list` 为空 | 停止："PR已存在" |

**模板搜索顺序**:
1. `.github/PULL_REQUEST_TEMPLATE/` 目录（如存在）
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

**默认 PR 格式**（无模板时）:
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
- **无 `gh` CLI**: 停止并提示安装
- **未认证**: 提示运行 `gh auth login`
- **分支分叉**: 使用 `git fetch && git rebase` 然后推送
- **大型 PR (>20 文件)**: 警告并建议拆分

**与其他命令集成**:
- 使用 `/code-review <number>` 审查 PR
- 使用 `gh pr merge <number>` 合并就绪的 PR
- 使用 `/plan-prd` 或 `/plan` 创建相关规划产物

---

### /review-pr

**用途**: 审查 GitHub PR 或本地未提交变更

**描述**: 审查 GitHub 上的 Pull Request 或本地未提交变更。PR 审查模式适配自 PRPs-agentic-eng。

**输入**: `$ARGUMENTS` - 可为 PR 编号、PR URL 或空白（本地审查）

---

#### 模式选择

| 输入 | 动作 |
|---|---|
| PR 编号（如 `42`） | 作为 PR 编号使用 |
| URL（如 `github.com/.../pull/42`） | 提取 PR 编号 |
| 分支名 | 通过 `gh pr list --head <branch>` 查找 PR |
| 空白 | 使用本地审查模式 |

---

#### 本地审查模式

**工作流**:
1. **GATHER** - 收集变更: `git diff --name-only HEAD`
2. **REVIEW** - 审查每个文件（安全问题、质量问题、Melhores-Práticas）
3. **REPORT** - 生成带严重级别和修复建议的报告

**审查清单**:

| 类别 | 检查项 |
|---|---|
| **安全问题 (CRITICAL)** | 硬编码凭证、SQL注入、XSS、缺少输入验证、不安全依赖、路径遍历 |
| **代码质量 (HIGH)** | 函数>50行、文件>800行、嵌套>4层、缺少错误处理、console.log、TODO/FIXME |
| **Melhores-Práticas (MEDIUM)** | 变更模式、emoji使用、新代码缺少测试、可访问性问题 |

**决策**:

| 条件 | 决策 |
|---|---|
| 存在 CRITICAL 或 HIGH 问题 | **BLOCK** - 阻止提交 |
| 仅 MEDIUM/LOW 问题 | 通过但带评论 |
| 无问题且验证通过 | **APPROVE** |

---

#### PR 审查模式

**工作流**:
1. **FETCH** - 获取 PR 元数据和 diff
2. **CONTEXT** - 构建审查上下文（项目规则、规划产物、变更文件）
3. **REVIEW** - 读取完整文件，应用 7 类审查清单
4. **VALIDATE** - 运行项目类型的验证命令
5. **DECIDE** - 根据发现形成建议
6. **REPORT** - 创建审查产物
7. **PUBLISH** - 发布审查到 GitHub
8. **OUTPUT** - 报告给用户

**7 类审查清单**:

| 类别 | 检查项 |
|---|---|
| **Correctness** | 逻辑错误、边界情况、空处理、竞态条件 |
| **Type Safety** | 类型不匹配、不安全类型转换、`any` 使用、缺少泛型 |
| **Pattern Compliance** | 符合项目约定（命名、文件结构、错误处理、导入） |
| **Security** | 注入、认证缺口、secret暴露、SSRF、路径遍历、XSS |
| **Performance** | N+1查询、缺少索引、无界循环、内存泄漏、大payload |
| **Completeness** | 缺少测试、缺少错误处理、不完整迁移、缺少文档 |
| **Maintainability** | 死代码、魔法数字、深嵌套、命名不清、缺少类型 |

**验证命令检测**:

| 项目类型 | 检测文件 | 验证命令 |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

**决策级别**:

| 级别 | 含义 | 动作 |
|---|---|---|
| CRITICAL | 安全漏洞或数据丢失风险 | 必须修复才能合并 |
| HIGH | 可能导致问题的 bug 或逻辑错误 | 合并前应修复 |
| MEDIUM | 代码质量或Melhores-Práticas缺失 | 建议修复 |
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

**用途**: 多模型协作开发

**描述**: 协调多个 AI 模型进行协作开发。

---

### /multi-backend（待完善）

> （待完善）命令详情待补充

**用途**: 后端多模型开发

**描述**: 以后端开发为中心的多模型协作。

---

### /multi-frontend（待完善）

> （待完善）命令详情待补充

**用途**: 前端多模型开发

**描述**: 以前端开发为中心的多模型协作。

---

### /multi-execute（待完善）

> （待完善）命令详情待补充

**用途**: 多模型协作执行

**描述**: 协调多个模型并行执行任务。

---

## 相关命令

- `/pr` - 创建 PR
- `/review-pr` - 审查 PR
- `/multi-workflow` - 多模型协作
