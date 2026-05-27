# Lệnh Hệ Thống Hookify

## Tổng quan

Hệ thống Hookify dùng để tạo và quản lý hooks, ngăn chặn behavior không mong muốn và tự động hóa workflow。

## Danh sách lệnh

### /hookify

**Mục đích**: Tạo hooks để ngăn behavior không mong muốn

**Mô tả**: Tạo custom hooks để ngăn chặn behavior không mong muốn.

**Khi nào sử dụng**:
- Ngăn chặn accidental delete operation
- Block dangerous command
- Auto formatting
- Quality check

**Hook types**:
- **PreToolUse**: Trước tool execution
- **PostToolUse**: Sau tool execution
- **Stop**: Khi phiên kết thúc

---

### /hookify-list

**Mục đích**: Liệt kê tất cả hookify rule đã configure

**Mô tả**: Hiển thị tất cả hookify rule đã configure hiện tại.

---

### /hookify-configure

**Mục đích**: Interactive enable/disable hookify rule

**Mô tả**: Interactive configure enable/disable state của hookify rule.

---

### /hookify-help

**Mục đích**: Hookify system help

**Mô tả**: Lấy detailed help information về Hookify system.

---

## Hook configuration example

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## Các lệnh liên quan

- `/hookify` - Tạo hook
- `/hookify-list` - Liệt kê rule
- `/hookify-configure` - Configure rule
- `/hookify-help` - Lấy help