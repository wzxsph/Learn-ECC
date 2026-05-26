# Dự án hai: Kiểm tra mã

Sử dụng ECC để xây dựng quy trình kiểm tra mã tự động.

## Mục tiêu dự án

**Thời gian ước tính**: 2-3 giờ

Tạo một hệ thống kiểm tra mã tự động, tự động kiểm tra chất lượng mã, vấn đề bảo mật, tuân thủ quy tắc.

## Yêu cầu chức năng

### 1. Các chiều kiểm tra
- **Chất lượng mã** - Độ phức tạp, trùng lặp, đặt tên
- **Kiểm tra bảo mật** - Lỗ hổng, thông tin nhạy cảm, injection
- **Kiểm tra quy tắc** - Quy tắc mã hóa, tính toàn vẹn của tài liệu
- **Phạm vi kiểm tra** - Tỷ lệ phạm vi, chất lượng kiểm tra

### 2. Quy trình kiểm tra
- Kiểm tra được kích hoạt khi commit
- Kiểm tra đa chiều song song
- Tạo báo cáo tổng hợp
- Phân loại vấn đề theo cấp độ

### 3. Khả năng tích hợp
- Tích hợp Git Hook
- Tích hợp CI/CD
- Bình luận GitHub PR

## Phương án kỹ thuật

### Thiết kế Agent
- **code-reviewer** - Agent kiểm tra chính
- **security-reviewer** - Chuyên về bảo mật
- **tdd-guide** - Đánh giá chất lượng kiểm tra
> Tham khảo: [/code-review](../TaiLieuThamKhao/commands/01-Workflow-Co-Ban.md) | [code-reviewer Agent](../TaiLieuThamKhao/agents/01-Kiem-Duyet-Ma.md)

### Định dạng đầu ra
```json
{
  "summary": { "critical": "0", "high": "2", "medium": "5" },
  "issues": [
    { "type": "security", "severity": "high", "file": "src/auth.js", "line": 42 }
  ]
}
```

## Tiêu chuẩn nghiệm thu

- [ ] Hỗ trợ kiểm tra JavaScript/TypeScript
- [ ] Tỷ lệ phát hiện > 80%
- [ ] Tỷ lệ báo động sai < 10%
- [ ] Định dạng báo cáo thân thiện
- [ ] Tích hợp với quy trình Git

---

[Quay lại mục lục dự án thực chiến](./README.md)