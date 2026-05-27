# Lệnh Quản Lý Phiên

## Tổng quan

Lệnh quản lý phiên dùng để lưu, khôi phục và quản lý trạng thái phiên Claude Code. Các lệnh này thiết yếu để duy trì context trong phiên dài, chuyển giao công việc giữa các phiên, và theo dõi tiến độ.

## Danh sách lệnh

### /save-session

**Mục đích**: Lưu trạng thái phiên - viết đầy đủ context của phiên hiện tại vào date file

**Mô tả**: Lưu tất cả context của phiên hiện tại vào file trong thư mục `~/.claude/session-data/`, bao gồm: đã xây dựng gì, cái gì đã hoạt động, cái gì đã thất bại, công việc còn lại, trạng thái file, bản ghi quyết định và blocker cần giải quyết.

**Quy ước đặt tên file phiên**:
- Format: `YYYY-MM-DD-<short-id>-session.tmp`
- Ký tự hợp lệ: chữ cái `a-z`/`A-Z`, số `0-9`, dấu gạch ngang `-`, dấu gạch dưới `_`
- Độ dài tối thiểu: 1 ký tự (nhưng không khuyến khích quá ngắn)
- Khuyến khích: 8+ ký tự chữ thường/số/dấu gạch ngang để tránh trùng lặp trong ngày

**Khi nào sử dụng**:
- Khi kết thúc công việc (trước khi kết thúc phiên)
- Trước khi approaching context limit (lưu trước, rồi mở phiên mới)
- Sau khi giải quyết vấn đề phức tạp (lưu successful pattern)
- Khi cần chuyển giao cho phiên tương lai

**Quy trình**:
1. **Thu thập context** - Đọc tất cả file đã sửa, xem lại discussion
2. **Tạo thư mục** - `mkdir -p ~/.claude/session-data`
3. **Viết file** - Tạo session file với timestamp
4. **Hiển thị cho user** - Hiển thị nội dung và yêu cầu xác nhận

**Session file format**:

```markdown
# Session: YYYY-MM-DD

**Started:** [approximate time if known]
**Last Updated:** [current time]
**Project:** [project name or path]
**Topic:** [one-line summary of what this session was about]

---

## What We Are Building
[1-3 paragraphs describing the feature, bug fix, or task]

## What WORKED (with evidence)
- **[thing that works]** — confirmed by: [specific evidence]

## What Did NOT Work (and why)
- **[approach tried]** — failed because: [exact reason / error message]

## What Has NOT Been Tried Yet
- [approach / idea]

## Current State of Files
| File              | Status         | Notes                      |
| ----------------- | -------------- | -------------------------- |
| `path/to/file.ts` | PASS: Complete | [what it does]             |

## Decisions Made
- **[decision]** — reason: [why this was chosen over alternatives]

## Blockers & Open Questions
- [blocker / open question]

## Exact Next Step
[The single most important thing to do when resuming]
```

**Nguyên tắc quan trọng**:
- Phần "What Did NOT Work" là quan trọng nhất - phiên tương lai sẽ blind retry các phương pháp thất bại
- Mỗi phiên một file - không bao giờ append vào phiên trước
- File để phiên sau đọc qua `/resume-session`

**Best practice**:
- Khi lưu giữa chừng (không phải lúc kết thúc), đánh dấu rõ ràng in-progress
- Include specific error message và reason for failure ("threw X error because Y")
- Exact next step phải đủ precise để không cần suy nghĩ vẫn biết bắt đầu từ đâu

---

### /resume-session

**Mục đích**: Khôi phục phiên đã lưu

**Mô tả**: Load và khôi phục context của phiên đã lưu từ `~/.claude/session-data/`.

**Khi nào sử dụng**:
- Khi bắt đầu phiên mới
- Khi cần tiếp tục công việc trước đó
- Khi cần xem lại quá trình giải quyết vấn đề trước đó

---

### /sessions

**Mục đích**: Browse + search + quản lý session history

**Mô tả**: Browse, search và quản lý tất cả session history đã lưu.

**Chức năng**:
- Liệt kê tất cả phiên
- Search theo ngày
- Quản lý session aliases
- Dọn dẹp phiên cũ

---

### /checkpoint

**Mục đích**: Tạo/xác minh workflow checkpoint

**Mô tả**: Tạo checkpoint tại các điểm quan trọng của workflow, đảm bảo progress có thể trace được.

**Khi nào sử dụng**:
- Khi bước quan trọng hoàn thành
- Khi cần xác minh milestone quan trọng
- Trong multi-stage task

---

### /aside

**Mục đích**: Trả lời câu hỏi phụ mà không mất context

**Mô tả**: Chuyển sang lateral conversation mode, xử lý vấn đề phụ rồi quay lại seamlessly với main task.

**Khi nào sử dụng**:
- Khi cần hỏi nhanh một câu
- Khi cần tra cứu tài liệu
- Khi cần thực hiện auxiliary task

---

## Các lệnh liên quan

- `/save-session` - Lưu phiên hiện tại
- `/resume-session` - Khôi phục phiên
- `/sessions` - Quản lý session history