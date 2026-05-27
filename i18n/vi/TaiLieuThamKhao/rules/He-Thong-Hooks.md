# Hệ thống Hooks

## Loại Hook

- **PreToolUse**: Trước khi thực thi công cụ (xác thực, sửa đổi tham số)
- **PostToolUse**: Sau khi thực thi công cụ (định dạng tự động, kiểm tra)
- **Stop**: Khi phiên kết thúc (xác minh cuối cùng)

## Tự động chấp nhận quyền

Sử dụng cẩn thận:
- Bật cho các kế hoạch đáng tin cậy, được xác định rõ ràng
- Tắt cho công việc khám phá
- Không bao giờ sử dụng flag dangerously-skip-permissions
- Định cấu hình `allowedTools` trong `~/.claude.json`

## Thực hành tốt nhất TodoWrite

Sử dụng công cụ TodoWrite để:
- Theo dõi tiến độ tác vụ nhiều bước
- Xác minh hiểu biết về hướng dẫn
- Bật steering thời gian thực
- Hiển thị các bước triển khai chi tiết

Danh sách Todo cho thấy:
- Các bước không theo thứ tự
- Các mục còn thiếu
- Các mục không cần thiết thừa
- Độ chi tiết sai
- Yêu cầu bị hiểu sai