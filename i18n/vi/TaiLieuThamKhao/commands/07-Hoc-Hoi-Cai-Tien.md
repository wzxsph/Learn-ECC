# Lệnh Vòng Lặp và Tự Động Hóa

## Tổng quan

Lệnh vòng lặp và tự động hóa dùng để khởi động và quản lý autonomous loop work mode.

## Danh sách lệnh

### /loop-start

**Mục đích**: Khởi động managed autonomous loop mode với safe default và explicit stop condition

**Mô tả**: Khởi động managed autonomous loop mode với safe default và explicit stop condition.

**Cách dùng**: `/loop-start [pattern] [--mode safe|fast]`

**Tham số**:

| Tham số | Giá trị | Mô tả |
|---|---|---|
| `pattern` | `sequential` | Execute task tuần tự, một sau một |
| | `continuous-pr` | Continuous PR creation và merge loop |
| | `rfc-dag` | RFC-based directed acyclic graph workflow |
| | `infinite` | Infinite loop (cần explicit stop condition) |
| `--mode` | `safe` (mặc định) | Strict quality gate và checkpoint |
| | `fast` | Giảm gate để tăng tốc |

**Quy trình**:
1. Xác nhận repo state và branch strategy
2. Chọn loop pattern và model tier strategy liên quan
3. Enable required hooks/profile cho pattern đã chọn
4. Tạo loop plan và viết runbook trong `.claude/plans/`
5. Print startup và monitoring command

**Bắt buộc safety check**:
- Xác minh test pass trước loop đầu tiên
- Đảm bảo `ECC_HOOK_PROFILE` không bị disable toàn cục
- Đảm bảo loop có explicit stop condition

**Tham số truyền**:
- `$ARGUMENTS`: `[pattern]` và optional `[--mode safe|fast]`
- Pattern là optional (mặc định sequential)
- Mode flag là optional (mặc định safe)

**Best practice**:
- Đảm bảo set explicit stop condition trước infinite mode
- Chỉ dùng fast mode trên code đã được test kỹ
- Monitor loop progress, dùng `/loop-status` để check status

**Common trap**:
- Khởi động loop mà không xác minh test
- Dùng infinite mode mà không có explicit stop condition
- Cố gắng dùng safe mode khi hooks bị disable toàn cục

**Tích hợp với các lệnh khác**:
- Dùng `/loop-status` để monitor loop đang chạy
- Dùng `/santa-loop` cho Santa-style loop
- Dùng `/checkpoint` để tạo checkpoint tại key node

---

### /loop-status

**Mục đích**: Check status của loop đang chạy

**Mô tả**: Xem status và progress của loop hiện tại đang chạy.

---

### /santa-loop

**Mục đích**: Santa-style autonomous loop

**Mô tả**: Một special style của autonomous loop mode.

---

## Các lệnh liên quan

- `/loop-start` - Khởi động loop
- `/loop-status` - Check status
- `/santa-loop` - Santa-style