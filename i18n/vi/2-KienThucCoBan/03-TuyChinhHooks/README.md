# 03 - Tùy chỉnh Hooks

> Học cách viết PreToolUse, PostToolUse và Stop Hooks, tự động hóa quality gate

## Mục tiêu module

Sau khi hoàn thành module này, bạn sẽ có khả năng:

- Hiểu các loại Hook của Claude Code và cơ chế trigger
- Tạo PreToolUse Hook để xác thực trước khi công cụ thực thi
- Tạo PostToolUse Hook để xử lý sau khi công cụ thực thi
- Cấu hình quality gate Hook để implement kiểm tra tự động

## Tổng quan loại Hook

| Loại Hook | Thời điểm trigger | Mục đích điển hình |
|-----------|----------|----------|
| **PreToolUse** | Trước khi công cụ thực thi | Xác thực tham số, kiểm tra quyền, quét bảo mật |
| **PostToolUse** | Sau khi công cụ thực thi | Format kết quả, ghi log, xử lý tự động |
| **Stop** | Khi kết thúc session | Tóm tắt session, lưu trạng thái, cleanup |

## PreToolUse Hook

### Nguyên lý hoạt động

```
Người dùng nhập → PreToolUse Hook → Công cụ thực thi → Kết quả trả về
```

### Kịch bản phổ biến

1. **Xác thực bảo mật**
   - Kiểm tra lệnh nguy hiểm (như `rm -rf`)
   - Xác thực đường dẫn file an toàn
   - Kiểm tra truy cập dữ liệu nhạy cảm

2. **Xác thực tham số**
   - Xác thực format tham số công cụ
   - Kiểm tra tham số bắt buộc có tồn tại không
   - Kiểm tra kiểu

3. **Kiểm soát quyền**
   - Kiểm tra có được phép thực hiện thao tác không
   - Xác thực quyền truy cập dự án

### Ví dụ cấu hình Hook

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('rm -rf')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "Cảnh báo: Phát hiện lệnh nguy hiểm rm -rf, vui lòng xác nhận an toàn trước khi thực thi!"
    }
  ]
}
```

## PostToolUse Hook

### Nguyên lý hoạt động

```
Công cụ thực thi → PostToolUse Hook → Kết quả trả về → Người dùng
```

### Kịch bản phổ biến

1. **Format kết quả**
   - Làm đẹp output format
   - Trích xuất thông tin quan trọng
   - Đơn giản hóa output dài

2. **Xử lý tự động**
   - Build thành công thì tự động chạy test
   - Test thất bại thì tự động ghi log
   - Trước commit thì tự động format code

3. **Kiểm tra chất lượng**
   - Kiểm tra thông báo lỗi trả về
   - Xác thực tính bảo mật của output
   - Phân tích dữ liệu hiệu năng

### Ví dụ cấu hình Hook

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm test')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Test hoàn thành: Đang kiểm tra coverage...'",
      "executeAfter": true
    }
  ]
}
```

## Stop Hook

### Nguyên lý hoạt động

```
Tín hiệu kết thúc session → Stop Hook → Thực thi cleanup → Session kết thúc
```

### Kịch bản phổ biến

1. **Tóm tắt session**
   - Tạo tóm tắt session
   - Ghi lại các nhiệm vụ đã hoàn thành
   - Liệt kê待办事项

2. **Lưu trạng thái**
   - Lưu file nháp
   - Persist tiến độ
   - Dọn file tạm

3. **Gửi thông báo**
   - Gửi thông báo hoàn thành
   - Tóm tắt báo cáo
   - Trigger quy trình tiếp theo

## Cấu trúc cấu hình Hook

```json
{
  "matcher": {
    "tool": "Tên công cụ",
    "operation": "Loại thao tác",
    "condition": "Biểu thức điều kiện"
  },
  "hooks": [
    {
      "type": "notification | command | stop",
      "when": "before | after",
      "message": "Thông báo",
      "command": "Lệnh cần thực thi",
      "continue": true
    }
  ]
}
```

## Kịch bản Hook thường dùng

### Kịch bản 1: Kiểm tra trước Git commit

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('git commit')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "npm test",
      "continue": false
    }
  ]
}
```

### Kịch bản 2: Thông báo build thành công

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm run build')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "Build hoàn thành! Đang chạy test...",
      "executeAfter": true
    }
  ]
}
```

### Kịch bản 3: Tóm tắt kết thúc session

```json
{
  "matcher": {
    "type": "stop"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "Session kết thúc. Hoàn thành: {summary}",
      "command": "node scripts/save-session.js"
    }
  ]
}
```

## Best practices phát triển Hook

1. **Giữ đơn giản**: Logic của Hook nên đơn giản và trực tiếp
2. **Thực thi nhanh**: PreToolUse Hook nên hoàn thành trong 200ms
3. **Degradegraceful**: Khi thất bại thì nên tiếp tục thực thi, không block
4. **Log rõ ràng**: Sử dụng tiền tố `[HookName]` để đánh dấu log
5. **Coveragetest**: Viết test cho Hook

## Tài liệu học tập đi kèm

Tài liệu Hooks đầy đủ nằm ở：`../../TaiLieuThamKhao/hooks/Hook类型.md`

## Bước tiếp theo

- Học [bài tập](./exercises/练习.md)
- Đọc [tài liệu đầy đủ về Hooks](../../TaiLieuThamKhao/hooks/Hook类型.md)