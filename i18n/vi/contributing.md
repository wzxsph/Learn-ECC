# Hướng dẫn đóng góp Learn-ECC

Chào mừng bạn đóng góp nội dung cho Learn-ECC! Hướng dẫn này giải thích cách thêm nội dung mới, cải thiện nội dung hiện có hoặc gửi thay đổi.

## Các loại đóng góp

### 1. Thêm nội dung mới

Các loại nội dung có thể đóng góp:
- Chương hoặc mô-đun mới
- Bài tập mới
- Dự án thực hành mới
- Bổ sung cheatsheet
- Cải thiện bản dịch

### 2. Cải thiện nội dung hiện có

- Sửa lỗi
- Làm rõ giải thích
- Thêm ví dụ
- Hoàn thiện đáp án bài tập
- Cập nhật thông tin lỗi thời

### 3. Báo cáo vấn đề

- Lỗi chính tả hoặc ngữ pháp
- Ví dụ mã không chạy được
- Liên kết không hoạt động
- Thiếu nội dung

## Tiêu chuẩn định dạng nội dung

### Định dạng Markdown

Tất cả nội dung phải sử dụng định dạng Markdown, bao gồm các phần sau:

```markdown
# Tiêu đề

## Mục tiêu học tập

Sau phần này, bạn sẽ có thể:
- Mục tiêu 1
- Mục tiêu 2

---

## Nội dung chính

### Tiêu đề phụ

Nội dung...

---

## Bài tập

### Bài tập 1

Mô tả nhiệm vụ...

### Phương pháp xác minh

1. Bước xác minh 1
2. Bước xác minh 2

---

## Bước tiếp theo

- Chương tiếp theo: [Liên kết](./next.md)
- Quay lại: [Mục lục](../README.md)
```

### Quy tắc đặt tên tệp

- Sử dụng tiếng Việt để đặt tên
- Phân tách từ bằng dấu gạch ngang `-`
- Tệp chương: `01-Tên chương.md`, `02-Tên chương.md`
- Tệp bài tập: `Bài tập-Tên.md`

### Tiêu chuẩn liên kết

- Liên kết tài liệu tham khảo: `./TaiLieuThamKhao/Tên tệp.md`
- Liên kết trong khóa học: `./Tên chương.md`
- Đường dẫn tương đối: Tính từ vị trí tệp hiện tại

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

| Tiêu đề 1 | Tiêu đề 2 | Tiêu đề 3 |
|------|------|------|
| Nội dung 1 | Nội dung 2 | Nội dung 3 |

## Gửi thay đổi

### Các bước

1. Fork repository
2. Tạo nhánh: `git checkout -b feature/Mô-tả-nội-dung-mới`
3. Thực hiện thay đổi
4. Commit: `git commit -m "feat: Thêm nội dung mới"`
5. Push lên remote: `git push origin feature/Mô-tả-nội-dung-mới`
6. Tạo Pull Request

### Tiêu chuẩn thông điệp commit

```
<type>: <description>

<optional body>
```

Các loại Type:
- `feat`: Tính năng mới
- `fix`: Sửa lỗi
- `docs`: Cập nhật tài liệu
- `test`: Cập nhật kiểm thử
- `refactor`: Tái cấu trúc

## Kiểm tra chất lượng

Trước khi commit, vui lòng kiểm tra:
- [ ] Định dạng Markdown đúng
- [ ] Tất cả liên kết hoạt động
- [ ] Ví dụ mã có thể chạy được
- [ ] Bài tập có phương pháp xác minh
- [ ] Không lỗi chính tả tiếng Việt
- [ ] Tuân thủ tiêu chuẩn định dạng

## Báo cáo vấn đề

Nếu phát hiện vấn đề hoặc có đề xuất, vui lòng tạo Issue bao gồm:
- Mô tả vấn đề
- Vị trí tệp
- Cách cải thiện đề xuất

---

Cảm ơn sự đóng góp của bạn!