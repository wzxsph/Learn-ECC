# Lệnh Workflow Cơ Bản

Tài liệu này giới thiệu các lệnh workflow cốt lõi trong ECC.

---

## /plan

**Mục đích**: Diễn giải lại yêu cầu, đánh giá rủi ro, và tạo kế hoạch thực hiện từng bước. **Chờ xác nhận từ người dùng** trước khi chạm bất kỳ code nào.

**Cách sử dụng**:
```
/plan [mô tả tính năng | đường dẫn file PRD]
```

**Khi nào sử dụng**:
- Bắt đầu phát triển tính năng mới
- Thực hiện thay đổi kiến trúc lớn
- Refactoring phức tạp
- Yêu cầu không rõ ràng hoặc mơ hồ

**Chi tiết**:
Lệnh này sẽ:
1. **Diễn giải lại yêu cầu** - Biểu đạt rõ ràng những gì cần xây dựng
2. **Xác định rủi ro** - Phát hiện vấn đề và rào cản tiềm ẩn
3. **Tạo kế hoạch từng bước** - Chia nhỏ implementation thành các giai đoạn
4. **Chờ xác nhận** - Phải có approval rõ ràng từ người dùng trước khi tiếp tục

Hỗ trợ nhiều chế độ input:
| Input | Chế độ | Hành vi |
|------|------|------|
| `path/to/name.prd.md` | Chế độ PRD | Đọc PRD, chọn delivery milestone tiếp theo |
| Đường dẫn markdown khác | Chế độ reference | Đọc file làm context và tạo plan inline |
| Text free-form | Chế độ dialog | Tạo plan inline |
| Input trống | Chế độ clarify | Hỏi cần lên kế hoạch gì |

---

## /code-review

**Mục đích**: Review code toàn diện cho thay đổi local chưa commit hoặc GitHub PR.

**Cách sử dụng**:
```
/code-review                 # Chế độ review local
/code-review [số PR | link PR]  # Chế độ PR review
```

**Khi nào sử dụng**:
- Review toàn diện trước khi commit code
- PR review để phát hiện vấn đề tiềm ẩn
- Học hỏi best practice từ codebase

**Chế độ Review Local**:
1. Thu thập các file đã thay đổi (`git diff --name-only HEAD`)
2. Kiểm tra vấn đề bảo mật (hardcoded credentials, SQL injection, XSS, etc.)
3. Kiểm tra code quality (hàm >50 dòng, file >800 dòng, etc.)
4. Tạo báo cáo có đánh dấu mức độ nghiêm trọng

**Chế độ PR Review**:
1. Lấy metadata và diff của PR
2. Kiểm tra spec và docs liên quan của project
3. Đọc toàn bộ file đã thay đổi
4. Áp dụng 7 category review (correctness, type safety, pattern compliance, security, performance, completeness, maintainability)
5. Chạy verification command (type check, lint, test, build)
6. Đăng review lên GitHub

---

## /build-fix

**Mục đích**: Phát hiện system build của project và từng bước sửa lỗi build/type.

**Cách sử dụng**:
```
/build-fix
```

**Khi nào sử dụng**:
- Khi build thất bại
- Lỗi kiểu TypeScript/JavaScript
- Lỗi compile
- Vấn đề phụ thuộc

**Quy trình làm việc**:
1. **Phát hiện build system** - Chọn lệnh build phù hợp theo loại file
2. **Phân tích lỗi** - Nhóm theo file và thứ tự phụ thuộc
3. **Sửa từng bước** - Mỗi lần sửa một lỗi
4. **Xác minh** - Chạy lại build sau mỗi lần sửa

Build systems được hỗ trợ:

| Indicator | Lệnh build |
|--------|----------|
| `package.json` + script `build` | `npm run build` |
| `tsconfig.json` (TypeScript) | `npx tsc --noEmit` |
| `Cargo.toml` | `cargo build` |
| `pom.xml` | `mvn compile` |
| `build.gradle` | `./gradlew compileJava` |
| `go.mod` | `go build ./...` |
| `pyproject.toml` | `python -m compileall` hoặc `mypy` |

---

## /verify

**Mục đích**: Xác minh code change có hoạt động như mong đợi. Dùng để verify PR, xác nhận fix hoặc test change.

**Cách sử dụng**:
```
/verify [quick]           # Xác minh nhanh (khuyến khích)
/verify full             # Xác minh đầy đủ
```

**Khi nào sử dụng**:
- Xác minh PR change
- Xác nhận fix có hiệu quả
- Test thay đổi thủ công
- Xác minh local trước khi push

**Các bước xác minh**:
1. Chạy test liên quan
2. Kiểm tra type safety
3. Xác minh build thành công
4. Kiểm tra linting

---

## /quality-gate

**Mục đích**: Chạy ECC quality pipeline cho file hoặc project scope và báo cáo các bước sửa.

**Cách sử dụng**:
```
/quality-gate [path|.] [--fix] [--strict]
```

**Khi nào sử dụng**:
- Chạy quality check theo yêu cầu
- Tích hợp vào CI pipeline
- Đảm bảo code đáp ứng standard của project

**Các bước pipeline**:
1. Phát hiện ngôn ngữ/tool của target
2. Chạy formatter check
3. Chạy lint/type check
4. Tạo danh sách fix ngắn gọn

---

## /tdd

**Mục đích**: Workflow phát triển driven bằng test. Tuân theo vòng red-green-refactor.

**Cách sử dụng**:
```
/tdd                      # Khởi động workflow TDD
```

**Khi nào sử dụng**:
- Implement tính năng mới
- Fix bug (viết test thất bại trước)
- Thêm test coverage
- Học phương pháp TDD

**Vòng TDD**:
```
RED    → Viết test thất bại
GREEN  → Viết code tối thiểu để test pass
REFACTOR → Cải thiện code, giữ test xanh
REPEAT → Test case tiếp theo
```

**Best practice**:
- **Viết test trước** - Trước bất kỳ implementation nào
- **Chạy test sau mỗi thay đổi** - Xác minh tiến độ
- **Test behavior, không implementation** - Tập trung vào interface không phải chi tiết
- **Bao gồm boundary case** - Null, empty, max, etc.

---

## Mối quan hệ tích hợp lệnh

```
Yêu cầu không rõ → /plan-prd (tạo PRD)
     ↓
Yêu cầu rõ ràng → /plan (tạo implementation plan)
     ↓
/tdd (hoặc /feature-dev) implement
     ↓
/build-fix sửa lỗi build
     ↓
/code-review review code
     ↓
/quality-gate quality gate
     ↓
/verify xác minh
     ↓
/pr tạo PR
```