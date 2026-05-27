# Quy tắc phong cách mã

## Tổng quan quy tắc

ECC Rules quy tắc phong cách mã định nghĩa các nguyên tắc cốt lõi và thực hành tốt nhất cho tổ chức mã. Các quy tắc này đảm bảo mã duy trì tính dễ đọc, khả năng bảo trì và tính nhất quán, bao gồm mọi thứ từ tính bất biến đến quy ước đặt tên.

## Yêu cầu cốt lõi

### Tính bất biến (Nguyên tắc then chốt)

**BẮT BUỘC**: Luôn tạo đối tượng mới, không bao giờ sửa đổi đối tượng hiện có.

```typescript
// Ví dụ sai - Sửa đổi đối tượng gốc
function modify(user, name) {
  user.name = name;  // Thay đổi đối tượng gốc
  return user;
}

// Ví dụ đúng - Trả về đối tượng mới
function update(user, name) {
  return { ...user, name };  // Tạo bản sao mới
}
```

**Lý do**: Dữ liệu bất biến ngăn chặn side effects ẩn, giúp debug dễ dàng hơn và hỗ trợ concurrency an toàn.

### Nguyên tắc thiết kế cốt lõi

| Nguyên tắc | Mô tả | Thực hành |
|------|------|----------|
| **KISS** | Keep It Simple | Ưu tiên giải pháp đơn giản nhất hoạt động, tránh tối ưu hóa sớm, ưu tiên rõ ràng hơn thông minh |
| **DRY** | Don't Repeat Yourself | Trích xuất logic trùng lặp thành hàm chia sẻ hoặc utility, tránh copy-paste dẫn đến drift triển khai |
| **YAGNI** | You Aren't Gonna Need It | Không xây dựng tính năng hoặc abstract chưa cần thiết, bắt đầu đơn giản, tái cấu trúc khi thực sự cần |

### Tổ chức file

| Yêu cầu | Mô tả |
|------|------|
| Nhiều file nhỏ > Ít file lớn | Kết dính cao, ghép nối thấp |
| Số dòng điển hình | 200-400 dòng, tối đa 800 dòng |
| Tách file lớn | Trích xuất utility từ module lớn |
| Cách tổ chức | Theo chức năng/lĩnh vực, không theo loại |

### Xử lý lỗi

| Yêu cầu | Mô tả |
|------|------|
| Xử lý rõ ràng | Xử lý lỗi rõ ràng ở mọi cấp |
| Mã giao diện người dùng | Cung cấp thông báo lỗi thân thiện với người dùng |
| Server-side | Ghi log context lỗi chi tiết |
| Hành vi cấm | Không được nuốt lỗi âm thầm |

### Xác thực đầu vào

| Yêu cầu | Mô tả |
|------|------|
| Xác thực ranh giới | Xác thực tất cả đầu vào ở ranh giới hệ thống |
| Xác thực Schema | Sử dụng xác thực dựa trên Schema khi có thể |
| Fail nhanh | Fail nhanh với thông báo lỗi rõ ràng |
| Dữ liệu bên ngoài | Không bao giờ tin tưởng dữ liệu bên ngoài (phản hồi API, đầu vào người dùng, nội dung file) |

## Chi tiết triển khai

### Quy ước đặt tên

| Loại | Quy ước | Ví dụ |
|------|------|------|
| Biến và hàm | camelCase + tên mô tả | `calculateTotal`, `userName` |
| Boolean | Tiền tố is/has/should/can | `isActive`, `hasPermission` |
| Interface, Type, Component | PascalCase | `UserProfile`, `ButtonComponent` |
| Hằng số | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_TIMEOUT` |
| Custom Hooks | camelCase + tiền tố use | `useUserData`, `useFetch` |

### Mùi mã

#### Lồng ghép sâu

Sử dụng early return để tái cấu trúc khi lồng hơn 4 tầng:

```typescript
// Tránh
function processOrder(order) {
  if (order) {
    if (order.items) {
      if (order.items.length > 0) {
        // Logic xử lý
      }
    }
  }
}

// Khuyến nghị
function processOrder(order) {
  if (!order || !order.items || order.items.length === 0) {
    return;
  }
  // Logic xử lý
}
```

#### Số ma thuật

Sử dụng hằng số có tên thay cho số ma thuật:

```typescript
// Tránh
setTimeout(callback, 300000);

// Khuyến nghị
const SESSION_TIMEOUT_MS = 5 * 60 * 1000; // 5 phút
setTimeout(callback, SESSION_TIMEOUT_MS);
```

#### Hàm dài

Tách hàm lớn thành các hàm nhỏ với trách nhiệm rõ ràng:

| Độ dài hàm | Khuyến nghị |
|----------|------------|
| <50 dòng | Lý tưởng |
| 50-100 dòng | Có thể chấp nhận, cân nhắc tách |
| >100 dòng | Bắt buộc tách |

## Xử lý vi phạm

### Danh sách kiểm tra chất lượng mã

Trước khi đánh dấu hoàn thành:

- [ ] Mã dễ đọc và có tên tốt
- [ ] Hàm nhỏ (<50 dòng)
- [ ] File tập trung (<800 dòng)
- [ ] Không lồng ghép sâu (>4 tầng)
- [ ] Xử lý lỗi phù hợp
- [ ] Không có giá trị hardcoded (sử dụng hằng số hoặc cấu hình)
- [ ] Sử dụng pattern bất biến (không mutation)

### Phân loại mức độ nghiêm trọng

| Mức độ | Ý nghĩa | Hành động |
|------|------|------|
| CRITICAL | Lỗ hổng bảo mật hoặc nguy cơ mất dữ liệu | **Ngăn chặn** - Phải sửa trước khi merge |
| HIGH | Bug hoặc vấn đề chất lượng nghiêm trọng | **Cảnh báo** - Nên sửa trước khi merge |
| MEDIUM | Vấn đề khả năng bảo trì | **Gợi ý** - Khuyến nghị sửa |
| LOW | Vấn đề kiểu dáng hoặc đề xuất nhỏ | **Ghi chú** - Tùy chọn |

## Quy tắc liên quan

| Quy tắc liên quan | Mô tả |
|----------|------|
| Quy tắc kiểm thử | Yêu cầu coverage kiểm thử |
| Quy tắc bảo mật | Mục kiểm tra bảo mật |
| Quy tắc kiểm tra mã | Tiêu chuẩn kiểm tra chi tiết |
| Quy trình phát triển | Quy trình TDD và kiểm tra mã |