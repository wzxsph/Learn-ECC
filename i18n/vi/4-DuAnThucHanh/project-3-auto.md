# Dự án ba: Quy trình tự động hóa

Xây dựng hệ thống quy trình tự động hóa từ đầu đến cuối.

## Mục tiêu dự án

**Thời gian ước tính**: 2-3 giờ

Thiết kế và triển khai một công cụ quy trình tự động hóa, xử lý các nhiệm vụ phát triển phổ biến.

## Yêu cầu chức năng

### 1. Định nghĩa quy trình
- Định nghĩa định dạng YAML
- Kết nối nối tiếp và song song của các bước
- Rẽ nhánh có điều kiện
- Xử lý vòng lặp

### 2. Quy trình có sẵn
- **Kiểm tra mã** - lint + type-check + test
- **Xây dựng và phát hành** - build + test + deploy
- **Sửa lỗi** - analyze + fix + verify

### 3. Giám sát và cảnh báo
- Theo dõi trạng thái thực thi
- Thông báo khi thất bại
- Thống kê thời gian

## Phương án kỹ thuật

### Ví dụ định nghĩa quy trình

```yaml
name: code-check
steps:
  - name: lint
    command: npm run lint
  - name: type-check
    command: npm run type-check
  - name: test
    command: npm test
    requires: [lint, type-check]
```

### Tích hợp Hook
- PreToolUse - Xác thực tham số
- PostToolUse - Xác thực đầu ra
> Tham khảo: [Hook类型](../TaiLieuThamKhao/hooks/Loai-Hook.md) | [Tự động hóa và kịch bản](../TaiLieuThamKhao/skills/自动化与脚本.md)

## Tiêu chuẩn nghiệm thu

- [ ] Hỗ trợ định nghĩa YAML
- [ ] Phân tích phụ thuộc bước chính xác
- [ ] Tối ưu hóa thực thi song song
- [ ] Cơ chế thử lại khi thất bại
- [ ] Nhật ký thực thi đầy đủ

---

[Quay lại mục lục dự án thực chiến](./README.md)