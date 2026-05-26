# Bài tập: Phát triển Skills

## Bài tập 1: Khám phá tất cả phân loại lệnh

### Mục tiêu
Tìm hiểu tất cả các phân loại lệnh của Claude Code, và nhận diện mỗi lệnh quan trọng trong mỗi phân loại.

### Các bước

1. **Liệt kê tất cả lệnh khả dụng**
   - Trong Claude Code, nhập `/` để xem tất cả lệnh
   - Chú ý cách tổ chức phân loại lệnh

2. **Khám phá từng phân loại**
   - Với mỗi phân loại, thử dùng `/help` hoặc `?` để lấy help
   - Ghi lại các lệnh cốt lõi và mục đích sử dụng của mỗi phân loại

3. **Ghi lại danh sách lệnh**
   Tạo bảng tra cứu lệnh của riêng bạn:

   | Phân loại | Lệnh | Mục đích | Tần suất sử dụng |
   |------|------|------|----------|
   | Quy trình phát triển | /plan | Tạo kế hoạch triển khai | ★★★ |
   | ... | ... | ... | ... |

### Tiêu chuẩn nghiệm thu
- [ ] Có thể liệt kê ít nhất 5 phân loại lệnh
- [ ] Đã ghi lại 20+ lệnh thường dùng
- [ ] Hiểu được logic phân loại lệnh

---

## Bài tập 2: Tổ hợp sử dụng nhiều lệnh

### Mục tiêu
Học cách tổ hợp nhiều lệnh để tạo workflow phát triển liên tục.

### Kịch bản
Sửa một lỗi build giả định, và thực hiện code review.

### Các bước

1. **Giả lập kịch bản**
   Giả sử bạn đang phát triển một dự án Go, gặp lỗi build:

   ```
   # Cài đặt kịch bản (không cần thực hiện thật)
   Giả sử có các lỗi build sau cần sửa:
   - import cycle detected
   - undefined: someFunction
   ```

2. **Bài tập tổ hợp lệnh**
   Theo thứ tự suy nghĩ và ghi lại tổ hợp lệnh sau:

   a. Đầu tiên dùng `/plan` lập kế hoạch sửa lỗi
   b. Dùng `/go-build` thử build
   c. Dùng `/build-fix` sửa lỗi
   d. Dùng `/code-review` review
   e. Dùng `/verify` xác minh sửa

3. **Ghi lại workflow**
   Tạo tài liệu workflow sửa lỗi của riêng bạn:

   ```markdown
   ## Workflow sửa lỗi build

   1. /plan - Phân tích vấn đề, soạn kế hoạch sửa
   2. /build-fix - Thực hiện sửa
   3. /code-review - Kiểm tra chất lượng sửa
   4. /verify - Xác minh chức năng bình thường
   ```

### Tiêu chuẩn nghiệm thu
- [ ] Hiểu được logic tổ hợp lệnh
- [ ] Đã tạo ít nhất 3 workflow tùy chỉnh
- [ ] Đã ghi lại best practices tổ hợp lệnh

---

## Bài tập 3: Tạo workflow tùy chỉnh

### Mục tiêu
Dựa trên nhu cầu dự án, tạo workflow tự động hóa từ tổ hợp lệnh.

### Kịch bản
Tạo workflow phát triển cho dự án Flutter giả định.

### Các bước

1. **Định nghĩa mục tiêu workflow**
   Dự án Flutter của bạn cần các workflow sau:

   - Phát triển hàng ngày: Viết code → Test → Build
   - Code review: Tự kiểm → Review → Sửa
   - Chuẩn bị release: Test → Build → Xác minh

2. **Thiết kế tổ hợp lệnh**

   **Workflow phát triển hàng ngày**:
   ```
   /flutter-build → /flutter-test → /verify
   ```

   **Workflow code review**:
   ```
   /flutter-review → /build-fix → /code-review
   ```

3. **Tạo tài liệu workflow**
   Tạo `CLAUDE_WORKFLOWS.md` trong dự án:

   ```markdown
   # Claude Code Workflow

   ## Flutter Development Workflow

   ### Phát triển hàng ngày
   1. Dùng `/plan` lập kế hoạch nhiệm vụ
   2. Dùng `/flutter-build` build
   3. Dùng `/flutter-test` test
   4. Dùng `/verify` xác minh

   ### Code review
   1. Dùng `/flutter-review` tự kiểm
   2. Dùng `/build-fix` sửa vấn đề
   3. Dùng `/code-review` yêu cầu review

   ### Chuẩn bị release
   1. Chạy full test suite
   2. Thực hiện build xác minh
   3. Dùng `/verify` xác nhận cuối cùng
   ```

4. **Thực hành xác minh**
   Nếu có dự án thật, thử execute workflow này.

### Tiêu chuẩn nghiệm thu
- [ ] Đã thiết kế 3 workflow đầy đủ
- [ ] Đã tạo tài liệu workflow
- [ ] Đã test ít nhất 1 workflow trong dự án thật

---

## Đáp án tham khảo

### Đáp án bài tập 1 - Danh sách lệnh mẫu

| Phân loại | Lệnh | Mục đích |
|------|------|------|
| Quy trình phát triển | /plan | Lập kế hoạch triển khai |
| Quy trình phát triển | /tdd | Phát triển driven bằng test |
| Quy trình phát triển | /build-fix | Sửa lỗi build |
| Quy trình phát triển | /verify | Xác minh thay đổi |
| Code review | /code-review | Code review thông thường |
| Code review | /security-review | Review bảo mật |
| Build | /go-build | Build Go |
| Build | /rust-build | Build Rust |
| Dự án | /project-init | Khởi tạo dự án |
| Dự án | /projects | Danh sách dự án |
| Kỹ năng | /skill-create | Tạo kỹ năng |
| Kỹ năng | /learn | Trích xuất học tập |
| Học tập | /deep-research | Nghiên cứu sâu |

### Đáp án bài tập 3 - Template workflow

```markdown
# [Tên dự án] Claude Code Workflow

## Quy trình phát triển
- Hàng ngày: /plan → [Viết code] → /verify
- Sửa lỗi: /build-fix → /code-review → /verify
- Release: /test → /build → /verify

## Hướng dẫn sử dụng
1. Dùng `/plan` bắt đầu bất kỳ nhiệm vụ nào
2. Sau khi viết code xong chạy `/verify`
3. Thay đổi quan trọng dùng `/code-review`
```

---

## Học tập sâu

- Đọc tài liệu lệnh đầy đủ: [../../../TaiLieuThamKhao/commands/01-核心工作流.md](../../../TaiLieuThamKhao/commands/01-核心工作流.md)
- Xem tất cả các phân loại lệnh khả dụng
- Học các tùy chọn và tham số ẩn của lệnh