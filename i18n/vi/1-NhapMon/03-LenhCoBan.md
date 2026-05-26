# 03 - Lệnh cơ bản

## Các loại lệnh

ECC cung cấp ba loại lệnh:

### 1. Lệnh gạch chéo (Slash Commands)

Sử dụng trực tiếp:

| Lệnh | Chức năng | Ví dụ |
|------|------|------|
| `/ecc:plan` | Tạo kế hoạch triển khai | `/ecc:plan "Thêm xác thực người dùng"` |
| `/ecc:code-review` | Review code | `/ecc:code-review` |
| `/ecc:build-fix` | Sửa lỗi build | `/ecc:build-fix` |
| `/ecc:learn` | Trích xuất pattern từ session | `/ecc:learn` |
| `/ecc:skill-create` | Tạo Skill từ lịch sử Git | `/ecc:skill-create` |
| `/ecc:sessions` | Quản lý lịch sử session | `/ecc:sessions` |

Lưu ý: Phiên bản cài thủ công có thể dùng lệnh không có tiền tố (như `[/plan](../TaiLieuThamKhao/commands/01-核心工作流.md)`), phiên bản plugin dùng tiền tố `/ecc:`.

### 2. Skills (Bề mặt workflow chính)

Skills là cách gọi workflow chính của ECC:

| Skill | Chức năng |
|-------|------|
| `[tdd-workflow](../TaiLieuThamKhao/skills/测试.md)` | Phát triển driven bằng test |
| `[e2e-testing](../TaiLieuThamKhao/skills/测试.md)` | Test đầu cuối |
| `security-review` | Review bảo mật |
| `verification-loop` | Vòng lặp verification |
| `search-first` | Workflow nghiên cứu trước |

Cách gọi:
```
Sử dụng skill [tdd-workflow](../TaiLieuThamKhao/skills/测试.md)
```

### 3. Lệnh Agent

Thực thi sub-task qua Agent:

| Agent | Mục đích |
|-------|------|
| `planner` | Lập kế hoạch triển khai |
| `code-reviewer` | Review code |
| `build-error-resolver` | Sửa lỗi build |
| `security-reviewer` | Review bảo mật |
| `architect` | Thiết kế kiến trúc |

Cách gọi:
```
@planner
```

## Workflow thường dùng

### Phát triển tính năng mới

```
/ecc:plan "Thêm xác thực người dùng"
→ planner tạo blueprint triển khai
Sử dụng skill [tdd-workflow](../TaiLieuThamKhao/skills/测试.md)
→ tdd-guide bắt buộc viết test trước
/ecc:code-review
→ code-reviewer kiểm tra code
```

### Sửa Bug

```
Sử dụng skill [tdd-workflow](../TaiLieuThamKhao/skills/测试.md)
→ tdd-guide: Viết test reproduce
→ Thực hiện fix, xác minh test pass
/ecc:code-review
→ code-reviewer: Kiểm tra regression
```

### Chuẩn bị deploy

```
/ecc:security-scan
→ security-reviewer: Audit OWASP Top 10
Sử dụng skill [e2e-testing](../TaiLieuThamKhao/skills/测试.md)
→ e2e-runner: Test các user flow quan trọng
/ecc:test-coverage
→ Xác minh coverage 80%+
```

## Lệnh MCP

| Lệnh | Chức năng |
|------|------|
| `/mcp` | Quản lý MCP server |
| `/ecc:status` | Kiểm tra trạng thái |

## Bước tiếp theo

- [Nhiệm vụ đầu tiên](./04-NhiemVuDauTien.md) - Hoàn thành nhiệm vụ đầu tiên của bạn
- [Quay lại thư mục nhập môn](../README.md)