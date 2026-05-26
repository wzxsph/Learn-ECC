# Tra cứu quy trình

## Các mẫu quy trình thường dùng

### 1. Quy trình phát triển TDD

```
Yêu cầu → Viết kiểm thử(RED) → Triển khai(GREEN) → Tái cấu trúc(REFACTOR)
```

**Sử dụng**: `/tdd`

---

### 2. Quy trình kiểm tra mã

```
Commit mã → Khởi động kiểm tra → Phân loại vấn đề → Sửa chữa → Kiểm tra lại → Merge
```

**Sử dụng**: `/code-review`

---

### 3. Quy trình sửa lỗi

```
Phát hiện lỗi → Phân tích nguyên nhân → Định vị mã → Sửa chữa → Xác minh → Kiểm thử
```

**Sử dụng**: `/build-fix`

---

### 4. Quy trình tạo Skill

```
Phân tích yêu cầu → Tham khảo mẫu có sẵn → Viết mẫu → Kiểm tra xác minh
```

**Sử dụng**: `/skill-create`

---

## Quy trình tùy chỉnh

Định dạng định nghĩa quy trình:

```yaml
name: my-workflow
description: Quy trình của tôi
steps:
  - name: step1
    command: do-something
  - name: step2
    command: do-other
    requires: [step1]
```

---

[Quay lại mục lục tra cứu nhanh](./README.md)