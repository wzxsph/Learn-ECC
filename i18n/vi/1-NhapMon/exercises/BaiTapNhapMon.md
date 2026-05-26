# Bài tập nhập môn

Hoàn thành các bài tập sau để củng cố kiến thức cơ bản về ECC.

## Bài tập 1: Xác minh môi trường

**Mục tiêu**: Xác nhận ECC đã cài đặt đúng

**Các bước**:
1. Chạy test suite: `node tests/run-all.js`
2. Xác minh tất cả test pass
3. Liệt kê cấu trúc thư mục `.claude/`

**Tiêu chuẩn nghiệm thu**:
- Tất cả test pass
- Có thể thấy thư mục agents, skills, hooks, rules

---

## Bài tập 2: Sử dụng lệnh gạch chéo

**Mục tiêu**: Làm quen với các lệnh gạch chéo thường dùng

**Các bước**:
1. Khởi động Claude Code
2. Lần lượt thực hiện các lệnh sau, quan sát output:
   - `/help` hoặc `/ecc:plan`
   - `/ecc:code-review`
3. Ghi lại format output của mỗi lệnh

**Tiêu chuẩn nghiệm thu**:
- Hiểu chức năng và format output của mỗi lệnh

---

## Bài tập 3: Tạo Agent đơn giản

**Mục tiêu**: Hiểu format file Agent

**Các bước**:
1. Xem các file Agent trong thư mục `agents/`
2. Tham khảo format của `planner.md`
3. Tạo một `hello-agent.md` đơn giản, chức năng là in lời chào

**Tiêu chuẩn nghiệm thu**:
- File Agent mới có format đúng
- Có YAML frontmatter (name, description, tools, model)

---

## Bài tập 4: Chạy Hook

**Mục tiêu**: Hiểu cơ chế Hook

**Các bước**:
1. Xem cấu hình Hook trong thư mục `hooks/`
2. Chạy `node scripts/hooks/run-with-flags.js --help` (nếu hỗ trợ)
3. Hiểu thời điểm trigger và logic thực thi của Hook

**Tiêu chuẩn nghiệm thu**:
- Có thể mô tả nguyên lý hoạt động của Hook
- Hiểu sự khác biệt giữa PreToolUse và PostToolUse

---

## Bài tập 5: Nhiệm vụ tổng hợp

**Mục tiêu**: Hoàn thành một dự án nhỏ

**Nhiệm vụ**: Tạo utility format tên người

**Yêu cầu chức năng**:
- `formatName(name)` - Format thành "Họ, Tên"
- `initials(name)` - Trả về chữ viết tắt

**Quy trình thực hiện**:
1. Sử dụng `/ecc:plan` lập kế hoạch
2. Sử dụng `/tdd` phát triển
3. Sử dụng `/ecc:code-review` review

**Tiêu chuẩn nghiệm thu**:
- Implement đầy đủ cả hai function
- Có test case
- Pass code review

---

[Quay lại thư mục nhập môn](../README.md)