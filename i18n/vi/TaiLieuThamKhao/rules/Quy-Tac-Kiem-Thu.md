# Quy tắc kiểm thử

## Tổng quan quy tắc

ECC Rules quy tắc kiểm thử định nghĩa các tiêu chuẩn kiểm thử bắt buộc để đảm bảo chất lượng mã. Quy tắc này bao gồm phát triển theo test (TDD), yêu cầu coverage tối thiểu và tiêu chuẩn cấu trúc kiểm thử, đảm bảo tất cả mã được kiểm thử đầy đủ.

## Yêu cầu cốt lõi

### Coverage kiểm thử tối thiểu: 80%

Phải bao gồm ba loại kiểm thử sau:

| Loại kiểm thử | Mô tả | Phạm vi |
|----------|------|----------|
| Kiểm thử đơn vị | Hàm độc lập, utility, component | Individual units |
| Kiểm thử tích hợp | API endpoint, thao tác database | Integration points |
| Kiểm thử đầu cuối | Luồng người dùng quan trọng | Critical user flows |

### Phát triển theo test (TDD)

**Quy trình bắt buộc**:

```
1. Viết kiểm thử (RED)   - Viết kiểm thử fail trước
2. Chạy kiểm thử          - Xác nhận kiểm thử fail
3. Viết triển khai tối thiểu (GREEN) - Viết code ít nhất để kiểm thử pass
4. Chạy kiểm thử          - Xác nhận kiểm thử pass
5. Tái cấu trúc (IMPROVE)    - Cải thiện cấu trúc mã, giữ kiểm thử xanh
6. Xác minh coverage        - Đảm bảo đạt 80%+
```

## Chi tiết triển khai

### Mẫu AAA (Arrange-Act-Assert)

Tất cả kiểm thử phải tuân theo cấu trúc AAA:

```typescript
test('tính cosine similarity đúng', () => {
  // Arrange - Chuẩn bị dữ liệu kiểm thử
  const vector1 = [1, 0, 0];
  const vector2 = [0, 1, 0];

  // Act - Thực hiện thao tác được kiểm thử
  const similarity = calculateCosineSimilarity(vector1, vector2);

  // Assert - Xác minh kết quả
  expect(similarity).toBe(0);
});
```

### Tiêu chuẩn đặt tên kiểm thử

Sử dụng tên mô tả giải thích hành vi được kiểm thử:

```typescript
// Khuyến nghị - Mô tả hành vi mong đợi
test('trả về mảng rỗng khi không có thị trường nào khớp query', () => {});
test('ném lỗi khi thiếu API key', () => {});
test('fallback về tìm kiếm substring khi Redis không khả dụng', () => {});

// Tránh - Đặt tên mơ hồ
test('test1', () => {});
test('edge case', () => {});
```

### Xử lý lỗi kiểm thử

Khi kiểm thử fail:

| Bước | Hành động |
|------|----------|
| 1 | Sử dụng agent **tdd-guide** |
| 2 | Kiểm tra cô lập kiểm thử |
| 3 | Xác minh Mock đúng |
| 4 | Sửa triển khai, không phải kiểm thử (trừ khi kiểm thử sai) |

### Hỗ trợ Agent

| Agent | Mục đích | Thời điểm sử dụng |
|-------|------|----------|
| **tdd-guide** | Hướng dẫn phát triển theo test | Bắt buộc khi phát triển tính năng mới, sửa lỗi |
| **code-reviewer** | Kiểm tra mã | Sau khi viết mã |

## Xử lý vi phạm

### Coverage không đủ

- Mã có coverage dưới 80% không được merge
- CI/CD pipeline phải ngăn commit có coverage thấp
- Sử dụng `c8` hoặc công cụ tương tự tạo báo cáo coverage

### Xử lý kiểm thử fail

```
1. Phân tích nguyên nhân fail
2. Chẩn đoán là vấn đề triển khai hay vấn đề kiểm thử
3. Nếu là vấn đề triển khai - Sửa mã triển khai
4. Nếu là vấn đề kiểm thử - Sửa mã kiểm thử
5. Đảm bảo tất cả kiểm thử pass trước khi commit
```

### Vấn đề thường gặp

| Vấn đề | Nguyên nhân | Giải pháp |
|------|------|----------|
| Kiểm thử không ổn định | Thao tác bất đồng bộ, race condition | Tăng thời gian chờ, sử dụng mock |
| Cô lập kiểm thử thất bại | Chia sẻ state gây ô nhiễm | Reset state trước mỗi kiểm thử |
| Mock không đúng | Giá trị mong đợi không khớp thực tế | Xác minh thiết lập mock đúng |

## Quy tắc liên quan

| Quy tắc liên quan | Mô tả |
|----------|----------|
| Quy tắc phong cách mã | Khả năng đọc và khả năng bảo trì mã |
| Quy tắc bảo mật | Yêu cầu kiểm thử bảo mật |
| Quy tắc kiểm tra mã | Kiểm tra coverage kiểm thử |
| Quy trình phát triển | Tích hợp quy trình TDD |