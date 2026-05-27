# Lệnh Lập Kế Hoạch và Kiến Trúc

## Tổng quan

Lệnh lập kế hoạch và kiến trúc dùng để lập kế hoạch sản phẩm, thiết kế tính năng và quyết định kiến trúc hệ thống. Các lệnh này đảm bảo có kế hoạch rõ ràng trước khi viết code, giảm làm lại và cải thiện chất lượng mã。

## Danh sách lệnh

### /plan

**Mục đích**: Universal implementation planning - diễn giải lại requirement, đánh giá risk, tạo step-by-step implementation plan

**Mô tả**: Tạo comprehensive implementation plan sau khi chờ user xác nhận, trước khi chạm bất kỳ code nào. Accept free-form requirement hoặc PRD markdown file.

**Input mode**:

| Input | Mode | Behavior |
|---|---|---|
| `path/to/name.prd.md` | PRD artifact mode | Đọc PRD, chọn next delivery milestone, generate `.claude/plans/{name}.plan.md` |
| Markdown path khác | Reference mode | Đọc file làm context, produce inline plan |
| Free-form text | Dialog mode | Produce inline plan |
| Empty input | Clarify mode | Ask what to plan |

**Key tham số**:
- `$ARGUMENTS`: `[feature description | path/to/*.prd.md]` - Mô tả tính năng hoặc PRD file path

**Quy trình**:
1. **Diễn giải lại requirement** - Làm rõ cần xây dựng gì
2. **Xác định risk** - Liệt kê potential issue và obstacle
3. **Tạo step plan** - Chia thành phased implementation
4. **Chờ xác nhận** - Phải có user approval trước khi tiếp tục

**Output format** (PRD artifact mode):
```markdown
# Plan: {Feature Name}

**Source PRD**: {path}
**Selected Milestone**: {milestone or phase name}
**Complexity**: {Small | Medium | Large}

## Summary
{2-3 sentences}

## Patterns to Mirror
| Category | Source | Pattern |
|---|---|---|
| Naming | `path:line` | {short description} |
| Errors | `path:line` | {short description} |
| Tests | `path:line` | {short description} |

## Files to Change
| File | Action | Why |
|---|---|---|
| `path` | CREATE / UPDATE / DELETE | {reason} |

## Tasks
### Task 1: {name}
- **Action**: {what to do}
- **Mirror**: {pattern to follow}
- **Validate**: {command that proves correctness}

## Validation
```bash
{project-specific validation commands}
```

## Risks
| Risk | Likelihood | Mitigation |
|---|---|---|
```

**Best practice**:
- Research existing pattern trong codebase trước khi dùng
- Trong PRD artifact mode, tạo `.claude/plans/` directory (nếu chưa có)
- Nếu PRD chứa `Delivery Milestones` table, chỉ update selected row status

**Common trap**:
- Try to plan mà không ask clarification khi input trống
- Bắt đầu viết code mà không chờ user xác nhận
- Skip Pattern Grounding step

**Tích hợp với các lệnh khác**:
- Sau khi plan, dùng `tdd-workflow` skill cho test-driven development
- Dùng `/build-fix` để sửa build error
- Dùng `/code-review` để review completed implementation
- Dùng `/pr` hoặc `/prp-pr` để mở Pull Request

---

### /plan-prd

**Mục đích**: Interactive PRD generator

**Mô tả**: Generate problem-centered product requirement document, bao gồm problem definition, user persona, success metric và MVP scope.

**Khi nào sử dụng**: 
- Khi xác định tính năng cần xây dựng
- Khi cần làm rõ product requirement
- Khi lập kế hoạch tính năng mới từ đầu

**Quy trình**:
1. FRAME - Understand user và problem
2. GROUND - Gather evidence
3. DECIDE - Determine scope và assumption
4. GENERATE - Generate PRD file

---

### /prp-plan

**Mục đích**: Comprehensive feature planning

**Mô tả**: Complete project planning, cover codebase analysis, risk assessment và step-by-step implementation plan.

**Khi nào sử dụng**:
- Detailed planning cho complex feature
- Cần codebase context analysis
- Multi-phase implementation project

---

### /prp-prd

**Mục đích**: PRP workflow PRD generator

**Mô tả**: Generate product requirement document sử dụng PRP (Plan-Record-Produce) workflow.

---

### /prp-implement

**Mục đích**: Execute PRP plan + verification loop

**Mô tả**: Execute plan theo PRP workflow, bao gồm continuous verification và check.

---

### /prp-pr

**Mục đích**: Create PR from PRP workflow

**Mô tả**: Create GitHub Pull Request từ PRP workflow result.

---

### /prp-commit

**Mục đích**: PRP verification commit

**Mô tả**: Execute verification và commit code trong PRP workflow.

---

### /multi-plan

**Mục đích**: Multi-model collaborative planning

**Mô tả**: Combine dual-model analysis của Codex và Gemini, generate comprehensive implementation plan.

**Khi nào sử dụng**:
- Cần multi-perspective analysis
- Complex cross-domain project
- Cần balance frontend và backend consideration

---

## Các lệnh liên quan

- `/plan` - Universal implementation planning
- `/code-review` - Code review
- `/build-fix` - Build fix