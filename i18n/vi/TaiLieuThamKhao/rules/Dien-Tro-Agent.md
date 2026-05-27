# Điều phối Agent

## Agent khả dụng

Nằm trong `~/.claude/agents/`:

| Agent | Mục đích | Thời điểm sử dụng |
|-------|------|----------|
| planner | Lập kế hoạch triển khai | Tính năng phức tạp, tái cấu trúc |
| architect | Thiết kế hệ thống | Quyết định kiến trúc |
| tdd-guide | Phát triển theo test | Tính năng mới, sửa lỗi |
| code-reviewer | Kiểm tra mã | Sau khi viết mã |
| security-reviewer | Phân tích bảo mật | Trước khi commit |
| build-error-resolver | Sửa lỗi build | Build thất bại |
| e2e-runner | Kiểm thử đầu cuối | Luồng người dùng quan trọng |
| refactor-cleaner | Dọn mã chết | Bảo trì mã |
| doc-updater | Cập nhật tài liệu | Cập nhật tài liệu |
| rust-reviewer | Kiểm tra mã Rust | Dự án Rust |
| harmonyos-app-resolver | Phát triển ứng dụng HarmonyOS | Dự án HarmonyOS/ArkTS |

## Sử dụng Agent ngay

Không cần nhắc người dùng:
1. Yêu cầu tính năng phức tạp → Sử dụng agent **planner**
2. Mã vừa viết/xửa → Sử dụng agent **code-reviewer**
3. Sửa lỗi hoặc tính năng mới → Sử dụng agent **tdd-guide**
4. Quyết định kiến trúc → Sử dụng agent **architect**

## Thực thi tác vụ song song

Với các thao tác độc lập, luôn sử dụng thực thi Task song song:

```markdown
# TỐT: Thực thi song song
Khởi chạy 3 agent song song:
1. Agent 1: Phân tích bảo mật module auth
2. Agent 2: Kiểm tra hiệu suất hệ thống cache
3. Agent 3: Kiểm tra loại công cụ

# XẤU: Thực thi tuần tự không cần thiết
Trước agent 1, sau đó agent 2, sau đó agent 3
```

## Phân tích đa góc nhìn

Với các vấn đề phức tạp, sử dụng sub-agents theo vai trò:
- Người kiểm tra thực tế
- Kỹ sư cao cấp
- Chuyên gia bảo mật
- Người kiểm tra tính nhất quán
- Người kiểm tra trùng lặp