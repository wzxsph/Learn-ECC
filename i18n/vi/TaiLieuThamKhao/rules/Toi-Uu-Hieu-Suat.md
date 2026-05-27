# Tối ưu hiệu suất

## Chiến lược lựa chọn mô hình

**Haiku 4.5** (90% khả năng Sonnet, 1/3 chi phí):
- Agent nhẹ được gọi thường xuyên
- Pair programming và sinh mã
- Agent worker trong hệ thống đa agent

**Sonnet 4.6** (Mô hình mã hóa tốt nhất):
- Công việc phát triển chính
- Điều phối workflow đa agent
- Tác vụ mã hóa phức tạp

**Opus 4.5** (Suy luận sâu nhất):
- Quyết định kiến trúc phức tạp
- Nhu cầu suy luận tối đa
- Tác vụ nghiên cứu và phân tích

## Quản lý cửa sổ ngữ cảnh

Tránh sử dụng 20% cuối của cửa sổ ngữ cảnh cho:
- Tái cấu trúc quy mô lớn
- Triển khai tính năng qua nhiều file
- Gỡ lỗi tương tác phức tạp

Tác vụ ít nhạy cảm ngữ cảnh:
- Chỉnh sửa file đơn
- Tạo utility độc lập
- Cập nhật tài liệu
- Sửa lỗi đơn giản

## Extended Thinking + Plan Mode

Extended thinking được bật theo mặc định, dành tới 31,999 tokens cho suy luận nội bộ.

Điều khiển extended thinking:
- **Chuyển đổi**: Option+T (macOS) / Alt+T (Windows/Linux)
- **Cấu hình**: Đặt `alwaysThinkingEnabled` trong `~/.claude/settings.json`
- **Ngân sách tối đa**: `export MAX_THINKING_TOKENS=10000`
- **Chế độ chi tiết**: Ctrl+O xem đầu ra suy luận

Với tác vụ phức tạp cần suy luận sâu:
1. Đảm bảo extended thinking được bật (theo mặc định)
2. Bật **Plan Mode** để tiếp cận có cấu trúc
3. Sử dụng nhiều vòng phê bình để phân tích kỹ
4. Sử dụng sub-agents theo vai trò để có góc nhìn đa dạng

## Xử lý lỗi build

Nếu build thất bại:
1. Sử dụng agent **build-error-resolver**
2. Phân tích thông báo lỗi
3. Sửa theo từng bước
4. Xác minh sau mỗi lần sửa