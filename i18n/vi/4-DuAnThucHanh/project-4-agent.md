# Dự án bốn: Hệ thống Agent

Xây dựng nền tảng hợp tác đa Agent.

## Mục tiêu dự án

**Thời gian ước tính**: 2-3 giờ

Tạo một hệ thống hợp tác đa Agent, nhiều Agent chuyên nghiệp phối hợp để hoàn thành các nhiệm vụ phức tạp.

## Yêu cầu chức năng

### 1. Vai trò Agent
- **architect** - Thiết kế hệ thống
- **developer** - Triển khai mã
- **code-reviewer** - Kiểm tra mã
- **tester** - Kỹ sư kiểm thử
> Tham khảo: [Agent kiểm tra mã](../TaiLieuThamKhao/agents/01-Kiem-Duyet-Ma.md) | [Agent kiến trúc](../TaiLieuThamKhao/agents/06-Kien-Truc.md)

### 2. Cơ chế phối hợp
- Phân phát nhiệm vụ
- Tổng hợp kết quả
- Giải quyết xung đột
- Đồng bộ hóa trạng thái

### 3. Chức năng quản lý
- Quản lý vòng đời Agent
- Hàng đợi nhiệm vụ
- Giám sát hiệu suất
- Lập lịch tài nguyên

## Phương án kỹ thuật

### Giao thức nhắn tin

```json
{
  "type": "task",
  "from": "architect",
  "to": "developer",
  "payload": { "task": "implement-auth", "context": {} }
}
```

### Máy trạng thái nhiệm vụ
- `pending` - Chờ phân công
- `running` - Đang thực thi
- `completed` - Đã hoàn thành
- `failed` - Thất bại
- `blocked` - Bị chặn

## Tiêu chuẩn nghiệm thu

- [ ] Hỗ trợ 4+ loại vai trò Agent
- [ ] Phân phát nhiệm vụ tự động
- [ ] Tổng hợp kết quả chính xác
- [ ] Cơ chế khôi phục khi thất bại
- [ ] Giám sát trạng thái thời gian thực

---

[Quay lại mục lục dự án thực chiến](./README.md)