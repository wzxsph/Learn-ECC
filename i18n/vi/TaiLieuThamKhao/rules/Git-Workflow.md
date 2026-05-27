# Quy trình Git

## Định dạng thông điệp Commit

```
<type>: <mô tả>

<body tùy chọn>
```

**Loại (type)**:
- `feat`: Tính năng mới
- `fix`: Sửa lỗi
- `refactor`: Tái cấu trúc
- `docs`: Tài liệu
- `test`: Kiểm thử
- `chore`: Việc vặt
- `perf`: Tối ưu hiệu suất
- `ci`: CI/CD

> Lưu ý: Gán attribution bị vô hiệu hóa toàn cục qua `~/.claude/settings.json`

## Quy trình Pull Request

Khi tạo PR:
1. Phân tích toàn bộ lịch sử commit (không chỉ commit mới nhất)
2. Sử dụng `git diff [base-branch]...HEAD` để xem tất cả thay đổi
3. Soạn tóm tắt PR toàn diện
4. Bao gồm TODOs cho kế hoạch kiểm thử
5. Nếu là nhánh mới, push với flag `-u`


## Đặt tên nhánh

Sử dụng tiêu chuẩn git flow:
- `feature/`: Tính năng mới
- `fix/`: Sửa lỗi
- `refactor/`: Tái cấu trúc
- `docs/`: Tài liệu

## Quy trình phát triển

Quy trình phát triển đầy đủ:
1. **Nghiên cứu & Tái sử dụng** - Tìm kiếm GitHub, tài liệu thư viện, registry gói
2. **Lập kế hoạch** - Sử dụng planner agent tạo kế hoạch triển khai
3. **Phương pháp TDD** - Viết test trước, sau đó triển khai
4. **Kiểm tra mã** - Sử dụng code-reviewer agent
5. **Commit & Push** - Thông điệp commit chi tiết, tuân thủ conventional commits