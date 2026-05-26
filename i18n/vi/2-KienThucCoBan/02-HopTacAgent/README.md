# 02 Agent协作

> Hiểu trách nhiệm của 6 loại Agent, tổ hợp sử dụng để hoàn thành nhiệm vụ phát triển phức tạp

## Mục tiêu module

Sau khi hoàn thành module này, bạn sẽ có khả năng:

- Nhận diện 6 loại Agent của Claude Code và trách nhiệm của từng loại
- Chọn Agent phù hợp theo loại nhiệm vụ
- Tổ hợp nhiều Agent để hoàn thành nhiệm vụ phức tạp
- Hiểu mô hình và best practices của Agent协作

## Hệ thống phân loại Agent

Agent của Claude Code chia thành 6 loại lớn:

| Loại Agent | Trách nhiệm cốt lõi | Kịch bản sử dụng |
|------------|----------|----------|
| **审查类** | Review chất lượng code, pattern chuẩn, lỗ hổng bảo mật | Review code, dọn tech debt |
| **构建类** | Biên dịch build, sửa lỗi, quản lý dependency | Build thất bại, lỗi biên dịch |
| **规划类** | Phân rã nhiệm vụ, thiết kế kiến trúc, phương án kỹ thuật | Tính năng mới, thay đổi kiến trúc |
| **测试类** | Viết test, nâng cao coverage, chiến lược test | TDD, bổ sung test |
| **安全类** | Audit bảo mật, quét lỗ hổng, kiểm tra tuân thủ | Code nhạy cảm bảo mật, module thanh toán |
| **架构类** | Thiết kế hệ thống, chọn pattern, lựa chọn kỹ thuật | Refactor lớn, tiến hóa kiến trúc |

## 6 loại Agent chi tiết

### 1. Agent审查类

**Trách nhiệm**: Review chất lượng code, kiểm tra tuân thủ pattern

Agent khả dụng:
- `code-reviewer` - Review code thông thường
- `python-reviewer` - Review chuyên biệt Python
- `go-reviewer` - Review ngôn ngữ Go
- `rust-reviewer` - Review bảo mật Rust
- `security-reviewer` - Review lỗ hổng bảo mật

**Kịch bản sử dụng**:
- Kiểm tra chất lượng trước khi commit code
- Phát hiện code smell tiềm ẩn
- Review code bên thứ ba

### 2. Agent构建类

**Trách nhiệm**: Biên dịch build, sửa lỗi, quản lý dependency

Agent khả dụng:
- `build-error-resolver` - Phân tích lỗi build
- `go-build` - Build Go
- `rust-build` - Build Rust
- `kotlin-build` - Build Kotlin
- `flutter-build` - Build Flutter

**Kịch bản sử dụng**:
- Sửa lỗi khi build thất bại
- Giải quyết xung đột dependency
- Vấn đề build cross-platform

### 3. Agent规划类

**Trách nhiệm**: Phân rã nhiệm vụ, thiết kế kiến trúc, kế hoạch triển khai

Agent khả dụng:
- `planner` - Tạo kế hoạch triển khai
- `architect` - Thiết kế kiến trúc
- `tdd-guide` - Hướng dẫn workflow TDD

**Kịch bản sử dụng**:
- Lập kế hoạch phát triển tính năng mới
- Đánh giá phương án kỹ thuật
- Phân rã nhiệm vụ phức tạp

### 4. Agent测试类

**Trách nhiệm**: Viết test, nâng cao coverage, chiến lược test

Agent khả dụng:
- `tdd-guide` - Phát triển driven bằng test
- `e2e-runner` - Test đầu cuối
- `test-coverage` - Phân tích coverage

**Kịch bản sử dụng**:
- Viết test cho tính năng mới
- Nâng cao test coverage
- Thiết kế kịch bản E2E test

### 5. Agent bảo mật类

**Trách nhiệm**: Audit bảo mật, quét lỗ hổng, kiểm tra tuân thủ

Agent khả dụng:
- `security-reviewer` - Review lỗ hổng bảo mật
- `security-scan` - Quét bảo mật tự động

**Kịch bản sử dụng**:
- Review code thanh toán và xác thực
- Kiểm tra xử lý dữ liệu bên ngoài
- Xác minh tuân thủ

### 6. Agent kiến trúc类

**Trách nhiệm**: Thiết kế hệ thống, chọn pattern, lựa chọn kỹ thuật

Agent khả dụng:
- `architect` - Thiết kế kiến trúc
- `refactor-cleaner` - Hướng dẫn refactor
- `api-design-reviewer` - Review thiết kế API

**Kịch bản sử dụng**:
- Tách microservice
- Thiết kế database
- Tiến hóa kiến trúc API

## Mô hình Agent协作

### Mô hình 1: Tuần tự协作

```
Nhiệm vụ → Planner → 实施 → Reviewer → Test → Hoàn thành
```

Phù hợp với: Quy trình tuyến tính, các bước có phụ thuộc

### Mô hình 2: Song song协作

```
            → Reviewer A ─→
Nhiệm vụ → Phân rã ─→ Reviewer B ─→ Tổng hợp → Hoàn thành
            → Reviewer C ─→
```

Phù hợp với: Review đa góc nhìn, review module độc lập

### Mô hình 3: 级联协作

```
Nhiệm vụ lớn → Architect → Planner → 实施 →各级 Reviewer → Merge
```

Phù hợp với: Dự án phức tạp, phát triển tính năng lớn

## Ví dụ tổ hợp Agent

### Kịch bản: Phát triển tính năng mới

```
1. Sử dụng /plan lập kế hoạch tính năng
2. Sử dụng tdd-guide phát triển TDD
3. Sử dụng code-reviewer review code
4. Sử dụng security-reviewer kiểm tra bảo mật
5. Sử dụng verify xác minh tính năng
```

### Kịch bản: Sửa lỗ hổng bảo mật

```
1. Sử dụng security-reviewer quét lỗ hổng
2. Sử dụng planner soạn kế hoạch sửa
3. Sử dụng build-fix thực hiện sửa
4. Sử dụng security-reviewer xác minh lại
5. Sử dụng verify xác nhận sửa
```

## Hướng dẫn chọn Agent

| Loại nhiệm vụ | Agent khuyến nghị | Lý do |
|----------|------------|------|
| Phát triển tính năng mới | planner + tdd-guide | Cần lập kế hoạch và TDD |
| Code review | code-reviewer + security-reviewer | Kiểm tra kép chất lượng và bảo mật |
| Build thất bại | build-error-resolver | Tập trung sửa lỗi |
| Refactor kiến trúc | architect + refactor-cleaner | Hướng dẫn ở cấp độ kiến trúc |
| Nhạy cảm bảo mật | security-reviewer (ưu tiên) | Bắt buộc phải qua bảo mật |
| Viết test | tdd-guide + test-coverage | Chiến lược test và coverage |

## Tài liệu học tập đi kèm

Tài liệu Agent đầy đủ nằm ở：`../../TaiLieuThamKhao/agents/01-Kiem-Duyet-Ma.md`

## Bước tiếp theo

- Học [bài tập](./exercises/练习.md)
- Đọc [tài liệu đầy đủ về Agent](../../TaiLieuThamKhao/agents/01-Kiem-Duyet-Ma.md)