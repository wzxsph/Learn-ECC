# 04 - Phát triển Skills

> Học cách tạo và sử dụng Skills, tích lũy pattern workflow thành kỹ năng có thể tái sử dụng

## Mục tiêu module

Sau khi hoàn thành module này, bạn sẽ có khả năng:

- Hiểu format và cấu trúc chuẩn của Skill
- Sử dụng `/learn` để trích xuất pattern từ session
- Sử dụng `/skill-create` để tạo Skill từ lịch sử git
- Tạo và quản lý Skills tùy chỉnh

## Format Skill

Mỗi Skill là một file Markdown, bao gồm các phần chuẩn hóa:

```markdown
# Tên Skill

## When to Use
Mô tả kịch bản khi nào sử dụng Skill này

## How It Works
Mô tả chi tiết nguyên lý hoạt động của Skill

## Examples
Ví dụ sử dụng

## Related Skills
Skills liên quan
```

### Các phần bắt buộc

| Phần | Mô tả | Bắt buộc |
|------|------|------|
| When to Use | Mô tả kịch bản sử dụng | Có |
| How It Works | Mô tả nguyên lý hoạt động | Có |
| Examples | Ví dụ sử dụng | Có |

### Các phần tùy chọn

| Phần | Mô tả |
|------|------|
| Related Skills | Skills liên quan |
| Prerequisites | Điều kiện tiên quyết |
| Tips | Mẹo sử dụng |

## Loại Skill

### 1. Skill loại Workflow

Mô tả workflow hoàn chỉnh:

```markdown
# TDD Workflow

## When to Use
Sử dụng phương pháp TDD khi phát triển tính năng mới hoặc sửa bug

## How It Works
1. Viết test (đỏ)
2. Implement chức năng (xanh)
3. Refactor tối ưu (xanh dương)

## Examples
/step 1: Viết test thất bại
/step 2: Implement code tối thiểu
/step 3: Chạy test xác minh
/step 4: Refactor
```

### 2. Skill loại Pattern

Tích lũy pattern code:

```markdown
# API Response Pattern

## When to Use
Sử dụng khi thiết kế format trả về API

## How It Works
Sử dụng format response thống nhất:
- success: Trạng thái thành công
- data: Dữ liệu phản hồi
- error: Thông báo lỗi (tùy chọn)

## Examples
{ "success": true, "data": {...}, "error": null }
```

### 3. Skill loại Best practices

Ghi lại best practices:

```markdown
# Git Commit Best Practices

## When to Use
Kiểm tra thông tin commit trước khi commit code

## How It Works
Tuân thủ định dạng Conventional Commits:
- feat: Tính năng mới
- fix: Sửa lỗi
- docs: Tài liệu
- test: Test
- refactor: Refactor

## Examples
feat: add user authentication
fix: resolve login bug
docs: update README
```

## Cách tạo Skill

### Cách 1: Sử dụng `/learn`

Trích xuất pattern từ session thành công:

```
/learn "Trích xuất workflow pattern từ session này"
```

### Cách 2: Sử dụng `/skill-create`

Tạo Skill từ lịch sử git:

```
/skill-create "Workflow sửa lỗi build"
```

### Cách 3: Tạo thủ công

Viết trực tiếp file Skill:

```markdown
# My Custom Skill

## When to Use
...

## How It Works
...
```

## Vị trí lưu trữ Skill

| Loại | Vị trí | Mô tả |
|------|------|------|
| Skills chính thức | `~/.claude/skills/` | Skills built-in của Claude Code |
| Skills tùy chỉnh | Project `skills/` | Dành riêng cho dự án |
| Rules Skills | `~/.claude/rules/` | Rules toàn cục |

## Lệnh quản lý Skill

| Lệnh | Chức năng |
|------|------|
| `/skill-scout` | Tìm kiếm Skills khả dụng |
| `/skill-stocktake` | Liệt kê tất cả Skills |
| `/skill-create` | Tạo Skill mới |
| `/skill-health` | Kiểm tra trạng thái khỏe mạnh của Skill |

## Sử dụng Skill

### Sử dụng trực tiếp

```
/skill-name
```

### Sử dụng trong nhiệm vụ

Tham chiếu Skill trong nhiệm vụ phức tạp:

```
Tôi cần phát triển một module thanh toán, sử dụng TDD workflow
```

## Best practices cho Skill

1. **Đơn trách nhiệm**: Mỗi Skill chỉ làm một việc
2. **Đặt tên rõ ràng**: Sử dụng tên mô tả
3. **Tài liệu đầy đủ**: Bao gồm tất cả các phần bắt buộc
4. **Xác minh thực tế**: Dựa trên workflow thực
5. **Cập nhật liên tục**: Cải thiện dựa trên phản hồi sử dụng

## Tài liệu học tập đi kèm

Tài liệu Skills đầy đủ nằm ở：`../../TaiLieuThamKhao/skills/最佳实践.md`

## Bước tiếp theo

- Học [bài tập](./exercises/练习.md)
- Đọc [tài liệu đầy đủ về Skills](../../TaiLieuThamKhao/skills/最佳实践.md)