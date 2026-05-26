# 01 - Làm chủ hệ thống lệnh

> Làm chủ 75+ lệnh của Claude Code, tổ hợp linh hoạt để phát triển hiệu quả

## Mục tiêu module

Sau khi hoàn thành module này, bạn sẽ có khả năng:

- Nhận diện và sử dụng các lệnh trong tất cả các phân loại
- Chọn lệnh phù hợp nhất theo kịch bản
- Tổ hợp nhiều lệnh để tạo workflow tùy chỉnh
- Tra cứu nhanh tài liệu lệnh

## Tổng quan lệnh

Claude Code cung cấp 75+ lệnh, bao gồm các phân loại sau:

| Phân loại | Số lệnh | Lệnh ví dụ |
|------|--------|----------|
| Quy trình phát triển | 15+ | `/plan`, `/tdd`, `/build-fix`, `/verify` |
| Code review | 10+ | `/code-review`, `/security-review`, `/python-review` |
| Build và test | 15+ | `/go-build`, `/rust-build`, `/flutter-build` |
| Quản lý dự án | 10+ | `/project-init`, `/projects`, `/santa-loop` |
| Quản lý kỹ năng | 10+ | `/skill-create`, `/skill-stocktake`, `/skill-scout` |
| Học tập và nghiên cứu | 10+ | `/learn`, `/learn-eval`, `/deep-research` |
| Công cụ hỗ trợ | 15+ | `/config`, `/jira`, `/pr`, `/e2e` |

## Cấu trúc tài liệu lệnh

Mỗi lệnh đều có cấu trúc tài liệu chuẩn:

```
Tên lệnh
├── Mô tả lệnh
├── Kịch bản sử dụng
├── Giải thích tham số
├── Ví dụ sử dụng
└── Lưu ý
```

## Tra cứu lệnh thường dùng

### Quy trình phát triển
- `/plan` - Tạo kế hoạch triển khai
- `/tdd` - Phát triển driven bằng test
- `/build-fix` - Sửa lỗi build
- `/verify` - Xác minh thay đổi code
- `/review` - Code review

### Build liên quan
- `/go-build`, `/rust-build`, `/kotlin-build` - Build các ngôn ngữ khác nhau
- `/cpp-build`, `/flutter-build` - Build ngôn ngữ biên dịch
- `/docker-build` - Build Docker image

### Quản lý dự án
- `/project-init` - Khởi tạo dự án mới
- `/projects` - Liệt kê tất cả dự án
- `/santa-loop` - Vòng lặp nhiệm vụ tự động

### Phát triển kỹ năng
- `/skill-create` - Tạo Skill từ lịch sử git
- `/skill-scout` - Tìm kiếm Skills khả dụng
- `/learn` - Trích xuất pattern từ session

## Cách sử dụng tài liệu lệnh

Tài liệu lệnh đầy đủ nằm ở：`../../TaiLieuThamKhao/commands/01-Workflow-Co-Ban.md`

Tài liệu bao gồm:
- Mô tả chi tiết của mỗi lệnh
- Giải thích tham số và tùy chọn
- Ví dụ sử dụng và output
- Đề xuất best practices

## Tổ hợp sử dụng lệnh

Lệnh có thể tổ hợp sử dụng, thực hiện workflow phức tạp:

```
/plan → /tdd → /build-fix → /code-review → /verify
```

Output của mỗi lệnh có thể truyền cho lệnh tiếp theo, hình thành pipeline tự động hóa.

## Bước tiếp theo

- Học [bài tập](./exercises/练习.md)
- Đọc [tài liệu đầy đủ về lệnh](../../TaiLieuThamKhao/commands/01-Workflow-Co-Ban.md)