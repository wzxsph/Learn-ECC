# Lệnh Pull Request Workflow

## Tổng quan

PR workflow commands dùng để tạo, quản lý và review GitHub Pull Requests.

## Danh sách lệnh

### /pr

**Mục đích**: Tạo GitHub PR từ current branch với unpushed commits

**Mô tả**: Discover template, phân tích thay đổi, push branch và tạo PR. Phân tích full commit history, dùng `git diff [base-branch]...HEAD` để xem tất cả thay đổi.

**Input**: `$ARGUMENTS` - Optional, có thể chứa base branch name và/hoặc flag (ví dụ `--draft`)

**Parsing rule**:
- Extract any recognized flag (ví dụ `--draft`)
- Use remaining non-flag text làm base branch name
- Mặc định `main` nếu không chỉ định

**Quy trình**:

| Phase | Mô tả |
|---|---|
| **Phase 1: VALIDATE** | Kiểm tra prerequisite (không trên base branch, không có uncommitted change, có ahead commits, không có PR existing) |
| **Phase 2: DISCOVER** | Search PR template, phân tích commit, phân tích file change, check related planning artifact |
| **Phase 3: PUSH** | Push branch (rebase trước nếu cần) |
| **Phase 4: CREATE** | Tạo PR dùng template hoặc default format |
| **Phase 5: VERIFY** | Xác minh PR tạo thành công |
| **Phase 6: OUTPUT** | Report PR URL và next step |

**PR creation validation check**:

| Check | Điều kiện | Action khi fail |
|---|---|---|
| Không trên base branch | Current branch ≠ base | Dừng: "Switch sang feature branch trước" |
| Working tree sạch | Không có uncommitted change | Cảnh báo: "Commit hoặc stash trước" |
| Có ahead commit | `git log origin/<base>..HEAD` không rỗng | Dừng: "Không có ahead commit" |
| Không có PR existing | `gh pr list` empty | Dừng: "PR đã tồn tại" |

**Template search order**:
1. `.github/PULL_REQUEST_TEMPLATE/` directory (nếu có)
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

**Default PR format** (khi không có template):
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

**Edge case handling**:
- **Không có `gh` CLI**: Dừng và prompt install
- **Chưa authenticated**: Prompt chạy `gh auth login`
- **Branch forked**: Dùng `git fetch && git rebase` rồi push
- **Large PR (>20 files)**: Cảnh báo và suggest split

**Tích hợp với các lệnh khác**:
- Dùng `/code-review <number>` để review PR
- Dùng `gh pr merge <number>` để merge PR sẵn sàng
- Dùng `/plan-prd` hoặc `/plan` để tạo related planning artifact

---

### /review-pr

**Mục đích**: Review GitHub PR hoặc local uncommitted changes

**Mô tả**: Review Pull Request trên GitHub hoặc local uncommitted changes. PR review mode adapted từ PRPs-agentic-eng.

**Input**: `$ARGUMENTS` - Có thể là PR number, PR URL, hoặc blank (local review)

---

#### Mode selection

| Input | Hành động |
|---|---|
| PR number (ví dụ `42`) | Use làm PR number |
| URL (ví dụ `github.com/.../pull/42`) | Extract PR number |
| Branch name | Find PR via `gh pr list --head <branch>` |
| Blank | Use local review mode |

---

#### Local review mode

**Quy trình**:
1. **GATHER** - Thu thập thay đổi: `git diff --name-only HEAD`
2. **REVIEW** - Review mỗi file (security issue, quality issue, best practice)
3. **REPORT** - Generate report có severity level và fix suggestion

**Review checklist**:

| Category | Check items |
|---|---|
| **Security issue (CRITICAL)** | Hardcoded credentials, SQL injection, XSS, thiếu input validation, unsafe dependency, path traversal |
| **Code quality (HIGH)** | Function >50 lines, file >800 lines, nesting >4 levels, thiếu error handling, console.log, TODO/FIXME |
| **Best practice (MEDIUM)** | Change pattern, emoji usage, new code thiếu test, accessibility issue |

**Decision**:

| Điều kiện | Decision |
|---|---|
| Có CRITICAL hoặc HIGH issue | **BLOCK** - Block submit |
| Chỉ MEDIUM/LOW issue | Pass nhưng có comment |
| Không issue và validation pass | **APPROVE** |

---

#### PR review mode

**Quy trình**:
1. **FETCH** - Lấy PR metadata và diff
2. **CONTEXT** - Build review context (project rules, planning artifact, changed files)
3. **REVIEW** - Đọc full file, apply 7 category review checklist
4. **VALIDATE** - Chạy project-type validation command
5. **DECIDE** - Form recommendation theo finding
6. **REPORT** - Tạo review artifact
7. **PUBLISH** - Publish review lên GitHub
8. **OUTPUT** - Report cho user

**7 category review checklist**:

| Category | Check items |
|---|---|
| **Correctness** | Logic error, boundary case, null handling, race condition |
| **Type Safety** | Type mismatch, unsafe type cast, `any` usage, thiếu generic |
| **Pattern Compliance** | Follow project convention (naming, file structure, error handling, import) |
| **Security** | Injection, authentication gap, secret exposure, SSRF, path traversal, XSS |
| **Performance** | N+1 query, thiếu index, unbounded loop, memory leak, large payload |
| **Completeness** | Thiếu test, thiếu error handling, incomplete migration, thiếu documentation |
| **Maintainability** | Dead code, magic number, deep nesting, unclear naming, thiếu type |

**Validation command detection**:

| Project type | Detection file | Validation command |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

**Decision level**:

| Level | Ý nghĩa | Hành động |
|---|---|---|
| CRITICAL | Security vulnerability hoặc data loss risk | Phải sửa trước khi merge |
| HIGH | Bug hoặc logic error có thể gây vấn đề | Nên sửa trước khi merge |
| MEDIUM | Code quality hoặc best practice thiếu | Suggest fix |
| LOW | Style issue hoặc minor suggestion | Optional |

**Publish review lên GitHub**:
```bash
# APPROVE
gh pr review <NUMBER> --approve --body "<summary>"

# REQUEST_CHANGES
gh pr review <NUMBER> --request-changes --body "<summary with required fixes>"

# COMMENT only (draft PR hoặc informational)
gh pr review <NUMBER> --comment --body "<summary>"
```

---

### /multi-workflow (Cần hoàn thiện)

> (Cần hoàn thiện) Chi tiết lệnh cần bổ sung

**Mục đích**: Multi-model collaborative development

**Mô tả**: Coordinate multiple AI model cho collaborative development.

---

### /multi-backend (Cần hoàn thiện)

> (Cần hoàn thiện) Chi tiết lệnh cần bổ sung

**Mục đích**: Backend multi-model development

**Mô tả**: Multi-model collaborative với backend development làm trung tâm.

---

### /multi-frontend (Cần hoàn thiện)

> (Cần hoàn thiện) Chi tiết lệnh cần bổ sung

**Mục đích**: Frontend multi-model development

**Mô tả**: Multi-model collaborative với frontend development làm trung tâm.

---

### /multi-execute (Cần hoàn thiện)

> (Cần hoàn thiện) Chi tiết lệnh cần bổ sung

**Mục đích**: Multi-model collaborative execution

**Mô tả**: Coordinate multiple model để execute task in parallel.

---

## Các lệnh liên quan

- `/pr` - Tạo PR
- `/review-pr` - Review PR
- `/multi-workflow` - Multi-model collaborative