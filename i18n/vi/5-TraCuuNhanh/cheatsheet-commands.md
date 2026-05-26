# Tra cứu lệnh

## Lệnh gạch chéo

| Lệnh | Chức năng | Kịch bản sử dụng |
|------|------|----------|
| `/tdd` | Quy trình phát triển định hướng kiểm thử | Phát triển tính năng mới |
| `/plan` | Tạo kế hoạch thực hiện | Lập kế hoạch nhiệm vụ phức tạp |
| `/code-review` | [Kiểm tra mã](../TaiLieuThamKhao/commands/01-核心工作流.md) | Kiểm tra chất lượng mã |
| `/build-fix` | [Sửa lỗi build](../TaiLieuThamKhao/commands/04-构建修复.md) | Khi build thất bại |
| `/learn` | Trích xuất mẫu từ phiên họp | Tích lũy kiến thức |
| `/skill-create` | Tạo Skill từ Git | Tái sử dụng kinh nghiệm |
| `/test` | [Lệnh liên quan đến kiểm thử](../TaiLieuThamKhao/commands/02-测试相关.md) | Liên quan đến kiểm thử |

## Lệnh script

| Script | Chức năng |
|------|------|
| `node scripts/hooks/run-with-flags.js` | Chạy hook với các cờ |
| `node tests/run-all.js` | Chạy toàn bộ kiểm thử |
| `node scripts/lib/utils.js` | Thư viện hàm tiện ích |

## Lệnh Agent

| Agent | Chức năng |
|-------|------|
| `/planner` | Lập kế hoạch nhiệm vụ |
| `/code-reviewer` | Kiểm tra mã |
| `/tdd-guide` | Hướng dẫn TDD |
| `/security-reviewer` | Kiểm tra bảo mật |
| `/build-error-resolver` | Sửa lỗi build |

## Tham số toàn cục

| Tham số | Mô tả |
|------|------|
| `--model` | Chỉ định mô hình |
| `--no-input` | Chế độ không tương tác |
| `--output` | Định dạng đầu ra |

## Biến môi trường Hook

| Biến | Mô tả |
|------|------|
| `ECC_HOOK_PROFILE` | Cấu hình hook |
| `ECC_DISABLED_HOOKS` | Danh sách hook bị vô hiệu hóa |

---

[Quay lại mục lục tra cứu nhanh](./README.md)