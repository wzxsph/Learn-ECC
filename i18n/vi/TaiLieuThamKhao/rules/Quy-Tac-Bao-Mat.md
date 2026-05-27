# Quy tắc bảo mật

## Tổng quan quy tắc

ECC Rules quy tắc bảo mật là hệ thống kiểm tra bảo mật bắt buộc, nhằm đảm bảo mã tuân thủ các tiêu chuẩn bảo mật cao nhất trước khi commit. Quy tắc này bao phủ mọi khía cạnh từ quản lý Secrets đến bảo mật MCP server, là đường cơ sở bắt buộc cho tất cả thay đổi mã.

## Yêu cầu cốt lõi

### Danh sách kiểm tra bắt buộc trước commit

Trước khi commit bất kỳ mã nào, phải hoàn thành các kiểm tra bảo mật sau:

| Mục kiểm tra | Mô tả |
|--------|------|
| Không có Secrets hardcoded | API keys, mật khẩu, token không được hardcode trong mã nguồn |
| Xác thực đầu vào người dùng | Tất cả đầu vào người dùng phải được xác thực |
| Phòng ngừa SQL injection | Sử dụng truy vấn tham số hóa để ngăn SQL injection |
| Phòng ngừa XSS | HTML escape đầu vào người dùng |
| Bảo vệ CSRF | Đảm bảo CSRF protection được bật |
| Xác thực auth/authorization | Xác nhận logic xác thực và ủy quyền đúng |
| Cơ chế rate limiting | Tất cả endpoint phải có rate limiting |
| Thông báo lỗi bảo mật | Thông báo lỗi không được tiết lộ dữ liệu nhạy cảm |