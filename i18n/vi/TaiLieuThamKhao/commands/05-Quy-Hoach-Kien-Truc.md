# Lệnh Học Hỏi và Cải Tiến

## Tổng quan

Lệnh học hỏi và cải tiến dùng để trích xuất pattern từ phiên, theo dõi instinct và quản lý kiến thức tổ chức.

## Danh sách lệnh

### /learn

**Mục đích**: Trích xuất pattern có thể tái sử dụng từ phiên hiện tại và lưu thành candidate skills

**Mô tả**: Phân tích phiên hiện tại, trích xuất bất kỳ pattern nào đáng lưu thành skills. Chạy `/learn` có thể dùng bất kỳ lúc nào trong phiên.

**Các loại nội dung được trích xuất**:

| Loại | Mô tả |
|---|---|
| **Error solution pattern** | Lỗi gì? Nguyên nhân gốc là gì? Sửa gì đã hiệu quả? Lỗi tương tự có thể tái sử dụng? |
| **Debug technique** | Các bước debug không hiển nhiên, combination tool hiệu quả, diagnostic pattern |
| **Workaround** | Quirks thư viện, API limitation, fix specific version |
| **Project-specific pattern** | Convention codebase discovered, architectural decision, integration pattern |

**Output format**: Lưu vào `~/.claude/skills/learned/[pattern-name].md`

```markdown
# [Descriptive Pattern Name]

**Extracted:** [Date]
**Context:** [Brief description of when this applies]

## Problem
[What problem this solves - be specific]

## Solution
[The pattern/technique/workaround]

## Example
[Code example if applicable]

## When to Use
[Trigger conditions - what should activate this skill]
```

**Quy trình**:
1. Xem xét các pattern có thể trích xuất từ phiên
2. Xác định insight có giá trị nhất/có thể tái sử dụng nhất
3. Soạn skill file
4. Yêu cầu user xác nhận trước khi lưu
5. Lưu vào `~/.claude/skills/learned/`

**Best practice**:
- Không trích xuất trivial fix (lỗi chính tả, simple syntax error)
- Không trích xuất one-time problem (API ngắt đặc biệt, etc.)
- Tập trung vào pattern tiết kiệm thời gian cho phiên tương lai
- Giữ skills tập trung - một pattern một skill

---

### /learn-eval

**Mục đích**: Trích xuất pattern + tự đánh giá chất lượng

**Mô tả**: Trích xuất pattern đồng thời đánh giá chất lượng.

---

### /evolve

**Mục đích**: Phân tích instinct + đề xuất cấu trúc tiến hóa

**Mô tả**: Phân tích instinct đã học, cung cấp đề xuất cấu trúc tiến hóa.

---

### /promote

**Mục đích**: Nâng cấp instinct project lên phạm vi toàn cục

**Mô tả**: Nâng cấp instinct đặc thù của project thành kiến thức có thể dùng toàn cục.

---

### /instinct-status

**Mục đích**: Hiển thị tất cả instinct đã học

**Mô tả**: Hiển thị trạng thái và confidence của tất cả instinct hiện tại.

---

### /instinct-export

**Mục đích**: Export instinct ra file

**Mô tả**: Export instinct thành file format có thể chia sẻ.

---

### /instinct-import

**Mục đích**: Import instinct từ file/URL

**Mô tả**: Import instinct vào system từ file hoặc URL.

---

### /skill-create

**Mục đích**: Phân tích git history + tạo skill file

**Mô tả**: Phân tích project git history, trích xuất pattern có thể tái sử dụng để tạo skill file.

**Quy trình**:
1. Phân tích git commit history
2. Xác định pattern lặp lại
3. Tạo skill file
4. Xác minh và lưu

---

### /skill-health

**Mục đích**: Skill portfolio health dashboard

**Mô tả**: Hiển thị skill portfolio health status và usage statistics.

---

### /rules-distill

**Mục đích**: Scan skills + trích xuất cross-domain principle

**Mô tả**: Scan tất cả skills, trích xuất cross-domain general principle.

---

## Các lệnh liên quan

- `/learn` - Trích xuất pattern
- `/skill-create` - Tạo skill từ git history
- `/instinct-status` - Xem instinct status