# 03-Mẫu nâng cao

Nắm vững các mẫu thiết kế nâng cao và thực hành tốt nhất của ECC.

## Phân loại mẫu

> Tham khảo: [Thực hành tốt nhất](../TaiLieuThamKhao/skills/最佳实践.md)

### 1. Mẫu quy trình công việc
- **Mẫu đường ống** - Dữ liệu đi qua nhiều giai đoạn xử lý
- **Mẫu phát sóng** - Một nhiệm vụ được phân phát đến nhiều người thực thi
- **Mẫu backpressure** - Kiểm soát tốc độ sản xuất và tiêu thụ nhiệm vụ

### 2. Mẫu Agent
- **Mẫu phân cấp** - Agent chính điều phối các Agent con
- **Mẫu thị trường** - Agents giành nhiệm vụ thông qua đấu thầu
- **Mẫu tổ ong** - Nhiều Agents cùng loại hợp tác

### 3. Mẫu xử lý lỗi
- **Mẫu circuit breaker** - Thất bại nhanh sau khi thất bại
- **Mẫu retry** - Thử lại với backoff指数
- **Hàng đợi thương chết** - Lưu trữ tạm thời các nhiệm vụ không thể xử lý

## Tối ưu hóa hiệu suất

### Quản lý ngữ cảnh
- Dọn dẹp ngữ cảnh không sử dụng kịp thời
- Sử dụng nén để giảm tiêu thụ token
- Tách dữ liệu nóng và lạnh

### Kiểm soát đồng thời
- Giới hạn số lượng Agent đồng thời
- Sử dụng tín hiệu để kiểm soát tài nguyên
- Triển khai graceful degradation

## Bảo mật

### Kiểm soát quyền
- Nguyên tắc đặc quyền tối thiểu
- Nhật ký kiểm toán thao tác
- Xác nhận hai bước cho các thao tác nhạy cảm

### Xác thực đầu vào
- Xác thực tất cả đầu vào người dùng
- Truy vấn tham số hóa để chống injection
- Ẩn dữ liệu nhạy cảm trong đầu ra

## Bước tiếp theo

- [Quay lại chương trước](./02-AgentTuChu.md) - Đại lý tự trị
- [Dự án thực chiến](../4-DuAnThucHanh/README.md) - Áp dụng những gì đã học
- [Quay lại mục lục chuyên gia](../README.md)