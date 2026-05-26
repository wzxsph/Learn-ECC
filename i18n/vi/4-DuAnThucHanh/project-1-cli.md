# Dự án một: Công cụ CLI

Sử dụng ECC để xây dựng một công cụ dòng lệnh.

## Mục tiêu dự án

**Thời gian ước tính**: 2-3 giờ

Tạo một công cụ `ecc-gen` để tạo các thành phần dự án ECC (Agent, Skill, Hook).

## Yêu cầu chức năng

### 1. Chức năng cốt lõi
- `ecc-gen agent <name>` - Tạo tệp Agent
- `ecc-gen skill <name>` - Tạo tệp Skill
- `ecc-gen hook <name>` - Tạo cấu hình Hook

### 2. Chức năng tương tác
- Tạo bằng câu hỏi tương tác
- Chọn mẫu
- Chỉ định đường dẫn đầu ra

### 3. Chức năng nâng cao
- Tạo hàng loạt
- Mẫu tùy chỉnh
- Hỗ trợ tệp cấu hình

## Phương án kỹ thuật

### Cấu trúc thư mục

```
ecc-gen/
├── src/
│   ├── commands/
│   │   ├── agent.js
│   │   ├── skill.js
│   │   └── hook.js
│   ├── generators/
│   │   └── templates/
│   └── index.js
├── package.json
└── README.md
```

### Thư viện sử dụng
- `commander` - Phân tích tham số CLI
- `inquirer` - Câu hỏi tương tác
- `ejs` - Công cụ mẫu

## Tiêu chuẩn nghiệm thu

- [ ] Ba lệnh con đều hoạt động bình thường
- [ ] Định dạng mẫu được tạo chính xác
- [ ] Hỗ trợ chỉ định đường dẫn đầu ra bằng `--output`
- [ ] Có unit test
- [ ] Tài liệu đầy đủ

## Điểm học tập

- Phát triển công cụ CLI
- Thiết kế hệ thống mẫu
- Xử lý đầu vào tương tác
- Kiến trúc mô-đun

> Lệnh tham khảo: [/build-fix](../TaiLieuThamKhao/commands/04-Sua-Loi-Build.md) | [/test](../TaiLieuThamKhao/commands/02-Kiem-Thu.md)

---

[Quay lại mục lục dự án thực chiến](./README.md)