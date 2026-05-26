# Hướng dẫn đóng góp Learn-ECC

Chào mừng bạn đóng góp nội dung cho Learn-ECC! Hướng dẫn này giải thích cách thêm nội dung mới, cải thiện nội dung hiện có hoặc gửi sửa đổi.

## Loại hình đóng góp

### 1. Thêm nội dung mới

Các loại nội dung có thể đóng góp:
- Chương hoặc mô-đun mới
- Bài tập mới
- Dự án thực chiến mới
- Bổ sung tra cứu nhanh
- Cải thiện bản dịch

### 2. Cải thiện nội dung hiện có

- Sửa lỗi
- Cải thiện tính rõ ràng của giải thích
- Thêm ví dụ
- Hoàn thiện đáp án bài tập
- Cập nhật thông tin lỗi thời

### 3. Báo cáo vấn đề

- Lỗi chính tả hoặc lỗi ngữ pháp
- Ví dụ mã không chạy được
- Liên kết không hoạt động
- Thiếu nội dung

## Tiêu chuẩn định dạng nội dung

### Định dạng Markdown

Tất cả nội dung phải sử dụng định dạng Markdown, bao gồm các phần sau:

```markdown
# Tiêu đề

## Mục tiêu học tập

Sau khi hoàn thành phần học này, bạn sẽ có thể:
- Mục tiêu1
- Mục tiêu2

---

## Nội dung chính

### Tiêu đề phụ

Nội dung...

---

## Bài tập

### Bài tập1

Mô tả nhiệm vụ...

### Phương pháp xác minh

1. Bước xác minh1
2. Bước xác minh2

---

## Bước tiếp theo

- Chương tiếp theo：[Liên kết](./next.md)
- Quay lại：[Mục lục](../README.md)
```

### Quy tắc đặt tên tệp

- Sử dụng tiếng Trung Quốc để đặt tên
- Sử dụng gạch ngang `-` để phân tách các từ
- Đặt tên tệp chương：`01-Tên chương.md`、`02-Tên chương.md`
- Đặt tên tệp bài tập：`Bài tập-Tên.md`

### Quy tắc liên kết

- Liên kết sách giáo khoa：`./TaiLieuThamKhao/Tên tệp.md`
- Liên kết trong khóa học：`./Tên chương.md`
- Đường dẫn tương đối：Tính từ vị trí tệp hiện tại

### Ví dụ mã

```javascript
// Ngôn ngữ mã
function example() {
  return "Hello ECC"
}
```

```bash
# Ví dụ dòng lệnh
node tests/run-all.js
```

### Định dạng bảng

| Tiêu đề1 | Tiêu đề2 | Tiêu đề3 |
|-------|-------|-------|
| Nội dung1 | Nội dung2 | Nội dung3 |

## Gửi sửa đổi

### Các bước

1. Fork repository
2. Tạo nhánh：`git checkout -b feature/Mô tả nội dung mới`
3. Thực hiện sửa đổi
4. Commit：`git commit -m "feat: Thêm nội dung mới"`
5. Đẩy lên remote：`git push origin feature/Mô tả nội dung mới`
6. Tạo Pull Request

### Quy tắc thông điệp commit

```
<type>: <description>

<optional body>
```

Loại Type:
- `feat`: Tính năng mới
- `fix`: Sửa lỗi
- `docs`: Cập nhật tài liệu
- `test`: Cập nhật kiểm thử
- `refactor`: Tái cấu trúc

## Kiểm tra chất lượng

Vui lòng kiểm tra trước khi gửi:

- [ ] Định dạng Markdown chính xác
- [ ] Tất cả liên kết có hiệu lực
- [ ] Ví dụ mã có thể chạy được
- [ ] Bài tập có phương pháp xác minh
- [ ] Tiếng Trung Quốc không có lỗi chính tả
- [ ] Tuân thủ tiêu chuẩn định dạng

## Phản hồi vấn đề

Nếu phát hiện vấn đề hoặc có đề xuất, vui lòng tạo Issue, bao gồm:
- Mô tả vấn đề
- Vị trí tệp
- Cách cải thiện được đề xuất

---

Cảm ơn sự đóng góp của bạn!